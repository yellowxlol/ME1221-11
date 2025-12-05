# ME1221-11 情绪感应智能灯

> 《工程学导论》第二小组项目代码

## 📖 项目简介
这是一个基于树莓派和情绪识别技术的智能台灯控制系统。系统通过摄像头捕获用户面部表情，利用 **DeepFace** 进行情绪分析，并通过蓝牙控制 **米家智能台灯** 变换相应的颜色和亮度（例如：开心时显示暖橙色，低落时显示冷蓝色，专注时显示白光）。

## 🛠️ 硬件配置
- **核心控制器**: 树莓派 4B (Raspberry Pi 4B)
- **视觉模块**: 500万像素 CSI/USB 摄像头
- **执行设备**: 米家智能台灯 MJTD06YL (或其他支持 BLE 的灯具)

## ⚙️ 环境依赖与安装

在运行程序前，请确保树莓派已完成以下配置：

### 1. 系统更新与依赖安装
```bash
# 更新系统
sudo apt-get update && sudo apt-get upgrade

# 安装 OpenCV 和蓝牙系统库
sudo apt-get install python3-opencv bluez

# 安装 Python 依赖库
pip3 install -r requirements.txt
```

### 2. 硬件设置
* **启用摄像头**: 运行 `sudo raspi-config` -> `Interface Options` -> `Camera` -> `Enable`。
* **获取蓝牙 MAC 地址**:
  ```bash
  sudo hcitool lescan
  # 记录下台灯的 MAC 地址，并填入 emotion_light.py 的 DEVICE_MAC 变量中
  ```

## 🚀 运行方法

确保已连接摄像头并打开蓝牙：

```bash
sudo python3 emotion_light.py
```
*(注意：操作蓝牙通常需要 root 权限，所以建议加 sudo)*

## 🎮 操作说明
- 程序启动后会显示摄像头实时画面。
- 画面左上角显示当前识别到的情绪及灯光参数。
- 按键盘 **'q'** 键安全退出程序。
