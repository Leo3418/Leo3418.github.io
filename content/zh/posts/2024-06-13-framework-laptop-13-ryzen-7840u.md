---
title: "Framework Laptop 13 AMD 锐龙 7 7840U 版本"
categories:
  - 博客
toc: true
header:
  overlay_image: /img/posts/2024-06-13-framework-laptop-13-ryzen-7840u/unbox.jpg
  og_image: /img/posts/2024-06-13-framework-laptop-13-ryzen-7840u/unbox.jpg
  overlay_filter: 0.5
  show_overlay_excerpt: false
---

[Framework 笔记本]虽然目前还未在中国大陆境内销售，但依然有不少人通过新闻或者视频了解过。提到 Framework，很多人的第一印象也许是“模块化”[^modularity-impression]，不过 Framework 自己标榜的产品设计理念主要是可维修、可客制化、以及可升级。一开始，Framework 笔记本用的全是英特尔酷睿处理器；在等待了两年后，Framework 终于推出了我心念已久的 AMD 锐龙版本，我遂购买了一台搭载锐龙 7 7840U 的 Framework Laptop 13，在 2023 年 11 月收到货后一直使用至今，直到现在终于有时间可以记录一下我的使用体验了。

[Framework 笔记本]: https://frame.work/

[^modularity-impression]: <https://www.bilibili.com/video/BV1r1421i7DT/>

## 背景

我首次了解到 Framework 就是在他们一开始推出使用 11 代酷睿处理器的型号的时候。当时我就说：“如果这笔记本用的是 AMD，我直接就买了。” Framework 最吸引我的地方在于，他们为了实现产品的可维修性，在官方[商城]上销售原装备件。我以前维修其它笔记本时，有过需要备件但买不到的经历，因此很欣赏 Framework 这一点。不过，我对 Framework 最初只使用英特尔处理器的选择感到比较遗憾，因为当时市面上已经有了更好的处理器可供笔记本厂商选用：自打锐龙 4000 系列推出开始，AMD 的轻薄本 CPU 在同功耗下的性能表现往往强于英特尔的对应型号[^amd-intel-cmp-1] [^amd-intel-cmp-2]，因此轻薄本使用 AMD 处理器一般就意味着其拥有比使用英特尔的型号更强的性能。

终于，到 2023 年，Framework 发布了搭载 AMD 锐龙 7040 系列处理器的型号，之前阻止我购买 Framework 的唯一理由也就消失了。因为个人原因，我无法在新型号发布之时立刻下单；等到终于可以下单时，我的订单已经排到了第 5 批（Framework 的新产品是按照购买时间分批发货的），故直到 11 月，前面几批订单都发货后，我才收到我的机器。Framework 同一时间还发布了全新的 16 寸型号（Framework Laptop 16），也使用 AMD 锐龙 7040 系列处理器，不过由于我更倾向于便携性，就还是选择了最原始的 13.5 寸型号（Framework Laptop 13）。

[商城]: https://frame.work/marketplace/parts

[^amd-intel-cmp-1]: <http://web.archive.org/web/20220824120147/https://zhuanlan.zhihu.com/p/124967122>
[^amd-intel-cmp-2]: <https://www.notebookcheck-cn.com/AMD-Ryzen-7-6800U-Zen3-Alder-Lake.623817.0.html>

## 配置

如开头所述，允许自定义配置是 Framework 笔记本的一大特点：顾客可以选择购买准系统——即在下单时，选择 DIY 版本（DIY Edition），然后不要内存和硬盘；也可以像买普通笔记本一样，选择提前组装好、预装 Windows、开箱即用的版本。在我下单时，因为 Framework 官方商城上的内存和固态硬盘比别的地方贵，所以我选择了这种准系统配置，然后在其它平台以更低价格购买了剩下的组件。

![在 Framework 官方商城选择准系统配置]({{< static-path img marketplace-barebone-system.png l10n >}})

在处理器方面，Framework 提供了两种锐龙 7040 系列型号的选择：

| 处理器 | CPU 核心 / 线程数 | CPU 最高频率 | 集显 | 集显核心数（CU） | 集显最高频率 |
| :-: | :-: | :-: | :-: | :-: | :-: |
| 锐龙 5 7640U | 6 / 12 | 4.9 GHz | Radeon 760M |  8 | 2.6 GHz |
| 锐龙 7 7840U | 8 / 16 | 5.1 GHz | Radeon 780M | 12 | 2.7 GHz |

