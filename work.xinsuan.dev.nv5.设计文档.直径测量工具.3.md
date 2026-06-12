---
id: dbm5tnlbkkonwdp79b0icdl
title: '3'
desc: ''
updated: 1781167504724
created: 1780919806967
---

# NVS 直径测量工具设计说明

## 1. 功能定义

### 1.1 术语集合
* **候选圆**：设定阶段，算法根据输入参数对全图（或 ROI）扫描后，提取出的所有符合边缘条件的圆集合。
* **指定圆**：下位机根据一定逻辑（如面积最大、或最接近上次选中目标），从“候选圆”集合中挑出的唯一一个圆。它是后续运行测算的基准靶心。
* **结果圆**：运行阶段，算法基于“指定圆”作为初始位置，在小范围内执行局部拟合后，输出的当前帧实际测量圆。

### 1.2 功能点分析与讨论项

* **算法结果的区分：候选圆和结果圆？上下位机的交互实现？**
  * 当前上位机与下位机定义的触发算法运行协议只有 `Trigger_Read`，且不附带参数。但在直径工具中，设定阶段和运行阶段需要算法做不同的事。
  * **建议方案**：在 `ATool_SetParam` 中新增参数 `exec_mode`。下位机根据此值决定后续动作：值为 1，触发算法提取候选圆；值为 0，触发算法运行局部拟合，返回唯一结果。
* **设定阶段，拖动 ROI 触发的算法行为？**
  * 触发的是算法直接输出唯一“结果圆”？还是输出“提取候选圆”再由下位机判断给出“指定圆”？
  * **建议方案**：触发算法提取候选圆 + 下位机判断后返回指定圆。
* **明暗方向中的全部方向和设定方向有什么差别？**
  * iv4 实验中，在高灵敏度情况下，二者的候选圆存在细微差异，但数量均是最多的。是否需要在此处做特殊过滤？
* **抽取直径：用户切换指定圆之后，拖动 ROI 或更新灵敏度/明暗方向等参数，此时产生的新候选圆，下位机按照什么逻辑确定当前候选圆中的指定圆并返回给上位机渲染？**
  * 方案 1：下位机缓存指定圆，候选圆变更后，默认选择与缓存指定圆直径最接近的圆，返回给上位机。
  * 方案 2：同方案 1，差异点：算法提供接口，从候选圆中匹配与旧指定圆最接近的圆。
  * 方案 3：不实现匹配逻辑，直接返回候选圆中居中的圆作为默认指定圆，除非用户手动切换，由用户指定。

---

## 2. 各端操作集合与交互映射

### 2.1 核心操作集合预定义

为了规范描述，消除上下位机交互过程中的语义歧义，特将各端在设定与运行阶段的原子动作定义如下：

#### 上位机指令行为 (A 系列)
* **[A1：上位机 更新内部工具参数缓存（未下发）]** 说明：仅在本地内存暂存用户的 UI 操作数据（如缩放倍率显示、极值按钮置灰等）。
* **[A2：上位机 下发设置参数 ATool_SetParam 指令]**
* **[A3：上位机 下发触发运行 Trigger_Read 指令]** 说明：需通过 `exec_mode` 传递参数让下位机判断是提取候选阶段还是运行阶段。
* **[A4：上位机 下发系统运行控制 Set_AutoSend / Run / Stop 指令]**
* **[A5：上位机 解析候选集并渲染交互图形]** 说明：解析底层提取返回的候选数组，渲染黄色虚线候选圆及绿色指定圆。
* **[A6：上位机 解析唯一结果并渲染实测图形与数值]** 说明：运行态时，解析推流报文，绘制实心结果圆并显示换算后的最终测量值。

#### 下位机动作行为 (B 系列)
* **[B1：下位机 更新业务参数缓存]** 说明：更新下位机内存中的缩放比率、OK/NG 判定上下限等，不触发算法。
* **[B2：下位机 组装配置并调用算法设定]** 说明：解析 JSON，将 ROI 坐标、掩码图、特征极性下发给算法层。
* **[B3：下位机 提取策略判定（选定基准/重关联匹配）]** 说明：业务逻辑。在拿到算法的候选集后，执行挑选最大/最小，或调用算法匹配接口寻找最接近的圆的索引。
* **[B4：下位机 运行态数值换算与阈值判定]** 说明：拿到算法像素结果后，执行乘除法缩放，并与上下限比对生成状态码。
* **[B5：下位机 组装事件报文并推流]** 说明：将最终的数据打包为 JSON，通过 `Report_Result` 返回给上位机。

