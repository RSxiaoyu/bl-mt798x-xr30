# CMCC XR30 Ubootmod Bootloader (BL2 + FIP)

[![Build](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml/badge.svg)](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml)

专为 **中国移动 CMCC XR30** 打造的纯粹现代 `ubootmod` Bootloader 自动构建仓库。

## 特性
- **根治坏块死锁**：彻底废除 MTK-NMBM 坏块映射，杜绝兆易 GD5F1GM7 闪存 `-5` 擦写报错与死锁。
- **UBI 环境变量**：环境变量迁移至 UBI 卷 (`ubootenv` / `ubootenv2`)，磨损均衡，原生透明屏蔽坏块。
- **现代化 Failsafe**：内置 DHCP 服务，网线直连 LAN 口自动分配 IP，浏览器打开 `http://192.168.1.1` 即可恢复。
- **FIT 原生支持**：完美适配 [ImmortalWrt 25.12 All-in-FIT 单固件](https://github.com/RSxiaoyu/immortalwrt-xr30)，支持 Web 界面直接刷写 `sysupgrade.itb` 与纯内存加载 `recovery.itb`。
- **出厂校准保护**：单独保留 2MB 独立 `factory` 分区（`0x180000 - 0x380000`），确保原厂 Wi-Fi 校准参数与 MAC 地址绝对安全。

## 硬件规格
| 项 | 规格 |
| :--- | :--- |
| **SoC** | MediaTek MT7981B (双核 Cortex-A53 @ 1.3GHz) |
| **内存 / 闪存** | 512MB DDR4 / 128MB SPI-NAND (GD5F1GM7 等) |
| **交换机芯片** | MT7531AE (2.5G SGMII / GMII) |

## 刷入指南
在现有 U-Boot Web 界面（`192.168.1.1`）中：
1. **升级 ATF BL2**：上传 `bl2-mt7981-cmcc_xr30-ubootmod.bin` 刷入。
2. **升级 U-Boot FIP**：上传 `fip-mt7981-cmcc_xr30-ubootmod.bin` 刷入。
3. 重启设备后即刻拥有纯粹的 ubootmod 现代引导环境。
