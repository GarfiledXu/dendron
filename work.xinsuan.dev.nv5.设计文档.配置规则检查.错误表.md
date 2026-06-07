---
id: 9w92d7fqdcfoiwqf5brw2ji
title: 错误表
desc: ''
updated: 1780543450763
created: 1780543116206
---

# 规则检查(CRC) 错误码规范

## 1. 区间分配

为明确隔离各层级的预检职责，CRC 模块采用 4 位整数作为基础错误码，并按系统架构的抽象程度从低到高进行严格的区间划分：

- **4000 - 4999 (系统全局层 SYSTEM)**：硬件物理限制（内存/磁盘）、环境版本、全局图像资产配额冲突预判。
- **3000 - 3999 (程序拓扑层 PROG)**：PROG 容器级别的节点挂载配额、拓扑流转、输入输出约束或状态机错误。
- **2000 - 2999 (通用节点层 NODE)**

> **注**：具体算法内部的运行时参数与业务错误（原 0000-1999 区间）已由底层通用异常上报模块统一接管，CRC 预检层主要负责配置下发前的静态与资源阻断校验。

---

## 2. 映射表

### 2.1 SYSTEM 层 (基址 4000): 系统资源与配额

负责全局硬件边界、图像资产容量与系统级环境的预判。

| 错误码 | 严重等级 | msg | 描述 (含核心限制数据) |
|---|---|---|---|
| `4001` | `FATAL` | Physical Flash space insufficient. | **物理 Flash 存储空间极低**。设备可用 Flash 空间不足总容量的 **10%**，拦截可能导致写盘失败的操作。 |
| `4002` | `WARN` / `FATAL` | Insufficient system memory heap. | **系统堆内存不足**。可用 RAM 低于 **300MB** 触发预警（潜在内存泄漏），低于 **100MB** 触发致命拦截，阻断新工具初始化。 |
| `4021` | `FATAL` | Global master images limit reached. | **全局主控存图总数超限**。当前设备所有程序累计的主控图数量已达硬件能力上限（最大 **1000** 张）。 |
| `4022` | `FATAL` | Program exceeds max master images limit. | **单程序主控图配额超限**。请求追加保存后，当前 Program 的主控图数量将超过单程序上限（最大 **32** 张）。 |
| `4023` | `FATAL` | Global learned images limit reached. | **全局学习图总数超限**。当前设备所有算法工具累计的学习图数量已达硬件能力上限（最大 **256** 张）。 |
| `4024` | `FATAL` | Tool exceeds learning images limit. | **单工具学习图配额超限**。请求追加样本后，当前 Tool 的学习图数量将超过单工具上限（最大 **32** 张）。 |
| `4050` | `FATAL` | Requested version is incompatible. | **配置版本不兼容**。上位机透传的配置文件版本号高于当前 NVS 固件能力集版本，拒绝加载。 |
| `4051` | `WARN` | System temperature overheat. | **设备温度过高**。当前硬件核心温度触及警戒线，系统可能面临主动降频，提示业务层注意耗时波动。 |

### 2.2 PROG 层 (基址 3000): 程序容器与拓扑连线

负责程序自身的拓扑合法性、组件完备性及运行状态校验。

工具数量限制，参考基恩士
1 个程序可以设定的工具数量如下。
• 最多可以设定65 个工具。
• 学习计数和学习OCR 分别最多可设定16 个工具。
• 含有位置修正工具或轮廓工具时
• 位置修正工具和轮廓工具的总数量有限制。
• 工具设定不同，可设定的工具数量也不同。参考标准是大约40
个工具。
• 位置修正工具的搜索算法设为高精度时，参考标准是大约20 个
工具



| 错误码 | 严重等级 | msg | 描述 |
|---|---|---|---|
| `3001` | `FATAL` | Exceeds max supported tool nodes. | **节点挂载溢出**。PROG 内挂载的 Node 数量超过了硬件能力集允许的最大上限。 |
| `3002` | `FATAL` | Topology validation failed. | **拓扑校验失败**。检测到连线存在逻辑死循环，或强依赖的上游输出节点断链。 |
| `3020` | `FATAL` | Missing required master image. | **缺失必须的主控图像**。当前 PROG 尚未保存主控基准图，不具备基础的对位与运行条件。 |
| `3021` | `WARN` | Output pins are unbound. | **输出引脚悬空**。程序的总逻辑结果或关键数据输出未绑定到实际的物理 IO 或通信协议端口上。 |
| `3022` | `FATAL` | Program is empty. | **程序为空**。该 PROG 容器内未添加任何算法工具，无法构成有效执行链。 |
| `3050` | `FATAL` | Modification forbidden while running. | **运行状态互斥**。PROG 当前正处于连续运行或学习模式，拦截任何试图修改其拓扑或参数的请求。 |

