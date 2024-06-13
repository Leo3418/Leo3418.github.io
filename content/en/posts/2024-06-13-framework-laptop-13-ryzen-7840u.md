---
title: "Framework Laptop 13 with AMD Ryzen 7 7840U"
categories:
  - Blog
toc: true
header:
  overlay_image: /img/posts/2024-06-13-framework-laptop-13-ryzen-7840u/unbox.jpg
  og_image: /img/posts/2024-06-13-framework-laptop-13-ryzen-7840u/unbox.jpg
  overlay_filter: 0.5
  show_overlay_excerpt: false
---

[Framework Laptops][framework-laptop] are a series of laptops designed with
user repairability, configurability, and upgradability in mind.  After two
years of launching exclusively models using Intel Core processors, Framework
started to ship laptops with AMD Ryzen processors, which are what I looked for
since I had heard of Framework.  So, I purchased a Framework Laptop 13 with AMD
Ryzen 7 7840U, received it in November 2023, and have been using it ever since,
but only recently have I found time to write an article about my experience
with it.

[framework-laptop]: https://frame.work/

## Backstory

After learning about the first generation of Framework Laptop based on 11th Gen
Intel Core processors, I said to myself: "I would have got it if they used
AMD." Availability of genuine spare parts through the official
[Marketplace][marketplace-parts] was the most appealing aspect of Framework's
implementation of repairability to me: with some laptops I had previously
owned, I had encountered situations where I would need replacement parts but
have no reliable ways to get genuine ones.  However, I was disappointed to see
Framework's choice of Intel processors, which were not the best option
available to laptop manufacturers.  Since the Ryzen 4000 Series, an AMD
ultrabook CPU often outperforms an Intel counterpart under the same power
consumption[^amd-intel-cmp-1] [^amd-intel-cmp-2]; as a result, AMD-powered
ultrabooks generally have better performance than Intel-powered ones.

Eventually, in 2023, Framework announced models using AMD Ryzen 7040 Series
processors, so it was time to fulfill the small promise I had made to myself.
I could not order the AMD-powered model at once after it was announced for
personal reasons, and when I was finally ready to order it, my order was queued
to Batch 5, which is why I received it in November after the previous batches.
Framework also launched a new 16" model (Framework Laptop 16) using AMD Ryzen
7040 Series alongside the AMD-powered models in the original 13.5" form factor
(Framework Laptop 13), but I still preferred a smaller, more portable laptop,
so I did not change my mind.

[marketplace-parts]: https://frame.work/marketplace/parts

[^amd-intel-cmp-1]: <https://www.notebookcheck.net/The-7-nm-AMD-Ryzen-7-Pro-4750U-destroys-the-Core-i7-10810U-and-Intel-has-no-answer-at-the-moment.506291.0.html>
[^amd-intel-cmp-2]: <https://www.notebookcheck.net/AMD-Ryzen-7-6800U-Efficiency-Review-Zen3-beats-Intel-Alder-Lake.623763.0.html>

## Configuration

Framework provided two processor choices from the Ryzen 7040 Series:

| Processor | CPU Core / Thread Count | CPU Boost Clock | Integrated GPU | GPU Core Count | GPU Boost Clock |
| :-: | :-: | :-: | :-: | :-: | :-: |
| Ryzen 5 7640U | 6 / 12 | 4.9 GHz | Radeon 760M |  8 | 2.6 GHz |
| Ryzen 7 7840U | 8 / 16 | 5.1 GHz | Radeon 780M | 12 | 2.7 GHz |

I chose to pay the premium for Ryzen 7 7840U for two major computing workloads
of mine: software package compilation on Gentoo Linux and light gaming.  In
software compilation, more available CPU threads generally help to reduce
compilation time and are thus preferable: an increase from 12 threads to 16
could translate to up to 25% reduction of a large software package's
compilation time.  The Radeon 780M integrated GPU that comes with 7840U can
also deliver a better frame rate in games than the 760M with 7640U thanks to
more GPU cores (a.k.a. Compute Units, CUs).

