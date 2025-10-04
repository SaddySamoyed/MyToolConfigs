# Win-Ubuntu 双系统安装, 从 0 开始, with a single disk

其实并非简单的一件事. 要考虑的事情还蛮多的

尤其针对我们..只有一个 M2 硬盘slot 的电脑

最典型的一件就是: 即便是刚刚买的机器, OS(C) 也是不可 shrink 的. (我900多个G 只能 shrink 出 10几个 G)

因为对于 win 系统而言, 如果只有一个硬盘, 那么它会被完全变成系统盘, 有一堆不可移动文件, 布满整个盘.

就是非常离谱



所以不是要重置电脑, 而是要直接重新安装系统, 在一开始就空余出一片未分配区域.

## 重新安装 win 11, 定制

记得备份一下本地文件, if you like

### 制作启动盘

1. **下载 win 11 iso **

   - 用微软官方工具制作安装盘

   - 下载地址: https://www.microsoft.com/en-us/software-download/windows11

     Download Windows 11 Disk Image (ISO) for x64 devices

2. **下载 rufus**

   - 下载 [Rufus](https://rufus.ie/). 这是个专门用来制作 os 启动硬盘的工具

   - 插入一个 ≥ 8GB 的 U 盘 (会被格式化, 不要选择有内容硬盘.) 

   - 打开 rufus, 选择：

     - **Device**: 你的 U 盘
     - **Boot selection**: 选刚刚下载的 Windows 11 ISO
     - **Partition scheme**: GPT
     - **Target system**: UEFI (non CSM)

     然后点击 **Start**, 等它写完.



### 重启进入 bios 下载

开机按 `F2`/`Del`/`Esc`/`F12` 进入 bios 后:

- 进入 boot menu, 选择 UEFI: USB DISK 3.0…. (刚才安装好的盘)
- 进入 Windows 安装界面
- **选择语言和键盘布局 → 下一步 → 安装**
- **选择安装类型 → 自定义安装 (Custom: Install Windows only)**
  - 不要选“升级 (Upgrade)”，那会保留旧系统

- 好的, 下面是最关键的部分: select location to install win11. 在这里我们将重新分配 disk.

  我们会看到一堆 partitions. 其中 disk0 是我们本来的系统, disk 1 是 U 盘.

  然后.. 

  - 我们把 disk 0 的所有 partitions 全部删掉, 它会全部**并进一个大的 unallocated space**

    ```
    Disk 0 Unallocated Space 			953 GB	...
    ```

  - 我们 create partition, 把它一分为二, 我决定分 512 GB 给 win, 剩下来给 Ubuntu

    create 完之后他会自动生成四个 partitions

    ```
    Disk 0 Partition 1	...		System
    Disk 0 Partition 2	...		MSR (Reserved)
    Disk 0 Partition 3 	...		Primary	#给 win 当系统盘
    Disk 0 Partition 4	...		Unallocated Space #以后留给
    ```

    其中, Partition 1 (EFI) 和 Partition 2 (MSR) 是 Windows 自动创建的
     👉 EFI 里放 Windows Boot Manager, 引导 Windows
     👉 MSR 是 Windows 内部用的隐藏分区

  - 在安装界面里，**选中 Partition 3 (Primary 512 GB)** → 点击 **Next**
     👉 Windows 就会安装在这个 512GB 的分区上

    至于 Partition 4 (Unallocated Space ≈ 454 GB) → 不用管, 留着空着
     👉 之后启动 Ubuntu 安装器时，选择 **Something else (手动分区)**, 在这里新建 Ubuntu 的分区

    - EFI (Ubuntu 专用, 300–500 MB, FAT32, `/boot/efi`)
    - 根 `/` (50 GB, ext4)
    - 家目录 `/home` (剩余空间 ext4)
    - (可选) swap (8–16 GB)

  - 来到 ready to install 界面, 直接点击 install

  - 然后就直接安装好系统, 进入初始化页面了. 和正常的新机开机没啥区别. 唯一的区别是有 0 个驱动. 我们先跳过, 之后再安装



### 完成 win11 安装 (最后安装一下各个驱动)

打开新电脑, 总之就是很干净的一个界面.

然后就可以打开 (1)win_new_computer_init 这个文件来 rewind 一下怎么搞了. 不过在这之前, 我们得安装一些驱动. 最重要的是 wifi 驱动 (还有显卡驱动什么的). 

为了安装这个. 我们还得随便找个另一个机器来下载. 可以复用之前的装win11系统的 u 盘, 去另一个机器上下载一下各种驱动.

如何找到驱动: **Shift + F10** → cmd, 输入：

```
wmic csproduct get name
```

它会直接打印出你电脑的型号. 我是 ROG Zephyrus G14 GA403UM

然后就可以找到电脑厂商官网下载一下各种驱动. 

我的是: https://rog.asus.com/us/laptops/rog-zephyrus/rog-zephyrus-g14-2025/helpdesk_download/



装上之后就可以联网了. 然后我们就可以在本地安装其他的驱动.

- 官方支持的显卡驱动
- 声卡驱动
- 精准 touchpad 驱动
- 各种等等, 有就装, 不坏事

>  ps: 如果你也是 rog 的话, 作为 rog 老学长我强烈建议不要安装 MyAsus, 推荐广告有点离谱
>
> 但是 armoury crate 还是有很多用处的, 可以装一下 (这个不是驱动是普通app): https://www.asus.com/supportonly/armoury%20crate/helpdesk_download/



