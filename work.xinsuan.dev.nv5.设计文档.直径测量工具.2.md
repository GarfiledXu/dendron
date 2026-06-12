---
id: kw4zaqixlc9c7ik2oj6l6hf
title: '2'
desc: ''
updated: 1780918938473
created: 1780886268379
---

# NVS 直径测量工具 (DIAM_TOOL) 系统架构与接口设计规范

## 1. 系统边界与三端职责划分

本节明确直径测量工具在上位机 (Host)、下位机 (Device) 与算法层 (Algorithm) 之间的工程职责边界。系统架构采用表示层、业务控制层与计算引擎严格解耦的设计规范，各层级履行以下职能：

* **上位机 (Host / UI 端)**：定位为无状态的表示层。负责交互指令的序列化下发，以及被动接收设备端推流数据进行视图渲染与绑定。前端不持有业务状态，且禁止在前端执行阈值判定（OK/NG）及物理单位的换算逻辑。
* **下位机 (Device / 业务控制层)**：定位为业务控制核心。负责协议解析、工具实例上下文及状态机的维护、算法生命周期调度。处理并闭环所有业务逻辑，包括特征提取策略、数据缩放标定及终态判定结果的生成。
* **算法层 (Algorithm / 底层计算核)**：定位为底层计算引擎。基于输入图像与约束参数执行几何特征计算与数值拟合，输出像素级几何图元。算法层与业务逻辑（如单位换算、上下限设定）完全隔离，保持无状态特性（上下文句柄除外）。

---

## 2. 术语集合及功能点定义

### 2.1 核心特征实体
* **候选圆 (Candidate Circles)**：设定阶段 (TEACH) 算法依据输入参数对感兴趣区域 (ROI) 执行扫描后，输出的合法几何边缘集合。该项属于瞬态数据，供下位机业务策略执行二次筛选。
* **指定圆 / 基准圆 (Reference Circle)**：下位机利用提取策略从候选圆集合中确定的唯一目标。该项作为持久化状态保存，是运行阶段执行局部搜索的基础参照系。
* **结果圆 (Result Circle)**：运行阶段 (RUN) 算法以基准圆为初始解，在局部容差范围内执行几何拟合所输出的当前帧实测圆。该项为高频易失数据，用于终态数值计算与判定。

### 2.2 核心机制决议
* **执行模式 (Exec Mode) 驱动机制**：
    协议层通过在 `ATool_SetParam` 指令中显式下发 `exec_mode` 字段驱动下位机状态机：
    * `exec_mode = 1`：设定态。下位机调度算法提取接口，返回候选圆集合，实测结果节点保留但数据置空。
    * `exec_mode = 0`：运行态。下位机调度算法拟合接口，返回唯一实测结果圆，候选圆集合节点保留但置为空数组。
* **拖动 ROI 交互行为约束**：
    采用特征重提取与匹配逻辑。拖动 ROI 将连续触发 `exec_mode=1` 的参数设置指令，下位机执行特征提取并返回全量候选集及匹配索引。
* **明暗方向提取差异**：
    高灵敏度配置下，“全部方向”将提取大量复合边缘特征。明确指定“暗到明”或“明到暗”作为特征过滤条件，用于抑制环境光及阴影干扰。
* **目标重关联策略**：
    当算法参数或 ROI 变更导致候选集刷新时，下位机调用算法匹配接口，传入历史缓存的基准圆与最新候选数组。算法通过空间代价度量计算最佳匹配项的索引，下位机以此作为新的默认指定圆。

---

## 3. 三端交互行为映射矩阵

下表定义不同交互场景下，上位机、下位机与底层算法的严格协同调用链路。

