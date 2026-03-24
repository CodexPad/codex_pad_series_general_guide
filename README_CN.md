# CodexPad 系列通用指南

[English](README.md)

## 概述

**CodexPad 系列通用指南​**是面向所有 CodexPad 系列蓝牙手柄用户的综合性技术文档。本指南面向全系列产品提供了统一的核心概念说明、连接配置方法以及跨平台的开发支持信息。

如需查阅特定型号的产品规格、详细操作步骤（如安装、获取地址等）或电气特性，请根据您持有的设备型号，选择下方对应的产品文档链接进行查看。为方便不同网络环境的用户访问，每个型号均提供了两个内容相同的镜像链接。

| CodexPad型号 | 产品详情链接 |
| :--- | :--- |
| CodexPad-C10 | [1. 大陆版（Gitee）链接（推荐中国大陆用户查看)](https://gitee.com/CodexPad/codex_pad_c10#codexpad-c10)<br>[2. 国际版（GitHub）链接](https://github.com/CodexPad/codex_pad_c10/blob/main/README_CN.md#codexpad-c10) |
| CodexPad-S10 | [1. 大陆版（Gitee）链接（推荐中国大陆用户查看)](https://gitee.com/CodexPad/codex_pad_s10#codexpad-s10)<br> [2. 国际版（GitHub）链接](https://github.com/CodexPad/codex_pad_s10/blob/main/README_CN.md#codexpad-s10) |

## Bluetooth Device Address(BD_ADDR)说明

在连接与开发时，可能需要使用到设备的**唯一**标识：Bluetooth Device Address(BD_ADDR)。它如同设备的“身份证号”，若需指定连接本设备而非广播中的其他设备，将会使用到此信息

> **注意：术语说明**  
> 本文档统一使用标准术语 Bluetooth Device Address(BD_ADDR)。请注意，在部分旧的示例代码、SDK或针对旧型号设备的文档中，可能会使用 MAC Address​ 来指代 Bluetooth Device Address(BD_ADDR)。在蓝牙上下文中，两者通常指向同一个地址。

它由12位十六进制字符组成，以冒号分隔，格式为`XX:XX:XX:XX:XX:XX`（其中`X`为 0-9 或 A-F），例如：`E4:66:E5:A2:24:5D`。

### 获取方法

请您根据所持 CodexPad 的具体型号，选择最便捷的方式：

1. **查看产品机身**：部分型号（如 **CodexPad-S10**）的**外壳上已直接印制了 Bluetooth Device Address**。这是最快速的获取方式。

2. **通过元数据功能读取**：如果您的设备机身**没有**印制Bluetooth Device Address（如 **CodexPad-C10**），您需要通过其 **USB 元数据访问功能** 来获取。当手柄通过USB连接电脑时，它会模拟为串口并自动上报包含**Bluetooth Device Address**在内的完整设备信息。详细操作指南：[《CodexPad 元数据访问功能》](metadata_cn.md#codexpad元数据访问功能)

## 连接方式说明

CodexPad提供了两种灵活的主机连接方式，您可以根据开发场景和需求进行选择。

### 方式一：Bluetooth Device Address 直连

此方式通过手柄唯一的 **Bluetooth Device Address** 进行精准连接。

- **工作原理**：在您的主机代码中，预先写入目标手柄的Bluetooth Device Address。程序启动后将直接尝试与这个特定地址的设备建立连接。

- **核心特点**：**指向明确，连接稳定**。适用于开发环境固定、手柄与主机配对关系确定的场景（例如，某台机器人固定使用某个特定手柄控制）。

- **使用前准备**：请根据您的CodexPad系列，查阅对应产品文档以获取 Bluetooth Device Address (BD_ADDR)​ 的具体操作方法，并完成记录。

### 方式二：按键掩码扫描连接

此方式是 **CodexPad 产品的特色功能**，它通过让手柄在广播时上报按键状态，实现基于物理交互的智能匹配连接。

- **工作原理**：在您的主机代码中，定义一个“按钮掩码”（例如：同时按住 **Start键 + A键**）。主机在扫描附近设备时，只会与那些**按键状态恰好与掩码匹配**（即按住了指定按键组合且未按其他键）的、信号最强（RSSI最大）的手柄建立连接。

- **核心特点**：

    1. **防止干扰**：在多个手柄的环境中，能有效避免意外连接到其他设备。

    2. **灵活切换**：代码不绑定具体手柄Bluetooth Device Address，您可以随时拿起任何一台处于可发现状态、并正确触发按键条件的手柄进行连接，实现设备的无缝切换。

- **适用场景**：适用于多设备环境、演示场景、需要灵活更换手柄或不想在代码中硬编码硬件地址的项目。

> **提示**：关于“按键掩码扫描连接”更详细的设计意图、优势及使用示例，请参阅对应开发平台的库的文档和示例代码。

## 连接与使用

### Arduino IDE库和示例代码

适用于在 **Arduino IDE** 或 **PlatformIO** 中进行开发。

| 支持的硬件平台 |
| :--- |
| ESP32 |
| ESP32-S2 |
| ESP32-S3 |
| ESP32-C3 |
| ESP32-C5 |
| ESP32-C6 |
| ESP32-H2 |
| ESP32-P4 |

*请根据您所在的网络环境，选择以下任一链接下载查看，内容完全相同*：

- [大陆版（Gitee）链接（推荐中国大陆用户使用)](https://gitee.com/CodexPad/codex_pad_arduino_lib#codexpad-arduino-lib)

- [国际版（GitHub）链接](https://github.com/CodexPad/codex_pad_arduino_lib/blob/main/README_CN.md#codexpad-arduino-lib)

---

### MicroPython库和示例代码

适用于在 **MicroPython** 固件上进行开发。

#### 支持的硬件平台

**理论支持范围**：本库理论上支持所有运行了内置标准**bluetooth**模块的**MicroPython**固件、且硬件本身具备 **低功耗蓝牙（BLE）** 功能的开发平台。您可以通过 [MicroPython官方下载页面（已筛选BLE功能）](https://micropython.org/download/?features=BLE)来查找和确认适合您设备的、支持蓝牙的官方固件。

下表列出了我们已测试可用的部分硬件平台：

| 支持的硬件平台 |
| :--- |
| ESP32 |
| ESP32-S3 |
| ESP32-C2 |
| ESP32-C3 |
| ESP32-C5 |
| ESP32-C6 |
| ESP32-P4 |
| Raspberry Pi Pico W |
| Raspberry Pi Pico 2 W |

*请根据您所在的网络环境，选择以下任一链接下载查看，内容完全相同*：

- [大陆版（Gitee）链接（推荐中国大陆用户使用)](https://gitee.com/CodexPad/codex_pad_mpy_lib#codexpad-micropython-lib)

- [国际版（GitHub）链接](https://github.com/CodexPad/codex_pad_mpy_lib/blob/main/README_CN.md#codexpad-micropython-lib)

---

### Micro:bit图形化扩展

适用于在 **MakeCode** 图形化编程环境中为 micro:bit 开发。

| 硬件平台 |
| :--- |
| micro:bit |

*请根据您所在的网络环境，选择以下任一链接下载查看，内容完全相同*：

- [大陆版（Gitee）链接（推荐中国大陆用户使用)](https://gitee.com/CodexPad/codex_pad_makecode_extension/blob/main/READMD_CN.md#codexpad-extension-for-microbit-makecode)

- [国际版（GitHub）链接](https://github.com/CodexPad/codex_pad_makecode_extension/blob/main/READMD_CN.md#codexpad-extension-for-microbit-makecode)

---

### 图形化

> TODO

---

## 手柄TX Power（发射功率）设置

### 发射功率概述

发射功率（TX Power）是指手柄蓝牙射频信号的输出强度，其单位为 **dBm（分贝毫瓦）**。这是一个用于表示功率绝对值的对数单位。

- **数值大小的意义**：在dBm的标度下，**数值越大，代表发射功率越强**。例如，`+3 dBm` 的功率就比 `0 dBm` 强，而 `0 dBm` 又比 `-5 dBm` 强。每增加大约 `3 dBm`，实际功率约翻一倍。
- **正负号的意义**：`0 dBm` 代表1毫瓦（mW）的功率。**正数（如 +3 dBm）表示功率大于1毫瓦，负数（如 -12 dBm）表示功率小于1毫瓦。**
- **作用与权衡**：**增大发射功率可有效提升通信距离和信号抗干扰能力，但同时也会显著增加功耗，缩短电池续航时间。** 用户应根据实际使用场景（如通信距离、环境障碍物、对续航的要求）在信号强度和功耗之间进行权衡，选择一个合适的固定值。

### 设置方法与注意事项

- **默认值**：手柄在每次与主机建立蓝牙连接后，其发射功率**默认会自动重置为 0 dBm**。
- **设置方式**：必须通过主机端代码调用专用API（相关库中已提供）将目标功率值发送给手柄，设置会**立即生效**。
- **重要提示**：为避免无线连接出现波动，建议在单次连接中**仅设置一次**，避免频繁更改。请在设备初始化阶段根据应用场景选定一个合适的功率档位。

### 支持档位

手柄支持通过主机编程设置以下12个档位的发射功率（**从弱到强**排列）：

| 发射功率档位 |
| :--- |
| -16 dBm |
| -12 dBm |
| -8 dBm |
| -5 dBm |
| -3 dBm |
| 0 dBm |
| +1 dBm |
| +2 dBm |
| +3 dBm |
| +4 dBm |
| +5 dBm |
| +6 dBm |

您可以在主机示例代码中找到相应的API进行设置。

---

## 重要协议说明

CodexPad系列手柄采用**标准的低功耗蓝牙协议**进行通信，这与常见的**BLE HID**协议有本质区别。

- **协议类型**：本产品使用专为嵌入式开发优化的自定义标准BLE协议，而非即插即用的BLE HID协议。

- **连接与使用**：这意味着您的电脑或手机操作系统（如Windows、macOS、Android、iOS、Linux）在扫描并成功连接本手柄后，**不会将其识别为标准的游戏控制器或输入设备**，因此无法直接在任何游戏或应用中使用。

- **正确使用方式**：本产品的设计初衷是作为**一个可编程的输入模块**，必须通过您自己编写的主机端代码来连接手柄、读取其数据，并实现您所需要的控制逻辑。

  - **主要支持模式**：我们为主流的硬件平台提供了完善的库和示例代码，这是推荐且官方技术支持的主要方向。

  - **进阶使用模式**：技术开发者也可以在**电脑（如使用Python、Node.js等）或手机（如使用Android Studio、Swift等）** 上编写主机端代码来连接并控制手柄。此为实现特定项目的进阶用法，**我司不提供此模式下的官方技术支持、库或示例**。关于其底层BLE GATT通信特征，我们后续将根据市场需求评估是否提供详细说明文档。

  - **理论兼容性说明**：从技术原理上讲，本手柄**理论上支持所有具备低功耗蓝牙（BLE）主设备功能**的硬件平台。若您需要在官方未明确支持的平台上使用（例如其他型号的单片机、单板计算机或特定设备），您需要**基于我们公开的通信协议自行开发主机端驱动**，或关注我们未来的更新以等待官方支持。

> **💡 请务必知悉**：本产品是为**可编程嵌入式项目**设计的开发工具。若您需要连接电脑或手机即插即用，本产品无法满足需求。若您是有能力的开发者，希望在PC或手机端集成本手柄，需要您自行研究其BLE通信协议。

---

## 了解更多

[《CodexPad 元数据访问功能》](metadata_cn.md)
