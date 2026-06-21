---
id: c6zc3w508zw6ayjdh53jmtu
title: '1'
desc: ''
updated: 1781680137512
created: 1781680074654
---

## 1. 功能定义

**设定阶段**  ：用户配置检测区域 (ROI)、特征提取条件（灵敏度/明暗方向）及业务参数（物理缩放比、判定上下限）。系统画面将实时展示提取到的“候选圆”集合，并最终锁定唯一的“指定圆”作为测算基准

**运行阶段**  ：基于已固化的基准目标，在每帧新图像中测量并输出当前的“结果圆”，输出数值缩放后的物理尺寸及 OK/NG 判定状态

---

### 1.1 概念集合

- **候选圆 (Candidate Circles)**  ：设定阶段，算法依据参数对检测区域处理后，提取出的所有符合边缘条件的圆集合
- **指定圆 / 基准圆 (Reference Circle)**  ：代表直径测量工具的测量基准。在不同生命周期下表现为两种状态：
  - **指定圆 (设定时)**  ：下位机从“候选圆”集合中按规则选定，或由用户在 UI 上手动切换的当前高亮目标。此状态为动态，用户拖动ROI和调整参数都能触发更新
  - **基准圆 (运行时)**  ：当抽取规则为指定圆时，在运行阶段，将会从候选结果中匹配与基准圆最接近的作为结果圆
- **结果圆 (Result Circle)**  ：运行阶段，算法基于“基准圆” or 最大/最小 抽取策略 执行测算后，输出的当前帧实际测量圆。用于物理尺寸换算及 OK/NG 判定

---

### 1.3 待讨论项

#### **1.3.1 IV4-抽取直径的规则？**

- 实验结论：
  - 明暗方向和灵敏度，决定ROI内是否能够检测到目标圆
  - **设定阶段**  ，[抽取详细设定]选项为指定圆时，运行阶段将匹配并输出与设定基准圆最接近的圆
  - **运行阶段**  ，[抽取详细设定]选项为最大/最小时，运行阶段将直接输出当前 ROI 内直径最大/最小的圆

---

#### **1.3.2 IV4-扩展功能-抽取详细设定-最大/最小选项的规则和作用域？**

- 从实验来看  **，**  当选择最大/最小选项时，不存在基准圆，不管设定/运行阶段，均是从当前ROI提取出的候选圆中选定 最大/最小圆 作为结果圆？

---

#### **1.3.3 算法层面的处理逻辑（**  **需算法确认**  **）**

- **在设定和运行阶段，算法均做全量提取，即返回候选圆结果**
  - 算法内部不区分阶段，统一每帧提取 ROI 内所有符合条件的候选圆，并输出每个圆对应的属性（直径/极性/明暗方向等），至于最终输出哪个圆（最大/最小，或是与基准圆最匹配的），全由软件通过算法结果后处理来实现
  - 该方式，在运行阶段是否会存在性能问题？  算法：全量提取会增加耗时，建议通过参数设置到算法，算法输出满足该限定的所有圆(如满足直径/极性/明暗方向等属性)，至于最终输出的是最大/最小的圆，或者离roi中心最近的圆（这类不确定的限定）由软件统一处理  
  - 软件：阈值不使用枚举，使用数值；返回结果圆时需要返回对应圆的参数属性，用于软件后处理筛选；
- 若当上述方案存在瓶颈时，算法内部是否需要 区分“设定”与“运行”
  - **设定阶段**  ：算法全量提取 ROI 内的候选圆并输出，供确定基准圆
  - **运行阶段**  ：算法直接基于已设定的基准圆特征进行局部匹配，每帧仅输出一个最终的“结果圆”

---

#### **1.3.4 明暗方向中的全部方向和主控方向的差异？**

- **IV4**    ：实验在高灵敏度设置下，二者的候选圆数量基本一致，但圆的边缘存在细微差异

---

#### **1.3.5 目标重关联策略**  ：当参数调整导致底层的“候选圆”集合整体刷新时，下位机应如何在新集合中重新找回刚才选中的“指定圆”？

- **待选方案 1（下位机自行就近匹配）**    ：下位机记录旧指定圆的坐标和半径，在新的候选集中遍历比对，直接把    **位置和大小最接近**    的圆设为新的指定圆
- **待选方案 2（调用算法匹配接口）**    ：下位机不写业务比对逻辑，直接将旧指定圆传给底层算法（调用     `match_circle`     接口），由    **算法内部评估**    并直接返回最佳匹配的圆索引
- **待选方案 3（无状态重置）**    ：放弃历史追踪。每当候选集刷新，下位机直接按固定规则（如默认取数组首位，或取画面居中的圆）给出一个新的指定圆。不考虑用户的历史选择，完全依赖用户在 UI 上手动重选

