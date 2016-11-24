OP-TEE Build Makefile targets
=============================

all 
---

* prepare 
  
  + @mkdir -p $(ROOT)/out

* arm-tf 
  
  + optee-os 
    - optee-os-common
  + edk2
    - edk2-common

* linux 

  + linux-common

* boot-img

  + Downloading Debian HiKey boot image ...

* lloader 

  + arm-tf
  + $(MAKE) -C $(LLOADER_PATH) BL1=? CROSS_COMPILE=? PTABLE_LST=linux-4g

* system-img 

  + prepare
  + Downloading Debian root fs (730MB) ...

* nvme 

  + prepare
  + wget $(NVME_IMG_URL) -O $(NVME_IMG)

* deb

  + xtest 
    - xtest-common
  + helloworld 
    - helloworld-common
  + optee-client
    - optee-client-common

send
----

* IP=111.222.333.444 make send

  + czf ... linux-image-*.deb | ssh linaro@$(IP) "cd /tmp; tar xvzf -"
  + dpkg --force-all -i

recovery
--------

* sudo python $(ROOT)/burn-boot/hisi-idt.py --img1=$(LLOADER_PATH)/l-loader.bin

flash
-----

* fastboot flash 

  + ptable 
  + fastboot
  + nvme
  + boot
  + system

flash-fip
---------

* fastboot flash 

  + fastboot

flash-boot-img
--------------

* boot-img
* fastboot flash 

  + boot

flash-system-img
----------------

* system-img
* fastboot flash 

  + system

aes-perf
--------

* optee-os 
* optee-client

sha-perf
--------

* optee-os 
* optee-client

clean
-----
 
arm-tf-clean edk2-clean linux-clean optee-os-clean optee-client-clean xtest-clean helloworld-clean boot-img-clean lloader-clean aes-perf-clean sha-perf-clean



cleaner
-------

clean prepare-cleaner linux-cleaner nvme-cleaner system-img-cleaner


help
----

1. WiFi on HiKey debian

Open /etc/network/interfaces and add:

::

  allow-hotplug wlan0
  iface wlan0 inet dhcp
  wpa-ssid "my-ssid"
  wpa-psk "my-wifi-password"

Reboot and you should have WiFi access
