# Ubootmod Bootloader for CMCC XR30

[![Build](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml/badge.svg)](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml)

**中国移动 CMCC XR30** 专用 U-Boot 引导链（ATF BL2 + U-Boot FIP）。

## 上游

上游为 [Yuzhii0718/bl-mt798x-dhcpd](https://github.com/Yuzhii0718/bl-mt798x-dhcpd)（`master` 分支，内置 DHCP 的 bl-mt798x 分支）。本仓库**零补丁、零 fork**，CI 以环境变量直调上游 `build.sh`：

```sh
SOC=mt7981 BOARD=cmcc_xr30 VERSION=SP2 VARIANT=ubootmod ./build.sh
```

Release tag 按上游 commit 编址：同一上游 commit 的重复构建原地更新既有 Release。

## ubootmod 特性（上游自带）

- 移除 MTK-NMBM 坏块映射，原生 MTD/UBI 直通
- 环境变量持久化于 UBI 卷（`ubootenv` / `ubootenv2`），磨损均衡
- Web 恢复控制台：LAN 口 DHCP + 浏览器打开 `http://192.168.1.1`，直接刷写 FIT 固件（`sysupgrade.itb` / `recovery.itb`）
- `factory` 分区（`0x180000-0x380000`）不触碰，保留原厂 Wi-Fi 校准与 MAC

## 产物

| 文件 | 用途 |
|---|---|
| `bl2-mt7981-cmcc_xr30-ubootmod.bin` | 升级 ATF BL2 |
| `fip-mt7981-cmcc_xr30-ubootmod.bin` | 升级 U-Boot FIP |
| `sha256sums` | 完整性校验 |

## 刷写

在现有 U-Boot Web 界面（`192.168.1.1`）中依次上传 BL2、FIP，重启即完成。

适配固件：[immortalwrt-xr30](https://github.com/RSxiaoyu/immortalwrt-xr30)（All-in-FIT 单镜像）。
