---
id: 69fscsnr4exu15zfqmtjzch
title: Dearpygui
desc: ''
updated: 1776310064727
created: 1776310056247
---

# DearPyGui Demo 指南

这份指南专为需要快速搭建底层视觉算法验证、DAG 节点调试工具的开发者准备。我们将利用 DearPyGui (DPG) 高效的 GPU 渲染特性，实现节点编辑、图像流预览（支持 JPG/YUV 转换）以及动态的交互式框选标注。

---

## 1. 依赖安装

在开始之前，需要准备好 DPG 以及处理图像矩阵的必备工具。打开终端执行以下命令：

```bash
pip install dearpygui numpy opencv-python
```

**依赖说明：**
* **dearpygui**: 核心 GUI 引擎。
* **numpy**: 作为 C 和 Python 之间的“内存粘合剂”，DPG 的纹理系统极其依赖一维浮点数组（Float32）。
* **opencv-python (cv2)**: 负责读取本地 JPG 文件，以及极其高效的 YUV 到 RGB 色彩空间转换（底层由 C++ 实现，速度极快）。

---

## 2. 核心功能与实现原理

在 DPG 的架构中，万物皆为 Item（组件），通过全局唯一的 Tag（通常是字符串或自动生成的整数）进行索引和绑定。我们拆解一下即将实现的核心功能：

### 2.1 节点编辑器 (Node Editor)
DPG 拥有原生的节点编辑器，非常适合映射底层 C/C++ 的 DAG（有向无环图）处理管线。
* **原理**：节点编辑器是一个独立的容器（`add_node_editor`）。内部包含多个节点（`add_node`），节点上拥有输入或输出引脚（`add_node_attribute`）。
* **连线逻辑**：当用户拖拽引脚形成连线时，引擎会触发你绑定的回调函数，在此回调中你可以记录管线的拓扑依赖关系，或者调用底层的 `nvs_prog` 流转逻辑。

### 2.2 纹理注册与渲染 (JPG 与 YUV 处理)
DPG 不像传统 GUI 提供现成的“图片框”直接传路径，它的底层是直接向 GPU 显存塞纹理数据。
* **纹理注册表 (Texture Registry)**：必须先开辟一个纹理缓冲区（`add_dynamic_texture`）。
* **数据格式**：无论是 JPG 还是相机的原始 YUV，最终送入 DPG 的都必须是 **RGBA 通道的归一化浮点数组 (Float32, 值域 0.0~1.0)**。
* **YUV 渲染管线**：对于底层相机拿到的 YUV/NV12 数据，最稳妥的做法是：接收 YUV 裸指针/字节流 -> NumPy 数组 -> OpenCV 转 RGB -> 除以 255 归一化 -> 更新到 DPG 动态纹理。虽然增加了一步 CPU 转换，但在几十 FPS 的验证场景下，NumPy 依然能轻松胜任。

### 2.3 自定义渲染与标注框选 (Drawlist)
这是视觉验证工具的灵魂：在图像上画线、画框、做交互。
* **绘图层 (Drawlist)**：DPG 提供了一个可以直接调用 GPU 2D 绘图指令的画布（`add_drawlist`）。我们将图像作为底图画在这个画布上（`draw_image`）。
* **交互机制**：通过注册**全局鼠标事件**（Mouse Click, Mouse Drag, Mouse Release），捕获当前鼠标在画布上的局部坐标。
* **动态框选**：在鼠标拖拽（Drag）的回调中，实时清理画布的上一帧矩形，并用新的起点和终点坐标调用 `draw_rectangle`，即可实现丝滑的 ROI（感兴趣区域）框选。

---

## 3. 完整演示源码

以下代码是一个“开箱即用”的独立单文件 Demo。为了保证你复制后直接能跑，代码内部使用 NumPy 生成了模拟的图像噪声（代替真实的本地 JPG 或 YUV），并实现了完整的拖拽框选和节点连线。

你可以将代码保存为 `nvs_tool_demo.py` 直接运行：

