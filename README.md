# TXCZ-RK3128-V3.1 机顶盒 Batocera 移植

> ⚠️ **重要提醒**
>
> 只适用 TXCZ-RK3128-V3.1 板卡的，并且板载存储为 NAND FLASH 类型的，其他的机型直接刷可能变砖！！！
>
> 刷机改造存在变砖风险。
>
> 请务必先备份原厂系统，仅建议具备一定动手能力的用户进行操作。
>
> 如果无法接受设备损坏的风险，请不要继续。

* 完成了：oepnwrt 系统移植
* 完成了：batocera 复古游戏机系统移植
* 后续适配其他系统也会汇总到这个文档里。

## 原始笔记：
  *《openwrt笔记.txt》是移植openwrt的时候记录的笔记。
  *《笔记.txt》，是从开始折腾到 batocera游戏机系统移植时记录的一些常用信息。

## 刷机工具：
    https://rkdevtool.com/
  我用的 RKDevTool_Release_v2.96 + DriverAssitant_v5.0

## 原厂设备树：
这个工程里的 **bingzun.dts**，这是从我的rk3128的老机顶盒上提取反编译的设备树。


## openwrt 系统：
### ⚠️一定要备份好之前的系统:
**如果刷机的话会覆盖原厂的 miniloader，原厂系统跟新的 miniloader 可能版本不兼容，这样原厂系统就无法启动了，而且原厂系统里的miniloader是读取备份不了的，如果想恢复只能找很多版本的 miniloader 挨个试，所以刷机前请三思。**

### 工程源码：
    
    https://github.com/mingrizaizao/openwrt
    https://gitee.com/mingrizaizao/openwrt


### 编译指令：

```
安装环境：
sudo apt update

sudo apt install -y \
  build-essential clang flex bison g++ gawk gcc-multilib \
  g++-multilib gettext git libncurses-dev libssl-dev \
  python3 python3-setuptools python3-dev rsync swig unzip \
  zlib1g-dev file wget curl time \
  libelf-dev libdw-dev libcap-dev \
  cpio ecj fastjar java-propose-classpath \
  libgmp3-dev libmpc-dev libmpfr-dev \
  libxml-parser-perl libyaml-dev xsltproc \
  qemu-utils mkisofs


下载源码：
cd ~

git clone \
  --branch rk3128-dev \
  --single-branch \
  git@github.com:mingrizaizao/openwrt.git \
  openwrt

cd ~/openwrt



安装依赖：
cd ~/openwrt

./scripts/feeds update -a
./scripts/feeds install -a


#配置：
cd ~/openwrt
cp txcz-rk3128-v3-1.config .config
make defconfig



#下载：#编译：
# make download -j8
make -j$(nproc) V=s


#查看镜像：
find bin/targets/rockchip/armv7/ -maxdepth 1 -type f -printf '%f\n'




烧录内存卡：
lsblk -o NAME,SIZE,MODEL,TRAN,FSTYPE,MOUNTPOINTS


cd ~/openwrt

ls -l bin/targets/rockchip/armv7/openwrt-rockchip-armv7-txcz_rk3128-v3-1-squashfs-sysupgrade.img.gz

FILE=bin/targets/rockchip/armv7/openwrt-rockchip-armv7-txcz_rk3128-v3-1-squashfs-sysupgrade.img.gz

sudo umount /dev/sda1 2>/dev/null || true
sudo umount /dev/sda2 2>/dev/null || true

gzip -dc "$FILE" | sudo dd of=/dev/sda bs=4M status=progress conv=fsync

sync

```
### 编译好的：
https://github.com/mingrizaizao/openwrt/releases/tag/v1.0.0
https://gitee.com/mingrizaizao/openwrt/releases/tag/v1.0.0

这里面两个文件
    
- **rk3128_loader_v2.12.263.bin**

  用于刷写到板载 NAND Flash 的 **Loader** 分区（地址 `0x00000000`）。

  我使用的是 **RKDevTool v2.96**。

- ** openwrt-rockchip-armv7-txcz_rk3128-v3-1-squashfs-sysupgrade.img.gz**

  openwrt 系统镜像。

  解压后写入 **TF 卡 / MicroSD 卡** 即可启动。


## batocera 复古游戏机系统：

### ⚠️一定要备份好之前的系统:
**如果刷机的话会覆盖原厂的 miniloader，原厂系统跟新的 miniloader 可能版本不兼容，这样原厂系统就无法启动了，而且原厂系统里的miniloader是读取备份不了的，如果想恢复只能找很多版本的 miniloader 挨个试，所以刷机前请三思。**


### 工程源码：
    
    https://github.com/mingrizaizao/batocera.linux
    https://gitee.com/mingrizaizao/batocera.linux

* Ubuntu 24.04.3 LTS，这是我用的版本
* 大概要占用 100G 左右的硬盘空间
* 戴尔灵越 7591 笔记本编译了大概 20 多个小时


```

编译指令：

git clone \
    --branch batocera41-txcz-rk3128-v3-1 \
    --single-branch \
    --depth 1 \
    git@github.com:mingrizaizao/batocera.linux.git

cd batocera.linux

git submodule update --init --recursive

docker build \
-t batoceralinux/batocera.linux-build:latest \
.

make rk3128-build
```

### 编译好的：

https://github.com/mingrizaizao/batocera.linux/releases/tag/batocera-rk3128-txcz-rk3128-v3-1-41-20260715

这里面两个文件
    
- **rk3128_loader_v2.12.263.bin**

  用于刷写到板载 NAND Flash 的 **Loader** 分区（地址 `0x00000000`）。

  我使用的是 **RKDevTool v2.96**。

- **batocera-rk3128-xxxx.img.gz**

  Batocera 系统镜像。

  解压后写入 **TF 卡 / MicroSD 卡** 即可启动。


## 致谢

* 感谢 chieunhatnang.de 开源的esp8089驱动。
* 感谢 openwrt 社区以及所有测试者。
* 感谢 Batocera 社区以及所有测试者。
* 感谢 瑞芯微 官方开放了很多资料。
* 感谢 @爱吃烩面的大圣 提供的复古游戏机思路。
* 感谢 各种 AI 大模型的支持。
* 感谢 广大网友支持，暂时想到这么多，如有遗忘后续补充~

## ❤️ 支持项目

视频号、抖音、b站、快手、公众号这些都叫：**明日再造**，欢迎关注~
如果本项目对你有所帮助，欢迎自愿打赏支持后续维护。

<table>
<tr>
<td align="center">

微信

<img src="docs/images/wechat.png" width="220">

</td>

<td align="center">

支付宝

<img src="docs/images/alipay.png" width="220">

</td>
</tr>
</table>


再次感谢大家的支持！