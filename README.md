# Ubootmod Bootloader for CMCC XR30

[![Build](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml/badge.svg)](https://github.com/RSxiaoyu/bl-mt798x-xr30/actions/workflows/build.yml)

中国移动 CMCC XR30 的 U-Boot 引导链（ATF BL2 + U-Boot FIP）。

## 上游

基于 [Yuzhii0718/bl-mt798x-dhcpd](https://github.com/Yuzhii0718/bl-mt798x-dhcpd)（`master` 分支，内置 DHCP 服务）。本仓库不修改源码，在 CI 中直接通过环境变量调用上游构建脚本：

```sh
SOC=mt7981 BOARD=cmcc_xr30 VERSION=SP2 VARIANT=ubootmod ./build.sh
```

Release tag 按上游 commit 编址（`ubootmod-<short_sha>`），同一 commit 不重复发版。

## ubootmod 特性

- 移除 NMBM 坏块映射层，改为原生 MTD/UBI 直通
- 环境变量保存在 UBI 卷 (`ubootenv` / `ubootenv2`)，具备磨损均衡
- 内置 Web 恢复台与 DHCP 服务，浏览器访问 `http://192.168.1.1` 即可上传固件 (`sysupgrade.itb` / `recovery.itb`)
- 不写入 `factory` 分区 (`0x180000-0x380000`)，保留原厂 Wi-Fi 射频校准数据与 MAC 地址

## 产物与刷写

| 产物文件 | 用途 |
|---|---|
| `bl2-mt7981-cmcc_xr30-ubootmod.bin` | ATF BL2 引导 |
| `fip-mt7981-cmcc_xr30-ubootmod.bin` | U-Boot FIP 引导 |
| `sha256sums` | 校验文件 |

在现有 U-Boot Web 界面 (`192.168.1.1`) 中依次上传 BL2 与 FIP，写入后重启。

配套固件可使用 [immortalwrt-xr30](https://github.com/RSxiaoyu/immortalwrt-xr30)（FIT 单镜像）。