#### 算法层接口行为 (C 系列)
* **[C1：算法 设置区域接口 nvs_alg_diam_set_region]** (配置)
* **[C2：算法 提取候选圆接口 nvs_alg_diam_extract_candidate]** (运行-设定态)
* **[C3：算法 执行空间匹配接口 nvs_alg_diam_match_circle]** (工具函数)
* **[C4：算法 注入基准圆接口 nvs_alg_diam_set_reference]** (配置)
* **[C5：算法 执行局部测算接口 nvs_alg_diam_run]** (运行-连续态)


### 2.2 交互行为映射表

| 序号  | UI 操作与界面                                                                                                                                 | 上位机行为                                                                                                                           | 下位机行为                                                                                                 | 算法行为                                                                 |
| :---: | :-------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------- |
| **1** | **追加屏蔽**<br>![alt text]({B1F5960D-D64B-4B9A-B93A-5A1E57ECBF9C}.png)                                                                       | [A2：下发 ATool_SetParam]<br>[A3：下发 Trigger_Read]<br>[A5：渲染交互图形]                                                           | [B2：调用算法设定]<br>[B5：组装事件推流]                                                                   | [C1：设置区域]<br>[C2：提取候选圆]                                       |
| **2** | **清除屏蔽**<br>![alt text]({D8AB6C33-E794-489C-B15D-8F500BB86454}.png)                                                                       | [A2：下发 ATool_SetParam]<br>[A3：下发 Trigger_Read]<br>[A5：渲染交互图形]                                                           | [B2：调用算法设定]<br>[B5：组装事件推流]                                                                   | [C1：设置区域]<br>[C2：提取候选圆]                                       |
| **3** | **拖动 ROI / 调节半径**<br>![alt text]({2B883C96-28C2-4441-B557-0EC1AD645EEB}.png)                                                            | 鼠标拖动实时高频触发：<br>[A2：下发 ATool_SetParam]<br>[A3：下发 Trigger_Read]<br>[A5：渲染交互图形]                                 | [B2：调用算法设定]<br>[B3：提取策略判定 (重关联)]<br>[B5：组装事件推流]                                    | [C1：设置区域]<br>[C2：提取候选圆]<br>[C3：执行空间匹配]                 |
| **4** | **抽取直径/灵敏度/切换圆**<br>![alt text]({3CA7A754-14C0-4A04-8DF7-65B703A6E2FD}.png)                                                         | 点击确定后：<br>[A1：更新本地状态]<br>[A2：下发 ATool_SetParam]<br>[A3：下发 Trigger_Read]<br>[A5：渲染交互图形]                     | [B2：调用算法设定]<br>[B5：组装事件推流]                                                                   | [C2：提取候选圆]                                                         |
| **5** | **直径模式 / 明暗方向**<br>![alt text]({05FD9EFA-3281-4DAC-A346-1FC84A2916B9}.png)<br>![alt text]({F55EF025-28DD-4F91-A2B9-04073FC2B094}.png) | 点击确定后：<br>[A1：更新本地状态]<br>[A2：下发 ATool_SetParam]<br>[A3：下发 Trigger_Read]<br>[A5：渲染交互图形]                     | [B2：调用算法设定]<br>[B3：提取策略判定 (寻极值)]<br>[B5：组装事件推流]                                    | [C2：提取候选圆]                                                         |
| **6** | **缩放设定**<br>![alt text]({33174E55-79DA-46F9-B7F6-837D600A257F}.png)                                                                       | [A1：更新 UI 显示截断]<br>[A2：下发 ATool_SetParam]                                                                                  | [B1：更新业务参数缓存]                                                                                     | 无操作                                                                   |
| **7** | **阈值调节**<br>![alt text]({99E8AB00-A61F-4C76-8FD3-2C7ABE5B2769}.png)                                                                       | [A1：更新本地防呆状态]<br>[A2：下发 ATool_SetParam]                                                                                  | [B1：更新业务参数缓存]                                                                                     | 无操作                                                                   |
| **8** | **开始运行 / 暂停 / 调阈值**<br>![alt text]({B3D2377B-A7B4-4B57-B1D7-2F23724C1A23}.png)                                                       | **开始**：[A4：发 Set_AutoSend -> Run]<br>-> 持续接收并 [A6：渲染结果]<br>**暂停**：[A4：发 Stop]<br>**调阈值**：[A2：下发 SetParam] | 收到 Run 指令后：<br>调用 [C4] 固化基准 -> 开启高频循环 -> 执行 [B4：数值换算与判定] -> [B5：组装事件推流] | 收到注入后：<br>[C4：注入基准圆]<br>硬触发到来时：<br>[C5：执行局部测算] |
<!-- ## 2. 各端操作集合与交互映射

