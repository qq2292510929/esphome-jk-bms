# JK-BMS ESP32-C3 ST7789 显示屏项目

这个项目使用 ESP32-C3 开发板和 2.8寸 ST7789 显示屏，通过蓝牙连接并监控 JK-BMS 电池管理系统。

## 功能特性

- 蓝牙连接 JK-BMS
- 2.8寸 TFT LCD 彩色显示 (240x320)
- 三个显示页面，通过按钮切换
- 实时显示 SOC、电压、电流、功率等
- 显示电芯电压详情
- 温度监控
- 充放电和均衡状态指示

## 硬件要求

- ESP32-C3 开发板
- 2.8寸 ST7789 液晶显示屏模块 (SPI 接口)
- 杜邦线若干
- 可选：按钮用于页面切换

## 接线说明

| ESP32-C3 | ST7789 显示屏 |
|---------|--------------|
| GPIO2   | MISO         |
| GPIO6   | SCK/CLK      |
| GPIO7   | MOSI/SDA     |
| GPIO5   | CS           |
| GPIO4   | DC           |
| GPIO10  | RES/RST      |
| GPIO8   | BLK/Backlight |
| 3.3V    | VCC          |
| GND     | GND          |

可选按钮接线：
| ESP32-C3 | 功能 |
|---------|-----|
| GPIO9   | 页面切换按钮 (接 GND 时触发) |

## 配置文件

### 快速配置 (esp32c3-st7789-jkbms-display.yaml)

这是一个简化版本，包含基本的显示功能。

### 完整配置 (esp32c3-st7789-full.yaml)

这是一个完整版本，包含：
- 三个显示页面切换
- 所有16个电芯电压显示
- 状态指示
- 温度和循环信息
- 版本信息

## 使用步骤

1. **安装 ESPHome**
   ```bash
   pip3 install esphome
   ```

2. **配置 WiFi 和蓝牙**
   - 创建 `secrets.yaml` 文件，包含 WiFi SSID 和密码
   - 修改配置文件中的 `mac_address` 为你的 JK-BMS 蓝牙地址
   - 根据你的 BMS 硬件版本选择正确的 `protocol_version`

3. **编译和上传**
   ```bash
   esphome run esp32c3-st7789-full.yaml
   ```

4. **查找 BMS 蓝牙地址**
   - 使用手机蓝牙扫描工具查找
   - 设备名通常以 "JK" 开头

## 协议版本选择

| 协议版本 | 硬件版本 |
|---------|---------|
| JK04    | ≤ 3.0   |
| JK02_24S | 6.0 - 10.x |
| JK02_32S | ≥ 11.0  |

## 显示页面说明

### 页面 0: 概览 (Overview)
- SOC 大字体显示和进度条
- 总电压、电流、功率
- 温度信息 (T1, T2, MOSFET)
- 电芯电压统计 (最小、最大、平均、压差)
- 充放电/均衡/加热状态

### 页面 1: 电芯详情 (Cell Details)
- 16个电芯电压 (两列显示)
- 颜色指示 (红色=异常, 黄色=接近限值, 白色=正常)
- 最大/最小值

### 页面 2: 信息和状态 (Info & Status)
- 剩余容量
- 循环次数
- 状态文本
- 硬件/软件版本

## 注意事项

- ESP32-C3 的内存有限，如遇到编译错误可减少字体大小
- 显示屏刷新间隔为 3 秒
- OTA 更新时会自动断开蓝牙连接
- 确保 JK-BMS 蓝牙已开启

## 故障排除

1. **显示屏不亮**
   - 检查接线是否正确
   - 确认背光控制引脚配置
   - 检查 SPI 引脚是否与其他功能冲突

2. **蓝牙无法连接**
   - 确认 MAC 地址正确
   - 检查协议版本设置
   - 查看日志输出

3. **显示数据不准确**
   - 检查协议版本是否匹配
   - 确认 BMS 正常工作

## 参考资料

- [ESPHome JK-BMS 组件](https://github.com/syssi/esphome-jk-bms)
- [ESPHome ST7789 显示驱动](https://esphome.io/components/display/st7789v.html)
- [ESPHome SPI 总线](https://esphome.io/components/spi.html)
