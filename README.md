![OpenWrt logo](readme/addedsugar.png)

Sweetr74 is, itself, a OpenWRT fork from version ``v25.12.1``. For whatever this project is used for initially - self detached build for WDR7400 (TL), i
just hope everyone who came accross this fork repo don't have to do a hit-or-miss when making themself an unique version for their shit WDR74 Router and 
please; Do backup your SPI flash firmware (the actual data in the SPI flash chip ) for future debugging.

Good luck understand my README.md.

I recommend to get docker image of linux then build image on it.

## Why this sh#t appeared?

As the one among many TP-LINK owners (you and me), pretty sure you came across Google for third party firmware to flash on your un-supported/ obsoleted-base-firmware device sometimes(haha). Though rarely miracle shown upon, you can only get some chinese TP-L manuals or forum posts from DD-wrt or OpenWRT - supposed no one made the firmware yet or at least some shop links.

And if SSVIP miracle does happen, a Chinese guys could make a tutorial on how-to-make or even give their work for-download, and in this case there are two peoples who have given us instruction ( i mean 1 guy told that he use a custom version of WDR7800 so it should work in theory).

> Mind you, both how-to-make are based on very old (2019) repository of OpenWRT so you know whats mean.

Based WDR6500 Instruction (redyellow.com): [here](https://www.red-yellow.net/tl-wdr7400-v2%E5%88%B7openwrt%E7%B3%BB%E7%BB%9F)

Based WDR7800 Confirmation (right.com.cn): [here](https://www.right.com.cn/forum/thread-504335-1-1.html)

## ? I think i need to say something here.

Yes. basically, all you need is the ART (Atheros Radio Test) file, a Firmware (custom build OpenWRT, DD-wrt, Padavan, etc) and a image flash .bin like ``BREED``
   sorry if i misspoke some Firmware brands

And yea, any device with same SOC / SAME chipset can be used as BASE TEMPLATE, i mean BASE TEMPLATE, BASE TEMPLATE. Crucial things must be said three times.

And since both 6500 and 7800 have different ART callibration and read offset, we need to modify this.

## I Take this repo and use WDR6500 as base like Red-yellow has done.

For 7 years (2026) i write this in August, OpenWRT has changed alot so what to modify has also changed. 

From this: 2019 repo

```
target/linux/ar71xx/base-files/etc/diag.sh
target/linux/ar71xx/base-files/etc/hotplug.d/firmware/11-ath10k-caldata
target/linux/ar71xx/base-files/etc/board.d/01_leds
target/linux/ar71xx/base-files/etc/board.d/02_network
target/linux/ar71xx/base-files/lib/ar71xx.sh
target/linux/ar71xx/base-files/lib/upgrade/platform.sh
target/linux/ar71xx/image/generic-tp-link.mk
target/linux/ar71xx/generic/config-default
target/linux/ar71xx/config-4.14
target/linux/ar71xx/files/arch/mips/ath79/machtypes.h
target/linux/ar71xx/files/arch/mips/ath79/mach-tl-wdr6500-v2.c
target/linux/ar71xx/files/arch/mips/ath79/Kconfig.openwrt
target/linux/ar71xx/files/arch/mips/ath79/Makefile
```
To this: 2025.12 repo

```
target/linux/ath79/image/generic-tp-link.mk
target/linux/ath79/dts/qca9561_tplink_tl-wdr6500-v2.dts
target/linux/ath79/generic/base-files/etc/board.d/02_network
target/linux/ath79/generic/base-files/etc/board.d/01_leds
```

And so i will not rely on red-yellow on some part.

### Cook recipe:

>Please get a 16 megabytes ( 128 megabits) SPI flash chip for the doablity.

First:
* Carefully take the SPI chip out and use SPI programmer or ESP32 to backup the data.

** You can leave it on board, but it might lead to something onboard can be activated. but don't worry as long you wired it correctly.**

* Now use a HEX editor, make a 64KB .bin file filled with ``0xFF`` and save it ( a decoy file to store copied ART later ). ( from right.com.cn )
* Copy the last 64KB in the backup firmware.bin over to the decoy file, and now thedeocy file should be the ART callibration file. But, don't delete the backup yet.

Second:
* Now we put into SPI flashchip the BREED web firmware, and let the router and SPI rest aside.
* Fetch the firmware.bin from this repo in RELEASE page. Or ( OPTIONAL ) build custom firmware.bin from this source - everything set so just see the Dev Docs below and run ``make menuconfig`` then run ``make`` ( Wait for a WHILE ).
* Use BREED web to flash Firmware and ART.

Third and last:
* Enjoy!


### Quickstart

1. Run `./scripts/feeds update -a` to obtain all the latest package definitions
   defined in feeds.conf / feeds.conf.default

2. Run `./scripts/feeds install -a` to install symlinks for all obtained
   packages into package/feeds/

3. Run `make menuconfig` to select your preferred configuration for the
   toolchain, target system & firmware packages.

4. Run `make` to build your firmware. This will download all sources, build the
   cross-compile toolchain and then cross-compile the GNU/Linux kernel & all chosen
   applications for your target system.

### Related Repositories (too lazy to remove)

The main repository uses multiple sub-repositories to manage packages of
different categories. All packages are installed via the OpenWrt package
manager called `opkg`. If you're looking to develop the web interface or port
packages to OpenWrt, please find the fitting repository below.

* [LuCI Web Interface](https://github.com/openwrt/luci): Modern and modular
  interface to control the device via a web browser.

* [OpenWrt Packages](https://github.com/openwrt/packages): Community repository
  of ported packages.

* [OpenWrt Routing](https://github.com/openwrt/routing): Packages specifically
  focused on (mesh) routing.

* [OpenWrt Video](https://github.com/openwrt/video): Packages specifically
  focused on display servers and clients (Xorg and Wayland).

## Support Information

For a list of supported devices see the [OpenWrt Hardware Database](https://openwrt.org/supported_devices)

### Documentation

* [Quick Start Guide](https://openwrt.org/docs/guide-quick-start/start)
* [User Guide](https://openwrt.org/docs/guide-user/start)
* [Developer Documentation](https://openwrt.org/docs/guide-developer/start)
* [Technical Reference](https://openwrt.org/docs/techref/start)

### Support Community

* [Forum](https://forum.openwrt.org): For usage, projects, discussions and hardware advise.
* [Support Chat](https://webchat.oftc.net/#openwrt): Channel `#openwrt` on **oftc.net**.

### Developer Community

* [Bug Reports](https://bugs.openwrt.org): Report bugs in OpenWrt
* [Dev Mailing List](https://lists.openwrt.org/mailman/listinfo/openwrt-devel): Send patches
* [Dev Chat](https://webchat.oftc.net/#openwrt-devel): Channel `#openwrt-devel` on **oftc.net**.

## License

OpenWrt is licensed under GPL-2.0
