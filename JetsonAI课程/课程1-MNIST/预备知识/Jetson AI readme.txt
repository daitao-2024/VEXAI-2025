以下是将全文翻译为中文的内容：

---

## 在 Jetson 上运行 VEX AI 示例程序
**工作原理及如何协同使用它们**

确保你使用的是 **VEXAI** SD 卡镜像，或者按照 JetsonImages 文件夹中的所有步骤从源代码构建。

**启动时，如果你的 Jetson 安装了正确的镜像，它会自动在后台运行 `highstakes.py`。如果你想停止后台运行，打开终端并输入：`sudo systemctl stop vexai`。这将停止当前会话的服务，但如果你重启 Jetson，该代码将再次在后台启动。**

**确保所有文件都在同一个文件夹中。** 这个文件夹应包含以下文件：`common.py, data_processing.py, labels.txt, model.py, highstakes.py, requirements.txt, V5Comm.py, V5MapPosition.py, V5Position.py, V5Web.py`。

主要的 Python 程序是 `highstakes.py`，它将所有辅助类整合在一起，以运行推理并从 Intel RealSense 相机返回物体信息。

要手动启动程序，在包含 `highstakes.py` 的根目录下打开终端并运行：

`python3 highstakes.py`

在 MainApp 类中，这将实例化 Intel RealSense 管道，该管道在 Camera 类中处理相机输入。我们将相机的深度和彩色分辨率设置为 640x480，帧率为 30 fps。

接下来，还会实例化另外 4 个类：
- `v5` 对象是来自 `V5Comm.py` 的 `V5SerialComms` 类，负责与 V5 Brain 进行串口通信。
- `v5Map` 对象使用 `MapPosition` 类，将从 2D 相机图像中推断出的物体投影到 3D 空间，以返回场地中每个物体的位置。
- `v5Pos` 对象是来自 `V5Position.py` 的 `v5GPS` 类，负责与 GPS 传感器进行串口通信。
- `v5Web` 是与 Web 仪表板通信的 WebSocket 服务器，该对象处理对相机、深度和物体数据的获取请求，并为 Jetson 设置 GPS 和 Intel RealSense 相机的偏移量。

> [!WARNING]
> **Jetson 中的 V5 GPS 偏移量不会自动反映到你的 Brain 代码中。你必须手动确保两者的偏移量一致，以便 Jetson 和 V5 Brain 的机器人位置相同。**

为了在相机图像上运行推理以检测 VEX HighStakes 的环和目标，我们使用 `model.py` 中的 `Model` 类。`Model` 类依赖两个辅助程序：`common.py` 由 NVIDIA 提供，简化了一些常用的 Tensor RT 方法；`data_processing.py` 处理数组调整和处理。我们的 VEX OverUnder 物体模型基于 YOLOv3 网络，你可以在这里阅读更多信息：https://arxiv.org/pdf/1804.02767.pdf。

`highstakes.py` 中的 *Processing* 类处理 Intel RealSense D435 相机的一个奇怪问题：在某些光照条件下，游戏物体的颜色会被错误读取，模型将无法准确检测物体。
> [!TIP]
> 我们建议在运行推理之前调整图像的色调、饱和度和明度分量，这将校正 RealSense 相机的 RGB 图像，使物体检测模型能够以更高的颜色准确性处理图像。
> 颜色校正可以通过 Web 仪表板实时更新。

你可以在下文看到模型训练时使用的渲染样本，以更好地理解它在现实生活中寻找的绿色、蓝色和红色范围。请注意，当 RealSense 相机安装在与下图相似的高度时，模型性能最佳。

<img src=assets/training_image.jpg width=48%> <img src=assets/training_image_random.jpg width=48%>

**GPS 和 Intel RealSense 相机偏移量：**

我们在 `V5Web.py` 中处理偏移量。`GPSOffset` 和 `CameraOffset` 类最初会初始化一个空的 JSON 文件，并从现有的 JSON 文件中读取和处理数据。它们被保存在 `V5Web.py` 所在的目录中。这应该是包含所有其他源文件的源目录（JetsonExample），但具体路径取决于你将 GitHub 仓库克隆到的位置。

你可以通过更改存储在 JSON 文件目录中的值来手动调整偏移量，而无需使用 Web 仪表板。编辑时要注意单位，但这种方法会在下次启动程序时更改相应的偏移量。

如果使用 Web 仪表板进行调整，`V5Web.py` 已处理在程序运行时实时更新 `V5MapPosition` 和 `V5Pos` 对象的新偏移量。这意味着你可以通过仪表板更新偏移量并立即在 Jetson 上看到变化。然而，这些变化不会反映到你的 V5 Brain 程序中，仍需手动更新。之所以能够做到这一点，是因为在 `overunder.py` 实例化 `v5Web` 对象时，`v5Pos` 和 `v5Map` 必须作为参数传入，因此可以在 `v5Web.py` 实例中直接调用这些对象，更新计算机器人位置和检测位置的偏移量变量的实际实例。

---

翻译保留了原文的技术细节和格式（如代码块和警告提示），确保内容清晰且易于理解。如果需要进一步调整或有其他问题，请告诉我！