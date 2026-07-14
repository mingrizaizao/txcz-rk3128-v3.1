# TXCZ-RK3128-V3.1 机顶盒 Batocera 移植

> ⚠️ **重要提醒**
>
> 刷机改造存在变砖风险。
>
> 请务必先备份原厂系统，仅建议具备一定动手能力的用户进行操作。
>
> 如果无法接受设备损坏的风险，请不要继续。

* 目前适配完成了：batocera 复古游戏机系统
* 后续适配其他系统也会汇总到这个文档里。

## 原始笔记：
  这个工程里的 **《笔记.txt》**，这是我从开始折腾到现在记录的一些常用信息，原始版本直接发上来了，后边随时整理随时更新~

## 刷机工具：
    https://rkdevtool.com/
  我用的 RKDevTool_Release_v2.96 + DriverAssitant_v5.0

## 原厂设备树：
这个工程里的 **bingzun.dts**，这是从我的rk3128的老机顶盒上提取反编译的设备树。

## batocera 复古游戏机系统：

### ⚠️一定要备份好之前的系统:
**如果刷机的话会覆盖原厂的 miniloader，原厂系统跟新的 miniloader 可能版本不兼容，这样原厂系统就无法启动了，而且原厂系统里的miniloader是读取备份不了的，如果想恢复只能找很多版本的 miniloader 挨个试，所以刷机前请三思。**


### 工程源码：
    
https://github.com/mingrizaizao/batocera.linux

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

https://github.com/mingrizaizao/batocera.linux/releases/tag/batocera-rk3128-txcz-rk3128-v3-1-41-20260712

这里面两个文件
    
- **rk3128_loader_v2.12.263.bin**

  用于刷写到板载 NAND Flash 的 **Loader** 分区（地址 `0x00000000`）。

  我使用的是 **RKDevTool v2.96**。

- **batocera-rk3128-xxxx.img.gz**

  Batocera 系统镜像。

  解压后写入 **TF 卡 / MicroSD 卡** 即可启动。


## 致谢

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