我选择了加钱上 7840U，主要是为了我个人最主要的两种使用场景：编译 Gentoo Linux 软件包和轻度游戏。编译软件时，CPU 线程数越多，一般编译就越快：对于大软件包而言，16 线程相比于 12 线程能节省至多 25% 编译时间。7840U 附带的 Radeon 780M 集显的规格也明显比 7640U 的 760M 更高，能换取更高的游戏帧数。

内存方面，我根据我的主要使用场景，买了两条 16 GB DDR5-5600 CL46 内存组成 32 GB 双通道。对于大概 99% 的软件包而言，16 线程并发编译用 32 GB 内存是绰绰有余；但还是有大约 1% 的软件包的编译流程会吃很多内存，比如 `net-libs/webkit-gtk`，就必须按照 Gentoo 的建议给每个线程预留 2 GiB 内存[^portage-makeopts]。为保险起见，我就选择了 32 GB 内存。（还有一种解决方法不需要加内存，那就是在 [`/etc/portage/package.env`][portage-package.env] 中减少编译这些大包时使用的线程数量。）双通道则是为了更好的集显性能，毕竟集显没有自己的显存，因此需要借用系统内存，故双通道带来的内存性能提升也可以让集显获益。

[portage-package.env]: https://wiki.gentoo.org/wiki//etc/portage/package.env#Use_different_MAKEOPTS_for_a_specific_package

[^portage-makeopts]: <https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage/zh-cn#MAKEOPTS>

## 操作系统

我现在用电脑，几乎所有任务都能在 Gentoo Linux 下完成，只有玩游戏会切换到以双系统方式安装的 Windows，因为感觉在 GNU/Linux 下折腾 Windows 兼容层更麻烦。由于几乎所有笔记本厂商都和微软有合作关系，所以大多笔记本在 Windows 下能平稳运行，但在 GNU/Linux 下就容易出现问题，小到各种小毛病，大到完全不能正常使用；至于 Framework 笔记本，在操作系统兼容性方面有着和其它品牌笔记本截然不同的光景。

### GNU/Linux

在我见识过的笔记本厂家中，Framework 在 GNU/Linux 支持方面是最认真的。在选择笔记本零部件时，他们会注重部件在 Linux 内核中是否有驱动这一因素[^frmw-linux]，以保证所有硬件功能在基于 Linux 的系统下都能正常使用。

要说哪个笔记本部件最常出现在 GNU/Linux 下不能用的情况，那恐怕是指纹模块；然而，Framework 笔记本的指纹模块在 GNU/Linux 下照样能使用。我之前用过的两台笔记本——分别是惠普 Envy x360 13-ay0000 和戴尔 XPS 15 9570——都是有指纹模块，但在 GNU/Linux 下完全无法使用。尤其是 Envy x360，情况简直离谱，说是指纹模块驱动程序曾被加进 Linux 内核过，但后来居然在指纹模块制造商的要求下被删除了[^hp-13-ay0000-fp]。我这台 Framework Laptop 13 还是第一次让我能在 GNU/Linux 下体验指纹解锁。

![GNU/Linux 上运行的 GNOME 显示的指纹识别选项]({{< static-path img gnu-linux-fingerprint.png >}})

[^frmw-linux]: <https://frame.work/linux>
[^hp-13-ay0000-fp]: 在该页面上搜索“ay0xxxx”：<https://gitlab.freedesktop.org/libfprint/wiki/-/wikis/Unsupported-Devices>

### Windows

在 Windows 支持方面，Framework 的做法倒是和其它笔记本厂商有点像了，应该也更符合微软的意思：对于最新发布的型号，他们只支持 Windows 11，未官方支持 Windows 10。尽管 AMD 官方在 7840U 上还是支持 Windows 10 的[^amd-7840u-specs]，但 Framework 没有给搭载 AMD 锐龙 7040 系列处理器的笔记本提供 Windows 10 下的驱动程序。

虽然 Framework 不给 Windows 10 驱动，但在 Windows 10 下装驱动的问题仍然能几乎完美地解决。[官方驱动整合包]可以用 [7-Zip] 解压，其中许多驱动都同时兼容 Windows 10 和 Windows 11，包括集显、蓝牙、指纹模块、声卡和无线网卡的驱动。整合包里的芯片组驱动不兼容 Windows 10，但是兼容 Windows 10 的芯片组驱动可以直接从 [AMD 官网]下载。

