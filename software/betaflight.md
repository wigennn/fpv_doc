# Betaflight 配置指南

Betaflight 是一款专为多旋翼无人机设计的开源飞控固件，提供高度可定制化的飞行性能调参功能。本文档涵盖从基础配置到高级调试的完整流程。

## 目录
1. [准备工作](#准备工作)
2. [连接飞控](#连接飞控)
3. [基础配置](#基础配置)
4. [PID 调节与滤波](#pid-调节与滤波)
5. [模式设置](#模式设置)
6. [CLI 常用命令](#cli-常用命令)
7. [备份与恢复](#备份与恢复)
8. [固件升级](#固件升级)
9. [注意事项](#注意事项)
10. [常见问题](#常见问题)

---

## 准备工作
1. ​**软件安装**​  
   下载 [Betaflight Configurator](https://github.com/betaflight/betaflight-configurator/releases)（支持Chrome插件或独立应用）。
2. ​**硬件连接**​  
   准备 Micro USB 数据线，确保飞控板供电（部分飞控需连接电池）。

---

## 连接飞控
1. 打开 Betaflight Configurator，点击右上角 ​**Connect**​ 按钮。
2. 选择正确的串口（如 `COM3` 或 `/dev/ttyACM0`），波特率默认 `115200`。
3. 成功连接后，顶部状态栏显示固件版本及飞控型号。

---

## 基础配置
### 端口 (Ports)
1. 在 ​**Ports**​ 标签页，启用接收机对应的 UART 端口（如 UART2 用于 SBUS）。
2. 关闭未使用的外设（如 GPS、LED）以释放资源。

### 接收机 (Receiver)
1. 前往 ​**Receiver**​ 标签页，选择协议类型（如 `SBUS`、`CRSF`）。
2. 检查遥控器通道映射是否正确（油门应响应通道3）。

### 电机 (Motors)
1. ​**重要：卸除螺旋桨！​**​  
   在 ​**Motors**​ 标签页，启用电机锁定开关，逐步推拉滑块测试各电机转向。
2. 若转向错误，通过 BLHeli Suite 调换电机线序或修改转向。

---

## PID 调节与滤波
### 基础原则
- ​**P**​（比例）：过高引起震荡，过低导致响应迟钝。
- ​**I**​（积分）：修正风速等外部干扰的误差。
- ​**D**​（微分）：抑制高频振动。

### 推荐步骤
1. 在 ​**PID Tuning**​ 标签页，选择预设（如 `RPM Filtering` 适用于现代穿越机）。
2. 根据飞行手感微调 Roll/Pitch/Yaw 的 PID 值（新手建议使用默认配置）。

---

## 模式设置
在 ​**Modes**​ 标签页绑定常用开关：
1. ​**Arm**​：解锁电机（通常为三段开关最低位）。
2. ​**Angle/Horizon**​：自稳模式，适合新手。
3. ​**Beeper**​：寻机蜂鸣器。
4. ​**Air Mode**​：保持电机怠速，提升机动性。

---

## CLI 常用命令
```bash
# 导出当前配置
dump

# 设置电机协议（DShot600）
set motor_pwm_protocol = DSHOT600
save

# 重置为默认设置
defaults nosave
save
```

--- 

## 备份与恢复
1. 备份​：在 ​Presets​ 标签页点击 ​Save to File​ 存储 .diff 文件。
2. ​恢复​：通过 ​Load from File​ 导入备份，或直接粘贴 CLI 命令。

---

## 固件升级
1. 断开飞控，进入 DFU 模式（按住飞控按钮后插入USB）。
2. 在 Betaflight Configurator 中选择 **​Firmware Flasher**，下载对应版本固件。
3. 勾选 **​Full Chip Erase**，点击 **​Flash Firmware**。

## 注意事项
1. 修改参数前务必备份配置。
2. 避免在 CLI 中随意执行 defaults（会重置所有参数）。
3. 升级固件可能导致配置丢失，需重新导入备份。

## 常见问题

### Q1: 无法连接飞控
- 检查 USB 数据线是否支持数据传输（部分线仅供电）。
- 尝试重新安装 USB 驱动（如 Zadig 替换驱动）。
### Q2: 接收机无信号
- 确认接收机与飞控的接线正确（地线/信号线）。
- 检查 Betaflight 中协议类型是否匹配遥控器输出。
### Q3: 电机无法启动
- 确认已解锁（Arm）且油门最低。
- 检查 ESC 校准是否正确完成。


---

​**备注**​：以上内容为 Betaflight 通用配置流程，具体参数需根据机型及硬件调整。建议参考官方文档 [Betaflight Wiki](https://github.com/betaflight/betaflight/wiki)。