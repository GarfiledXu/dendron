---
id: sudt6qeimpddbdjfn127rez
title: Pyqt
desc: ''
updated: 1776312039929
created: 1776312025076
---

# PyQt/PySide + QML Demo 指南

欢迎回到“正规军”的阵地！如果说 DearPyGui 是单兵作战的极客利器，那么 **Qt + QML** 就是用来打造对标基恩士商业级视觉控制器（CV-X/XG-X）的重型装甲。

在 Qt 的现代架构（尤其是 Qt6 / PySide6）中，有一个对于 C/C++ 开发者来说极其重要的架构剧变需要提前说明：**底层的 GPU 渲染管线升级了**。

### ⚠️ 关于 QML 与 OpenGL 的架构认知升级
在 Qt5 时代，QML 的底层确实是强绑定 OpenGL 的。但到了现代 Qt6，官方引入了 **RHI (Rendering Hardware Interface)**。
这意味着 QML 界面默认不再死磕 OpenGL，而是根据你的操作系统自动调用最高效的图形 API：
* Windows -> **Direct3D 11/12**
* macOS -> **Metal**
* Linux -> **Vulkan / OpenGL**

**因此，在 Python + QML 架构下，工业界最标准的“图像上屏”范式是：**
1. **Python / C++ 层**：利用 OpenCV (NumPy) 将外部的 JPG/YUV 裸数据处理成统一的像素矩阵，并打包成 `QImage`。
2. **桥梁层 (QQuickImageProvider)**：这是 Qt 专门为 QML 提供的图像数据泵。它负责将内存中的像素数据推送到前端。
3. **QML 前端层**：QML 的 `Image` 控件接收到数据后，其底层的 RHI 引擎会自动将数据转为 **GPU 纹理 (Texture)** 并通过显卡硬件加速渲染出来。你完全不需要手写任何着色器 (Shader) 或是考虑显存绑定。

---

## 1. 依赖安装

我们这里采用 Qt 官方亲儿子 **PySide6**（它是 PyQt6 的官方替代品，协议对商业闭源更友好，且 QML 支持更完善），搭配你已经熟悉的 numpy 和 opencv。

```bash
pip install PySide6 numpy opencv-python
```

---

## 2. 核心演示架构

在这个 Demo 中，我们将实现一个最经典的**前后端分离**架构，模拟 NVS 框架的数据流转：
* **`frontend.qml` (内嵌在代码中)**：用声明式语法编写一个极其现代、极具基恩士风格的深色工业 UI。
* **`ImageProvider`**：作为 C++ 内存和 QML GPU 纹理之间的桥梁。
* **`BackendController`**：Python 调度核心。负责加载图像（复用之前讨论过的 YUV/JPG 转 RGB 逻辑），并通知 QML 刷新。

---

## 3. 完整演示源码

为了让你能直接“一键运行”，我把 QML 脚本直接作为字符串内嵌到了 Python 代码中。你可以将其保存为 `qml_nvs_demo.py` 直接运行：

