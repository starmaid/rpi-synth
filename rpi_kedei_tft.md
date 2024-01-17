# Raspberry Pi Kedei TFT display

written 2024/01/14

3.5" SPI display. I have hardware v6.2

## From manufacturer

Unhelpful.

[Screen info page](https://osoyoo.com/category/osoyoo-raspi-kit/screen-raspberry-pi/)

[full kernel build how to](https://osoyoo.com/2016/05/26/osoyoo-lcd-touch-screen-for-raspberry-pi-installation-guide/)

[drivers how to](https://osoyoo.com/2016/09/13/install-raspberry-pi-3-5-touch-screen-driver-for-raspbian-jessie/)

[Link to drivers v6.1.3 (latest?) i also have a download of this](https://drive.google.com/drive/folders/1B4RzLAjtJUNOKBrYfgRgPbdloLtNbPCC)

sftp transfer the file...

sftp keys@keys2.local <<< 'put LCD_show_v6_1_3.tar.gz'

tar xzvf LCD*.tar.gz

./LCD_show_v6_1_3/LCD35_v

[link to built kernels](https://drive.google.com/drive/folders/1PP_jwPz2s5WfnCRTAvWt0RBmE4HuU88p)

## Open source

[raspberry pi forums thread](https://forums.raspberrypi.com/viewtopic.php?t=124961&sid=6674a1aa9ba2dc26a0f458453c1d9408&start=200)

[updated firmware for hw v6.3 but probably wont work on mine](https://github.com/kpishere/fbcp-ili9341)

[kernel version for hw v6.2](https://github.com/lzto/FBTFT_KeDei_35_lcd_v62/tree/master)

[someones test program for hw v6.2](https://github.com/lzto/RaspberryPi_KeDei_35_lcd_v62)

[official FBTFT drivers - as of today still not added](https://github.com/torvalds/linux/tree/master/drivers/staging/fbtft)

[adjusting FBTFT settings for rpi](https://www.circuitbasics.com/raspberry-pi-touchscreen-calibration-screen-rotation/)

## Installing 

```bash
# Install kernel header

sudo apt install raspberrypi-kernel-headers

# Clone modified stuff

git clone https://github.com/lzto/FBTFT_KeDei_35_lcd_v62.git

cd FBTFT_KeDei_35_lcd_v62/

# Compile and Install Kernel Module

make

sudo mkdir /lib/modules/`uname -r`/kernel/misc

cp /home/"$USER"/FBTFT_KeDei_35_lcd_v62/*.ko /lib/modules/`uname -r`/kernel/misc && sudo depmod -a


# Install dtb overlay

./install_dtb.sh
```

/home/keys/FBTFT_KeDei_35_lcd_v62/fbtft.h:315:12: error: initialization of ‘void (*)(struct spi_device *)’ from incompatible pointer type ‘int (*)(struct spi_device *)’ [-Werror=incompatible-pointer-types]
  315 |  .remove = fbtft_driver_remove_spi,                                 \
      |            ^~~~~~~~~~~~~~~~~~~~~~~

Linux keys2 6.1.21-v7+ #1642 SMP Mon Apr  3 17:20:52 BST 2023 armv7l GNU/Linux
gcc (Raspbian 10.2.1-6+rpi1) 10.2.1 20210110

-Werror=incompatible-pointer-types

make CXXFLAGS="-Wno-error=incompatible-pointer-types"

CC = gcc -Werror -Wno-error=incompatible-pointer-types



make -C /lib/modules/`uname -r`/build M=$PWD modules
make[1]: Entering directory '/usr/src/linux-headers-6.1.21-v7+'
  CC [M]  /home/keys/FBTFT_KeDei_35_lcd_v62/fbtft-core.o
  CC [M]  /home/keys/FBTFT_KeDei_35_lcd_v62/fbtft-sysfs.o
  CC [M]  /home/keys/FBTFT_KeDei_35_lcd_v62/fbtft-bus.o
  CC [M]  /home/keys/FBTFT_KeDei_35_lcd_v62/fbtft-io.o
  LD [M]  /home/keys/FBTFT_KeDei_35_lcd_v62/fbtft.o
  CC [M]  /home/keys/FBTFT_KeDei_35_lcd_v62/fb_kedei62.o
/home/keys/FBTFT_KeDei_35_lcd_v62/fb_kedei62.c: In function ‘write_vmem’:
/home/keys/FBTFT_KeDei_35_lcd_v62/fb_kedei62.c:92:5: warning: ISO C90 forbids mixed declarations and code [-Wdeclaration-after-statement]
   92 |     int i;
      |     ^~~
In file included from /home/keys/FBTFT_KeDei_35_lcd_v62/fb_kedei62.c:13:
/home/keys/FBTFT_KeDei_35_lcd_v62/fb_kedei62.c: At top level:
/home/keys/FBTFT_KeDei_35_lcd_v62/fbtft.h:315:12: error: initialization of ‘void (*)(struct spi_device *)’ from incompatible pointer type ‘int (*)(struct spi_device *)’ [-Werror=incompatible-pointer-types]
  315 |  .remove = fbtft_driver_remove_spi,                                 \
      |            ^~~~~~~~~~~~~~~~~~~~~~~
/home/keys/FBTFT_KeDei_35_lcd_v62/fb_kedei62.c:205:1: note: in expansion of macro ‘FBTFT_REGISTER_DRIVER’
  205 | FBTFT_REGISTER_DRIVER(DRVNAME, "kedei62", &display);
      | ^~~~~~~~~~~~~~~~~~~~~
/home/keys/FBTFT_KeDei_35_lcd_v62/fbtft.h:315:12: note: (near initialization for ‘fbtft_driver_spi_driver.remove’)
  315 |  .remove = fbtft_driver_remove_spi,                                 \
      |            ^~~~~~~~~~~~~~~~~~~~~~~
/home/keys/FBTFT_KeDei_35_lcd_v62/fb_kedei62.c:205:1: note: in expansion of macro ‘FBTFT_REGISTER_DRIVER’
  205 | FBTFT_REGISTER_DRIVER(DRVNAME, "kedei62", &display);
      | ^~~~~~~~~~~~~~~~~~~~~
cc1: some warnings being treated as errors
make[2]: *** [scripts/Makefile.build:250: /home/keys/FBTFT_KeDei_35_lcd_v62/fb_kedei62.o] Error 1
make[1]: *** [Makefile:2012: /home/keys/FBTFT_KeDei_35_lcd_v62] Error 2
make[1]: Leaving directory '/usr/src/linux-headers-6.1.21-v7+'
make: *** [Makefile:54: default] Error 2






/home/keys/FBTFT_KeDei_35_lcd_v62/fbtft.h:286:9: error: ‘return’ with a value, in function returning void [-Werror=return-type]
  286 |  return fbtft_remove_common(&spi->dev, info);                       \
      |         ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


### LINE 282
static void fbtft_driver_remove_spi(struct spi_device *spi)                 \
{                                                                          \
	struct fb_info *info = spi_get_drvdata(spi);                       \
									   \
	return fbtft_remove_common(&spi->dev, info);                       \
}                                                                          \



/home/keys/FBTFT_KeDei_35_lcd_v62/fbtft_device.c:1093:11: error: implicit declaration of function ‘spi_busnum_to_master’ [-Werror=implicit-function-declaration]
 1093 |  master = spi_busnum_to_master(spi->bus_num);
      |           ^~~~~~~~~~~~~~~~~~~~


https://forums.raspberrypi.com/viewtopic.php?t=352916



old buster from archive website
Linux keys2 4.19.97-v7+ #1294 SMP Thu Jan 30 13:15:58 GMT 2020 armv7l GNU/Linux


fresh from installer 2023/12
Linux keys2 6.1.21-v8+ #1642 SMP PREEMPT Mon Apr  3 17:24:16 BST 2023 aarch64 GNU/Linux
