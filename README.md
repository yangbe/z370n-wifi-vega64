# z370n-wifi-vega64

![125710pabvb4hvpv4807ey](https://user-images.githubusercontent.com/9880101/70448149-e8d9c500-1ada-11ea-8ea5-7a9769028a36.jpg)

# 系统版本
10.15.2 (19C57)


# Bios设置
Bios已经升到F12，除了要解锁CFG LOck，其它与之前用的F3 ~F10基本一样，还是不建议升到F12。下面的Bios设置，除了USB的要注意，其它的并没有多大影响。

关于Bios升级到F12，请阅读[解锁CFG Lock](https://www.tonymacx86.com/threads/success-b1s-mac-mini-killer-with-macos-mojave-i7-8700-gigabyte-z370n-rx560-16gb-ram.260337/post-1934546)。或者[阅读有关CFG Lock更详细的解锁方法](http://www.misonsky.cn/115.html)。



>1. Save & Exit → Load Optimized Defaults
>
>2. ~~M.I.T. → Advanced Memory Settings Extreme Memory Profile(X.M.P.) : Profile1--~~ <br />
>```此项用来开启内存XMP超频，非必要，而且会导致睡眠后USB的U盘会自动退出，关闭即可，在**Config → Boot → XMPDetection=Yes**同样可以开启内存超频```
>
>3. BIOS → Fast Boot : Disabled
>
>4. BIOS → LAN PXE Boot Option ROM : Disabled
>
>5. BIOS → Storage Boot Option Control : UEFI
>
>6. Peripherals → Trusted Computing → Security Device Support : Disable
>
>7. Peripherals → Network Stack Configuration → Network Stack : Disabled
>
>8. Peripherals → USB Configuration → Legacy USB Support : Auto **(必须开启)**
>
>9. Peripherals → USB Configuration → XHCI Hand-off : Enabled **(必须开启，不然开不了机)**
>
>10. Chipset → Vt-d : Disabled
>
>11. Chipset → Wake on LAN Enable : Disabled
>
>12. Chipset → IOAPIC 24-119 Entries : Enabled

# 使用核显
先在Bios里按如下设置

>1.Peripherals → Initial Display Output : IGFX
>
>2.Chipset → Integrated Graphics : Enabled
>
>3.Chipset → DVMT Pre-Allocated :128M

解决UHD630核显双屏显示问题
在**config → Devices → Properties** 已添加了相关的补丁
参考：https://www.tonymacx86.com/threads/guide-general-framebuffer-patching-guide-hdmi-black-screen-problem.269149/

# 使用Vega64
先在Bios按如下设置
>1.Peripherals → Initial Display Output : PCIE
>
>2.Chipset → Integrated Graphics : Disabled(Disabled后只能使用独显。不建议同时开启核显，避免产生其它问题)

我用的是XFX Vega64（公版），风冷的Vega64基本上都有风扇暴走和温度的问题，降压调频调Powertable会解决问题，但是LED灯的问题仍然没有解决

## 4K屏幕下开机苹果Logo扁平问题
请阅读[更新显卡GOP](https://www.amder.club/thread-962-1-1.html)，或者联系我提供已修改的GOP文件

## Vega64参数调整

> 在10.14.5之后，Vega64的显卡可以正常使用，即使不调整参数，但是我在使用时发现温度会比较，以Luxmark为例，默认情况下最高温去到82度，但调整参数后（10.14.5之前的版本调整参数主要是为了解决风扇暴走的问题），温度保持在71度左右，实际性能差不多。

**使用[VGTab](https://www.tonymacx86.com/threads/tool-vgtab-control-your-vega-in-macos-without-flashing-the-vbios.268965/)调整Vega64的问题**

对于A卡来说，降压调频往往可以提升性能，参数调整可参考如下：

- **Core Frequency**
[核心频率](https://user-images.githubusercontent.com/9880101/56672127-6d143f00-66e8-11e9-88dd-3ebc1072ff52.png)

- **Core Voltage**
[核心电压](https://user-images.githubusercontent.com/9880101/56672286-c5e3d780-66e8-11e9-930c-901f6c360562.png)

- **Memory Frequency**
[显存频率](https://user-images.githubusercontent.com/9880101/56672604-4c001e00-66e9-11e9-9a1f-c7d2d424a93a.png)

- **Memory Voltage**
[显存电压](https://user-images.githubusercontent.com/9880101/56673243-7c948780-66ea-11e9-9d51-1efba55baaae.png)

- **Fan风扇**
[风扇转速和温度](https://user-images.githubusercontent.com/9880101/56673392-c7160400-66ea-11e9-839e-f4863dbc7fb5.png)

## 加载Vega64的补丁

> ~ **方式一：使用VGTab可以自动生成Kext，放在/CLOVER/Kexts/Other/下** ~
~（问题：即使在Force to Patch强制加载，有时会加载不了，但通常重启一下就可以解决）~
>
>~ /CLOVER/kexts/Other/**Vega64.kext**, 是我自己调试过的kext补丁，直接放在这个文件夹即可 ~

>**方式二：在config → Devices → Properties** 
具体请看文件设置的参数（现在用的是这种方法）
>添加独显的识别地址，手动添加相关参数.
>也可以使用[**VGTabMerge**](https://github.com/corpnewt/VGTabMerge)自动合并到config中。
>
> 详细说明如何使用[**VGTab和VGTabMerge**](https://www.tonymacx86.com/threads/guide-injection-of-amd-vega-power-and-fan-control-properties.267519/)

方式二是我目前使用的方法，没什么问题，具体性能跟方法A一毛一样

## Vega64 跑分如图：
[Luxmark](https://user-images.githubusercontent.com/9880101/56673621-2c69f500-66eb-11e9-8387-d234d73bec1d.png)
[OpenCL](https://user-images.githubusercontent.com/9880101/56673816-91254f80-66eb-11e9-8613-a1f18767d557.png)


# USB

使用Hackintool定制需要工作的USB口，生成USBpower.kext、 SSDT-EC.aml 、 SSDT-UIAC.aml

- SSDT-EC.aml 、 SSDT-UIAC.aml要放在Clover/ACPI/patched 
- USBpower.kext放在Clover/Kexts/Other


SSDT的定制参考以下的图表，不要超过15个端口即可

|端口编号|Buffer编号|主板位置|USB协议|
|:------|:----|:----|:----:|
|HS01|0x01|usb3.0插针|USB2|
|HS02|0x02|usb3.0插针|USB2|
|HS07|0x07|P1|USB2|
|HS08|0x08|P2|USB2|
|HS05|0x05|P3|USB2|
|HS06|0x06|P4|USB2|
|HS03|0x03|P5|USB2|
|HS04|0x04|P6|USB2|
|HS09|0x09|TypeC-SW|--|
|HS10|0x0A|蓝牙|USB2|
|HS11|0x0B|usb2.0插针|USB2|
|HS12|0x0C|usb2.0插针|USB2|
|SS01|0x11|usb3.0插针|USB3|
|SS02|0x12|usb3.0插针|USB3|
|SS07|0x17|P1|USB3|
|SS08|0x18|P2|USB3|
|SS05|0x15|P3|USB3|
|SS06|0x16|P4|USB3|
|SS03|0x13|P5|USB3|
|SS04|0x14|P6|USB3|