```python
import sys
import cv2
import numpy as np
from PySide6.QtCore import QObject, Signal, Slot, QByteArray, QUrl, Qt
from PySide6.QtGui import QImage, QColor
from PySide6.QtQml import QQmlApplicationEngine
from PySide6.QtQuick import QQuickImageProvider
from PySide6.QtWidgets import QApplication

# ==========================================
# 1. 图像数据桥梁：QQuickImageProvider
# ==========================================
class NvsImageProvider(QQuickImageProvider):
    """
    专门负责向 QML 引擎输送图像帧的提供者。
    QML 会通过 image://nvs_stream/xxxx 这种 URL 格式向这里要图片。
    """
    def __init__(self):
        # 声明我们提供的是 QImage 类型的数据
        super().__init__(QQuickImageProvider.Image)
        # 初始化一张纯黑色的占位图 (宽高 640x480, 格式 RGB888)
        self._current_image = QImage(640, 480, QImage.Format_RGB888)
        self._current_image.fill(QColor("black"))

    def requestImage(self, id, size, requestedSize):
        """当 QML 端的 Image 控件需要刷新时，底层会自动调用此函数"""
        return self._current_image

    def update_frame(self, qimage):
        """供 Python 后端调用的接口，用于更新内存中的图像帧"""
        self._current_image = qimage

# ==========================================
# 2. 业务逻辑控制枢纽：BackendController
# ==========================================
class BackendController(QObject):
    """
    这是前后端的粘合剂。负责处理底层管线的调度，并将信号发送给 QML 触发 UI 更新。
    """
    # 定义一个信号，用于通知 QML：底层图像已就绪，请重新拉取！
    frameReady = Signal()

    def __init__(self, image_provider):
        super().__init__()
        self.provider = image_provider
        self.frame_counter = 0

    @Slot()
    def trigger_pipeline(self):
        """
        这个槽函数会被 QML 界面的按钮点击触发。
        模拟从底层 NVS 框架的 [VI 取图节点] 拉取一帧图像。
        """
        self.frame_counter += 1
        print(f"[底层管线] 正在执行第 {self.frame_counter} 次流转调度...")

        # 1. 模拟生成图像裸数据 (这里用 NumPy 生成随机彩色噪点模拟相机输入)
        # 真实场景中：这里可以换成 cv2.imread 读取 JPG，或者 np.fromfile 读取 YUV，
        # 并通过 cv2.cvtColor 转为 RGB 格式。
        mock_rgb_matrix = np.random.randint(0, 255, (480, 640, 3), dtype=np.uint8)

        # 2. 跨语言内存绑定：NumPy (C内存) -> QImage (C++内存)
        # 注意：QImage.Format_RGB888 对应 OpenCV 的 cv2.COLOR_BGR2RGB 转换后的结果
        height, width, channel = mock_rgb_matrix.shape
        bytes_per_line = 3 * width
        
        # ⚠️ 极其关键的一步：这只是浅拷贝封装指针，没有性能损耗！
        qimg = QImage(mock_rgb_matrix.data, width, height, bytes_per_line, QImage.Format_RGB888)

        # 3. 将新帧推入 Provider，并通知 QML 前端刷新
        self.provider.update_frame(qimg)
        self.frameReady.emit()

# ==========================================
# 3. 现代工业风 QML 前端源码
# ==========================================
QML_SOURCE = """
import QtQuick 2.15
import QtQuick.Controls 2.15
import QtQuick.Layouts 1.15

ApplicationWindow {
    visible: true
    width: 1024
    height: 768
    title: "NVS 视觉验证控制台 (QML 架构)"
    
    // 基恩士风格深色背景
    color: "#1e1e1e"

    // 将 Python 后端注入的 controller 对象存为局部属性
    property var backend: nvsController

    // 核心布局：左右分栏
    RowLayout {
        anchors.fill: parent
        anchors.margins: 10
        spacing: 15

        // ==============================
        // 左侧：视觉图像显示区 (GPU 硬件加速)
        // ==============================
        Rectangle {
            Layout.fillWidth: true
            Layout.fillHeight: true
            color: "#000000"
            border.color: "#3c3c3c"
            border.width: 2
            
            // 图像渲染控件
            Image {
                id: cameraView
                anchors.centerIn: parent
                // 保持图像比例，不拉伸变形
                fillMode: Image.PreserveAspectFit
                
                // 绑定到我们用 Python 写的 Provider
                // 初始化时请求一次默认黑图
                source: "image://nvs_stream/init"
                
                // 使用底层 RHI/OpenGL 进行平滑缩放滤波
                smooth: true
                antialiasing: true
                
                // 动态刷新函数：通过在 URL 后面加随机数时间戳，强制 QML 放弃缓存重新拉取 GPU 纹理
                function reloadFrame() {
                    cameraView.source = "image://nvs_stream/frame_" + new Date().getTime()
                }
            }

            // 图像区域左上角的 OSD (屏幕显示) 文字
            Text {
                anchors.top: parent.top
                anchors.left: parent.left
                anchors.margins: 10
                text: "LIVE PREVIEW | GPU Accelerated"
                color: "#00ff00"
                font.pixelSize: 14
                font.bold: true
            }
        }

        // ==============================
        // 右侧：控制与参数面板
        // ==============================
        Rectangle {
            Layout.preferredWidth: 300
            Layout.fillHeight: true
            color: "#252526"
            border.color: "#3c3c3c"
            border.width: 1

            ColumnLayout {
                anchors.fill: parent
                anchors.margins: 20
                spacing: 20

                Text {
                    text: "DAG 管线调度"
                    color: "#ffffff"
                    font.pixelSize: 18
                    font.bold: true
                }

                Rectangle {
                    Layout.fillWidth: true
                    height: 2
                    color: "#3c3c3c"
                }

                // 核心触发按钮
                Button {
                    Layout.fillWidth: true
                    height: 50
                    text: "触发单次流转 (Trigger)"
                    font.pixelSize: 16
                    font.bold: true
                    
                    // 按钮点击事件：直接调用 Python 后端的槽函数！
                    onClicked: {
                        backend.trigger_pipeline()
                    }
                    
                    // 自定义按钮外观 (体验 QML 强大的样式系统)
                    contentItem: Text {
                        text: parent.text
                        font: parent.font
                        color: parent.down ? "#ffffff" : "#e0e0e0"
                        horizontalAlignment: Text.AlignHCenter
                        verticalAlignment: Text.AlignVCenter
                    }
                    background: Rectangle {
                        color: parent.down ? "#005a9e" : "#0078d7" // 科技蓝
                        radius: 4
                    }
                }
                
                // 占位符，把上面的按钮顶到上方
                Item { Layout.fillHeight: true }
            }
        }
    }

    // ==============================
    // 信号连接器：监听 Python 发来的信号
    // ==============================
    Connections {
        target: backend // 监听我们注入的 nvsController
        
        // 当 Python 发射 frameReady 信号时，执行以下 QML 动作
        function onFrameReady() {
            cameraView.reloadFrame() // 触发图像控件重绘
        }
    }
}
"""

# ==========================================
# 4. 主程序入口
# ==========================================
if __name__ == "__main__":
    # 启用高 DPI 缩放支持 (现代显示器必备)
    QApplication.setHighDpiScaleFactorRoundingPolicy(Qt.HighDpiScaleFactorRoundingPolicy.PassThrough)
    
    app = QApplication(sys.argv)
    engine = QQmlApplicationEngine()

    # 1. 注册核心 ImageProvider 
    # QML 里面写的 "image://nvs_stream/..." 就是指向这里
    img_provider = NvsImageProvider()
    engine.addImageProvider("nvs_stream", img_provider)

    # 2. 实例化后端控制器，并注入到 QML 引擎的上下文中
    controller = BackendController(img_provider)
    # 这一步极其关键：让 QML 里面能认识 "nvsController" 这个变量名
    engine.rootContext().setContextProperty("nvsController", controller)

    # 3. 加载 QML 字符串启动界面
    # 真实项目中通常是 engine.load("main.qml")，这里为了单文件演示使用 loadData
    engine.loadData(QByteArray(QML_SOURCE.encode('utf-8')))

    if not engine.rootObjects():
        sys.exit(-1)

    sys.exit(app.exec())
```

---

## 4. 架构灵魂解析 (如何打通 C++ 与 QML)

在这个架构中，最优雅的地方在于**零拷贝（Zero-Copy）思想的延续**：

在 `BackendController` 类的 `trigger_pipeline` 槽函数中，我们从 NumPy 创建 `QImage` 时：
`QImage(mock_rgb_matrix.data, width, height, bytes_per_line, ...)`
这里并没有申请新的内存！`QImage` 直接绑定了原 NumPy 数组（即底层算法吐出的 YUV 转换后的 C 连续内存）。

当 QML 的 `Image` 控件调用 `reloadFrame()` 时，Qt 底层的 RHI 引擎会直接将这块物理内存的内容通过 PCIe 总线 DMA 推送到显卡的 GPU 显存中进行绘制渲染。

**如果你未来要在 QML 里画框标注 (ROI)：**
相比于 DearPyGui 需要在 Python 里手写鼠标坐标捕捉，QML 则提供了一整套异常强大的 `MouseArea` 和 `Canvas` 组件。你只需在 QML 中声明一个透明的矩形块，允许用户拖拽，然后将其 X/Y/Width/Height 属性实时单向绑定到 Python 后端的一个配置结构体上，就能完美实现基恩士那样高度流畅的交互反馈。