I also picked a 32 GB DDR5-5600 CL46 memory kit with two 16 GB DIMMs for these
workloads.  With 16 maximum concurrent threads running to compile a software
package, 32 GB of RAM is more than enough for about 99% of software packages;
the rest 1% are very large software packages that might need 2 GiB per thread
as per Gentoo's recommendation[^portage-makeopts], such as `net-libs/webkit-gtk`, so I went
for 32 GB just to be safe.  (An alternative solution, which relaxes the RAM
requirement, would be to run fewer concurrent jobs just for those very large
packages using [`/etc/portage/package.env`][portage-package.env].)  I chose a
kit of two 16 GB DIMMs rather than a single 32 GB DIMM to have dual-channel
memory, which would benefit the integrated GPU's performance since integrated
GPU does not have dedicated video memory and thus has to share the system
memory.

[portage-package.env]: https://wiki.gentoo.org/wiki//etc/portage/package.env#Use_different_MAKEOPTS_for_a_specific_package

[^portage-makeopts]: <https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage#MAKEOPTS>

## Operating Systems

I have been using Gentoo Linux for almost all tasks that I do on a computer
except gaming; for games, I would dual-boot Microsoft Windows to avoid the
hassle of setting up a Windows compatibility layer on GNU/Linux.  Most laptops
work fine under Windows because of Microsoft's partnership with most laptop
manufacturers, but under GNU/Linux, many laptops exhibit issues that could
range from foibles to complete breakages.  In this regard, the Framework
Laptops are different from other laptops.

### GNU/Linux

Framework is the computer manufacturer that makes the most serious commitment I
have seen with regards to official GNU/Linux support.  They make sure the
hardware components of their laptops have drivers in the Linux
kernel[^frmw-linux], so all hardware features are functional on a Linux-based
operating system.

One hardware component that often does not work under GNU/Linux on other
laptops but works on Framework Laptops is fingerprint reader.  Both of the two
laptops I previously own -- an HP Envy x360 13-ay0000 and a Dell XPS 15 9570 --
have a fingerprint reader, but neither laptop's fingerprint reader is
functional under GNU/Linux.  In fact, it is ridiculous that the driver for HP
Envy x360's fingerprint reader was once added to the Linux kernel but then
removed upon the fingerprint reader vendor's request[^hp-13-ay0000-fp].  The
Framework Laptop 13 gave me the first chance to experience fingerprint unlock
on GNU/Linux.

![Fingerprint settings in GNU/Linux running GNOME]({{< static-path img
gnu-linux-fingerprint.png >}})

[^frmw-linux]: <https://frame.work/linux>
[^hp-13-ay0000-fp]: Search for "ay0xxxx" in <https://gitlab.freedesktop.org/libfprint/wiki/-/wikis/Unsupported-Devices>

### Windows

Interestingly, for Windows support, Framework seems to follow what Microsoft
might want laptop manufacturers to do as well as what most other manufacturers
are doing -- quite the opposite of Framework's attitude to GNU/Linux support:
they support only Windows 11 on the latest models and do not officially support
Windows 10.  Even though AMD officially supports Windows 10 on
7840U[^amd-7840u-specs], Framework does not provide Windows 10 drivers for
laptops using AMD Ryzen 7040 Series processors.

The lack of Windows 10 drivers from Framework can be almost fully solved with
some very easy workarounds.  The [official driver bundle] can be unpacked using
[7-Zip] to get drivers for the integrated GPU, Bluetooth, the fingerprint
reader, audio, and Wi-Fi, which should be compatible with both Windows 10 and
Windows 11.  The chipset software in the driver bundle is incompatible with
Windows 10, but Windows 10 users can access [AMD official
website][amd-ryzen-mobile-chipset-drivers] to download chipset software
compatible with Windows 10 instead.

After these drivers are installed, there should be only one device without
driver left, which has hardware ID `MSFT0200`, as shown in the following
screenshot.  This should be the [Microsoft Pluton security processor] providing
TPM functionality[^frmw-msft0200].  I cannot find any Microsoft Pluton driver
for Windows 10, which means that the TPM has to be left dysfunctional on
Windows 10.  To personal users, BitLocker drive encryption is the Windows
feature most significantly impacted by the lack of a TPM[^windows-tpm].
Because I do not need BitLocker since I use Windows basically only for gaming
and store no sensitive data on the Windows partition, I take this as a
negligible sacrifice for using Windows 10 on the laptop.

![The only driver-less device left on Windows 10]({{< static-path img
devmgmt.png >}})