### 2.1 各端职责定义
* **用户操作**：在 UI 面板上拖拽框体、点击按钮、调节滑块。
* **上位机行为**：无状态显示器。负责捕获用户操作，组装参数下发，并根据下位机返回的数据绘制图形和数值，不进行逻辑判断。
* **下位机行为**：状态管家与业务中枢。负责保存参数配置、调用对应的算法接口、执行单位换算（缩放）及 OK/NG 上下限判断。
* **算法行为**：纯数学计算。根据给定的参数和图像，计算并返回圆的像素坐标及半径。

### 2.2 交互行为映射表

| UI 操作与界面                                                                                                                                 | 上位机行为                                                                                                                                  | 下位机行为                                                                             | 算法行为                                |
| :-------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------- | :-------------------------------------- |
| **追加屏蔽**<br>![alt text]({B1F5960D-D64B-4B9A-B93A-5A1E57ECBF9C}.png)                                                                       | `ATool_SetParam` + `Trigger_Read`(提取候选)<br>结果渲染：不可移动，圆心固定，相交的候选圆不显示。                                           | 重新设置算法 ROI + 屏蔽区域掩码图，更新参数，调用算法提取候选圆。                      | 提取候选圆                              |
| **清除屏蔽**<br>![alt text]({D8AB6C33-E794-489C-B15D-8F500BB86454}.png)                                                                       | `ATool_SetParam` + `Trigger_Read`<br>结果渲染：去除所有屏蔽区域。                                                                           | 清除掩码缓存，重设 ROI 和参数，调用算法提取候选圆。                                    | 提取候选圆                              |
| **拖动 ROI / 调节半径**<br>![alt text]({2B883C96-28C2-4441-B557-0EC1AD645EEB}.png)                                                            | 鼠标拖动时实时高频发送 `ATool_SetParam` + `Trigger_Read`。显示唯一的指定圆。                                                                | 更新 ROI 与参数，调用算法提取候选圆。返回候选圆数组及当前指定圆的索引。                | 提取候选圆 + (若采用方案2) 匹配最接近圆 |
| **抽取直径/灵敏度/切换圆**<br>![alt text]({3CA7A754-14C0-4A04-8DF7-65B703A6E2FD}.png)                                                         | 点击确定后：`ATool_SetParam` + `Trigger_Read`。渲染所有候选圆（黄色）及当前选定圆（绿色+箭头）。处理极值时的按钮置灰逻辑。                  | 更新参数，提取候选圆，返回候选圆数组及选中索引。                                       | 提取候选圆                              |
| **直径模式 / 明暗方向**<br>![alt text]({05FD9EFA-3281-4DAC-A346-1FC84A2916B9}.png)<br>![alt text]({F55EF025-28DD-4F91-A2B9-04073FC2B094}.png) | 点击确定后：`ATool_SetParam` + `Trigger_Read`。最大最小模式下仅高亮唯一圆。                                                                 | 更新参数，提取候选圆。基于模式（最大/最小/指定）在内部筛选出对应的指定圆索引并返回。   | 提取候选圆                              |
| **缩放设定**<br>![alt text]({33174E55-79DA-46F9-B7F6-837D600A257F}.png)                                                                       | 仅发送 `ATool_SetParam`，不需要 `Trigger_Read`。<br>处理显示逻辑：最高 3 倍并保留最高位，尾数抹零。                                         | 更新内部参数缓存（保存物理转换比率）。                                                 | 无操作                                  |
| **阈值调节**<br>![alt text]({99E8AB00-A61F-4C76-8FD3-2C7ABE5B2769}.png)                                                                       | 仅发送 `ATool_SetParam`，不需要 `Trigger_Read`。<br>注：下限不可拖动到上限右侧。切换比率(x2/x10)为纯显示逻辑，不发给下位机。                | 更新内部参数缓存（保存判定上下限）。                                                   | 无操作                                  |
| **开始运行 / 暂停 / 调阈值**<br>![alt text]({B3D2377B-A7B4-4B57-B1D7-2F23724C1A23}.png)                                                       | 开始：发送 `Set_AutoSend` -> `Run`，持续接收 `Report_Result` 渲染结果。<br>暂停：`Stop` -> 取消 AutoSend。<br>调阈值：发 `ATool_SetParam`。 | 启动：进入运行态，持续接收相机图像，调用算法处理，并发送推流报文。<br>暂停：停止推流。 | 定时器/触发器驱动 -> 运行局部测算       |