```python
import dearpygui.dearpygui as dpg
import numpy as np

# ==========================================
# 1. 初始化与全局状态
# ==========================================
dpg.create_context()

# 图像的宽高
IMG_WIDTH = 640
IMG_HEIGHT = 480

# 全局状态：用于保存用户鼠标框选的坐标
bbox_state = {
    "is_drawing": False,
    "start_pos": [0, 0],
    "end_pos": [0, 0]
}

# ==========================================
# 2. 图像/纹理处理逻辑
# ==========================================
def generate_mock_image():
    """模拟从底层框架(如VI节点或JPG解码)拿到了一帧图像数据"""
    # 生成随机彩色噪声 (高度, 宽度, RGBA)
    img_data = np.random.rand(IMG_HEIGHT, IMG_WIDTH, 4).astype(np.float32)
    # 将 NumPy 的高维数组展平为 DPG 唯一能接受的 1D Float 数组
    return img_data.flatten()

# 创建纹理注册表，并将初始图像注册到 GPU 显存中
with dpg.texture_registry(show=False):
    # 使用 Dynamic Texture，意味着我们可以在主循环中实时更新这块显存
    dpg.add_dynamic_texture(width=IMG_WIDTH, height=IMG_HEIGHT, 
                            default_value=generate_mock_image(), 
                            tag="preview_texture")

def update_image_frame():
    """模拟视频流刷新，供外部/定时器调用"""
    new_frame = generate_mock_image()
    # 零拷贝更新底层纹理
    dpg.set_value("preview_texture", new_frame)

# ==========================================
# 3. 鼠标交互与框选标注回调
# ==========================================
def on_mouse_click(sender, app_data):
    # 判断鼠标是否点在绘图区内
    if dpg.is_item_hovered("image_drawlist"):
        # 获取鼠标相对于 drawlist 左上角的局部坐标
        mouse_pos = dpg.get_drawing_mouse_pos()
        bbox_state["is_drawing"] = True
        bbox_state["start_pos"] = mouse_pos
        bbox_state["end_pos"] = mouse_pos

def on_mouse_drag(sender, app_data):
    if bbox_state["is_drawing"]:
        mouse_pos = dpg.get_drawing_mouse_pos()
        bbox_state["end_pos"] = mouse_pos
        # 实时重绘画布
        refresh_drawlist()

def on_mouse_release(sender, app_data):
    if bbox_state["is_drawing"]:
        bbox_state["is_drawing"] = False
        print(f"[底层协议] ROI下发 -> X1:{bbox_state['start_pos'][0]}, Y1:{bbox_state['start_pos'][1]}, X2:{bbox_state['end_pos'][0]}, Y2:{bbox_state['end_pos'][1]}")

def refresh_drawlist():
    """清理并重绘画布（底图 + 框选几何体）"""
    dpg.delete_item("image_drawlist", children_only=True)
    
    # 第一步：把底图画上去
    dpg.draw_image("preview_texture", pmin=(0, 0), pmax=(IMG_WIDTH, IMG_HEIGHT), parent="image_drawlist")
    
    # 第二步：把参考线画上去（比如中心十字准星）
    center_x, center_y = IMG_WIDTH // 2, IMG_HEIGHT // 2
    dpg.draw_line((center_x, 0), (center_x, IMG_HEIGHT), color=(0, 255, 0, 100), thickness=1, parent="image_drawlist")
    dpg.draw_line((0, center_y), (IMG_WIDTH, center_y), color=(0, 255, 0, 100), thickness=1, parent="image_drawlist")
    
    # 第三步：如果正在框选，绘制动态矩形框
    if bbox_state["start_pos"] != bbox_state["end_pos"]:
        dpg.draw_rectangle(bbox_state["start_pos"], bbox_state["end_pos"], 
                           color=(255, 0, 0, 255), thickness=2, parent="image_drawlist")

# 绑定全局鼠标事件（左键点击、拖拽、松开）
with dpg.handler_registry():
    dpg.add_mouse_click_handler(button=0, callback=on_mouse_click)
    dpg.add_mouse_drag_handler(button=0, callback=on_mouse_drag)
    dpg.add_mouse_release_handler(button=0, callback=on_mouse_release)

# ==========================================
# 4. 节点连线回调 (DAG 拓扑逻辑接口)
# ==========================================
def link_callback(sender, app_data):
    # app_data 包含元组: (引脚1的ID, 引脚2的ID)
    node_out, node_in = app_data
    # 在界面上画出连线
    dpg.add_node_link(node_out, node_in, parent=sender)
    print(f"[流转调度] 节点拓扑更新: 接口 {node_out} 已连接至 接口 {node_in}")

# ==========================================
# 5. 主 UI 布局构建
# ==========================================
with dpg.window(label="底层架构调试控制台", width=1200, height=800):
    
    # --- 顶栏工具 ---
    with dpg.group(horizontal=True):
        dpg.add_button(label="模拟触发取图", callback=update_image_frame)
        dpg.add_text("在下方图像区按住鼠标左键可拖拽绘制 ROI。")
        
    dpg.add_separator()

    # --- 左右分栏布局 ---
    with dpg.group(horizontal=True):
        
        # 左侧：视觉图像与标注区 (Drawlist)
        with dpg.child_window(width=660, height=520, border=True):
            dpg.add_text("算法验证预览窗")
            # 创建绘图面板
            with dpg.drawlist(width=IMG_WIDTH, height=IMG_HEIGHT, tag="image_drawlist"):
                pass
            # 首次渲染画布
            refresh_drawlist()

        # 右侧：算法管线节点编辑器 (Node Editor)
        with dpg.child_window(width=500, height=520, border=True):
            dpg.add_text("DAG 节点流转拓扑")
            
            with dpg.node_editor(callback=link_callback, width=-1, height=-1):
                
                # 节点1：取图入口
                with dpg.node(label="VI 取图节点 (I0)", pos=[20, 50]):
                    with dpg.node_attribute(attribute_type=dpg.mvNode_Attr_Output):
                        dpg.add_text("YUV 内存输出")
                        
                # 节点2：OCR 或其他处理
                with dpg.node(label="OCR 核心识别节点", pos=[250, 50]):
                    with dpg.node_attribute(attribute_type=dpg.mvNode_Attr_Input):
                        dpg.add_text("Pipeline 黑板图")
                    with dpg.node_attribute(attribute_type=dpg.mvNode_Attr_Output):
                        dpg.add_text("识别结果 (JSON)")

# ==========================================
# 6. 引擎启动与渲染循环
# ==========================================
dpg.create_viewport(title='DearPyGui 极客验证工具', width=1280, height=850)
dpg.setup_dearpygui()
dpg.show_viewport()

# 保持默认的主循环（底层为 Retained Mode 代理）
dpg.start_dearpygui()
dpg.destroy_context()
```