| 业务场景 | 触发条件 / 用户交互 | 上位机 (Host) 行为 | 下位机 (Device) 行为 | 算法 (Algorithm) 行为 |
| :--- | :--- | :--- | :--- | :--- |
| **区域屏蔽** | 追加屏蔽 / 清除屏蔽，触发应用 | 下发 `ATool_SetParam` (含屏蔽坐标) + `Trigger_Read` (请求候选数据)。接收响应后更新视图。 | 更新 ROI 与掩码缓存，调用提取接口，返回候选圆集合。 | `set_region` -> `extract_candidate` (执行扫描，输出特征数组)。 |
| **空间重定** | 拖动 ROI / 调节半径边界 | 高频下发 `ATool_SetParam` + `Trigger_Read`。接收响应并依据匹配索引渲染目标高亮。 | 接收实时 ROI 执行提取，读取缓存基准执行重关联匹配，返回候选集及匹配索引 `matched_idx`。 | `set_region` -> `extract_candidate` -> `match_circle` (计算空间最佳匹配项)。 |
| **特征过滤** | 调整提取灵敏度 / 切换明暗方向 | 缓存参数并下发 `ATool_SetParam` + `Trigger_Read`。渲染更新后的候选圆及指定圆。 | 更新特征提取参数缓存，触发提取动作，返回符合条件的新候选集及默认索引。 | `extract_candidate` (基于设定的方向和灵敏度条件输出特征数组)。 |
| **模式切换** | 切换直径模式 (最大/最小/指定) | 下发 `ATool_SetParam` + `Trigger_Read`。依据模式配置对应的高亮视图展示。 | 触发特征提取，在内部执行业务提取策略（按半径寻找极值或指定项），返回对应索引。 | `extract_candidate` (仅负责特征输出，不涉及最大/最小等业务排序逻辑)。 |
| **数值标定** | 扩展功能 -> 缩放设定 (单位换算) | 仅下发 `ATool_SetParam` (更新 `scale_param`)。不触发 `Trigger_Read`。 | 接收并更新换算比率缓存。 | **无操作** (不触发算法执行)。 |
| **边界判定** | 阈值设定 (上限/下限配置) | 仅下发 `ATool_SetParam` (更新 `judge_param`)。不触发 `Trigger_Read`。 | 接收并更新判定边界缓存。 | **无操作** (不触发算法执行)。 |
| **连续运行** | 启动开始运行 / 停止运行 | 启动：下发 `Set_AutoSend`。被动接收 `Report_Result` 并渲染实测图形与数值。停止：发送 `Stop`。 | 启动：调用基准注入接口。随触发信号循环调用拟合接口，执行数值缩放与判定，组装事件报文并推流。 | `set_reference` -> 随信号高频执行 `run` (执行局部拟合，输出唯一结果)。 |

---

## 4. 工具生命周期与执行流程

1.  **实例初始化**：系统调度创建 `DIAM_TOOL` 节点，分配上下文内存，调用 `nvs_alg_diam_create()` 实例化底层算法句柄。
2.  **设定态 (TEACH - `exec_mode=1`)**：
    * 定位：系统参数配置与特征关联。所有操作由上位机指令 (`SetParam` + `TriggerRead`) 驱动。
    * 数据流：写入参数 -> 算法全量特征提取 -> 业务层匹配索引 -> 上位机渲染候选集。
    * 状态固化：用户确认配置，下位机将选定目标持久化存储为 `Reference Circle`。
3.  **运行态 (RUN - `exec_mode=0`)**：
    * 定位：连续信号驱动的闭环计算。系统转为事件驱动的主动推流模式 (`Report_Result`)。
    * 数据流：注入基准特征 -> 等待触发信号 -> 算法执行局部拟合 -> 下位机执行换算与边界判定 -> 报文推流。
4.  **实例销毁**：接收节点删除指令，调用 `nvs_alg_diam_destroy()` 释放算法句柄与关联内存。

---

## 5. 上下位机通信协议与报文定义

### 5.1 ATool_SetParam 与 Report_Result 契约

