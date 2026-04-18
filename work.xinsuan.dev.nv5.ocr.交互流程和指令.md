---
id: um7z69uj30rgtgl9wn5jfnc
title: 交互流程和指令
desc: ''
updated: 1776135947348
created: 1776049027558
---

# 当前已有开发进度对应的指令流程

```json
{
    // ==========================================
    // 1. 全局配置与模式路由
    // ==========================================
    "ocrmode": "date",                  // [String] 模式路由。当前设为日期模式，但下方的判定条件却缺少日历同步字段，可能是旧版兼容或不完整的测试用例
    "ocr_tool_name": "666",             // [String] 工具界面展示名称
    "threshold": 80,                    // [Int] 置信度阈值。低于80分的字符将被剔除
    "pos_adjust_ref": "T0",             // [String] ⭐位置补正依赖：检测前，regions内所有点的坐标必须叠加上 T0 工具输出的偏移矩阵

    // ==========================================
    // 2. 检测区域设定 (ROI)
    // 注意：当前报文中 base, mask, unmask 坐标完全重叠，这在实际业务中通常是占位符或无效配置
    // ==========================================
    "regions": [
        {
            "op": "base",               // [String] 基础检测框
            "shape": "rect",            // [String] 声明为四边形，约束 UI 渲染
            "pts": [                    // [Array] 4个物理像素顶点坐标
                { "x": 240, "y": 190 },
                { "x": 260, "y": 190 },
                { "x": 260, "y": 210 },
                { "x": 240, "y": 210 }
            ]
        },
        {
            "op": "mask",               // [String] 掩膜屏蔽框 (告诉算法忽略此区域)
            "shape": "rect",
            "pts": [
                { "x": 240, "y": 190 },
                { "x": 260, "y": 190 },
                { "x": 260, "y": 210 },
                { "x": 240, "y": 210 }
            ]
        },
        {
            "op": "unmask",             // [String] 取消掩膜框 (在掩膜中重新开放的区域)
            "shape": "rect",
            "pts": [
                { "x": 240, "y": 190 },
                { "x": 260, "y": 190 },
                { "x": 260, "y": 210 },
                { "x": 240, "y": 210 }
            ]
        }
    ],

    // ==========================================
    // 3. 字符识别白名单 (加速检索，降低误判)
    // ==========================================
    "read_type": {
        "Read_letters": 0,              // [Int] 0:不读字母
        "Read_numbers": 0,              // [Int] 0:不读数字
        "Read_symbol": 0,               // [Int] 0:不读符号
        "read_data": 0                  // [Int] 0:不读复杂数据/汉字
    },

    // ==========================================
    // 4. 判定标准 (OCV 验证逻辑)
    // ==========================================
    "Judgment_criteria": {
        "Control_character": "1234",    // [String] 主控对比字符串。由于上面 ocrmode 是 date，这里如果缺少动态日历参数，系统可能回退为纯静态比对
        "ocr_string_num": {             // [Object] 字符数量限制验证
            "Character_count_enable": 0,// [Int] 字符数校验总开关 (0:关闭)
            "max_char_count": 0,        // [Int] 允许的最大字符数
            "min_char_count": 0         // [Int] 允许的最小字符数
        }
    },

    // ==========================================
    // 5. 高级搜索与底层模型配置
    // ==========================================
    "Search_char_enable": 0,            // [Int] 漫游搜索开关 (0:关闭)
    "Search_char_range_enable": 0,      // [Int] 旋转角度搜索开关 (0:关闭)
    "Search_char_range": 0,             // [Float] 允许的旋转搜索角度
    "High_speed_mode": 0,               // [Int] 图像降采样加速开关 (0:关闭)
    "Escape_character": 32,             // [Int] 逃逸符设定：32 对应 ASCII 码的“空格 (Space)”，遇到空格时的跳过或分词逻辑
    "Dictionary": 64                    // [Int] 字典模型 ID：64 代表底层加载某一种特定字体的权重文件或模板库
}
```

# 上位机 交互定义与配置

待补充

# 下位机 判定逻辑实现

## 1. 下位机当前处理逻辑：透传算法层的位置ocv结果

- **位置约束**  ：校验字符的水平线性度（是否同行）、字符间距及固定占位（空格）
- **内容全等**  ：待匹配字符必须与模板字符   **一致**  （包括字间距、空格、外围字符、换行）

---

## 2. 新增：下位机需实现的判断逻辑

### 2.1 宽松匹配

1. **排版容错**
1. ：自动忽略字符间的随机空格
1.     - *示例*  ：模板   `20250101`   实际 OK   `2025  01 01`  

1. **内容容错**
1. ：允许出现无关前缀和后缀
1.     - *示例*  ：模板   `2025`   实际 OK   `批号:2025 A`  

1. **结构容错**
1. ：忽视换行。
1.     - *示例*  ：模板   `1234567`   实际 OK   `123;456;7`  

1. **组合校验（行约束 + 宽松匹配）**
1.     - *处理逻辑*  ：优先校验字符是否符合模板预设的行结构归属，通过后再对每一行字符独立执行上述第 1、2 项容错处理



### 2.2 拦截重码

- **场景**  ：模板   `2025`   $\rightarrow$ 实际 NG   `20252025`   或   `2025;2025`  （  `;`   表示换行）
- **目的**  ：防止喷码机复打


### 2.3 字符数量限制

- **逻辑**  ：将 ROI 内检测到的 OCR 字符总数作为宽松匹配的前提条件，若字符数量超过阈值（如背景干扰）则直接判定 NG


