# CMCC XR30 Modern Bootloader (BL2 + FIP)

[![Build CMCC XR30 Modern Bootloader](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml/badge.svg)](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml)

专为**中国移动 CMCC XR30** 路由器打造的极简、高性能、现代化 Bootloader 自动构建仓库。

源码直通上游官方 [Yuzhii0718/bl-mt798x-dhcpd](https://github.com/Yuzhii0718/bl-mt798x-dhcpd)，零冗余修改，完全依赖上游规范。

---

## 🌟 核心特性与设计优势

1. **彻底解决兆易创新 (GigaDevice) NAND 坏块死锁与擦除报错 `-5`**：
   - 官方 Release 的默认版本强开 MTK-NMBM 坏块映射，在 GD5F1GM7 芯片上会误判坏块导致备用表溢出只读，且将 U-Boot 环境变量直接存放在 Raw MTD 上，导致擦写频繁报错 `-5` (`-EIO`)。
   - 本仓库构建的 **`ubootmod`** 变体彻底废除 NMBM，改用 Linux 原生 `spi-nand0` 直通。
   - 环境变量完全迁移至 UBI 卷 (`ubootenv` / `ubootenv2`)，具备 UBI 磨损均衡与透明坏块屏蔽，永久告别 `saveenv` 失败。

2. **原生 All-in-FIT 单固件支持**：
   - 完美适配主流 OpenWrt / [ImmortalWrt 25.12 All-in-FIT 架构](https://github.com/RSxiaoyu/immortalwrt-xr30)（Linux 6.6 / 6.12，`root=/dev/fit0 rootwait`）。
   - 支持在 Web 控制台直接升级 `sysupgrade.itb`。
   - 支持在 `/initramfs.html` 纯内存免擦写直接引导 `recovery.itb`。

3. **现代化 Failsafe 控制台**：
   - 基于 Bootstrap 的现代化响应式 Web 界面（支持中英文切换）。
   - 内置 **DHCP 服务器**：网线直连路由器任意 LAN 口自动分配 IP，无需电脑手动设置静态 IP。
   - 访问地址：`http://192.168.1.1` 或 `http://failsafe.lan`。
   - 支持一键备份全盘与 Factory 分区。

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

## 📦 固件变体说明

| 变体名称 | 适用场景与固件类型 | 说明 |
| :--- | :--- | :--- |
| **`ubootmod`** *(强烈推荐)* | **ImmortalWrt 24.10/25.12、OpenWrt 官方 ubootmod FIT 固件** | 彻底关闭 NMBM，环境变量写入 UBI 卷，支持 All-in-FIT 单固件，无任何报错 |
| **`nonmbm`** | **传统/第三方无 NMBM 固件** | 关闭 NMBM，但保留传统 raw MTD 分区与环境变量 |

---

## 🛠️ 刷入指南

### 1. 进入现有 U-Boot Web Failsafe
1. 拔掉路由器电源。
2. 按住机身背面的 **Reset 键** 不放，插上电源。
3. 保持按住约 5~8 秒，待指示灯闪烁后松开。
4. 电脑网线连接 LAN 口，浏览器打开 `http://192.168.1.1/`。

### 2. 更新 Bootloader
1. 在 Web 页面进入 **升级 ATF BL2**（Upgrade BL2），上传 `bl2-mt7981_cmcc_xr30_SP2*.bin` 并刷入。
2. 在 Web 页面进入 **升级 U-Boot**（Upgrade FIP），上传 `fip-mt7981_cmcc_xr30_SP2*-fit.bin` 并刷入。
3. 设备重启后即刻拥有全新的 ubootmod 现代引导环境！

---

## 📄 许可证与致谢

- 遵循 GPL-2.0 协议。
- 源码上游：[Yuzhii0718/bl-mt798x-dhcpd](https://github.com/Yuzhii0718/bl-mt798x-dhcpd)
- 特别鸣谢：[hanwckf](https://github.com/hanwckf/bl-mt798x) 与 MediaTek Filogic 开源社区。
