# NomadBSD Handbooklet

## Content
1. [Intro](#intro)
1. [Installation](#installation)
	1. [Choosing a USB flash drive](#choosemedia)
	1. [Downloading and writing the image](#download)
1. [Overview](#overview)
1. [Key bindings](#keybindings)
1. [Enable/Disable desktop components, and auto-start programs](#autostart)
1. [Adding applications to the *plank* panel](#plank)
1. [Filesystem](#filesystems)
	1. [Automount](#automount)
	1. [Extending filesystem support](#fssupport)
		1. [exFat](#exfat)
		1. [BTRFS, ReiserFS, XFS](#linuxfs)
1. [Networking](#networking)
	1. [Wireless Networking](#wifi)
1. [Multihead setup](#multihead)
1. [Advanced Topics](#advanced)
	1. [Resetting NomadBSD](#reset)
		1. [Limitations](#reset_limits)
	1. [Disabling the graphics driver menu](#disablegfxmenu)
	1. [Disabling automatic graphics driver setup](#disableinitgfx)
	1. [Installing NomadBSD on a hard disk](#hddinstall)
	1. [Running NomadBSD in VirtualBox<sup>™</sup>](#vbox)
1. [Troubleshooting](#troubleshooting)
	1. [Errata](#errata)
	1. [Graphics](#ts_graphics)
		1. [ATI/AMD](#ts_ati_amd)
		1. [NVIDIA](#ts_nvidia)

<a name="intro"></a>
## Intro

NomadBSD is a 64bit live system for USB flash drives, based on
FreeBSD<sup>®</sup>. Together with automatic hardware detection and setup, it
is configured to be used as a desktop system that works out of the box, but
can also be used for data recovery, for educational purposes, or to test
FreeBSD<sup>®</sup>'s hardware compatibility.

<a name="installation"></a>
## Installation

### Choosing a USB flash drive
NomadBSD performs well on USB 2.X flash drives, but writing many small files
can be very slow. To improve performance, you should consider using a USB 3.X
flash drive even on a USB 2.X port, as they tend to be faster. See
[USB 3.0 Flash Drive Roundup](https://www.anandtech.com/show/4523/usb-30-flash-drive-roundup/6). Do not use cheap no-name thumb drives they sell at super markets
and drug stores. These drives are very slow and unreliable.

### Downloading and writing the image
Instructions for writing the image to a flash drive from different operating
systems can be found [here](http://nomadbsd.org/download.html).

<a name="overview"></a>
## Overview
![](images/nomadbsd-overview.png)

1. [Openbox](http://freshports/x11-wm/openbox) menu. You can reach it by
	pressing the Windows<sup>®</sup> key (or Super key)/⌘ key
	(Mac<sup>®</sup>), or by right-clicking on the background image
	(root window).
2. [DSBBatmon](http://freshports.org/sysutils/dsbbatmon). By hovering over
	the icon you can see the battery's current status and charge. Clicking
	on it brings up the configuration menu.
3. [DSBMixer](http://freshports.org/audio/dsbmixer). By hovering over the
	icon you can see the current volume of the master
	channel. Using the mouse wheel on it lets you change the
	master volume. Clicking on it brings up the main window of
	[DSBMixer](http://freshports.org/audio/dsbmixer).
4. [DSBMC](http://freshports.org/sysutils/dsbmc). Clicking on the icon brings
	up the main window in which you	can see all the mountable storage devices
	attached to the system. Use	the context menu of the device icons to select
	an action (un/mounting,	opening, playing, ejecting) or double click to
	mount and open the device in your default file manager. You can use the
	preferences menu to change the file manager, autoplay setting, and
	multimedia programs.
5. Date and time. Clicking in that area brings up a calendar.

<a name="keybindings"></a>
## Key bindings
`<Alt>+<F2>`

* Open DSBExec to execute a command.

`<Ctrl>+<Alt>+L`

* Lock the screen.

`<Ctrl>+<Space>`

* Open [dmenu-run](https://www.freshports.org/x11/dmenu/) to execute a
command.

`<Print>`

* Open XFCE 4 screenshooter.

<a name="autostart"></a>
## Enable/Disable desktop components, and auto-start programs
The program [DSBAutostart](http://freshports.org/sysutils/dsbautostart)
([Openbox menu](#overview) -> *Settings* -> *DSBAutostart*) allows you to control which
programs are automatically executed when the graphical interface starts.
Further, it allows you to enable/disable some components of the NomadBSD
desktop. The changes take place after logging out and in again.

![](images/dsbautostart-screenshot.png)
<a name="plank"></a>
## Adding applications to the *plank* panel
Open your prefered graphical file manager, and navigate to `/usr/local/share/applications`.
You can also get there by clicking the shortcut *Applications* on the side pane.
Use Drag&Drop to add application icons to the *plank* panel.

<a name="filesystems"></a>
## Filesystems
NomadBSD comes with a bunch of pre-installed filesystems (CD9660, FAT, HFS+,
NTFS, Ext2/3/4). You can mount storage devices via
[DSBMC](http://freshports.org/sysutils/dsbmc) (see [Overview](#overview)),
which is a graphical client for [DSBMD](http://freshports.org/sysutils/dsbmd).
<a name="automount"></a>
### Automount
Execute the command `dsbmc-cli -a` to automount all currently connected
storage devices, and to enable automounting on devices attached later to the
system. To start this command automatically on session start, open
[DSBAutostart](#autostart), and add a new entry for the above command.

<a name="fssupport"></a>
### Extending filesystem support
The following subsections describe how to extend the filesystems support.
Rebooting the system, or restarting
[DSBMD](http://freshports.org/sysutils/dsbmd) is not necessary.

<a name="exfat"></a>
#### ExFat
Unfortunately,
[sysutils/fusefs-exfat](https://freshports.org/sysutils/fusefs-exfat)
requires a license from Microsoft<sup>®</sup>, and so it can't be
pre-installed. You have to build it yourself by using the ports or the
[Git repo](https://github.com/relan/exfat.git):

	# pkg install autoconf
	# pkg install automake
	# git clone https://github.com/relan/exfat.git
	# cd exfat
	# autoreconf --install
	# ./configure
	# make && make install

<a name="linuxfs"></a>
#### BTRFS, ReiserFS, XFS
Install the package
[fusefs-lkl](https://www.freshports.org/sysutils/fusefs-lkl) for BTRFS,
ReiserFS, and XFS support.

	# pkg install fusefs-lkl

<a name="networking"></a>
## Networking
<a name="wifi"></a>
### Wireless Networking
The program [wifimgr](http://freshports.org/net-mgmt/wifimgr)
([Openbox menu](#overview) -> *Network* -> *WiFi Networks Manager*) allows
you to connect to a wireless network.

<a name="multihead"></a>
## Multihead setup
By default, NomadBSD enables all connected outputs (monitors). The tool
[ArandR](http://freshports.org/x11/arandr)
([Openbox menu](#overview)-> *Settings* -> *ArandR*) allows you to configure
the position, resolution, etc. of your monitors. Save your changes to
`~/.screenlayout/default.sh` which is automatically executed on session start.

<a name="advanced"></a>
## Advanced Topics

<a name="reset"></a>
### Resetting NomadBSD

If you are a tester, or your experiments with the systems left a total mess,
you might want to reset NomadBSD.

- - -

**Warning:** The reset will delete `/home`, `/private`, `/etc`,
`/var`, `/root`, and `/usr.local.etc`. Make a backup if there are any files
you want to keep.

- - -

You can reset NomadBSD as follows:

1. Boot into single-user mode by (re)booting and choosing `2` in the boot
menu.
2. Execute `/usr/libexec/nomadbsd-reset`

After rebooting you'll be greeted by the setup again.

<a name="reset_limits"></a>
#### Limitations
If you have modified or deleted system files from directory trees other than
`/home`, `/private`, `/etc`, `/var`, `/root`, `/tmp`, and `/usr.local.etc`,
you might not be able to cleanly reset NomadBSD.

<a name="disablegfxmenu"></a>
### Disabling the graphics driver menu
If you want to disable the graphics driver menu, add

`initgfx_menu="NO"` to `/etc/rc.conf`.

By default, `initgfx` will try autodetection, but you can instead define a
default driver to use by setting `initgfx_default` to `"scfb"` or `"vesa"` in
`/etc/rc.conf`.

<a name="disableinitgfx"></a>
### Disabling automatic graphics driver setup
If you want to create your own graphics driver settings, you can disable
`initgfx` by adding

`initgfx_enable="NO"` to `/etc/rc.conf`.

<a name="hddinstall"></a>
### Installing NomadBSD on a hard disk
Start [Openbox menu](#overview) -> *System* -> *NomadBSD Installer* and
follow the instructions.

![](images/installer-screenshot.png)

<a name="vbox"></a>
### Running NomadBSD in Virtualbox<sup>™</sup>
1. [Download and extract](http://nomadbsd.org/download.html) an image you intend to run.
2. NomadBSD will use the remaining space on a USB flash drive for its `/home`
partitition, but since we intend to run it from an image file, we increase the
(potential) size of the image as follows:
`truncate -s +4G nomadbsd-x.y.z.img`. If you need more or less extra space, change the
`-s` parameter accordingly.
3. Create a vmdk file: `VBoxManage internalcommands createrawvmdk -filename ~/nomadbsd.vmdk -rawdisk /full/path/to/nomadbsd-x.y.z.img`
4. Start VirtualBox<sup>™</sup>, and create a new virtual machine. Select
*Use an existing virtual hard disk file* in the *Hard disk* settings, and
choose *nomadbsd.vmdk* which we created in 3.
![](images/create-vbox-machine.png)

5. Go to *Settings* -> *Display* and set the video memory to 128MB or more.
6. Go to *Settings* -> *System* -> *Processor* and set the number of
processors to 2.

<a name="troubleshooting"></a>
## Troubleshooting
<a name="errata"></a>
### Errata
If you experience any problems, consult the
[NomadBSD Errata](http://nomadbsd.org/download/errata-1.2.txt) first.
<a name="ts_graphics"></a>
### Graphics
<a name="ts_ati_amd"></a>
#### ATI/AMD
If you are booting a system with ATI/AMD graphics via UEFI, you might
experience some problems. Due to
[a conflict with the EFI framebuffer](https://wiki.freebsd.org/Graphics#AMD_Graphics),
NomadBSD might crash or hang when the graphics driver gets loaded, or it just
isn't able to start the X window system.

Try the following workaround:

1. (Re)boot and enter the boot submenu `Boot Options` (`6`).
2. Change `Disable syscons` to `On` by pressing the key matching the item
number.
3. Go back to main menu, and press `<Enter>` to boot.

- - -

**Note:** You won't see any boot messages until the graphics driver gets loaded.

- - -

<a name="ts_nvidia"></a>
#### NVIDIA
If you see an error message like `device_attach: nvidia0 attach returned 6`
you could try to add `debug.acpi.disabled="sysres"` to `/boot/loader.conf`.