---

#### **1.3.6 不同执行逻辑的触发条件**  ：当前上下位机工具的相关交互均是复用   `ATool_SetParam`  （上位机->下位机）和   `Report_Result`  （下位机->上位机）

但实际情况存在多个先后的独立行为，如上位机除了需要提取候选圆以外，在指定模式下需下发指定圆；以及下位机在不同阶段下，report_result存在不同返回结果时（设定阶段返回所有候选圆，运行阶段返回一个结果圆），

需要通过上位机的ATool_SetParam指令行为以及内容进行条件控制

**核心**  ：上位机下发   `ATool_SetParam`  时，附带action 字段

- action 字段区分上位机当前行为
- 字段不存或者值为0时，表示处于运行阶段，下位机triggerrad后返回运行结果；
- 字段值为1时，表示处于设定阶段，并且上位机需要下位机提取候选圆返回结果
- 字段值为0时，表示处于设定阶段，并且上位机下发对应指定圆；

---

#### **1.3.7 设定阶段 ROI 拖动时，算法的触发逻辑？**

- **IV4**    ：拖动 ROI 过程中（    `MouseDown`        `MouseMove`    不需要鼠标按键释放）就有实时的结果反馈

---

## 2. 操作集合与交互映射

### 2.1 操作集合预定义

为了规范描述，消除上下位机交互过程中的语义歧义，各端在设定与运行阶段的原子动作定义如下：

#### 上位机指令行为 (A)

- **[A1：上位机 更新内部参数缓存]**   说明：在本地内存暂存用户的 UI 操作数据（如缩放倍率显示、用户点击切换的指定圆等）。
- **[A2：上位机 下发设置参数 ATool_SetParam 指令]**   说明：通用参数下发。通过   `action`   字段区分单次触发行为（0: 仅更新参数, 1: 提取候选圆, 2: 锁定基准圆）。
- **[A3：上位机 下发触发执行 Trigger_Read 指令]**   说明：通用触发指令。驱动下位机执行后续逻辑。
- **[A4：上位机 下发系统运行控制 Set_AutoSend / Run / Stop 指令]**   说明：系统进入或退出连续运行阶段。
- **[A5：上位机 解析全量候选集并渲染图形]**   说明：设定阶段。解析下位机返回的全量候选数组，渲染虚线候选圆及高亮指定圆。
- **[A6：上位机 解析测算结果并渲染实测图形与数值]**   说明：运行阶段。解析下位机回传的单元素数组，绘制实心结果圆并显示测量数值。

#### 下位机动作行为 (B)

- **[B1：下位机 更新业务参数缓存]**   说明：更新内存中的缩放比率、上下限等静态持久化参数。
- **[B2：下位机 解析并记录 action 状态]**   说明：收到   `ATool_SetParam`   时，记录   `action`   字段取值，用于决定单次触发的逻辑分支。
- **[B3：下位机 解析并调用算法设置接口]**   说明：将 ROI 区域、边缘极性、灵敏度同步给底层算法。若用户选择“主控方向”，下位机在此处将其转化为基准圆的具体极性（1 或 2）传给算法。
- **[B4：下位机 调用算法运行并执行目标筛选]**   说明：收到触发指令后，调用算法统一运行接口获取全量候选圆及属性。若为运行态 (  `action=0`  )，结合已保存的基准圆或规则进行局部匹配筛选；若为设定态 (  `action=1/2`  )，则直接供打包回传。
- **[B5：下位机 锁定并持久化基准圆]**   说明：当   `action=2`   时，更新并存盘传入的基准圆坐标及   `select_mode`   规则。
- **[B6：下位机 执行换算与判定]**   说明：将筛选出的纯像素结果转换为物理尺寸，并判断 OK/NG。
- **[B7：下位机 整理结果数据并上报]**   说明：根据执行阶段组装 JSON 报文并回传（设定态包含全量数组，运行态仅保留 1 个结果圆）。完成后重置   `action`   为 0。

#### 算法层接口行为 (C)

- **[C1：算法 设置区域接口 nvs_alg_diam_set_region]**   说明：设置图像检测 ROI。
- **[C2：算法 设置参数接口 nvs_alg_diam_set_param]**   说明：设置特征提取参数（纯粹的物理极性、灵敏度）。
- **[C3：算法 运行接口 nvs_alg_diam_run]**   说明：传入图像，算法统一提取并返回所有符合条件的候选圆及其特征属性（坐标、半径、极性）。不涉及任何基准圆逻辑。

---

### 2.2 交互行为映射表