```json
// =================================================================================
// 协议定义：直径测量工具 (DIAM_TOOL) 状态驱动执行协议
// 结构规范：维持 Schema 字段的完整性，通过 exec_mode 区分节点数据的有效性。
//           - exec_mode = 1 (设定态): candidates 数组存入数据, diam_result 置空
//           - exec_mode = 0 (运行态): candidates 数组置空, diam_result 存入实测数据
// =================================================================================

// ---------------------------------------------------------------------------------
// [交互过程 1] 设定态：参数下发与特征提取
// ---------------------------------------------------------------------------------

// [REQ] 上位机下发参数请求
{
  "id": "T01",
  "config": {
    "exec_mode": 1,            // 执行模式：1 代表设定提取态
    "edge_direction": 0,
    "extract_sensitivity": 1,
    "reference_circle": {      // 提取阶段字段保留，valid 标识置 0
      "valid": 0,
      "x": 0.0,
      "y": 0.0,
      "radius": 0.0
    },
    "pos_adjust_ref": "",
    "regions": [
      {
        "op": "base",
        "shape": "annulus",
        "pts": [
          {"x": 640.0, "y": 480.0, "r1": 100.0, "r2": 200.0}
        ]
      }
    ]
  }
}

// [RESP] 下位机被动响应提取结果 (Trigger_Read 回调)
{
  "res_id": 376,
  "tot_res": { /* 框架全局状态字段 */ },
  "atools": [
    {
      "node_id": "T01",
      "node_type": "DIAM_TOOL",
      "status": 0,
      "exec_mode": 1,          
      "cost_time": 45,
      "num_candidates": 3,
      "matched_idx": 0,        
      "candidates": [          // 返回全量候选数组
        {"idx": 0, "x": 640.5, "y": 480.2, "radius": 150.5},
        {"idx": 1, "x": 638.0, "y": 479.0, "radius": 145.0},
        {"idx": 2, "x": 645.2, "y": 485.5, "radius": 160.2}
      ],
      "diam_result": {}        // 保持结构完整，内容置空
    }
  ]
}

// ---------------------------------------------------------------------------------
// [交互过程 2] 运行态：基准设定与连续推流
// ---------------------------------------------------------------------------------

// [REQ] 上位机下发基准固化请求
{
  "id": "T01",
  "config": {
    "exec_mode": 0,            // 执行模式：0 代表运行计算态
    "edge_direction": 0,
    "extract_sensitivity": 1,
    "reference_circle": {      // 下发实际生效的基准特征参数
      "valid": 1,              
      "x": 640.5,
      "y": 480.2,
      "radius": 150.5
    },
    "pos_adjust_ref": "",
    "regions": [ /* ROI 坐标数据 */ ]
  }
}

// [EVENT] 下位机主动推流运行结果 (AutoSend 回调)
{
  "res_id": 377,
  "tot_res": { /* 框架全局状态字段 */ },
  "atools": [
    {
      "node_id": "T01",
      "node_type": "DIAM_TOOL",
      "status": 0,             // 综合业务判定码 (-1: 超下限, 0: OK, 等)
      "exec_mode": 0,          
      "cost_time": 15,         
      "num_candidates": 0,     
      "matched_idx": -1,       
      "candidates": [],        // 候选数组保留节点结构，置空
      "diam_result": {         // 返回唯一实测运算结果
        "x": 641.0,            
        "y": 480.0,            
        "radius": 150.2,
        "display_val": 300.4   // 业务缩放处理后的前端显示数值
      }
    }
  ]
}
```

---

## 6. 算法层接口与数据结构定义 (C Header)

