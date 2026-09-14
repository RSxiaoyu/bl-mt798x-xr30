# Ubootmod Bootloader for CMCC XR30

[![Build](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml/badge.svg)](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml)

**中国移动 CMCC XR30** 专用 U-Boot 引导链（ATF BL2 + U-Boot FIP）。

## 上游

基于 [Yuzhii0718/bl-mt798x-dhcpd](https://github.com/Yuzhii0718/bl-mt798x-dhcpd) (`master` 分支，内置 DHCP 的 bl-mt798x 改进版)。本仓库**零补丁、零源码 fork**，CI 以原生环境变量直调上游构建：

```sh
SOC=mt7981 BOARD=cmcc_xr30 VERSION=SP2 VARIANT=ubootmod ./build.sh
```

Release tag 按上游 commit 编址（`ubootmod-<short_sha>`），同一 commit 不重复发版。

## ubootmod 特性

- **废除 NMBM**：移除老式坏块映射层，原生 MTD/UBI 直通
- **磨损均衡**：环境变量保存在 UBI 卷 (`ubootenv` / `ubootenv2`)
- **Web 恢复台**：内置 DHCP 服务，浏览器直接访问 `http://192.168.1.1` 上传固件 (`sysupgrade.itb` / `recovery.itb`)
- **保护校准**：完全不触碰 `factory` 分区 (`0x180000-0x380000`)，完整保留原厂 Wi-Fi 射频校准与 MAC

## 产物与刷写

| 产物文件 | 用途 |
|---|---|
| `bl2-mt7981-cmcc_xr30-ubootmod.bin` | ATF BL2 引导 |
| `fip-mt7981-cmcc_xr30-ubootmod.bin` | U-Boot FIP 引导 |
| `sha256sums` | 校验文件 |

**刷写方法**：在现有 U-Boot Web 界面 (`192.168.1.1`) 中依次上传 BL2 与 FIP，重启即可。

**推荐配套固件**：[immortalwrt-xr30](https://github.com/RSxiaoyu/immortalwrt-xr30) (All-in-FIT 单镜像)。