### 2.3 NODE 层 (基址 2000): 节点状态与专有校验

包含所有 Tool 的通用生命周期拦截，以及各具体算法专有的参数验证。

| 错误码 | 严重等级 | msg | 描述 |
|---|---|---|---|
| `2001` | `FATAL` | Target Node entity not found. | **实体丢失**。在节点管理器中未找到指定的 Target ID（上位机可能传入了已被删除的幽灵节点）。 |
| `2002` | `WARN` | Tool environment is not learned. | **算法未学习**。该 Tool 强依赖样本特征，但当前尚未完成学习流程，无法参与运算。 |
| `2003` | `FATAL` | Tool configuration incomplete. | **配置不完备**。工具缺乏运行所必需的前提条件（如：未绘制搜索 ROI 框，或必需的输入引脚未连线）。 |
| `2004` | `FATAL` | Tool max samples reached. | **样本追加超限**。当前工具尝试追加的特征样本/模板数量已达其算法能力集上限。 |
| `2101` | `FATAL` | OCR initialization mode missing. | **【OCR】缺失运行模式**。算法底层句柄生成强依赖特定的运行 `mode`，上位机下发为空或非法枚举。 |
| `2102` | `FATAL` | Threshold parameter out of bound. | **【OCR】阈值越界**。分数或面积等参数超出了物理极值（如 threshold 不在 `[0, 255]` 范围内）。 |
| `2201` | `FATAL` | Match template invalid. | **【Match】模板无效**。匹配工具的参考模板无效或未能成功提取出边缘/像素特征。 |
| `2202` | `FATAL` | Match ROI out of image boundary. | **【Match】搜索区越界**。设定的搜索 ROI 区域部分或全部超出了输入原图的像素物理边界。 |

---

## 3. 下位机头文件定义