装好上述驱动之后，设备管理器里就应该只剩一个没有驱动的硬件了；如下图所示，该硬件的硬件 ID 为 `MSFT0200`。这个硬件应该是[微软 Pluton 安全芯片][微软 Pluton 安全芯片][^frmw-msft0200]，负责提供 TPM 功能。我找了一圈，并没有找到兼容 Windows 10 的 Pluton 驱动，所以这台笔记本的 TPM 在 Windows 10 下也没法使用了。对于个人用户而言，没有 TPM，受影响最大的 Windows 功能是 BitLocker 驱动器加密[^windows-tpm]。但对我来说 BitLocker 加密没什么用，毕竟我用 Windows 基本只打游戏，在 Windows 分区上不存储敏感数据，所以为了用 Windows 10 而牺牲 TPM 也无所谓。

![Windows 10 中仅剩的无驱动的硬件]({{< static-path img devmgmt.png >}})

我见过有些人在这种情况下会建议升级到 Windows 11，原因是升级确实能解决没有 Pluton 驱动的问题，而且 Windows 10 的支持在 2025 年 10 月 14 日过后就结束了。Windows 10 停止支持并非升级到 Windows 11 的有力理由，因为这次个人消费者也可以购买 Windows 10 的扩展安全更新（ESU）[^windows-esu]，与以往只有批量许可用户能购买的情况不同，所以任何肯付钱的人都可以在停止支持后继续享受 Windows 10 的安全更新。能使用 Windows 10 LTSC 的用户甚至能一直享受支持到 2032 年。

[官方驱动整合包]: https://knowledgebase.frame.work/framework-laptop-13-bios-and-driver-releases-amd-ryzen-7040-series-r1rXGVL16
[7-Zip]: https://www.7-zip.org/
[AMD 官网]: https://www.amd.com/zh-cn/support/downloads/drivers.html/chipsets/laptop-chipsets/amd-ryzen-and-athlon-mobile-chipset.html
[微软 Pluton 安全芯片]: https://learn.microsoft.com/zh-cn/windows/security/hardware-security/pluton/pluton-as-tpm

[^amd-7840u-specs]: <https://www.amd.com/zh-cn/products/processors/laptop/ryzen/7000-series/amd-ryzen-7-7840u.html>
[^frmw-msft0200]: <https://community.frame.work/t/unknown-device-acpi-msft0200-in-windows-10-pro/39199>
[^windows-tpm]: <https://learn.microsoft.com/zh-cn/windows/security/hardware-security/tpm/how-windows-uses-the-tpm>
[^windows-esu]: <https://learn.microsoft.com/zh-cn/windows/whats-new/extended-security-updates>

## 性能

如前面所述，我等待 AMD 型号、选择了 7840U，就是为了更强的性能；尤其是 7840U 的 8 核 Zen 4 架构 CPU 和 RDNA 3 架构集显，让我对它的性能充满期待。因此，收到货后，一装好操作系统，我就迫不及待地开始测试这台笔记本的性能，看看相比于我之前用的惠普 Envy x360 13-ay0000 有多大的提升。这台惠普的笔记本用的是 AMD 在 2020 年推出的锐龙 7 4700U，所以这两台机器的性能差异也能反映出 AMD 轻薄本处理器在过去三年的性能提升。当然了，这样的比较有些勉强，毕竟锐龙 4000 系列中和 7840U 对位的型号应该是 4800U，因为两者都是 8 核 16 线程；而 4700U 没有超线程，所以虽然同为 8 核，但只有 8 线程。由于我没有搭载 4800U 的机器，所以只能用 4700U 来和 7840U 比较了。

### 关于电源适配器的注意事项

如果经常运行高负载应用（例如软件编译和游戏），那么推荐使用 100 瓦 USB PD 电源适配器，以避免笔记本频繁从电池“偷电”。如果电源适配器的输出功率不足以应付高负载应用的高功耗，那么这台笔记本会通过从电池取电来满足额外的电力需求。从电池取电后，充电指示灯可能会短暂变为橙色，说明刚才取电造成了电量下降，以至于电池重新被充满。然而，这样的频繁充放电可能缩减电池寿命，尤其是在电池电量长期被维持在 100% 左右的情况下。

为了探究电源适配器功率对电池取电行为的影响，我将笔记本连接到不同功率的电源适配器，然后用 AIDA64 记录了在同样应用场景（运行同一游戏）下电池充电功率的图表。

65 瓦适配器的图表如下。最开始，处理器 TDP 为 35 瓦时，电池充电功率频繁波动，说明此时笔记本频繁从电池取电，导致电池重新被充满。一段时间后，笔记本将处理器 TDP 降至 30 瓦，电池充电功率也随之稳定在 0 左右，说明功耗需求降低后，电源适配器提供的功率就可以满足了。

![65 瓦适配器的电池充电功率图表]({{< static-path img bat-charge-rate-65w.png >}})

