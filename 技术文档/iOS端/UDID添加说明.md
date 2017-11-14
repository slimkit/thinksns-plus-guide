## 什么是 UDID？

UDID 是由子母和数字组成的 40 个字符串的序号，用来区别每一个唯一的 iOS 设备，包括 iPhones, iPads, 以及 iPod Touches，这些编码看起来是随机的，实际上是跟硬件设备特点相联系的。

例如，一个典型的 UDID 类似这样：

`37f2f993bae681636e30e74b04d6b8955ba36f29`


## UDID 和 内测发布

如果 iOS 设备要安装以 内测发布(`Ad Hoc`) 方式打包的 iOS 应用时，必须将该设备的 UDID 加入打包应用时的证书文件（.mobileprovision文件），才可以在该设备上正常安装。

## 如何获取 iOS 设备 UDID？

设备的 UDID 可以在电脑上下载并安装 iTunes 或者 Xcode 软件来获得，不过会比较麻烦。


或者，你也可以在 iOS 设备上打开下面的网址，即可方便的获取到当前设备的 UDID。

`https://www.pgyer.com/udid`