Some people would suggest upgrading to Windows 11 in this case because it *is*
a solution to this problem, and Windows 10 will no longer be supported after
October 14, 2025.  The end of Windows 10 support is a weak argument for
upgrading to Windows 11 because, unlike for previous versions of Windows,
Microsoft will offer the Extended Security Updates program for Windows 10 to
*individual users* in addition to organization customers, which will allow
every paying user to continue to get Windows 10 security updates past the end
of support[^windows-esu].  Users who have access to Windows 10 LTSC are also
supported until 2032.

[official driver bundle]: https://knowledgebase.frame.work/framework-laptop-13-bios-and-driver-releases-amd-ryzen-7040-series-r1rXGVL16
[7-Zip]: https://www.7-zip.org/
[amd-ryzen-mobile-chipset-drivers]: https://www.amd.com/en/support/downloads/drivers.html/chipsets/laptop-chipsets/amd-ryzen-and-athlon-mobile-chipset.html
[Microsoft Pluton security processor]: https://learn.microsoft.com/en-us/windows/security/hardware-security/pluton/pluton-as-tpm

[^amd-7840u-specs]: <https://www.amd.com/en/products/processors/laptop/ryzen/7000-series/amd-ryzen-7-7840u.html>
[^frmw-msft0200]: <https://community.frame.work/t/unknown-device-acpi-msft0200-in-windows-10-pro/39199>
[^windows-tpm]: <https://learn.microsoft.com/en-us/windows/security/hardware-security/tpm/how-windows-uses-the-tpm>
[^windows-esu]: <https://learn.microsoft.com/en-us/windows/whats-new/extended-security-updates>

## Performance

As discussed above, the great performance that 7840U delivers -- from both the
8-core Zen 4 CPU and the integrated RDNA 3 GPU -- was what I looked forward to
when I purchased the device.  I was eager to see how AMD ultrabook processors'
performance had improved in the past three years by comparing it to my previous
daily-driver laptop: the HP Envy x360 13-ay0000 with Ryzen 7 4700U, which was
launched in 2020.  This is not the fairest comparison because the best-matching
counterpart of 7840U in the Ryzen 4000 Series is 4800U, which also has 8 cores
and 16 threads; 4700U does not have SMT, so it has only 8 threads albeit having
8 cores as well.  However, I have never owned a device with 4800U, so I could
only compare 7840U to 4700U.

### A Note on Power Adapter Output

Users who often run sustained heavy workloads (like me -- software compilation
and gaming both belong to this category) are recommended to get an 100 W USB PD
power adapter to avoid having the laptop frequently "borrowing" power from the
battery.  If the power adapter cannot deliver sufficient power output to meet a
heavy workload's high power consumption, then the laptop may briefly draw power
from the battery to supplement.  Once this happens, the side LED next to the
charging port may blink amber as the battery is recharged for the power just
drawn.  Frequent recharges -- especially at 100% -- may significantly reduce
the battery lifespan.

To explore how the power adapter's power output affects such power draws from
the battery, I used AIDA64 to log graphs of battery charge rate during the same
sustained heavy workload (running the same game) with different power adapter
output wattages.

The following graph is for a 65 W adapter.  At first, when the processor's TDP
was 35 W, battery charge rate fluctuated significantly, indicating frequent
battery discharges and recharges.  Then, the laptop lowered the TDP to 30 W,
and battery charge rate stabilized near zero because the power adapter's output
became sufficient for the reduced power consumption.

![Battery charge rate graph with 65 W adapter]({{< static-path img
bat-charge-rate-65w.png >}})

The graph below is for an 100 W adapter.  Battery charge rate was stable near
zero during both TDP stages, showing how an adapter with higher power output
helped.

![Battery charge rate graph with 100 W adapter]({{< static-path img
bat-charge-rate-100w.png >}})

### CPU

I would typically use Cinebench for standardized CPU benchmarks.  The following
are the scores I got after running different Cinebench releases on Framework
Laptop 13 with 7840U.  For comparison, I also ran Cinebench on my older HP Envy
x360 with 4700U.

| Item | Ryzen 7 7840U | Ryzen 7 4700U |
| :-- | --: | --: |
| Cinebench R15 (Multi Core) |  2,290 | 1,151 |
| Cinebench R20 (Multi Core) |  5,541 | 2,853 |
| Cinebench R23 (Multi Core) | 13,913 | 7,211 |

