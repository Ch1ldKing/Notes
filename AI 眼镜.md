| 眼镜                     | soc           | 其他参数                                   |
| ---------------------- | ------------- | -------------------------------------- |
| Ray-Ban meta           | 高通骁龙AR1 Gen 1 | 五麦克风，12MP摄像头，32GBROM，1920x1440显示，电池 4h |
| Meta Lens Chat         |               |                                        |
| solos airgo3           |               |                                        |
| RayNeo Air 2           | 高通骁龙XR2       |                                        |
| Xreal air2s            | 高通骁龙XR2       |                                        |
| Amazon Echo Frames（音频） |               |                                        |
| Lucyd glasses          | 高通            |                                        |
| MYVU                   | 高通骁龙W5+ Gen 1 |                                        |
| INMO air2              | WTM2101（国产）   |                                        |
| Gyges Labs(显示集成在镜框中)   | 高通            |                                        |
其他可穿戴设备芯片


| producer | soc      |
| -------- | -------- |
| 联发科      | MT8788   |
| 骁龙       | 八核高通骁龙芯片 |
SOC（34%）、ROM+RAM（7%）、OEM（9%）、摄像头（5%）、电池（4%）、PCB（4%）和音响（3%）
![image.png](https://s2.loli.net/2024/09/20/K9tjrIXhTYCZD5H.png)

## 硬件
**soc**

| 品牌  | 价格  |
| --- | --- |
|     |     |
**eMCP**

| 品牌  | 价格        | 配置                                                       |
| --- | --------- | -------------------------------------------------------- |
| 佰维  | 70-100    | https://www.biwin.com.cn/product/detail/19               |
| 金士顿 | 200+      | https://www.kingston.com/cn/embedded/emcp-embedded-flash |
| 三星  | 咸鱼大量40-50 | 这个不清楚能不能用于可穿戴设备，好像比较大                                    |
**wifi/蓝牙**

| 品牌        | 价格  |
| --------- | --- |
| 高通QCA2066 |     |
**电源管理**
![7febd38c75e7b07fa19644ffbd9a3a5.jpg](https://s2.loli.net/2024/09/24/xb3jdvowYX1rZlM.jpg)
![346b21b30860eb76bdc1308ca0d76b3.jpg](https://s2.loli.net/2024/09/24/ow7Q6YGVdxIEUNW.jpg)

# Snapdragon W5+ Gen 1 Wearable Platform
cpu制程：4nm
平台总大小：边长<9.5mm,高度 0.48mm
核心soc：4核心 Arm架构 Cortex®-A53 处理器，频率1.7GHz
核心GPU：Qualcomm® Adreno™ A702核心，图形库OpenGL® ES 3.1，频率最高1GHz
协处理器：核心Cortex M55，图形架构2.5D，机器学习核心ARM Ethos-U55（NPU）
闪存：eMMC 4.5
内存：16位2133Mhz频率 DDR4
无线通信：WiFi 4，支持2.4和5频道，蓝牙5.3，支持蜂窝移动4G
射频：高通RFFE
图像处理器：支持16MP双摄像头
安全系统：高通TEE
AI引擎：Qualcomm® Hexagon™ V66K处理器
操作系统支持：Wear OS，RTOS，Android
接口：屏幕接口MIPI-DSI，存储支持DDR的QSPI，通用USB2.0
电源管理IC
![image.png](https://s2.loli.net/2024/09/25/zIZckAOngjX7xR3.png)
做实验专用的开发套件：
https://www.thundercomm.com/product/w5-development-kit/
价格2000美元
# Snapdragon XR2+ Gen 2 Platform
设备端 AI，Wi-Fi 7
# Snapdragon AR2 Gen 1 Platform
设备端 AI
# Snapdragon AR1 Gen 1 Platform
设备端 AI
# Snapdragon Wear 4100+ Platform
cpu制程：12nm
平台总大小：主处理器边长10 mm,PMIC(电源管理IC)28mm^2高度 0.69mm
核心soc：Arm架构 Cortex®-A53 处理器，频率2GHz
核心GPU：Qualcomm® Adreno™ A504核心，图形库OpenGL® ES 3.1
闪存：eMMC 4.5
内存：750Mhz DDR3
无线通信：WiFi 4，蓝牙5.0,4.2，支持蜂窝移动4G，各种卫星定位
射频：高通RFFE
图像处理器：支持 1080p @ 30 fps
安全系统：高通TEE
操作系统支持：Wear OS
接口：屏幕接口MIPI-DSI，通用USB2.0
电源管理IC

# 联发科 Linklt 平台
# 联发科MT2523
大小：9.0\*6.2mm（TFBGA）
核心CPU：ARM® Cortex®‐M4F MCU架构
内存：4MB PSRAM
闪存：4MB
无线通信:蓝牙2.1，4.0（丐版）
图像处理器：640 x 480
接口：屏幕MIPI(320\*320)，上麦克风，USB
# 三星7270
- **CPU 制程**: 14nm FinFET 制程
- **平台总大小**: 10x10mm (SiP-ePoP 封装)
- **核心 SoC**: 双核 Cortex-A53，1.0GHz
- **核心 GPU**: Mali-T720 MP1
- **闪存**: eMMC 5.0, SD 3.0
- **内存**: LPDDR3
- **无线通信**: WiFi, 蓝牙 4.2, FM 收音机
- **射频**: LTE Cat.4，下载速率 150Mbps，上传速率 50Mbps
- **图像处理器**: 支持 5MP 摄像头，30fps 录像和回放
- **操作系统支持**: WearOS
- **接口**: 显示支持最高 qHD (960x540)，视频支持 HEVC (H.264), VP8 编解码器
- **电源管理 IC**: 集成在 SiP-ePoP 封装中
# 三星W920
# 海思 W610

1. 硬件设计的基本流程中，是否需要先挑选好想购买的芯片
2. 对于硬件上的软件开发，是否需要先设计好硬件
3. 硬件参数的选择应该如何进行？如果选择平台，是否意味着更高的成本和更差的灵活性？
4. 如果是小公司或个人工程师，如何查找参数与价格，来挑选想购买的芯片
⚠️upload failed, check dev console