--- -->

## 3. 下位机工具节点内部实现

下位机作为业务中枢，内部需实现以下核心逻辑：
1. **状态机管理 (exec_mode)**：根据上位机下发的 `exec_mode` 切换工具处于“特征提取”还是“测量运行”状态。
2. **基准持久化**：用户在设定阶段确认后，下位机需将当前高亮的候选圆数据保存为 `Reference Circle`，断电不丢失。
3. **数据换算 (Scaling)**：在运行态拿到算法返回的像素半径后，根据设定的缩放比率进行乘除法计算，生成最终的 `display_val`。
4. **结果判定 (Judgment)**：将换算后的数值与设定的 `lower_limit` 和 `upper_limit` 进行比对，生成最终的 `status` (如 0: OK, -1: NG低于下限, -2: NG高于上限)。

## 4. 上位机开发注意事项

1. **逻辑剥离**：上位机只做数据绑定和 UI 渲染，禁止在前端代码中写死像素转毫米的计算公式，禁止在前端判断产品是否 OK/NG。
2. **防呆限制**：阈值滑块的下限不能拖过上限；缩放设定的最高倍率和尾数抹零仅在 UI 层限制。
3. **性能控制**：在“拖动 ROI”这种高频交互场景下，上位机需要做好防抖发送，避免瞬间产生过多的 TCP 交互导致下位机消息队列阻塞。

## 5. 工具生命周期与执行流程

1. **实例化**：新增工具，下位机调用算法的 `create` 接口分配内存。
2. **设定态 (Teach)**：多次执行 `ATool_SetParam (exec_mode=1)` 与 `Trigger_Read`。界面完成特征提取和基准选定。
3. **运行态 (Run)**：系统收到 `Run` 指令。下位机将选定的基准圆 `set_reference` 给算法，随后跟随图像采集高频调用算法 `run` 接口，并通过 `Report_Result` 将结果推给上位机。
4. **销毁**：删除工具，下位机调用算法的 `destroy` 接口释放资源。

---

### 6.1 设定态：提取候选圆 (Teach - Extract)

**[TCP] TX: ATool_SetParam (上位机 -> 下位机)**

```json

{
  "id": "T01",
  "config": {
    "exec_mode": 1,            // 1: 提取模式
    "edge_direction": 0,       // 参考 nvs_alg_diam_edge_direction_e
    "extract_sensitivity": 1,  // 参考 nvs_alg_diam_extract_sens_e
    "reference_circle": {      // 提取阶段未固化，各字段设为 0
      "x": 0.0, "y": 0.0, "r": 0.0, "inner_r": 0.0
    },
    "regions": [
      {
        "op": 0,               // NVS_REGION_OP_OBJECT
        "shape": 1,            // NVS_SHAPE_CIRCLE
        "param": {
          "circle": { "x": 640.0, "y": 480.0, "r": 200.0, "inner_r": 100.0 }
        }
      }
    ]
  }
}
```

**[TCP] RX: Report_Result (Trigger_Read 响应)**

```json

{
  "res_id": 376,
  "atools": [
    {
      "node_id": "T01",
      "exec_mode": 1,
      "num_candidates": 3,
      "matched_idx": 0,
      "candidates": [
        // 结构严格对齐 nvs_circle_t
        {"x": 640.5, "y": 480.2, "r": 150.5, "inner_r": 0.0},
        {"x": 638.0, "y": 479.0, "r": 145.0, "inner_r": 0.0},
        {"x": 645.2, "y": 485.5, "r": 160.2, "inner_r": 0.0}
      ],
      "diam_result": {}
    }
  ]
}

```

### 6.2 运行态：固化基准与连续运行 (Run)

**[TCP] TX: ATool_SetParam (上位机 -> 下位机)**

```json
{
  "id": "T01",
  "config": {
    "exec_mode": 0,            // 0: 运行测算模式
    "reference_circle": {      // 下发固化的基准圆 (对齐 nvs_circle_t)
      "x": 640.5, "y": 480.2, "r": 150.5, "inner_r": 0.0
    },
    "regions": [ /* 同上，ROI 配置维持不变 */ ]
  }
}
```

**[TCP] RX: Report_Result (AutoSend 推流)**