|编号|UI 操作与界面|上位机行为|下位机行为|算法行为|
|---|---|---|---|---|
|**1**|**追加屏蔽**,![image.png](http://172.18.1.12/atlas/files/public/6a29153ce4965e8c3844a758/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjZhMjkxNTNjZTQ5NjVlOGMzODQ0YTc1OCIsIjZhMjkxNTU1ZTQ5NjVlOGMzODQ0YTc1YSIsIjZhMjkxNTZiZTQ5NjVlOGMzODQ0YTc1YiIsIjZhMjkxNTc2ZTQ5NjVlOGMzODQ0YTc1YyIsIjZhMjkxNTdkZTQ5NjVlOGMzODQ0YTc1ZCIsIjZhMjkxNjQ2ZTQ5NjVlOGMzODQ0YTc2NCIsIjZhMjkxNjRjZTQ5NjVlOGMzODQ0YTc2NSIsIjZhMjkxNjUwZTQ5NjVlOGMzODQ0YTc2NiIsIjZhMjkxNjU0ZTQ5NjVlOGMzODQ0YTc2NyJdLCJpYXQiOjE3ODE2Nzk5MDAsImV4cCI6MTc4MTY5MDcwMH0.Gh_-Gyr9jwherWtjzW4RbsIviUztkz8s4UD7O6bxOd8),|[A2：上位机 下发设置参数 ATool_SetParam 指令 (action=1)],[A3：上位机 下发触发执行 Trigger_Read 指令],[A5：上位机 解析全量候选集并渲染图形]|[B2：下位机 解析并记录 action 状态],[B3：下位机 解析并调用算法设置接口],[B4：下位机 调用算法运行并执行目标筛选],[B7：下位机 整理结果数据并上报]|[C1：算法 设置区域接口 nvs_alg_diam_set_region],[C3：算法 运行接口 nvs_alg_diam_run]|
|**2**|**清除屏蔽**,![image.png](http://172.18.1.12/atlas/files/public/6a291555e4965e8c3844a75a/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjZhMjkxNTNjZTQ5NjVlOGMzODQ0YTc1OCIsIjZhMjkxNTU1ZTQ5NjVlOGMzODQ0YTc1YSIsIjZhMjkxNTZiZTQ5NjVlOGMzODQ0YTc1YiIsIjZhMjkxNTc2ZTQ5NjVlOGMzODQ0YTc1YyIsIjZhMjkxNTdkZTQ5NjVlOGMzODQ0YTc1ZCIsIjZhMjkxNjQ2ZTQ5NjVlOGMzODQ0YTc2NCIsIjZhMjkxNjRjZTQ5NjVlOGMzODQ0YTc2NSIsIjZhMjkxNjUwZTQ5NjVlOGMzODQ0YTc2NiIsIjZhMjkxNjU0ZTQ5NjVlOGMzODQ0YTc2NyJdLCJpYXQiOjE3ODE2Nzk5MDAsImV4cCI6MTc4MTY5MDcwMH0.Gh_-Gyr9jwherWtjzW4RbsIviUztkz8s4UD7O6bxOd8),|[A2：上位机 下发设置参数 ATool_SetParam 指令 (action=1)],[A3：上位机 下发触发执行 Trigger_Read 指令],[A5：上位机 解析全量候选集并渲染图形]|[B2：下位机 解析并记录 action 状态],[B3：下位机 解析并调用算法设置接口],[B4：下位机 调用算法运行并执行目标筛选],[B7：下位机 整理结果数据并上报]|[C1：算法 设置区域接口 nvs_alg_diam_set_region],[C3：算法 运行接口 nvs_alg_diam_run]|
|**3**|**拖动 ROI / 调节半径**,![image.png](http://172.18.1.12/atlas/files/public/6a29156be4965e8c3844a75b/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjZhMjkxNTNjZTQ5NjVlOGMzODQ0YTc1OCIsIjZhMjkxNTU1ZTQ5NjVlOGMzODQ0YTc1YSIsIjZhMjkxNTZiZTQ5NjVlOGMzODQ0YTc1YiIsIjZhMjkxNTc2ZTQ5NjVlOGMzODQ0YTc1YyIsIjZhMjkxNTdkZTQ5NjVlOGMzODQ0YTc1ZCIsIjZhMjkxNjQ2ZTQ5NjVlOGMzODQ0YTc2NCIsIjZhMjkxNjRjZTQ5NjVlOGMzODQ0YTc2NSIsIjZhMjkxNjUwZTQ5NjVlOGMzODQ0YTc2NiIsIjZhMjkxNjU0ZTQ5NjVlOGMzODQ0YTc2NyJdLCJpYXQiOjE3ODE2Nzk5MDAsImV4cCI6MTc4MTY5MDcwMH0.Gh_-Gyr9jwherWtjzW4RbsIviUztkz8s4UD7O6bxOd8),|**鼠标拖动实时高频触发：**,[A2：上位机 下发设置参数 ATool_SetParam 指令 (action=1)],[A3：上位机 下发触发执行 Trigger_Read 指令],[A5：上位机 解析全量候选集并渲染图形]  
,候选圆范围必须覆盖 ROI 的圆心  
候选圆必须完全处于 ROI 范围内（不得超出 ROI 边界）
仅当候选圆同时满足上述两项规则时，才会被判定为有效目标并纳入输出集合,  
|[B2：下位机 解析并记录 action 状态],[B3：下位机 解析并调用算法设置接口],[B4：下位机 调用算法运行并执行目标筛选],[B7：下位机 整理结果数据并上报]|[C1：算法 设置区域接口 nvs_alg_diam_set_region],[C3：算法 运行接口 nvs_alg_diam_run]|
|**4**|**抽取直径/灵敏度/切换圆**,![image.png](http://172.18.1.12/atlas/files/public/6a291576e4965e8c3844a75c/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjZhMjkxNTNjZTQ5NjVlOGMzODQ0YTc1OCIsIjZhMjkxNTU1ZTQ5NjVlOGMzODQ0YTc1YSIsIjZhMjkxNTZiZTQ5NjVlOGMzODQ0YTc1YiIsIjZhMjkxNTc2ZTQ5NjVlOGMzODQ0YTc1YyIsIjZhMjkxNTdkZTQ5NjVlOGMzODQ0YTc1ZCIsIjZhMjkxNjQ2ZTQ5NjVlOGMzODQ0YTc2NCIsIjZhMjkxNjRjZTQ5NjVlOGMzODQ0YTc2NSIsIjZhMjkxNjUwZTQ5NjVlOGMzODQ0YTc2NiIsIjZhMjkxNjU0ZTQ5NjVlOGMzODQ0YTc2NyJdLCJpYXQiOjE3ODE2Nzk5MDAsImV4cCI6MTc4MTY5MDcwMH0.Gh_-Gyr9jwherWtjzW4RbsIviUztkz8s4UD7O6bxOd8),|**点击确定或在画面圆切换后：**,**[A1：上位机 更新内部参数缓存]**,**[A2：上位机 下发设置参数 ATool_SetParam 指令 (切换圆锁定 action=2, 若仅调灵敏度 action=1)]**,**[A3：上位机 下发触发执行 Trigger_Read 指令]**,**[A5：上位机 解析全量候选集并渲染图形]**|[B2：下位机 解析并记录 action 状态],[B5：下位机 锁定并持久化基准圆] (若为锁定意图),[B3：下位机 解析并调用算法设置接口],[B4：下位机 调用算法运行并执行目标筛选],[B7：下位机 整理结果数据并上报]|[C2：算法 设置参数接口 nvs_alg_diam_set_param],[C3：算法 运行接口 nvs_alg_diam_run]|
|**5**|**直径模式 / 明暗方向**,![image.png](http://172.18.1.12/atlas/files/public/6a29157de4965e8c3844a75d/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjZhMjkxNTNjZTQ5NjVlOGMzODQ0YTc1OCIsIjZhMjkxNTU1ZTQ5NjVlOGMzODQ0YTc1YSIsIjZhMjkxNTZiZTQ5NjVlOGMzODQ0YTc1YiIsIjZhMjkxNTc2ZTQ5NjVlOGMzODQ0YTc1YyIsIjZhMjkxNTdkZTQ5NjVlOGMzODQ0YTc1ZCIsIjZhMjkxNjQ2ZTQ5NjVlOGMzODQ0YTc2NCIsIjZhMjkxNjRjZTQ5NjVlOGMzODQ0YTc2NSIsIjZhMjkxNjUwZTQ5NjVlOGMzODQ0YTc2NiIsIjZhMjkxNjU0ZTQ5NjVlOGMzODQ0YTc2NyJdLCJpYXQiOjE3ODE2Nzk5MDAsImV4cCI6MTc4MTY5MDcwMH0.Gh_-Gyr9jwherWtjzW4RbsIviUztkz8s4UD7O6bxOd8),,![image.png](http://172.18.1.12/atlas/files/public/6a291646e4965e8c3844a764/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjZhMjkxNTNjZTQ5NjVlOGMzODQ0YTc1OCIsIjZhMjkxNTU1ZTQ5NjVlOGMzODQ0YTc1YSIsIjZhMjkxNTZiZTQ5NjVlOGMzODQ0YTc1YiIsIjZhMjkxNTc2ZTQ5NjVlOGMzODQ0YTc1YyIsIjZhMjkxNTdkZTQ5NjVlOGMzODQ0YTc1ZCIsIjZhMjkxNjQ2ZTQ5NjVlOGMzODQ0YTc2NCIsIjZhMjkxNjRjZTQ5NjVlOGMzODQ0YTc2NSIsIjZhMjkxNjUwZTQ5NjVlOGMzODQ0YTc2NiIsIjZhMjkxNjU0ZTQ5NjVlOGMzODQ0YTc2NyJdLCJpYXQiOjE3ODE2Nzk5MDAsImV4cCI6MTc4MTY5MDcwMH0.Gh_-Gyr9jwherWtjzW4RbsIviUztkz8s4UD7O6bxOd8),|**点击确定后：**,**[A1：上位机 更新内部参数缓存]**,**[A2：上位机 下发设置参数 ATool_SetParam 指令 (action=1)]**,**[A3：上位机 下发触发执行 Trigger_Read 指令]**,**[A5：上位机 解析全量候选集并渲染图形]**,,,,,**目标筛选模式（如最大/最小直径）下位机存储，并上位机直接使用下位机返回的指定圆**|[B2：下位机 解析并记录 action 状态],[B3：下位机 解析并调用算法设置接口],[B4：下位机 调用算法运行并执行目标筛选],[B7：下位机 整理结果数据并上报]|~~[C2：算法 设置参数接口 nvs_alg_diam_set_param]~~,~~[C3：算法 运行接口 nvs_alg_diam_run]~~|
|**6**|**缩放设定**,![image.png](http://172.18.1.12/atlas/files/public/6a29164ce4965e8c3844a765/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjZhMjkxNTNjZTQ5NjVlOGMzODQ0YTc1OCIsIjZhMjkxNTU1ZTQ5NjVlOGMzODQ0YTc1YSIsIjZhMjkxNTZiZTQ5NjVlOGMzODQ0YTc1YiIsIjZhMjkxNTc2ZTQ5NjVlOGMzODQ0YTc1YyIsIjZhMjkxNTdkZTQ5NjVlOGMzODQ0YTc1ZCIsIjZhMjkxNjQ2ZTQ5NjVlOGMzODQ0YTc2NCIsIjZhMjkxNjRjZTQ5NjVlOGMzODQ0YTc2NSIsIjZhMjkxNjUwZTQ5NjVlOGMzODQ0YTc2NiIsIjZhMjkxNjU0ZTQ5NjVlOGMzODQ0YTc2NyJdLCJpYXQiOjE3ODE2Nzk5MDAsImV4cCI6MTc4MTY5MDcwMH0.Gh_-Gyr9jwherWtjzW4RbsIviUztkz8s4UD7O6bxOd8),|[A1：上位机 更新内部参数缓存],[A2：上位机 下发设置参数 ATool_SetParam 指令 (action=0)]|[B1：下位机 更新业务参数缓存]|无操作|
|**7**|**阈值调节**,![image.png](http://172.18.1.12/atlas/files/public/6a291650e4965e8c3844a766/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjZhMjkxNTNjZTQ5NjVlOGMzODQ0YTc1OCIsIjZhMjkxNTU1ZTQ5NjVlOGMzODQ0YTc1YSIsIjZhMjkxNTZiZTQ5NjVlOGMzODQ0YTc1YiIsIjZhMjkxNTc2ZTQ5NjVlOGMzODQ0YTc1YyIsIjZhMjkxNTdkZTQ5NjVlOGMzODQ0YTc1ZCIsIjZhMjkxNjQ2ZTQ5NjVlOGMzODQ0YTc2NCIsIjZhMjkxNjRjZTQ5NjVlOGMzODQ0YTc2NSIsIjZhMjkxNjUwZTQ5NjVlOGMzODQ0YTc2NiIsIjZhMjkxNjU0ZTQ5NjVlOGMzODQ0YTc2NyJdLCJpYXQiOjE3ODE2Nzk5MDAsImV4cCI6MTc4MTY5MDcwMH0.Gh_-Gyr9jwherWtjzW4RbsIviUztkz8s4UD7O6bxOd8),|[A1：上位机 更新内部参数缓存],[A2：上位机 下发设置参数 ATool_SetParam 指令 (action=0)]|[B1：下位机 更新业务参数缓存]|无操作|
|**8**|**开始运行 / 暂停 / 调阈值**,![image.png](http://172.18.1.12/atlas/files/public/6a291654e4965e8c3844a767/origin-url?token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWRfZm9yX3B1YmljX2ltYWdlIjoiOWUxOGY5ZjNlYTE3NDk3Mzg2ZmQ1YTZlZDFmNjE4OGMiLCJ0ZWFtX2Zvcl9wdWJsaWNfaW1hZ2UiOiI2NGNiNmUwZTU1MTM5MmVmOGQ0ZjE2ODgiLCJwdWJsaWNfaW1hZ2VfaWRzIjpbIjZhMjkxNTNjZTQ5NjVlOGMzODQ0YTc1OCIsIjZhMjkxNTU1ZTQ5NjVlOGMzODQ0YTc1YSIsIjZhMjkxNTZiZTQ5NjVlOGMzODQ0YTc1YiIsIjZhMjkxNTc2ZTQ5NjVlOGMzODQ0YTc1YyIsIjZhMjkxNTdkZTQ5NjVlOGMzODQ0YTc1ZCIsIjZhMjkxNjQ2ZTQ5NjVlOGMzODQ0YTc2NCIsIjZhMjkxNjRjZTQ5NjVlOGMzODQ0YTc2NSIsIjZhMjkxNjUwZTQ5NjVlOGMzODQ0YTc2NiIsIjZhMjkxNjU0ZTQ5NjVlOGMzODQ0YTc2NyJdLCJpYXQiOjE3ODE2Nzk5MDAsImV4cCI6MTc4MTY5MDcwMH0.Gh_-Gyr9jwherWtjzW4RbsIviUztkz8s4UD7O6bxOd8),|**开始：**,**[A4：上位机 下发系统运行控制 Set_AutoSend / Run / Stop 指令]**,**-> 持续接收并 [A6：上位机 解析测算结果并渲染实测图形与数值]**,,**暂停：**,**[A4：上位机 下发系统运行控制 Set_AutoSend / Run / Stop 指令]**,,**调阈值：**,**[A1：上位机 更新内部参数缓存]**,**[A2：上位机 下发设置参数 ATool_SetParam 指令 (action=0)]**|**硬触发到来时：**,**[B2：默认处于 action=0 运行态]**,**[B4：下位机 调用算法运行并执行目标筛选]**,**[B6：下位机 执行换算与判定]**,**[B7：下位机 整理结果数据并上报]**|每次硬触发到来时：,[C3：算法 运行接口 nvs_alg_diam_run]|

---

## 3. 下位机内部逻辑

1. **逻辑分支判断**  ：依靠   `ATool_SetParam`   下发的   `action`   字段（0: 运行测算, 1: 提取候选圆, 2: 锁定指定圆）决定下位机单次触发的执行逻辑。收到   `Trigger_Read`   后，下位机进入对应分支。单次触发执行完毕后，状态自动重置为   `action=0`  。
1. **算法调用与结果筛选**  ：算法接口统一为全量提取。下位机软件层拿到算法返回的候选数组后，若当前处于运行阶段，由软件层自行遍历数组，根据已存盘的   `select_mode`  （最大/最小/指定）及基准圆进行特征匹配，筛选出最终唯一的目标圆。
1. **返回结果控制**  ：根据执行阶段决定   `Report_Result`   返回的数组大小。所有阶段共用同一套数据结构，设定阶段（  `action=1`   或   `2`  ）返回包含所有候选圆的数组；运行阶段（  `action=0`  ）数组内仅保留 1 个最终结果圆。
1. **尺寸换算**  ：获取算法返回的像素半径   `r`   后，执行物理换算（如：  `measure_val = r * 2.0 * scale_ratio`  ），生成实际输出值。
1. **OK/NG 判定**  ：将换算出的   `measure_val`   与   `lower_limit`  、  `upper_limit`   进行比对，得出最终的工具状态码   `status`  （0: OK, -1: NG）。

---

## 4. 上位机实现注意事项

1. **action 字段下发**  ：  `action`  字段用于控制下位机单次触发逻辑，该字段不写入配置（config）文件。
1.     - 涉及画面重绘的操作（如拖动 ROI、调节灵敏度），下发   `action=1`  。
    - 锁定特征的操作（如点击指定圆），下发   `action=2`   并附加目标坐标。
    - 仅调节业务参数（如上下限阈值），下发   `action=0`  。

1. **触发限制**  ：拖动 ROI 框会高频触发数据交互。上位机需在前端进行频率限制，防止网络拥堵导致下位机处理异常。
1. **参数校验**  ：阈值滑块防呆、缩放倍率上限及负数拦截等，必须在上位机前端完成，不向下位机发送非法数值。

---

## 5. 工具生命周期

1. **创建**  ：添加工具时，下位机调用算法   `create`   接口分配内存句柄，初始化默认参数与 ROI 区域。
1. **设定阶段**  ：上位机下发带有特定   `action`  （1 或 2）的   `ATool_SetParam`   与触发指令，下位机调用算法统一提取候选圆，软件层处理目标锁定，并通过全量数组返回结果。
1. **运行阶段**  ：上位机不再干预（未下发参数或   `action=0`  ），触发源转为系统连续触发或外部硬触发。下位机调用算法提取，并在软件层完成单目标匹配与尺寸换算，通过单元素数组回传结果。

---

## 6. 上下位机通信协议及报文示例

### 6.1 设定阶段触发场景

在设定阶段，  `ATool_SetParam`   指令通过   `action`   字段区分动作：

- **场景一：提取候选圆 (**  `action = 1`  **)**
  - **操作**  ：用户拖动 ROI、修改灵敏度、或变更极性。
  - **下位机**  ：驱动算法全量提取，直接返回包含所有候选圆的数组。
- **场景二：锁定基准圆 (**  `action = 2`  **)**
  - **操作**  ：用户选择筛选规则或在画面中点击特定圆。
  - **下位机**  ：将参数中的   `select_mode`   和   `reference_circle`   保存至内存。驱动算法提取后，返回全量数组供重绘。

---

#### **执行逻辑路由**

```
@startuml
skinparam backgroundcolor transparent
skinparam componentStyle rectangle
skinparam component {
    BackgroundColor #E3F2FD
    BorderColor #1976D2
    FontColor #0D47A1
    FontSize 14
    FontStyle bold
}
skinparam arrow {
    Color #546E7A
    Thickness 2
}
skinparam note {
    BackgroundColor #FFF9C4
    BorderColor #FBC02D
}

title 下位机指令处理逻辑

package "上位机 (Host)" #F8F9FA {
    [UI 交互] as UI #C8E6C9
}

package "下位机 (Device)" #F0F4F8 {
    [逻辑判断] as Router #90CAF9
    
    package "执行分支 (软件层处理)" #FFFFFF {
        [执行全量提取] as Task1 #BBDEFB
        [保存锁定参数] as Task2 #BBDEFB
        [匹配测算结果] as Task3 #BBDEFB
    }
}

UI -right-> Router : [1] ATool_SetParam (含 action)
UI -right-> Router : [2] Trigger_Read

Router -right-> Task1 : action = 1
Router -right-> Task2 : action = 2
Router -right-> Task3 : action = 0

Task1 ..> UI : Report_Result (全量数组)
Task2 ..> UI : Report_Result (全量数组)
Task3 ..> UI : Report_Result (1 个元素的数组)

@enduml
```

---

### 6.2 设定阶段 JSON 协议示例

**[TCP] TX: ATool_SetParam (场景一：提取候选圆) [上位机->下位机]**

```
{
  "id": "T01",
  "config": {
    "action": 1,               // 单次动作指令：1=提取候选圆
    "select_mode": 1,          // 保存至 config：1=最大直径
    "edge_direction": 0,       
    "extract_sensitivity": 50, 
    "reference_circle": {},    // 提取动作下置空或忽略
    "regions": [
      {
        "op": 0,               
        "shape": 1,            
        "param": { "circle": { "x": 640.0, "y": 480.0, "r": 200.0, "inner_r": 100.0 } }
      }
    ],
    "scale_ratio": 2.0,        
    "enable_upper_limit": 1,   
    "lower_limit": 290.0,      
    "upper_limit": 310.0       
  }
}
```

**[TCP] TX: ATool_SetParam (场景二：锁定基准圆) [上位机->下位机]**

```
{
  "id": "T01",
  "config": {
    "action": 2,               // 单次动作指令：2=锁定基准圆
    "select_mode": 0,          
    "edge_direction": 0,       
    "extract_sensitivity": 50,  
    "reference_circle": {      // 锁定的基准圆坐标
      "x": 640.5, 
      "y": 480.2, 
      "r": 150.5, 
      "inner_r": 0.0
    },
    "regions": [
      {
        "op": 0,               
        "shape": 1,            
        "param": { "circle": { "x": 640.0, "y": 480.0, "r": 200.0, "inner_r": 100.0 } }
      }
    ],
    "scale_ratio": 2.0,        
    "enable_upper_limit": 1,   
    "lower_limit": 290.0,      
    "upper_limit": 310.0       
  }
}
```

**[TCP] RX: Report_Result (设定阶段响应) [下位机->上位机]**

```
{
  "res_id": 376,
  "atools": [
    {
      "node_id": "T01",
      "node_type": "DIAM_TOOL",
      "status": 0,            
      "num_candidates": 3,     
      "target_idx": 2,         // 基准圆在数组中的索引
      "candidates": [          // 返回所有候选圆坐标
        {"x": 640.5, "y": 480.2, "r": 150.5, "inner_r": 0.0},
        {"x": 638.0, "y": 479.0, "r": 145.0, "inner_r": 0.0},
        {"x": 645.2, "y": 485.5, "r": 160.2, "inner_r": 0.0} 
      ],
      "measure_val": 300.4     // 物理尺寸
    }
  ]
}
```

---

### 6.3 运行阶段 JSON 协议示例

运行阶段下位机内部使用已保存的规则与参数筛选出结果，并减少数组长度。

**[TCP] RX: Report_Result (运行阶段响应)**

```
{
  "res_id": 377,
  "atools": [
    {
      "node_id": "T01",
      "node_type": "DIAM_TOOL",
      "status": 0,             // 状态 (-1: NG, 0: OK)
      "num_candidates": 1,     // 字段统一，数值固定为 1
      "target_idx": 0,         // 索引固定为 0
      "candidates": [          // 数组内只保留 1 个结果圆
          {"x": 640.5, "y": 480.2, "r": 150.5, "inner_r": 0.0}
      ],
      "measure_val": 300.4     
    }
  ]
}
```

---

### 6.4 config 参数存储约定

|字段名|属性分类|说明|
|---|---|---|
|  `action`  |**单次动作指令**|仅单次控制，  **不写入 config 持久化**  。字段不存在时，默认该值为0|
|  `select_mode`  |业务规则|写入 config，断电保存，用于目标筛选。|
|  `reference_circle`  |业务基准参数|写入 config，软件层记录基准圆坐标用于运行时匹配。|
|  `edge_direction`  |算法提取参数|写入 config，记录边缘极性。|
|  `extract_sensitivity`  |算法提取参数|写入 config，记录灵敏度。|
|  `regions`  |算法检测参数|写入 config，记录 ROI 区域。|
|  `scale_ratio`  |工具换算参数|写入 config，物理缩放系数值。|
|  `enable_upper_limit`  |工具判定参数|写入 config，上限判定开关。|
|  `lower/upper_limit`  |工具判定参数|写入 config，判定上下限阈值。|

---

## 7. 算法接口与数据结构定义

```c++
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

/** * @brief 直径测算算法句柄 */
typedef void* nvs_alg_diam_handle_t;

// ==========================================================================
// 2. 数据结构与枚举
// ==========================================================================

/** * @brief 边缘方向极性 (输入参数与输出属性共用)
 */
typedef enum {
    NVS_ALG_DIAM_DIRECTION_ALL           = 0, // 全部方向 (仅作为输入参数时可用)
    NVS_ALG_DIAM_DIRECTION_DARK_TO_LIGHT = 1, // 暗到明
    NVS_ALG_DIAM_DIRECTION_LIGHT_TO_DARK = 2  // 明到暗
} nvs_alg_diam_edge_direction_e;

/** * @brief 提取参数 
 */
typedef struct {
    int32_t edge_direction;      // 提取极性限制，参考 nvs_alg_diam_edge_direction_e
    int32_t extract_sensitivity; // 灵敏度数值阈值
} nvs_alg_diam_extract_param_t;

/** * @brief 单个候选圆及其特征属性 
 */
typedef struct {
    nvs_circle_t circle;         // 几何坐标与半径
    int32_t edge_direction;      // 该圆的实际边缘极性，参考 nvs_alg_diam_edge_direction_e
} nvs_alg_diam_circle_info_t;

/** * @brief 候选圆数组 
 */
typedef struct {
    uint8_t count;                                        
    nvs_alg_diam_circle_info_t circles[NVS_ALG_DIAM_MAX_CANDIDATE_NUM]; 
} nvs_alg_diam_candidate_circle_array_t;

// ==========================================================================
// 3. 接口定义
// ==========================================================================

nvs_alg_diam_handle_t nvs_alg_diam_create(void);
void nvs_alg_diam_destroy(nvs_alg_diam_handle_t in_handle);

// --------------------------------------------------------------------------
// 参数配置接口
// --------------------------------------------------------------------------

/**
 * @brief 设置检测区域 (ROI)
 */
int32_t nvs_alg_diam_set_region(nvs_alg_diam_handle_t in_handle, const nvs_region_t *in_regions, int32_t in_region_count);

/**
 * @brief 设置提取参数 (极性、灵敏度)
 */
int32_t nvs_alg_diam_set_param(nvs_alg_diam_handle_t in_handle, const nvs_alg_diam_extract_param_t *in_param);

// --------------------------------------------------------------------------
// 动作接口
// --------------------------------------------------------------------------

/**
 * @brief 运行提取与测算
 * @note  算法底层不再维护基准圆，每次运行统一返回所有符合设定参数的候选圆及属性。目标匹配由上层软件处理。
 * @param[in]  in_img              图像帧
 * @param[out] out_candidate_array 输出包含圆属性的全量候选数组
 */
int32_t nvs_alg_diam_run(
    nvs_alg_diam_handle_t in_handle, 
    const nvs_image_t *in_img, 
    nvs_alg_diam_candidate_circle_array_t *out_candidate_array
);

#ifdef __cplusplus
}
#endif

#endif // __NVS_ALG_DIAM_H__
```