100 瓦适配器的图表如下。电池充电功率在两个 TDP 阶段都能稳定在 0 左右，说明更高功率的适配器是有明显作用的。

![100 瓦适配器的电池充电功率图表]({{< static-path img bat-charge-rate-100w.png >}})

### CPU

CPU 跑分我一般用 Cinebench。以下是这台 Framework Laptop 13 上的 7840U 在常见 Cinebench 版本下的分数，以及我之前用的惠普 Envy x360 上的 4700U 的分数作为对比：

| 项目 | 锐龙 7 7840U | 锐龙 7 4700U |
| :-- | --: | --: |
| Cinebench R15（多核） |  2290 | 1151 |
| Cinebench R20（多核） |  5541 | 2853 |
| Cinebench R23（多核） | 13913 | 7211 |

![Cinebench 跑分结果]({{< static-path img cinebench.png >}})

有了 Zen 4 和超线程，7840U 的 Cinebench 跑分几乎是基于 Zen 2 的 4700U 的两倍，让我更加期待它在实际 CPU 使用场景中的性能进步，例如软件编译。

为了对比两台笔记本的 CPU 在软件编译方面的性能，我收集了两批数据。第一批是《Linux From Scratch》（LFS）一书中的[“标准构建时长单位”（*Standard Build Unit*, SBU）][lfs-sbu]，即 LFS 书中使用的用来估计软件包构建时长的时间单位；我遵照 [LFS 书中的步骤][lfs-measure-sbu]，选择在 tmpfs 上单线程编译 GNU binutils 2.42 来测得 1 SBU 所对应的时间。第二批数据是使用 [`qlop`][qlop] 工具查询得到的 Gentoo 软件包在 Portage 下的构建时长。以下是所有数据：

| 项目 | 锐龙 7 7840U | 锐龙 7 4700U |
| :-- | --: | --: |
| LFS SBU               |  1'55" |  2'26" |
| `sys-devel/binutils`  |    48" |    58" |
| `sys-devel/gcc`       | 20'28" | 30'43" |
| `sys-devel/llvm`      | 15'41" | 29'41" |
| `net-libs/webkit-gtk` | 27'18" | 49'32" |

其中，`sys-devel/llvm` 的构建速度在 7840U 上几乎是 4700U 的 2 倍，与 Cinebench 跑分的提升吻合。至于其它几项数据，因为构建过程并非完全并行，所以根据[阿姆达尔定律]，构建速度提升也相对较低。尤其是 SBU，因为是通过单线程编译软件包测得的，所以本质相当于单线程跑分，提升幅度自然不会和 Cinebench 多核心跑分一样。`sys-devel/llvm` 的构建几乎能充分利用所有线程，故构建速度提升接近 2 倍；不过，即使是 `sys-devel/gcc` 和 `net-libs/webkit-gtk` 这些构建过程不完全并行的大包，每个包在 7840U 上也能比 4700U 快至少 10 分钟，我觉得已经是很大的提升了。

[lfs-sbu]: https://www.linuxfromscratch.org/lfs/view/stable/chapter04/aboutsbus.html
[lfs-measure-sbu]: https://www.linuxfromscratch.org/lfs/view/stable/chapter05/binutils-pass1.html
[qlop]: https://wiki.gentoo.org/wiki/Q_applets/zh-cn#.E4.BB.8E_emerge_.E6.97.A5.E5.BF.97.E6.8F.90.E5.8F.96.E4.BF.A1.E6.81.AF_.28qlop.29
[阿姆达尔定律]: https://zh.wikipedia.org/zh-cn/%E9%98%BF%E5%A7%86%E8%BE%BE%E5%B0%94%E5%AE%9A%E5%BE%8B

### 集显

在显卡游戏性能测试方面，我一般用 3DMark 跑分外加直接跑游戏进行测试。以下是 Radeon 780M 集显在 3DMark Time Spy 中的跑分结果，该跑分反映的是显卡的 DirectX 12 性能：

| 项目 | 分数 |
| :-- | --: |
| Time Spy 分数 | 3376 |
| 显卡分数 | 3026 |
| CPU 分数 | 9811 |

![Time Spy 结果]({{< static-path img 3dmark-time-spy.png >}})

以上分数基于 AMD 24.5.1 版驱动包、驱动程序版本 31.0.24033.1003。如果用 2023 年的驱动版本（23.x 版本）的话，分数会比这个更低：图形分会在 2700--2800 这个范围。之前也提到，内存性能也会影响集显性能。