```json
{
  "res_id": 377,
  "atools": [
    {
      "node_id": "T01",
      "status": 0,             // nvs_output_status_e
      "exec_mode": 0,
      "diam_result": {         // 运行态输出实测结果 (对齐 nvs_circle_t)
        "x": 641.0, 
        "y": 480.0, 
        "r": 150.2,
        "inner_r": 0.0,
        "display_val": 300.4   // 业务缩放后的物理值
      }
    }
  ]
}
```

*注：`Set_AutoSend`、`Run`、`Stop` 为系统级通用总线指令，遵循 NVS 现有协议框架，不附带特定算法 Payload。*

---

## 7. 算法接口与数据结构定义

```c
#ifndef __NVS_ALG_DIAM_H__
#define __NVS_ALG_DIAM_H__

#include <stdint.h>
#include "nvs_base_type.h" 

#ifdef __cplusplus
extern "C" {
#endif

// ==========================================================================
// 1. 宏与句柄定义
// ==========================================================================
#define NVS_ALG_DIAM_MAX_CANDIDATE_NUM 16

typedef void* nvs_alg_diam_handle_t;

// ==========================================================================
// 2. 核心数据结构与枚举定义
// ==========================================================================

// 边缘查找方向枚举
typedef enum {
    NVS_ALG_DIAM_DIRECTION_ALL           = 0, // 全部方向
    NVS_ALG_DIAM_DIRECTION_MASTER        = 1, // 主控方向 (沿用基准特征)
    NVS_ALG_DIAM_DIRECTION_DARK_TO_LIGHT = 2, // 暗到明
    NVS_ALG_DIAM_DIRECTION_LIGHT_TO_DARK = 3  // 明到暗
} nvs_alg_diam_edge_direction_e;

// 提取灵敏度档位枚举
typedef enum {
    NVS_ALG_DIAM_EXTRACT_SENS_LOW    = 0, // 低灵敏度
    NVS_ALG_DIAM_EXTRACT_SENS_MEDIUM = 1, // 中灵敏度
    NVS_ALG_DIAM_EXTRACT_SENS_HIGH   = 2  // 高灵敏度
} nvs_alg_diam_extract_sens_e;

// 提取参数结构体
typedef struct {
    int32_t edge_direction;      
    int32_t extract_sensitivity;e 
} nvs_alg_diam_extract_param_t;

// 候选圆数组
typedef struct {
    uint8_t count;
    nvs_circle_t circles[NVS_ALG_DIAM_MAX_CANDIDATE_NUM]; 
} nvs_alg_diam_candidate_circle_array_t;

// 基准圆结构体
typedef struct {
    nvs_circle_t circle;    
} nvs_alg_diam_reference_t;

// 结果圆结构体
typedef struct {
    nvs_circle_t circle;    
} nvs_alg_diam_result_t;

// ==========================================================================
// 3. 接口定义
// ==========================================================================

nvs_alg_diam_handle_t nvs_alg_diam_create(void);
void nvs_alg_diam_destroy(nvs_alg_diam_handle_t in_handle);

// 配置态提取接口
int32_t nvs_alg_diam_set_region(nvs_alg_diam_handle_t in_handle, const nvs_region_t *in_regions, int32_t in_region_count);
int32_t nvs_alg_diam_extract_candidate(nvs_alg_diam_handle_t in_handle, const nvs_image_t *in_img, const nvs_alg_diam_extract_param_t *in_param, nvs_alg_diam_candidate_circle_array_t *out_candidate_array);
// int32_t nvs_alg_diam_match_circle(const nvs_alg_diam_reference_t *in_old_reference, const nvs_alg_diam_candidate_circle_array_t *in_candidate_array, int32_t *out_matched_idx);

// 运行态执行接口
int32_t nvs_alg_diam_set_reference(nvs_alg_diam_handle_t in_handle, const nvs_alg_diam_reference_t *in_reference);
int32_t nvs_alg_diam_run(nvs_alg_diam_handle_t in_handle, const nvs_image_t *in_img, nvs_alg_diam_result_t *out_result);

// 模型导入导出预留
uint32_t nvs_alg_diam_get_model_size(nvs_alg_diam_handle_t in_handle);
int32_t nvs_alg_diam_export_model(nvs_alg_diam_handle_t in_handle, void *out_buf, uint32_t in_buf_len);
int32_t nvs_alg_diam_import_model(nvs_alg_diam_handle_t in_handle, const void *in_buf, uint32_t in_data_len);

#ifdef __cplusplus
}
#endif

#endif // __NVS_ALG_DIAM_H__
```