### 2.4 字符通配

- **逻辑**  ：允许用户在定义主控内容时设置通配符，符合通配逻辑的识别结果判定为 OK


## 3. 待确认问题

1. **汉字需求**  ：是否有汉字识别需求？  `有`  
1. **分隔符冲突 当前方案：使用**  `**\n**`  
1. ：  ~~算法需求 PPT 中提到的 ~~  `~~;~~`  ~~ 是否仅作为逻辑换行符？~~
1.     - ~~若文字内容本身包含 ~~  `~~;~~`  ~~，需评估字符冲突风险并制定规避协议。~~
    - ~~or：直接定义字符的“行索引属性”替代字符分隔符逻辑~~



---


# 算法层 数据源补齐

基于待实现的宽松逻辑需求，算法库需要提供所有ocr识别的字符结果用于业务层与主控字符比对，故 在结果结构体中

新增  `nvs_alg_ocr_raw_res_t`  数组，表示算法识别到的所有从左到右从上到下位置顺序的字符对应的字符框和类别，以及整个字符串对应的rect，其中表示换行，直接使用换行符  `\n`  

新增  `encoding_type`  ，用于表示字符编码格式是   `ascii `  或者   `utf-8`  

满足  **业务层实现宽松匹配、排版容错及重码拦截等定制化需求**

## 算法头文件新增内容

```c++
/**
 * @brief 所有ocr识别到的字符输出，包括换行符 (用于业务层宽松匹配)
 */
typedef struct {
    /** * 包含所有识别出的逻辑字符 数组内字符的位置顺序“从左到右、从上到下”
     * 额外的'\n'作为换行分隔符
     */
    char raw_string[NVS_ALG_OCR_CHAR_OUTPUT_NUM]; 

    /** * [单字符精确坐标] 与 raw_string 中的字符（\n则所有数据填充-1）一一对应
     *
     */
    nvs_rect_t raw_char_rects[NVS_ALG_OCR_CHAR_OUTPUT_NUM];

    /** * [字符总数] 字符数量(包括\n) 
     */
    int32_t raw_char_count;

    /** * [源区域] 该字符串整体所属的 ROI 区域坐标
     */
    nvs_rect_t string_rect;

} nvs_alg_ocr_raw_res_t;

//原有
typedef struct {
    uint8_t result_string[NVS_ALG_OCR_CHAR_OUTPUT_NUM];  //解码结果
    nvs_rect_t rect;  //定位框   
} nvs_alg_ocr_decode_result_t;

//原有
typedef struct {
    int32_t status;     // 判定结果 (0: OK, -1: NG)
    int32_t score;  //相似度得分
    int32_t num_results;  //解码个数
    nvs_alg_ocr_decode_result_t results[NVS_ALG_OCR_CHAR_OUTPUT_NUM];

    // ==> 新增
    // 编码类型：全局统一标识 (0: ASCII, 1: UTF-8)
    int32_t encoding_type;
    // 原始模式结果
    int32_t num_raw_results; 
    nvs_alg_ocr_raw_res_t raw_results[NVS_ALG_OCR_CHAR_OUTPUT_NUM];
} nvs_alg_ocr_result_t;

/**
 * @brief 算法处理 (run)
 * @param handle         算法句柄
 * @param img            当前要处理的图像
 * @param result_out     [输出] 算法运行结果
 * @return NVS_ALGO_OK: 成功, 其他: 失败
 */
int32_t nvs_alg_ocr_run(nvs_alg_ocr_handle_t handle, 
                            const nvs_image_t *img, 
                            nvs_alg_ocr_result_t *result_out);


```

## 下位机 宽松匹配逻辑实现

```c++
#include <stdio.h>
#include <string.h>
#include <stdint.h>
#include <stdbool.h>

#define MAX_STR_LEN 1024
#define MAX_LINES 16

// =========================================================
// 1. 算法层输出 (Mock 接口 C)
// =========================================================
typedef struct {
    int32_t status;                  // 遗留的接口A状态
    char raw_string[MAX_STR_LEN];    // 接口C：包含全量字符和 ';' 换行符的原始字符串
} nvs_alg_ocr_mock_out_t;

// =========================================================
// 2. 业务层判定策略配置 (对应 JSON 的 Judgment_criteria)
// =========================================================
typedef enum {
    NVS_MATCH_STRICT = 0,   // 严格模式 (依赖算法原生状态)
    NVS_MATCH_LOOSE  = 1,   // 宽松子串模式 (无视杂质)
    NVS_MATCH_MULTI  = 2    // 进阶多行模式
} nvs_match_strategy_e;

typedef struct {
    nvs_match_strategy_e strategy;
    
    // 全局防呆配置
    int32_t char_count_enable;
    int32_t max_char_count;
    int32_t min_char_count;
    int32_t allow_duplicate;        // 0: 重码拦截(NG), 1: 允许重码
    
    // 单行宽松匹配目标
    char single_target[128];        
    
    // 进阶多行匹配目标
    int32_t multi_line_count;
    char multi_targets[MAX_LINES][128]; 
} nvs_ocr_business_config_t;

// =========================================================
// 3. 业务层核心处理函数声明
// =========================================================
int32_t nvs_ocr_business_process(nvs_ocr_business_config_t *cfg, nvs_alg_ocr_mock_out_t *algo_out);
```

### 待确认的问题

1. 算法库的base和搜索区域是可以设置多个的吗？日期模式和字符串模式是否都可以？还是默认只能有一个搜索区域？
2. 对应问题1，算法run接口抛出的结果是怎么样的？