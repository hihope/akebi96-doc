# BSP build

Build will be done under work/bsp

```sh
#download codes
mkdir work
cd work
git clone https://github.com/hihope/akebi96-bsp.git bsp

#build 
cd bsp/buildroot
make BR2_EXTERNAL=../br2_bsp_1_2 akebi96_defconfig
make
```
* Do not add "-j" optione. It will be optimised by "buildroot".
* Internet connection must be available on PC for using build. 
    * Packages needed to be build will be download automatically at the first build.
    * After the first time, these will be cached under "bsp/br2_bsp_1_2_local/dl" . 

* For buildroot, please refer following urls.
    * https://buildroot.org/
    * https://buildroot.org/downloads/manual/manual.html

---
# Android
All Android source code are not included because of file size.
Please get "android-9.0.0_r34" from AOSP, and replace differences.
* NOTE: The version of AOSP was changed from the last release. Do not use the last one.

## AOSP download

```sh
mkdir work/Android
cd work/Android
repo init -u https://android.googlesource.com/platform/manifest -b android-9.0.0_r34
repo sync
```

## akebi96 patch apply
```sh
mkdir work/temp
cd work/temp 
git clone https://github.com/hihope/akebi96-aosp.git

cd work/Android
cp -rfa ../temp/akebi96-aosp/* .
```

## Android build

```sh
cd work/Android
source ./build/envsetup.sh
lunch akebi96-userdebug
make -j 8
make_romimage.sh
```
* Lunch option is changed from "akebi96-userdebug" to "akebi96_atv-userdebug" for Android TV mode.
  Android ROM will be installed "work/bsp/buildroot/output/images" after these procedure.

---
# Burn ROM image
## Prepare USB stick

Copy all files under "work/bsp/buildroot/output/images" into "usb" folder in USB stick.

```sh
mkdir -p <Path/to/USB memory>/usb
cp -rfa work/bsp/buildroot/output/images/* <Path/to/USB memory>/usb
```

Next, preparation of target board.
Boot mode must be set as "BE mode"
* Set as "BE BOOT" on "SW2002".

Power on, and stop uboot prompt by any key.
After that, enter "run update_from_usb", then start to burn all rom images.

```
U-Boot 2019.01 (Feb 28 2019 - 14:31:04 +0900)

SoC:   LD20 (model 1, revision 2)
Model: Akebi96 Board
DRAM:  3 GiB
SC:    Micro Support Card (CPLD version 15.15)
NAND:  nand_base: timeout while waiting for chip to become ready
nand_base: No NAND device found
0 MiB
MMC:   sdhc@5a000000: 0
Loading Environment from MMC... OK
In:    serial@54006800
Out:   serial@54006800
Err:   serial@54006800
MODE:  eMMC Boot (STM: OFF)
Net:   
Warning: ethernet@65000000 (eth0) using random MAC address - 3a:57:2d:50:4e:26
eth0: ethernet@65000000
Hit any key to stop autoboot:  0 
=> 
=> run update_from_usb
5269 bytes read in 26 ms (197.3 KiB/s)
## Executing script at 8c000000
***************************************
*** set GPT partition ...           ***
***************************************
switch to partitions #0, OK
mmc0(part 0) is current device
Writing GPT: success!
***************************************
*** Writing to boot partition 0 ... ***
***************************************
switch to partitions #1, OK
mmc0(part 1) is current device
39705 bytes read in 31 ms (1.2 MiB/s)

MMC write: dev # 0, block # 0, count 256 ... 256 blocks written: OK
425472 bytes read in 23 ms (17.6 MiB/s)

MMC write: dev # 0, block # 256, count 2816 ... 2816 blocks written: OK
***************************************
*** Writing to normal area ...      ***
***************************************
switch to partitions #0, OK
mmc0(part 0) is current device
6610395 bytes read in 95 ms (66.4 MiB/s)

MMC write: dev # 0, block # 80, count 24576 ... 24576 blocks written: OK
23773 bytes read in 26 ms (892.6 KiB/s)

MMC write: dev # 0, block # 24656, count 64 ... 64 blocks written: OK
6899565 bytes read in 87 ms (75.6 MiB/s)

MMC write: dev # 0, block # 24720, count 131072 ... 131072 blocks written: OK
6612992 bytes read in 71 ms (88.8 MiB/s)

MMC write: dev # 0, block # 155792, count 16520 ... 16520 blocks written: OK
12673024 bytes read in 156 ms (77.5 MiB/s)

MMC write: dev # 0, block # 172312, count 40824 ... 40824 blocks written: OK
107151930 bytes read in 725 ms (140.9 MiB/s)
Uncompressed size: 268435456 = 0x10000000

MMC write: dev # 0, block # 213136, count 524288 ... 524288 blocks written: OK
126873443 bytes read in 1442 ms (83.9 MiB/s)
Uncompressed size: 268435456 = 0x10000000

MMC write: dev # 0, block # 737424, count 524288 ... 524288 blocks written: OK
93456661 bytes read in 1077 ms (82.8 MiB/s)
Uncompressed size: 268435456 = 0x10000000

MMC write: dev # 0, block # 1261712, count 524288 ... 524288 blocks written: OK
118157455 bytes read in 1355 ms (83.2 MiB/s)
Uncompressed size: 268435456 = 0x10000000

MMC write: dev # 0, block # 1786000, count 524288 ... 524288 blocks written: OK
42349274 bytes read in 309 ms (130.7 MiB/s)
Uncompressed size: 268435456 = 0x10000000

MMC write: dev # 0, block # 2310288, count 524288 ... 524288 blocks written: OK
260547 bytes read in 32 ms (7.8 MiB/s)
Uncompressed size: 268435456 = 0x10000000

MMC write: dev # 0, block # 2834576, count 524288 ... 524288 blocks written: OK
54509644 bytes read in 639 ms (81.4 MiB/s)

MMC Sparse write: dev # 0, block # 3358864 ... Flashing Sparse Image
........ wrote 54509568 bytes to '0x66812000'
1737068 bytes read in 42 ms (39.4 MiB/s)

MMC Sparse write: dev # 0, block # 3621008 ... Flashing Sparse Image
........ wrote 1736704 bytes to '0x6e812000'
319756 bytes read in 27 ms (11.3 MiB/s)

MMC Sparse write: dev # 0, block # 12286096 ... Flashing Sparse Image
........ wrote 319488 bytes to '0x176f12000'
***************************************
*** USB update is finished.         ***
***************************************
```

After this message, burning was finished.

Next step, reset the target board and initialize uboot parameters.

```
=> reset
U-Boot 2019.01 (Feb 28 2019 - 14:31:04 +0900)

SoC:   LD20 (model 1, revision 2)
Model: Akebi96 Board
DRAM:  3 GiB
SC:    Micro Support Card (CPLD version 15.15)
NAND:  nand_base: timeout while waiting for chip to become ready
nand_base: No NAND device found
0 MiB
MMC:   sdhc@5a000000: 0
Loading Environment from MMC... OK
In:    serial@54006800
Out:   serial@54006800
Err:   serial@54006800
MODE:  eMMC Boot (STM: OFF)
Net:   
Warning: ethernet@65000000 (eth0) using random MAC address - 3a:57:2d:50:4e:26
eth0: ethernet@65000000
Hit any key to stop autoboot:  0 
=> 
=> env default -a
## Resetting to default environment
=> saveenv
Saving Environment to MMC... Writing to MMC(0)... OK
```

After these operation, execute reset or power off -> on, then the board will be wake up by new ROM.