因为我现在不怎么玩游戏了，所以方便用于显卡性能测试、且性能测试结果还有一定参考意义的游戏恐怕只有 GTA V 了。这游戏是我硬盘上安装的唯一内置基准测试的游戏；基准测试可以提供标准化的游戏帧数数据，方便在不同机器间比较。以下是 Radeon 780M 运行 GTA V 基准测试的结果以及我使用的画质设置（点击图片可查看大图）：

```
Frames Per Second (Higher is better) Min, Max, Avg
Pass 0, 41.443390, 103.412613, 72.060959
Pass 1, 42.708942, 167.861282, 75.391861
Pass 2, 45.226563, 147.035034, 74.737999
Pass 3, 49.950550, 210.207687, 79.189964
Pass 4, 35.867619, 189.533920, 80.756622

Frames under 16ms (for 60fps):
Pass 0: 550/667 frames (82.46%)
Pass 1: 600/694 frames (86.46%)
Pass 2: 535/683 frames (78.33%)
Pass 3: 656/732 frames (89.62%)
Pass 4: 8086/9001 frames (89.83%)

Frames under 33ms (for 30fps):
Pass 0: 667/667 frames (100.00%)
Pass 1: 694/694 frames (100.00%)
Pass 2: 683/683 frames (100.00%)
Pass 3: 732/732 frames (100.00%)
Pass 4: 9001/9001 frames (100.00%)
```

{{<fig class="third">}}
[![GTA V 画质设置 1]({{< static-path img gta-v-gfx-settings-1.png l10n >}})]({{< static-path img gta-v-gfx-settings-1.png l10n >}})
[![GTA V 画质设置 2]({{< static-path img gta-v-gfx-settings-2.png l10n >}})]({{< static-path img gta-v-gfx-settings-2.png l10n >}})
[![GTA V 画质设置 3]({{< static-path img gta-v-gfx-settings-3.png l10n >}})]({{< static-path img gta-v-gfx-settings-3.png l10n >}})
[![GTA V 画质设置 4]({{< static-path img gta-v-gfx-settings-4.png l10n >}})]({{< static-path img gta-v-gfx-settings-4.png l10n >}})
{{</fig>}}

在我现在还在玩的几款游戏中，780M 集显可以满足我 1080p 60 FPS 的游戏需求，而且我在 GTA V 中甚至能稍微调高一点画质。虽然游戏帧数偶尔会掉到 60 FPS 以下，但基本仍能保持在 45 FPS 以上，对我来说完全可以接受。作为一颗集显，这样的表现已经很好了。

当然了，跑分更高不代表实际性能也会一直更高。虽然 780M 的 3DMark 跑分高于我的 XPS 15 9570 上的英伟达 GeForce GTX 1050 Ti Max-Q，但 780M 的实际游戏帧数有时会低于后者——GTA V 就是例子之一。GTA V 是 DirectX 11 游戏，所以我在两台机器上又跑了 3DMark Fire Strike 进行比较，因为 Fire Strike 是 DirectX 11 测试。以下是跑分结果：

| 项目 | Radeon 780M | GTX 1050 Ti Max-Q |
| :-- | --: | --: |
| Fire Strike 分数 |  7855 |  6621 |
| 显卡分数 |  8503 |  7524 |
| 物理分数 | 25328 | 10093 |
| 综合分数 |  3015 |  2661 |

以下是在使用相同画质设置时，GTX 1050 Ti Max-Q 运行 GTA V 基准测试的结果：

```
Frames Per Second (Higher is better) Min, Max, Avg
Pass 0, 47.708786, 137.780899, 96.168571
Pass 1, 57.501667, 125.212860, 93.112434
Pass 2, 69.121399, 140.849030, 100.829323
Pass 3, 35.229626, 148.829453, 101.437538
Pass 4, 40.175808, 150.308136, 97.573715

Frames under 16ms (for 60fps):
Pass 0: 888/896 frames (99.11%)
Pass 1: 867/871 frames (99.54%)
Pass 2: 937/937 frames (100.00%)
Pass 3: 933/941 frames (99.15%)
Pass 4: 10949/11024 frames (99.32%)

Frames under 33ms (for 30fps):
Pass 0: 896/896 frames (100.00%)
Pass 1: 871/871 frames (100.00%)
Pass 2: 937/937 frames (100.00%)
Pass 3: 941/941 frames (100.00%)
Pass 4: 11024/11024 frames (100.00%)
```

所以，这是跑分更高但实际游戏帧数更低的又一个例子。

#### BIOS 中显存大小设置的影响