```c
#ifndef __NVS_ALG_DIAM_H__
#define __NVS_ALG_DIAM_H__

#include <stdint.h>
#include "nvs_base_type.h" 

#ifdef __cplusplus
extern "C" {
#endif

// ==========================================================================
// 1. 宏定义与算法句柄声明
// ==========================================================================
#define NVS_ALG_DIAM_MAX_CANDIDATE_NUM 16

typedef void* nvs_alg_diam_handle_t;

// ==========================================================================
// 2. 核心数据结构与枚举项
// ==========================================================================

// 边缘查找方向规范
typedef enum {
    NVS_ALG_DIAM_DIRECTION_ALL           = 0, // 无方向约束
    NVS_ALG_DIAM_DIRECTION_MASTER        = 1, // 参考主控特征方向
    NVS_ALG_DIAM_DIRECTION_DARK_TO_LIGHT = 2, // 限制为暗至明过渡
    NVS_ALG_DIAM_DIRECTION_LIGHT_TO_DARK = 3  // 限制为明至暗过渡
} nvs_alg_diam_edge_direction_e;

// 特征提取灵敏度规范
typedef enum {
    NVS_ALG_DIAM_EXTRACT_SENS_LOW    = 0, // 低灵敏度约束
    NVS_ALG_DIAM_EXTRACT_SENS_MEDIUM = 1, // 默认标准灵敏度
    NVS_ALG_DIAM_EXTRACT_SENS_HIGH   = 2  // 高灵敏度约束
} nvs_alg_diam_extract_sens_e;

// 特征提取参数结构
typedef struct {
    int32_t edge_direction;      // 配置提取约束方向
    int32_t extract_sensitivity; // 配置提取灵敏度等级
} nvs_alg_diam_extract_param_t;

// 候选特征存储结构
typedef struct {
    uint8_t count;
    nvs_circle_t circles[NVS_ALG_DIAM_MAX_CANDIDATE_NUM]; 
} nvs_alg_diam_candidate_circle_array_t;

// 基准特征存储结构
typedef struct {
    nvs_circle_t circle;         // 基准几何特征定义
} nvs_alg_diam_reference_t;

// 运算结果存储结构
typedef struct {
    int32_t status_code;         // 底层算法运行状态码 (0 标识正常)
    nvs_circle_t circle;         // 拟合输出结果特征
} nvs_alg_diam_result_t;


// ==========================================================================
// 3. 句柄生命周期控制接口
// ==========================================================================

/**
 * @brief 实例化直径测量算法对象
 * @return 返回算法上下文句柄，内存不足返回 NULL
 */
nvs_alg_diam_handle_t nvs_alg_diam_create(void);

/**
 * @brief 销毁算法对象并释放内存
 * @param in_handle 算法上下文句柄
 */
void nvs_alg_diam_destroy(nvs_alg_diam_handle_t in_handle);


// ==========================================================================
// 4. 设定态 (TEACH) 控制接口
// ==========================================================================

/**
 * @brief 更新算法底层感兴趣区域 (Region)
 * @param in_handle       算法上下文句柄
 * @param in_regions      区域坐标参数集合
 * @param in_region_count 区域包含对象的数量
 * @return int32_t        操作状态码
 */
int32_t nvs_alg_diam_set_region(
    nvs_alg_diam_handle_t in_handle, 
    const nvs_region_t *in_regions, 
    int32_t in_region_count
);

/**
 * @brief 执行全图扫描提取合法候选圆集合
 * @param in_handle           算法上下文句柄
 * @param in_img              图像内存数据对象
 * @param in_param            提取阈值及方向参数
 * @param out_candidate_array [输出] 返回符合特征约束的圆集合
 * @return int32_t            操作状态码
 */
int32_t nvs_alg_diam_extract_candidate(
    nvs_alg_diam_handle_t in_handle,
    const nvs_image_t *in_img,
    const nvs_alg_diam_extract_param_t *in_param,
    nvs_alg_diam_candidate_circle_array_t *out_candidate_array
);

/**
 * @brief 执行候选集与历史基准的空间相似度度量计算
 * @param in_old_reference    前置基准参数
 * @param in_candidate_array  当前特征候选数组
 * @param out_matched_idx     [输出] 返回匹配度最高的目标索引 (-1 代表未检出)
 * @return int32_t            操作状态码
 */
int32_t nvs_alg_diam_match_circle(
    const nvs_alg_diam_reference_t *in_old_reference,
    const nvs_alg_diam_candidate_circle_array_t *in_candidate_array,
    int32_t *out_matched_idx
);


// ==========================================================================
// 5. 运行态 (RUN) 控制接口
// ==========================================================================

/**
 * @brief 设定并固化运行期的基准约束参数
 * @param in_handle    算法上下文句柄
 * @param in_reference 基准几何特征定义
 * @return int32_t     操作状态码
 */
int32_t nvs_alg_diam_set_reference(
    nvs_alg_diam_handle_t in_handle,
    const nvs_alg_diam_reference_t *in_reference
);

/**
 * @brief 执行局部特征拟合并输出实测结果
 * @param in_handle  算法上下文句柄
 * @param in_img     图像内存数据对象
 * @param out_result [输出] 返回实测拟合输出及状态
 * @return int32_t   操作状态码
 */
int32_t nvs_alg_diam_run(
    nvs_alg_diam_handle_t in_handle, 
    const nvs_image_t *in_img, 
    nvs_alg_diam_result_t *out_result
);

// ==========================================================================
// 6. 模型流化与持久化扩展接口
// ==========================================================================

/**
 * @brief 请求算法上下文数据的流化规模
 * @param in_handle 算法上下文句柄
 * @return uint32_t 所需字节数
 */
uint32_t nvs_alg_diam_get_model_size(nvs_alg_diam_handle_t in_handle);

/**
 * @brief 序列化算法上下文状态数据至外部缓冲
 * @param in_handle   算法上下文句柄
 * @param out_buf     目标缓存地址
 * @param in_buf_len  目标缓存配额容量
 * @return int32_t    实际写入长度，负值代表溢出或其他异常
 */
int32_t nvs_alg_diam_export_model(
    nvs_alg_diam_handle_t in_handle, 
    void *out_buf, 
    uint32_t in_buf_len
);

/**
 * @brief 从序列化数据流重建算法上下文状态
 * @param in_handle   算法上下文句柄
 * @param in_buf      数据流起始地址
 * @param in_data_len 有效数据长度
 * @return int32_t    操作状态码
 */
int32_t nvs_alg_diam_import_model(
    nvs_alg_diam_handle_t in_handle, 
    const void *in_buf, 
    uint32_t in_data_len
);

#ifdef __cplusplus
}
#endif

#endif // __NVS_ALG_DIAM_H__
```