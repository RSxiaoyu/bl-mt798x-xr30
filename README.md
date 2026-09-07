# CMCC XR30 Ubootmod Bootloader (BL2 + FIP)

[![Build CMCC XR30 Ubootmod Bootloader](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml/badge.svg)](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml)

专为**中国移动 CMCC XR30** 路由器量身打造的极简、高性能、现代化 `ubootmod` Bootloader 自动构建仓库。

直通上游官方 [Yuzhii0718/bl-mt798x-dhcpd](https://github.com/Yuzhii0718/bl-mt798x-dhcpd)，追求零冗余配置，专注于最佳适配方案。

---

## 🌟 为什么必须使用 `ubootmod`？

1. **彻底解决兆易创新 (GigaDevice) NAND 坏块死锁与擦除报错 `-5`**：
   - 官方 Release 默认版本强开 MTK-NMBM 坏块映射，在 GD5F1GM7 芯片上会误判坏块导致备用表溢出只读，且将 U-Boot 环境变量直接存放在 Raw MTD 上，导致擦写频繁报错 `-5` (`-EIO`) 与 `save failed`。
   - 本仓库的 **`ubootmod`** 变体彻底废除 NMBM，改用 Linux 原生 `spi-nand0` 直通。
   - 环境变量完全迁移至 UBI 卷 (`ubootenv` / `ubootenv2`)，具备 UBI 磨损均衡与透明坏块屏蔽，永久告别 `saveenv` 失败。

2. **原生 All-in-FIT 单固件支持**：
   - 完美适配主流 OpenWrt / [ImmortalWrt 25.12 All-in-FIT 架构](https://github.com/RSxiaoyu/immortalwrt-xr30)（Linux 6.6 / 6.12，`root=/dev/fit0 rootwait`）。
   - 支持在 Web 控制台直接升级 `sysupgrade.itb`。
   - 支持在 `/initramfs.html` 纯内存免擦写直接引导 `recovery.itb`。

3. **现代化 Failsafe 控制台**：
   - 基于 Bootstrap 的现代化响应式 Web 界面（支持中英文切换）。
   - 内置 **DHCP 服务器**：网线直连路由器任意 LAN 口自动分配 IP，无需电脑手动设置静态 IP。
   - 访问地址：`http://192.168.1.1` 或 `http://failsafe.lan`。

4. **独立 Factory 分区保护**：
   - 分区表单独保留 2MB `factory` 分区（`0x180000 - 0x380000`），确保原厂 Wi-Fi 校准参数与 MAC 地址绝对安全。

---

## 硬件规格

| 参数项 | 规格说明 |
| :--- | :--- |
| **SoC** | MediaTek MT7981B (Filogic 820 双核 Cortex-A53 @ 1.3GHz) |
| **内存** | DDR4 512MB |
| **闪存** | 128MB SPI-NAND (兼容 GigaDevice GD5F1GM7 等) |
| **网络芯片** | MT7531AE (2.5G SGMII / GMII) |

---

## 🛠️ 刷入指南

### 1. 进入现有 U-Boot Web Failsafe
1. 拔掉路由器电源。
2. 按住机身背面的 **Reset 键** 不放，插上电源。
3. 保持按住约 5~8 秒，待指示灯闪烁后松开。
4. 电脑网线连接 LAN 口，浏览器打开 `http://192.168.1.1/`。

### 2. 更新 Bootloader
前往 [Releases 页面](https://github.com/RSxiaoyu/bl-mt798x-xr30/releases) 下载最新产物：
1. 在 Web 页面进入 **升级 ATF BL2**（Upgrade BL2），上传 `bl2-mt7981-cmcc_xr30-ubootmod.bin` 并刷入。
2. 在 Web 页面进入 **升级 U-Boot**（Upgrade FIP），上传 `fip-mt7981-cmcc_xr30-ubootmod.bin` 并刷入。
3. 重启设备后即刻拥有纯粹的 ubootmod 现代引导环境！

---

## 📄 许可证与致谢

- 遵循 GPL-2.0 协议。
- 源码上游：[Yuzhii0718/bl-mt798x-dhcpd](https://github.com/Yuzhii0718/bl-mt798x-dhcpd)
- 特别鸣谢：[hanwckf](https://github.com/hanwckf/bl-mt798x) 与 MediaTek Filogic 开源社区。