这款机型目前的 3.05 版 BIOS 中有个“iGPU Memory”选项，可以在“自动”和“游戏”两个模式间切换，以调节给集显预分配的显存大小。我安装了 32 GB 内存，所以自动模式下预分配的显存是 512 MB，而游戏模式下是 4 GB。

![3.05 版 BIOS 中的显存大小设置]({{< static-path img bios-igpu-memory.jpg >}})

我并没有发现这个设置对集显性能有什么明显的影响。游戏模式下，Time Spy 显卡分可以跑到 3050 分，提升幅度小于 1%。在 GTA V 的基准测试里，有些场景帧数更高，有些帧数不变，有些甚至会降低，但每个场景的帧数变化都在 ±2 FPS 以内。根据我在网上看到的其它评测，在大多游戏中，预分配更多显存带来的帧数提升也就 1 FPS[^vram-alloc-cmp]。

[^vram-alloc-cmp]: <https://www.bilibili.com/video/BV1dn4y1R7p7/>

## 扩展卡系统

Framework Laptop 13 的主板上只有 4 个 USB-C 接口，就和苹果 MacBook Pro 在 2016 到 2020 年间的不少机型一样。因此，这些 MacBook Pro 机型的用户和 Framework 用户一样，如果想使用不是 USB-C 接口的设备，就必须常备 USB-C 转接头。不过，Framework 用户不需要担心忘带转接头的问题，因为他们可以选择能直接嵌入 Framework 笔记本机身的转接头：[扩展卡]。