![Cinebench scores]({{< static-path img cinebench.png >}})

The combination of Zen 4 and SMT contributed to nearly doubled Cinebench scores
from 4700U based on Zen 2 to 7840U.  This showed a promising sign of
significant performance improvement in real-world workloads such as software
compilation, which is a CPU-intensive task.

Then, I measured and compared the two CPUs' performance in software compilation
through two types of metrics.  The first was the [*Standard Build Unit*
(SBU)][lfs-sbu], a concept from the *Linux From Scratch* (LFS) book for the
purpose of estimating software packages' build times; I measured it by
compiling GNU binutils 2.42 without parallelism on a tmpfs using [LFS's
instructions][lfs-measure-sbu].  The other type of metrics was build times of
Gentoo packages under Portage reported by [`qlop`][qlop].  The table below
shows the build time metrics for the CPUs being compared.

| Item | Ryzen 7 7840U | Ryzen 7 4700U |
| :-- | --: | --: |
| LFS SBU               |  1'55" |  2'26" |
| `sys-devel/binutils`  |    48" |    58" |
| `sys-devel/gcc`       | 20'28" | 30'43" |
| `sys-devel/llvm`      | 15'41" | 29'41" |
| `net-libs/webkit-gtk` | 27'18" | 49'32" |

On `sys-devel/llvm`, we can observe nearly 2 times speedup of the build
process, which matches the increase in Cinebench scores.  For other packages or
metrics, because their corresponding build processes would involve less
parallelism, the speedup is smaller than 2 times based on [Amdahl's law].  In
particular, the SBU is effectively a single-thread benchmark due to how it
would be measured, so it is natural that the improvement in SBU does not scale
as the multi-core Cinebench scores.  The build process of `sys-devel/llvm` is
almost fully parallelized, hence the ~2x speedup; however, even for
less-parallelized long builds like `sys-devel/gcc` and `net-libs/webkit-gtk`,
the switch from 4700U to 7840U means saving 10 minutes or even more per
package, which is an impressive improvement in my opinion.

[lfs-sbu]: https://www.linuxfromscratch.org/lfs/view/stable/chapter04/aboutsbus.html
[lfs-measure-sbu]: https://www.linuxfromscratch.org/lfs/view/stable/chapter05/binutils-pass1.html
[qlop]: https://wiki.gentoo.org/wiki/Q_applets#Extracting_information_from_emerge_logs_.28qlop.29
[Amdahl's law]: https://en.wikipedia.org/wiki/Amdahl%27s_law

### Integrated GPU

For GPU benchmarks with emphasis on gaming performance, I would typically use
3DMark as well as some video games themselves.  The following table has the
scores that the integrated Radeon 780M attained in 3DMark Time Spy, a benchmark
for DirectX 12 performance:

| Item | Score |
| :-- | --: |
| Time Spy Score | 3,376 |
| Graphics Score | 3,026 |
| CPU Score      | 9,811 |

![Time Spy result]({{< static-path img 3dmark-time-spy.png >}})

For what it is worth, these scores are for driver version 31.0.24033.1003 from
AMD Adrenalin 24.5.1.  With driver versions released in 2023 (Adrenalin 23.x),
I had got lower graphics scores around the 2,700--2,800 range.  As mentioned
before, memory performance can also impact the integrated GPU's performance.

As I have been gaming only infrequently and lightly for a while, the only
still--relatively-popular game I have available for benchmarking is GTA V (if
you recently have ever checked my GTA Online guides on this site, then you
might have noticed that it has not been updated for 3 years now because I
barely play this game anymore).  An important reason that I like to use GTA V
for benchmarking purposes is, it is the only game with built-in benchmark tests
that I still install often, making it easy to consistently compare this game's
performance on different machines.  The result of GTA V built-in benchmark
tests on this Radeon 780M as well as the graphics settings I used are follows
(click an image to view it in a larger size):

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
[![GTA V graphics settings 1]({{< static-path img gta-v-gfx-settings-1.png l10n >}})]({{< static-path img gta-v-gfx-settings-1.png l10n >}})
[![GTA V graphics settings 2]({{< static-path img gta-v-gfx-settings-2.png l10n >}})]({{< static-path img gta-v-gfx-settings-2.png l10n >}})
[![GTA V graphics settings 3]({{< static-path img gta-v-gfx-settings-3.png l10n >}})]({{< static-path img gta-v-gfx-settings-3.png l10n >}})
[![GTA V graphics settings 4]({{< static-path img gta-v-gfx-settings-4.png l10n >}})]({{< static-path img gta-v-gfx-settings-4.png l10n >}})
{{</fig>}}

The Radeon 780M can satisfy my 1080p 60 FPS gaming need in several games I
still play, like GTA V, in which I can even use medium-level graphics settings.
Frame rates might sometimes drop below 60 FPS but can still generally stay
above 45 FPS, which, to me, still looks reasonably smooth.  For an integrated
GPU, this is excellent.

One interesting phenomenon I noticed was that, although the Radeon 780M could
score higher in 3DMark than the NVIDIA GeForce GTX 1050 Ti Max-Q on the XPS 15
9570 that I own, its frame rates in actual games would not always exceed the
1050 Ti Max-Q; GTA V is one example.  Because GTA V uses DirectX 11, for best
comparison, I ran 3DMark Fire Strike -- a DirectX 11 benchmark -- on both
machines and got these scores:

| Item | Radeon 780M | GTX 1050 Ti Max-Q |
| :-- | --: | --: |
| Fire Strike Score |  7,855 |  6,621 |
| Graphics Score    |  8,503 |  7,524 |
| Physics Score     | 25,328 | 10,093 |
| Combined Score    |  3,015 |  2,661 |

And, here is the GTA V benchmark tests result on the GTX 1050 Ti Max-Q under
the same graphics settings:

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

So, the moral here is that higher scores in benchmark software do not always
translate to better performance in actual workloads.

#### The Impact of the "iGPU Memory" BIOS Option

As of BIOS 3.05, the BIOS of Framework Laptop 13 AMD Ryzen 7040 Series has an
"iGPU Memory" BIOS option under the "Advanced" section, which can be changed
from the default "Auto" to "Gaming" to pre-allocate more memory to the
integrated GPU.  On my laptop,  with 32 GB memory installed, the "Gaming"
setting increases the pre-allocated iGPU memory from 512 MB to 4 GB.

![The "iGPU Memory" option in BIOS 3.05]({{< static-path img
bios-igpu-memory.jpg >}})

I could not observe any non-trivial change in integrated GPU performance caused
by changing this option to "Gaming".  With the "Gaming" setting, Time Spy
graphics score could raise to 3,050, which constitutes an increase of less than
1%.  In GTA V benchmark tests, the frame rate increasd in some passes, stayed
constant in some other passes, and dropped in the rest passes, but in every
case, the fluctuation was within ±2 FPS.  In some other people's benchmarks I
found, changing the pre-allocated iGPU memory would just increase the frame
rate by 1 FPS in many games[^vram-alloc-cmp].

[^vram-alloc-cmp]: <https://www.youtube.com/watch?v=2EErzIEERKw>

## The Expansion Card System

Every Framework Laptop 13 mainboard has only 4 USB-C ports, just like many
Apple MacBook Pro models released from 2016 to 2020.  Both those MacBook Pro
users and Framework Laptop users need to carry USB-C adapters with them to use
non--USB-C peripherals, but Framework Laptop users never need to worry about
forgetting their adapters because they can just use the adapters designed to
fit onto their laptop itself -- the [Expansion Cards].  With an Expansion Card,
each of the 4 USB-C ports can be either kept as a USB-C port or converted to a
USB-A port, an HDMI port, a microSD card slot, or even a port that is rarely
found on the latest laptops -- a full-size DP port.  By selecting different
Expansion Cards, customers can decide the types of ports available on their
Framework Laptop, unlike other laptops, on which the manufacturer makes the
decisions on the ports.

In my opinion, the greatest benefit that the Expansion Card system offers is
the ability to move a port to the other side of the laptop.  If a user of a
latest MacBook Pro model wants to connect their laptop to a projector on their
left-hand side, and the projector's HDMI cable is too short, then good luck to
them because, on recent MacBook Pro models, the HDMI port is located at the
laptop's right-hand side.  However, if the user is using a Framework Laptop,
and an HDMI Expansion Card is inserted into a slot on the right-hand side, then
all the user needs to do in this case is to swap the HDMI Expansion Card to the
left-hand side, which is doable without tools.

[Expansion Cards]: https://frame.work/marketplace/expansion-cards

## Display

As of writing, Framework has just launched a new 2.8K display option that
features a higher 2880 × 1920 resolution and 120 Hz refresh rate.  Of course,
my laptop would not feature this new display because I placed my order long
before it became available.  I am not too picky about the display, so I am
happy with the previous 2256 × 1504 @ 60 Hz display option, which came with my
laptop.  I do appreciate the new matte display option though.

I wish that two things about the display were different...

### Resolution

Personally, I actually hope that the display resolution was lower, like 1920 ×
1280, so the display could be used without scaling.  As of writing, on GNOME --
the GNU/Linux desktop environment that I use, some applications that do not use
Wayland natively still look blurry when fractional scaling is active.  On my HP
Envy x360, which has a 13.3" 1920 × 1080 display, I can use GNOME comfortably
without scaling; on Framework Laptop 13, I need to enable 125% scaling to make
UI elements appear in similar sizes as they do on 13.3" 1920 × 1080.

### Aspect Ratio

Framework Laptop 13's display aspect ratio is 3:2.  In my opinion, it would
have been better if the aspect ratio was 16:10.

The 3:2 aspect ratio could benefit some productivity tasks, but 16:10 could be
more balanced between productivity and entertainment.  Like 3:2, 16:10 also
provides more vertical space than 16:9 to benefit those productivity tasks
(although less than what 3:2 adds).  16:10 deviates from 16:9 less than 3:2
does, which alleviates the [letterbox effect] while a 16:9 video is being
watched or a game that only supports 16:9 is being played.

With a taller display, the laptop's chassis must also be vertically stretched
to match the display size.  With the 13.5" 3:2 display, Framework Laptop 13's
depth is comparable to a 15" laptop.  What this could mean is, if I am using a
small table with limited vertical space, like a tray table on an airplane, and
the table is too small to fit a 15" laptop, then it is unlikely to fit
Framework Laptop 13 either.  If a bag is not wide enough to fit a 15" laptop,
then it probably also cannot fit Framework Laptop 13.

{{<fig>}}
![Framework Laptop 13 with a 13.3" laptop and a 15.6" laptop]({{< static-path
img size-cmp.jpg >}})

{{<figcaption>}}
Comparison of Framework Laptop 13's (center) size to 13.3" HP Envy x360
13-ay0000 (left) and 15.6" Dell XPS 15 9570 (right).
{{</figcaption>}}
{{</fig>}}

If I still have any reason to carry around and use the HP Envy x360 despite
Framework Laptop 13's better performance and support for convenient fingerprint
unlock on GNU/Linux, then it is the Envy x360's more portable form factor.  It
is only wider than Framework Laptop 13 by about 1 cm (0.4 in), but vertically,
it is shorter than Framework Laptop 13 by about 3.4 cm (1.35 in).

![Framework Laptop 13 compared with HP Envy x360 13-ay0000]({{< static-path img
size-cmp-hp-fw.jpg >}})

However, the 3:2 aspect ratio might have enabled some better laptop
capabilities: with more internal chassis space added to accommodate the
display, the chassis can also fit larger battery and speakers.  A look at
Framework Laptop 13's internal[^frmw-13-internal] shows that the Expansion Card
system costs non-trivial space inside the chassis, and the extra chassis space
added for the 3:2 display would offset it.  If the display was 16:10, then
battery capacity, speaker quality, or even both might have been sacrificed for
relatively less space.

![Framework Laptop 13's internal](https://d3t0tbmlie281e.cloudfront.net/igi/framework/xTNw2tQFvMhsdySS.large)

[letterbox effect]: https://en.wikipedia.org/wiki/Letterboxing_(filming)

[^frmw-13-internal]: <https://guides.frame.work/Guide/Checking+out+the+inside+of+a+Framework+Laptop+13/101#s553>

## Repairability

Repairability is one of Framework's major values and selling
points[^frmw-about].  Once before, I saw a person asking in the r/framework
subreddit whether it would be worth it to pay the premium for a Framework
Laptop and the benefits that people commonly associate with it, such as
repairability.  After all, laptops cheaper than a Framework Laptop but have
similar hardware specification do exist.

After seeing this question, I started to reflect on the most important reason I
chose a Framework Laptop over other manufacturers' laptops.  Was it the
availability of official repair guides?  Both HP Envy x360
13-ay0000[^service-guide-hp-ay0000] and Dell XPS 15
9570[^service-guide-dell-xps-9570] have an official service guide available
(though in HP's case, the guide's intended audience does not seem to be the end
users, and the guide is no longer available on HP official website as of
writing, which is why I had to include a Wayback Machine link instead).  Was it
the ease of performing a repair?  I am reluctant to obey Framework's guidance
on keeping the battery connected during repairs[^frmw-battery-connector] since
this would typically pose a risk of short circuiting and thus device damage.
Framework suggested this because Framework Laptop 13 mainboards' battery
connector is fragile.  In their official repair guides, every time when the
battery needs to be reconnected, their instructions are to double check for any
bent battery connector pins[^frmw-check-battery-pins]; if bent pins are found,
customers are advised to stop the repair and contact Framework Support.  This
seems to complicate repairs instead of ease them for those who want to
disconnect the battery just to be safe.  On any other laptop, I have never
heard of the need to refrain from disconnecting the battery due to a fragile
connector.

Then, I thought of a thing that Framework has but few other laptop
manufacturers have: an official online store where *all* spare parts are
available for purchase.  A long time ago, I broke a Wi-Fi antenna connector on
an Asus laptop and had difficulty finding replacement parts, not only because
of the lack of an Asus official store for spare parts, but even third-party
sources like AliExpress did not sell the same antenna as the one I broke.  The
health of my HP Envy x360's battery has degraded, but the official HP Parts
Store does not sell replacement batteries for my model; as of writing, I can
only find M.2 SSDs and Wi-Fi modules for my model on the HP
Store[^parts-hp-ay0000], which is not particularly helpful because these are
just generic components that I can buy elsewhere.  The Wi-Fi antenna and the
battery on these laptops are technically easy to replace, but it is infeasible
to perform even the easiest part replacement operation if the replacement part
is unavailable.  With Framework Marketplace, I am assured that I can get
genuine replacement Wi-Fi antenna and battery when I need them.

To summarize, laptops that are technically easier to repair than Framework
Laptops may exist, but the availability of reliable spare parts is a
prerequisite of many repairs and is thus also important.  Framework is
committed to making all spare parts available to customers, which is something
that other laptop manufacturers rarely do.  This, in my opinion, is the best
value that Framework offers with its products.

If I could have one aspect of Framework Laptop 13 design changed to further
improve repairability, then it would be a more robust battery connector as
mentioned above since disconnecting the battery before repairs is important for
safety.  A connector like the one on Framework Laptop 16[^frmw-16-battery] (as
shown below) would be great because the connector features thicker blades and
is thus more robust.  I do not know what trade-off Framework had to make to
favor the fragile connector on Framework Laptop 13; it could be because it
would allow for a higher battery capacity, or it was the only solution
available on the market when the first generation of Framework Laptop 13 was
launched.

![Framework Laptop 16's battery connector](https://d3t0tbmlie281e.cloudfront.net/igi/framework/4CsjVHfRoYtZjd2W.large)

[^frmw-about]: <https://frame.work/about>
[^service-guide-hp-ay0000]: <https://web.archive.org/web/20220430145037/http://h10032.www1.hp.com/ctg/Manual/c06618429.pdf>
[^service-guide-dell-xps-9570]: <https://dl.dell.com/content/manual77530376-xps-15-service-manual.pdf?language=en-us>
[^frmw-battery-connector]: <https://frame.work/battery-connector>
[^frmw-check-battery-pins]: <https://guides.frame.work/Guide/Battery+Replacement+Guide/85#s1533>
[^parts-hp-ay0000]: <https://parts.hp.com/hpparts/Search_Results.aspx?SearchIn=Product&SearchType=A&searchvalue=A&ProductName=17J58PA&ProductDescription=HP+ENVY+x360+Convertible+13-ay0078AU&SearchCriteria=17J58PA&CustNum=>
[^frmw-16-battery]: <https://frame.work/products/16-battery>
