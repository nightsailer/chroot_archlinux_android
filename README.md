# chroot_archlinux_android
Install chroot Archlinux on Android

# Requirment

1. rooted android tablet (RK3066)
2. busybox installed (it should installed when rooted)
3. Install Android tools for mac

# Prepare arm version archlinux image

1. Download general/multiplatform version from http://archlinuxarm.org/developers/downloads
  $ wget http://archlinuxarm.org/os/ArchLinuxARM-armv7-latest.tar.gz

2. Push package to tablet

  # Note: my adb can't work with usb cable, so I prepare TCP connection first.
  $ adb kill server
  $ adb tcpip 5555
  $ adb connect 192.168.1.3
  # unplug usb cable
  $ adb push ArchLinuxARM-armv7-latest.tar.gz /sdcard/
  $ adb shell

3. Extract linux package
  # note, we must extract into /data, because the /sdcard fs type is vfat, I don't want re-format it.
  $ cd /data
  $ mkdir archlinux
  $ cd archlinux
  $ busybox tar zxvf /sdcard/ArchLinuxARM-armv7-latest.tar.gz

4. Prepare chroot enviroment
  
  $ mount -o bind /dev/ /data/archlinux/dev/
  $ mount -t proc proc  /data/archlinux/proc/
  $ mount -t sysfs sysfs /data/archlinux/sys/
  $ mount -t devpts devpts /data/archlinux/dev/pts/

5. Chroot!
  $ busybox chroot /data/archlinux/ /bin/bash