![在 Framework 笔记本上安装扩展卡](https://static.frame.work/eim1sw277dv3w4iqpffoy6h1utam)

[扩展卡]: https://frame.work/marketplace/expansion-cards

Framework Laptop 13 机身左侧和右侧各有 2 个扩展卡槽，每个卡槽对应主板上的一个 USB-C 接口。只需插入对应的扩展卡，即可将扩展卡槽变为 USB-C 接口、USB-A 接口、HDMI 接口、microSD 卡槽等接口，甚至还能变为笔记本上十分少见的标准 DP 接口。其它笔记本上的接口类型是由厂商决定的，而 Framework 用户通过使用不同的扩展卡，可以自己选择他们笔记本上的接口类型。

其实，扩展卡的本质就是 USB-C 到各种接口的转接头（或者，对于[存储扩展卡]而言，USB-C 接口的 U盘），插在其它笔记本上也可以使用，只不过是做成了能嵌入 Framework 笔记本机身的样子。

在我看来，这套扩展卡系统最好的地方在于：用户可以将笔记本的接口挪到机身的另一侧。举个例子，如果使用最新款 MacBook Pro 的用户需要把电脑连接到左手边的投影仪上，但是投影仪的 HDMI 线不够长，那么操作可能十分费劲，因为新款 MacBook Pro 的 HDMI 接口在机身右侧。而如果这名用户用的是 Framework 笔记本，那么只要有 HDMI 扩展卡，即使插在了机身右侧，也可以当场将扩展卡换到机身左边，即可解决 HDMI 线过短的问题，并且插拔扩展卡无需任何工具。

[存储扩展卡]: https://frame.work/products/storage-expansion-card-2nd-gen

## 显示屏

我的这台 Framework Laptop 13 的显示屏面板是京东方 NE135FBM-N41，分辨率为 2256 × 1504，屏幕刷新率为 60 Hz。我对屏幕不是很挑剔，所以完全可以接受这个平庸的刷新率。我这块显示屏是雾面屏，是 Framework 在 2023 年新推出的屏幕选项；相比于最开始的镜面屏，雾面屏这一点我倒是更喜欢。而在本文编写之时，Framework 刚刚又推出了全新的 2.8K 屏幕选项，规格为 2880 × 1920 @ 120 Hz，既可在购买新机时选配，也可以单独购买，以升级手头已有的笔记本的显示屏。

假如可以的话，我倒是想对这块显示屏做两处改动……

### 分辨率

我个人反而希望屏幕分辨率更低一些，比如 1920 × 1280，这样的话即使不启用显示缩放，也仍然能看清屏幕内容。我在 GNU/Linux 上使用的桌面环境是 GNOME，而在本文编写之时，如果在 GNOME 上开启了非整数倍缩放，那么非原生 Wayland 应用显示会模糊。我的惠普 Envy x360 的屏幕是 13.3 寸 1920 × 1080 的，在关闭缩放的情况下，GNOME 界面的大小对我来说依然合适；而在 Framework Laptop 13 上，我就得开启 125% 缩放才能让界面大小和 13.3 寸 1920 × 1080 上差不多。

### 长宽比例

Framework Laptop 13 显示屏的长宽比是 3:2，但我更希望它是 16:10。

3:2 对于一些生产力任务更合适，但 16:10 能更好地在生产力和娱乐之间平衡。16:10 与 3:2 一样，都有比 16:9 更多的纵向空间，利于生产力任务（虽然 3:2 多出的纵向空间比 16:10 更多）。但在观看 16:9 视频和玩只支持 16:9 的游戏时，16:10 的屏幕黑边会比 3:2 更小。

因为 3:2 的显示屏更高了，所以笔记本机身整体也得跟着纵向拉伸。这样一来，这台 13.5 寸笔记本的纵向长度就几乎赶上了 15 寸笔记本。因此，在诸如飞机小桌板、学校宿舍里外接键盘放在笔记本前这样的纵向空间受限的场景下，Framework Laptop 13 占用的空间就可能和 15 寸笔记本感觉差不多。同样地，宽度不足以容纳 15 寸笔记本的电脑包也不一定能塞得下 Framework Laptop 13。

{{<fig>}}
![Framework Laptop 13 与 13.3 寸和 15.6 寸笔记本对比]({{< static-path img size-cmp.jpg >}})

{{<figcaption>}}
Framework Laptop 13（中）与 13.3 寸的惠普 Envy x360 13-ay0000（左）和 15.6 寸的戴尔 XPS 15 9570（右）并排比较。
{{</figcaption>}}
{{</fig>}}

尽管这台 Framework Laptop 13 的性能全面超越了我的惠普 Envy x360，而且在 GNU/Linux 下可以方便地用指纹解锁，但我在外出时，有时还是更喜欢携带惠普，就是因为惠普的机身更小巧便携：惠普的机身横向上只比 Framework 长 1 cm，但纵向上短 3.4 cm。

![Framework Laptop 13 与 HP Envy x360 13-ay0000 对比]({{< static-path img size-cmp-hp-fw.jpg >}})

当然了，3:2 显示屏也许对 Framework Laptop 13 的内部设计带来了至关重要的好处：纵向拉伸机身是容纳 3:2 屏幕所需，但也给机身内部增加了更多空间，可以容纳更大的电池和扬声器。通过观察 Framework Laptop 13 内部构造[^frmw-13-internal]，可以发现扩展卡系统需要占用机身内部不少空间，而 3:2 显示屏为机身带来的额外空间可以抵消这部分空间占用。而如果显示屏是 16:10 的话，内部空间可能缩减，也许就需要牺牲电池容量或者扬声器音质——甚至可能要同时牺牲两者。

![Framework Laptop 13 内部](https://d3t0tbmlie281e.cloudfront.net/igi/framework/xTNw2tQFvMhsdySS.large)

[^frmw-13-internal]: <https://guides.frame.work/Guide/Checking+out+the+inside+of+a+Framework+Laptop+13/101#s553>

## 所谓“模块化”与可维修性

如开头所述，在国内网络社区中，许多人看到 Framework 都会从“模块化”角度参与讨论，久而久之也就给 Framework 笔记本贴上了“模块化”的标签。

有网友称，Framework 笔记本算不上真正的模块化，因为它的处理器和现阶段的主流笔记本一样，都是焊在主板上的，无法像蓝天准系统笔记本使用台式机插槽处理器那样轻松自主更换。我认同这一观点：如果想升级处理器这一影响电脑性能的核心因素，那么只能通过更换主板（或者自己焊接）实现，因此单在这一点上，Framework 笔记本的模块化程度和其它大部分笔记本是一样低的。不过，处理器直接焊在主板上恐怕是无法避免的设计，因为 Framework 推出第一款笔记本型号的一个目标就是在不牺牲轻薄和性能的情况下，做可升级、可客制化、可维修的产品[^frmw-13-og-news]，而想要控制机身厚度的话就得采取焊接处理器的方案。并且，在其它方面，Framework Laptop 13 的模块化做得还是到位的：双内存插槽[^frmw-13-memory]是很多 13 寸左右的笔记本根本没有的（其它的大多都是板载），而扩展卡系统相当于模块化接口。

但是，Framework 自己其实很少用“模块化”来标榜 Framework Laptop 13。Framework 官网主页现在对 Framework Laptop 13 的描述是“客制化、升级、维修”，并没有以模块化（modularity）为重点。在他们的“关于我们”页面上[^frmw-about]，连“modularity”或者“module”字样都搜不到；反之，该页面上谈论的是 Framework 通过设计可维修的产品来延长电子产品生命周期、减少电子垃圾，同时锦上添花地让产品具备可升级、可客制化的属性。Framework Laptop 16 倒是更强调模块化：不但可以在 C 面选择安装数字小键盘模块、宏键盘模块、灯带模块、LED 点阵模块等，而且机身尾部还可以插入诸如独立显卡、M.2 固态硬盘转接卡等扩展模块[^frmw-16-news]。然而，即使机身更大，Framework Laptop 16 的处理器仍然是焊在主板上的。有趣的是，国内网络社区给 Framework 贴的“模块化”标签是在 Framework Laptop 16 发布前就有的，而当时只有 Framework Laptop 13。

![Framework 官网主页对 Framework Laptop 13 的描述]({{< static-path img framework-homepage.png l10n >}})

不过，即使从维修操作难易度的角度而言，Framework 笔记本和其它笔记本相比也未必有明显优势。“拧下几颗螺丝、取下盖板，就可以接触到电脑内部元件”的基本维修流程，适用于 Framework，但也适用于很多别的笔记本。开始接触内部元件前，为防止误碰元件造成短路导致设备损坏，通常都建议先断开电池连接；然而 Framework 官方居然不推荐在维修前断开电池[^frmw-battery-connector]，原因是电池排线插座里的针脚过于脆弱，容易弯曲。在 Framework Laptop 13 的官方维修指南里，如果有必须断开电池的维修步骤（比如更换电池本身），那么到重连电池的时候，维修指南会强调检查电池排线插座里有没有针脚已经弯曲，而如果有的话，应中断维修并联系客服寻求帮助[^frmw-check-battery-pins]。也许 Framework Laptop 13 的主板有拆机自动断电机制，但谁知道呢？这样一来，用户在维修时，要么顶着安全风险，遵照官方建议，不断开电池，要么冒着损坏脆弱的电池针脚的风险，断开电池——维修反而没那么简单了。而在其它没有标榜可维修性的笔记本上，我还没有听说过因为电池接口过于脆弱，所以维修时不应该拔电池的说法。

然而，“可维修性”的范畴仅限于操作难易度吗？正所谓“巧妇难为无米之炊”：就算笔记本再好拆，如果有零部件损坏了，需要更换，但是买不到靠谱的备件，那也没法完成维修。多年前，我弄坏了我当时用的一台华硕笔记本的无线网卡天线，但即使是在淘宝上也找不到卖那台笔记本的天线的。我现在手头的惠普 Envy x360 的电池容量已经衰减了，如果我想换块新电池让它满血复活，那么就算能在淘宝上买到我这个型号的电池，也得用自己的生命和财产安全赌它的质量是否合格、会不会哪一天突然爆炸。如果我在 Framework 笔记本上遇到这类需要
更换零部件的情况，就不用担心备件的问题，因为官方商城上可以买到所有的原装备件——无线网卡天线、电池、甚至螺丝。如果确实需要更换 Framework 笔记本的电池，那电池针脚脆弱又怎样？至少可以谨慎操作，避免弄弯针脚；电池可以从官方买到原装的替换件，不需要拿个人生命和财产安全来赌博。

官方商城上售卖原装备件可供所有消费者购买——*这*在我看来才是 Framework 产品最突出的特点，而不是什么模块化。其它笔记本厂商里没有多少会给个人消费者提供购买原装备件的渠道。

如果我可以改 Framework Laptop 13 的一处设计，那么肯定是电池接口，毕竟维修前断开电池对于安全十分重要。我觉得改成 Framework Laptop 16 用的那种接口[^frmw-16-battery]就不错（如下图所示），它用的是更宽、更坚固的金手指，而非细细的针脚。我也不清楚 Framework 在一开始为什么选用这种使用脆弱针脚的方案；或许这样能把电池容量做得更大，又或许是在发布第一批 Framework Laptop 13 的时候市面上只有这种方案能用。

![Framework Laptop 16 的电池接口](https://d3t0tbmlie281e.cloudfront.net/igi/framework/4CsjVHfRoYtZjd2W.large)

[^frmw-13-og-news]: <https://frame.work/blog/introducing-the-framework-laptop>
[^frmw-13-memory]: <https://guides.frame.work/Guide/Memory+Replacement+Guide/94#s472>
[^frmw-16-news]: <https://frame.work/blog/introducing-the-framework-laptop-16>
[^frmw-about]: <https://frame.work/about>
[^frmw-battery-connector]: <https://frame.work/battery-connector>
[^frmw-check-battery-pins]: <https://guides.frame.work/Guide/Battery+Replacement+Guide/85#s1533>
[^frmw-16-battery]: <https://frame.work/products/16-battery>