```c
/**
 * @file nvs_rule_check_err.h
 * @brief NVS 规则检查 (CRC) 模块全局错误码定义与映射
 */

#ifndef NVS_RULE_CHECK_ERR_H
#define NVS_RULE_CHECK_ERR_H

/* ==================================================================
 * 1. 错误码区间基址 (Base Addresses)
 * ================================================================== */
#define NVS_CRC_BASE_SYSTEM       4000
#define NVS_CRC_BASE_PROG         3000
#define NVS_CRC_BASE_NODE         2000

/* ==================================================================
 * 2. SYSTEM 层预检错误 (物理资源与全局配额)
 * ================================================================== */
// 物理资源类 (4001 - 4019)
#define NVS_CRC_ERR_SYS_FLASH_FULL        (NVS_CRC_BASE_SYSTEM + 1)  // 4001: 物理 Flash 存储空间不足 
#define NVS_CRC_ERR_SYS_MEM_FULL          (NVS_CRC_BASE_SYSTEM + 2)  // 4002: 系统堆内存不足

// 全局配额类 (4020 - 4049)
#define NVS_CRC_ERR_SYS_GLOBAL_IMG_REACHED      (NVS_CRC_BASE_SYSTEM + 21) // 4021: 全局主控存图总数已达硬件上限
#define NVS_CRC_ERR_SYS_PROG_IMG_REACHED        (NVS_CRC_BASE_SYSTEM + 22) // 4022: 单个程序(Prog)主控图已达能力集上限
#define NVS_CRC_ERR_SYS_GLOBAL_LEARN_REACHED    (NVS_CRC_BASE_SYSTEM + 23) // 4023: 全局学习图总数已达硬件上限
#define NVS_CRC_ERR_SYS_TOOL_LEARN_REACHED      (NVS_CRC_BASE_SYSTEM + 24) // 4024: 单个工具(Tool)学习图已达能力集上限

// 运行环境类 (4050 - 4099)
#define NVS_CRC_ERR_SYS_VERSION_INCOMPAT  (NVS_CRC_BASE_SYSTEM + 50) // 4050: 请求的配置版本与当前固件不兼容
#define NVS_CRC_ERR_SYS_OVERHEAT          (NVS_CRC_BASE_SYSTEM + 51) // 4051: 设备温度过高，可能降频

/* ==================================================================
 * 3. PROG 层预检错误 (容器拓扑与可执行状态)
 * ================================================================== */
// 拓扑与依赖类 (3001 - 3019)
#define NVS_CRC_ERR_PROG_TOOL_OVERFLOW    (NVS_CRC_BASE_PROG + 1)    // 3001: 超过支持的最大工具节点数
#define NVS_CRC_ERR_PROG_TOPO_BROKEN      (NVS_CRC_BASE_PROG + 2)    // 3002: 拓扑校验失败：存在死循环或断链

// 完备性与可执行状态类 (3020 - 3049)
#define NVS_CRC_ERR_PROG_MASTER_IMG_MISS  (NVS_CRC_BASE_PROG + 20)   // 3020: 程序缺失必须的主控图像，无法运行
#define NVS_CRC_ERR_PROG_OUT_UNBOUND      (NVS_CRC_BASE_PROG + 21)   // 3021: 程序总结果/逻辑结果未绑定物理输出引脚
#define NVS_CRC_ERR_PROG_NO_ATOOL         (NVS_CRC_BASE_PROG + 22)   // 3022: 程序为空，未添加任何算法工具

// 状态互斥类 (3050 - 3099)
#define NVS_CRC_ERR_PROG_RUNNING_MUTEX    (NVS_CRC_BASE_PROG + 50)   // 3050: 程序正在运行或处于学习模式，禁止修改拓扑

/* ==================================================================
 * 4. NODE 层预检错误 (算法工具状态与参数)
 * ================================================================== */
// 4.1 工具通用完备性与配额错误 (2000 - 2099)
#define NVS_CRC_BASE_NODE_COMMON          (NVS_CRC_BASE_NODE + 0)
#define NVS_CRC_ERR_NODE_NOT_FOUND        (NVS_CRC_BASE_NODE_COMMON + 1) // 2001: 未找到目标节点实体
#define NVS_CRC_ERR_NODE_NOT_LEARNED      (NVS_CRC_BASE_NODE_COMMON + 2) // 2002: 工具必须的样本未学习
#define NVS_CRC_ERR_NODE_CFG_INCOMPLETE   (NVS_CRC_BASE_NODE_COMMON + 3) // 2003: 工具配置不完备 (如没画 ROI 框)
#define NVS_CRC_ERR_NODE_MAX_SAMP_REACHED (NVS_CRC_BASE_NODE_COMMON + 4) // 2004: 该工具的追加样本数已达上限

// 4.2 OCR 工具专属预检错误 (2100 - 2199)
#define NVS_CRC_BASE_NODE_OCR             (NVS_CRC_BASE_NODE + 100)
#define NVS_CRC_ERR_OCR_MODE_MISSING      (NVS_CRC_BASE_NODE_OCR + 1)    // 2101: 缺失初始化运行模式
#define NVS_CRC_ERR_OCR_THRESH_OUT_BOUND  (NVS_CRC_BASE_NODE_OCR + 2)    // 2102: 分数/面积阈值参数越界

// 4.3 Match 匹配工具专属预检错误 (2200 - 2299)
#define NVS_CRC_BASE_NODE_MATCH           (NVS_CRC_BASE_NODE + 200)
#define NVS_CRC_ERR_MATCH_TMPL_INVALID    (NVS_CRC_BASE_NODE_MATCH + 1)  // 2201: 匹配模板无效或未提取特征
#define NVS_CRC_ERR_MATCH_ROI_OUT_OF_IMG  (NVS_CRC_BASE_NODE_MATCH + 2)  // 2202: 搜索区域超出图像边界

/* ==================================================================
 * 5. 辅助判断宏
 * ================================================================== */
#define NVS_CRC_IS_SYSTEM_ERR(err)        ((err) >= NVS_CRC_BASE_SYSTEM && (err) < (NVS_CRC_BASE_SYSTEM + 1000))
#define NVS_CRC_IS_PROG_ERR(err)          ((err) >= NVS_CRC_BASE_PROG   && (err) < (NVS_CRC_BASE_SYSTEM))
#define NVS_CRC_IS_NODE_ERR(err)          ((err) >= NVS_CRC_BASE_NODE   && (err) < (NVS_CRC_BASE_PROG))

#endif // NVS_RULE_CHECK_ERR_H