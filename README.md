Patches to the Realtek 8188/8192 USB WIFI drivers to build on Linux 3.8 and BeagleBone.

Original drivers are from http://www.realtek.com.tw/downloads/

### Building 

#### On BBB :

 Get Kernel header :
```sh

sudo apt-get install linux-headers-`uname -r`

``` 

 Update Makefile with your favorite editor (here Nano as example) :

```sh
nano Makefile
```

Choose : 


```sh
...
CONFIG_PLATFORM_BEAGLEBONE = y
CONFIG_PLATFORM_BEAGLEBONE_CROSS = n
...
```
To exit Nano, use Ctrl+x . It will ask to if you want to save. Choose Yes.

#### With Cross-compilation :

 Update Makefile with your favorite editor


Choose : 


```sh
...
CONFIG_PLATFORM_BEAGLEBONE = n
CONFIG_PLATFORM_BEAGLEBONE_CROSS = y
...
```

At line 278 : Modify KSRC with the path to the BB-kernel 

```sh
ifeq ($(CONFIG_PLATFORM_BEAGLEBONE_CROSS), y)
EXTRA_CFLAGS += -DCONFIG_LITTLE_ENDIAN
ARCH=arm
CROSS_COMPILE := arm-linux-gnueabi-
KVER  := $(shell uname -r)
KSRC ?= /Path/to/BBB_KERNEL/bb-kernel/KERNEL
MODDESTDIR := /lib/modules/$(KVER)/kernel/drivers/net/wireless/
INSTALL_PREFIX :=
endif
...
```

#### Testing

The driver is built by running `make`, and can be tested on the BBB by loading the
built module using `insmod`:

```sh
$ make
$ sudo insmod 8812au.ko
```

After loading the module, a wireless network interface named __Realtek 802.11n WLAN Adapter__ should be available.

### Installing

```sh
$ sudo make install
$ sudo depmod
```

The driver module should now be loaded automatically.
