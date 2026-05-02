---
id: cpzdioi50c3wtfhkgovls9b
title: Ai_read
desc: ''
updated: 1776241883814
created: 1776238154116
---

## 模块一：核心数据结构剖析 (Core Data Structures)

### 一、分类
1. **上下文/管理类**：把控全局生命周期与程序切换（如 `nvs_prog_mgr_t`, `nvs_prog_t`）。
2. **业务载体类**：封装具体算法逻辑的标准单元（如 `nvs_node_t`, `nvs_node_ops_t`）。
3. **配置/拓扑类**：声明数据流向与执行依赖（如 `nvs_node_inputs_info_t`）。
4. **数据总线类**：承载节点间零拷贝数据交换的共享内存模型（如 `nvs_prog_pipefrm_t`）。

### 二、 核心数据结构拆解

#### 1. `nvs_prog_t` (程序主上下文)
* **结构体名片**：定义于 `nvs_prog.h`。扮演“车间主任”角色，代表一个完整且独立的视觉检测配方。
* **生命周期**：由 `nvs_prog_create` 动态分配，存活周期长，伴随该配方直到被删除或系统销毁时调用 `nvs_prog_destroy` 释放。
* **核心成员**：
  * `nvs_node_mng_list_t atool_nodes`：业务工具容器（底层为 std::map），存放 OCR 等具体工具。
  * `nvs_prog_controller_t *prog_controller`：调度引擎，负责运行时的排序与调用。
  * `nvs_node_t *sched_node_seq[]`：一维数组，存放拓扑排序后**确定好的执行顺序**。
  * `nvs_prog_pipefrm_t *pipeline_frame`：当前配方的公共数据黑板。
* **关联接口**：`nvs_prog_init()`, `nvs_prog_run_once()`, `nvs_prog_add_tool()`。

#### 2. `nvs_node_t` (通用工具节点)
* **结构体名片**：定义于 `nvs_node.h`。扮演“加工流水线工位”角色，是所有底层算法（OCR、定位）的标准外壳。
* **生命周期**：配方加载或用户添加时由工厂函数动态分配，随节点被删除（`nvs_prog_del_tool`）时触发 `ops->destroy` 释放。
* **核心成员**：
  * `char node_id_str[]`：工具的唯一身份（如 "T00"）。
  * `nvs_node_inputs_info_t input_info`：记录该节点运行前**必须具备的输入依赖**。
  * `nvs_node_ops_t *ops`：函数指针表（虚函数表），定义了初始化、运行、学习等标准动作。
  * `void *priv`：指向具体工具的私有结构体（如你定义的 `nvs_ocr_init_param_t` 可存在此处）。
* **关联接口**：`nvs_node_call_ops_set_config_json_obj()`, `nvs_node_call_ops_process()`。

#### 3. `nvs_prog_pipefrm_t` (公共数据黑板)
* **结构体名片**：定义于 `nvs_pipeframe.h`。扮演“数据总线”角色，解决节点间的高耦合问题。
* **生命周期**：随 `nvs_prog_t` 创建。它不拥有业务数据的内存，只借用指针。每一帧检测结束后，挂载的临时指针会被清空。
* **核心成员**：基于字符串 Key（如 "I0", "L01"）映射到对应数据内存块的哈希表结构。
* **关联接口**：`nvs_prog_pf_set_obj()` (写入), `nvs_prog_pf_get_obj()` (读取)。

### 三、 架构层级与数据流向
* **静态包含关系树**：
  ```text
  [Owns] nvs_prog_mgr_t
    └── [Points to] nvs_prog_t *progs[]
          ├── [Owns] nvs_prog_pipefrm_t *pipeline_frame
          └── [Owns] nvs_node_mng_list_t atool_nodes
                └── [Points to] nvs_node_t *node
                      ├── [Owns] nvs_node_inputs_info_t (输入依赖)
                      └── [Points to] nvs_node_ops_t *ops (行为模板)
  ```
* **动态数据流向图**：节点 A（产生数据） -> 调用 `pf_set_obj` 挂载到 `pipeline_frame` -> 节点 B（需用数据） -> 运行时调用 `pf_get_obj` 从 `pipeline_frame` 取出。全过程 A 与 B 互不知晓。

---

## 模块二：核心调用链路和时序 (Call Chains & Workflows)

### 一、 核心业务场景划分
NP 模块主要承载三大核心场景：
1. **配置加载与节点初始化**（系统启动或 UI 点击保存时）。
2. **单次图像检测的主循环**（相机硬件触发时）。
3. **交互式学习模式控制**（用户录入标准模板时）。

### 二、 关键链路时序拆解

