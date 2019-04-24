# z370n-wifi-vega64

# Bios设置
我的Bios升到F10，与之前用的F5基本一样，但是不建议升到F11，显卡的设置会发生改变，核显和独显都会有问题。下面的Bios设置，除了USB的要注意下，其它的并没有多大影响。

>1. Save & Exit → Load Optimized Defaults
>
> ~~2. M.I.T. → Advanced Memory Settings Extreme Memory Profile(X.M.P.) : Profile1--~~
> ```此项用来开启内存XMP超频，非必要，而且会导致睡眠后USB的U盘会自动退出，关闭即可，在**Config → Boot → XMPDetection=Yes**同样可以开启内存超频```
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

解决UHD630核显双屏显示问题,(有时间再补充)
参考：https://www.tonymacx86.com/threads/guide-general-framebuffer-patching-guide-hdmi-black-screen-problem.269149/

# 使用Vega64
先在Bios按如下设置
>1.Peripherals → Initial Display Output : PCIE

我用的是XFX Vega64（公版），风冷的Vega64基本上都有风扇暴走和温度的问题，降压调频调Powertable会解决问题，但是LED灯的问题仍然没有解决

**使用VgTab调参**
参数调整可参考如下：

**Core Frequency**
[核心频率](https://user-images.githubusercontent.com/9880101/56672127-6d143f00-66e8-11e9-88dd-3ebc1072ff52.png)

**Core Voltage**
[核心电压](https://user-images.githubusercontent.com/9880101/56672286-c5e3d780-66e8-11e9-930c-901f6c360562.png)

**Memory Frequency**
[显存频率](https://user-images.githubusercontent.com/9880101/56672604-4c001e00-66e9-11e9-9a1f-c7d2d424a93a.png)

**Memory Voltage**
[显存电压](https://user-images.githubusercontent.com/9880101/56673243-7c948780-66ea-11e9-9d51-1efba55baaae.png)

**Fan风扇**
[风扇转速和温度](https://user-images.githubusercontent.com/9880101/56673392-c7160400-66ea-11e9-839e-f4863dbc7fb5.png)

调完参数后，有两种方法加载补丁

> **A. 生成Kext，放在/CLOVER/Kexts/Other/下**（问题：有时会加载不了，即使在Kexts to patch 强制加载，但通常重启一下就可以解决）
>
>/CLOVER/kexts/Other/**Vega64.kext**, 是我自己调试过的kext补丁，直接放在这个文件夹即可

>**B.在config → Devices → Properties** 
>添加独显的识别地址，手动添加相关参数.
>也可以使用**VGTabMerge**自动合并到config中。（有时间再补充）

方法B，目前使用没什么问题，具体性能跟方法A一毛一样

跑分如图：
[Luxmark](https://user-images.githubusercontent.com/9880101/56673621-2c69f500-66eb-11e9-8387-d234d73bec1d.png)
[OpenCL](https://user-images.githubusercontent.com/9880101/56673816-91254f80-66eb-11e9-8613-a1f18767d557.png)