#### 场景 1：配置加载与节点初始化 (Config Load)
* **触发起点**：上位机（或本地配置服务）调用全局 API 传入全量 JSON 配置文件。
* **核心调用栈**：
  `nvs_pm_set_full_params_json_obj`
  `-> nvs_prog_import_prog_priv_param_json_obj` (解析单程序配置)
  `--> nvs_prog_import_node_list_routine` (遍历 atool 节点数组)
  `---> nvs_prog_add_tool` (若不存在则创建工具外壳)
  `---> nvs_node_call_ops_set_config_json_obj(node, param_json)` 
* **【💡 开发注入点】**：最后的 `set_config` 将触发 OCR 节点的具体实现。这里是你解析 JSON 中的 `scene` 和 `perf`，组合成结构体并调用 `nvs_alg_ocr_create(param)` 初始化底层算法的地方。

#### 场景 2：单次图像检测的主循环 (Run Once)
* **触发起点**：硬件触发或外部软件调用 `nvs_prog_run_once`。
* **核心调用栈**：
  `nvs_prog_run_once`
  `-> nvs_prog_controller_schedule_update` (检查配置变更，执行拓扑排序)
  `-> nvs_prog_controller_call_nodes` (进入死循环，遍历 `sched_node_seq`)
  `--> nvs_node_call_ops_process(node, prog, pipeline_frame)`
  `-> nvs_prog_call_output_cb` (触发向上层的数据推送)
* **【💡 开发注入点】**：`process` 是算法执行地。在这里，你通过 `pipeline_frame` 拿到原图和定位矩阵，执行底层推理，并在拿到 raw results 后，使用 C 语言字符串操作执行**宽松匹配逻辑**，最后将结果存入 `node_res`。

#### 场景 3：交互式学习模式 (ML Mode)
* **触发起点**：用户在 UI 端选中某工具，并指定一张图像作为标准模板。
* **核心调用栈**：
  `nvs_prog_learning_mode_add_master_img(prog, "T01", "img_id")`
  `-> nvs_prog_node_list_get_obj` (找到你的 OCR 节点)
  `-> nvs_node_call_ops_ml_add_sample_frm_master_ctl_img(node, img_ptr, ...)`
* **状态流转**：节点从 `NORMAL` 态切入 `LEARNING` 态。框架充当搬运工，将存储层的物理图像指针 `img_ptr` 直接“喂”给工具节点，工具自行调用算法完成特征提取与保存。

### 三、 异常与分支逻辑
* **核心异常点**：依赖缺失。如果 UI 连线错误，导致 OCR 需要 L01 但 L01 被删除了。
* **容错机制**：框架极其健壮。在 `nvs_prog_run_once` 执行前，如果检测到依赖无效，调用 `nvs_prog_delete_invalid_node_dependency` 直接剔除脏依赖，确保引擎在排序和执行时不会因空指针引发 Core Dump（段错误）。

---

## 模块三：模块边界与跨层契约 (Boundaries & Interfaces)

### 一、 模块架构定位
* **层次归属**：NP 模块属于视觉系统的**核心业务调度层**。
* **核心职责**：承上启下。它屏蔽了底层具体算法的复杂调用细节，为上层 UI 提供了一套标准化的“节点图编排与执行引擎”。

### 二、 边界与接口定义
1. **向下接口 (Southbound)**：
   * 依赖底层的 `nvs_vi_t`（Video Input）模块获取物理图像资源。
   * 依赖统一的 `nvs_node_ops_t` 接口调用各具体算法的动态链接库（.so/.a）。
2. **向上接口 (Northbound)**：
   * 提供一系列以 `nvs_pm_` 和 `nvs_prog_` 开头的 API 供服务层调用。
   * 通过 `nvs_prog_callback_t output_cb`，在检测完成后主动将渲染图像和结果 JSON 推送给上层。

### 三、 跨层契约与规范 (Contracts)
作为底层开发人员，在接入此框架时需严格遵守以下契约：
1. **内存所有权 (Memory Ownership)**：
   * **JSON 配置**：框架在解析时大量使用了 `cJSON_Duplicate`。NP 层完全接管了配置的生命周期，节点内部**严禁**直接保存入参的 cJSON 指针，必须解析提取为 C 结构体或自行深拷贝。
   * **黑板数据**：`pipeline_frame` 采用“弱引用”。节点可以把 malloc 的内存挂载上去，但 `pipeline_frame` 不负责释放它。节点自身产生的数据结果内存，必须在 `ops->free_res_buf` 中由节点自己清理。
2. **阻塞与异步 (Sync vs Async)**：
   * `nvs_prog_run_once` 是一个**严格同步阻塞**的调用。它会锁住当前执行线程，直到所有算法（包括最耗时的 OCR）全部跑完。因此，节点内部的 `process` 不允许出现死循环或极长的挂起，否则会导致整条流水线节拍超时。