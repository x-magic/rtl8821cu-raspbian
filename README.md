# Realtek RTL8811CU/RTL8821CU USB wifi adapter driver version 5.4.1 for Linux 4.4.x up to 5.x

## -- Modified for Rasbian, tested with 5.4.51-v7+ kernel

## First, install dependencies
```
sudo apt install build-essentials bc git raspberrypi-kernel-headers linux-headers-rpi
```

## Then, clone this repository
```
cd ~
git clone https://github.com/x-magic/rtl8821cu-raspbian.git
```
## Build and install with DKMS

DKMS is a system which will automatically recompile and install a kernel module when a new kernel gets installed or updated. To make use of DKMS, install the dkms package.

```
sudo apt-get install dkms
```
To make use of the **DKMS** feature with this project, just run:
```
./dkms-install.sh
```
If you later on want to remove it, run:
```
./dkms-remove.sh
```

### Plug your USB-wifi-adapter into your PC
If wifi can be detected, congratulations.
If not, maybe you need to switch your device usb mode by the following steps in terminal:
1. find your usb-wifi-adapter device ID, like "0bda:1a2b", by type:
```
lsusb
```
2. switch the mode by type: (the device ID must be yours.)

Need install `usb_modeswitch`
```
sudo usb_modeswitch -KW -v 0bda -p 1a2b
systemctl start bluetooth.service - starting Bluetooth service if it's in inactive state
```

It should work.

### Make it permanent

If steps above worked fine and in order to avoid periodically having to make `usb_modeswitch` you can make it permanent (Working in **Ubuntu 18.04 LTS**):

1. Edit `usb_modeswitch` rules:

   ```bash
   sudo nano /lib/udev/rules.d/40-usb_modeswitch.rules
   ```

2. Append before the end line `LABEL="modeswitch_rules_end"` the following:

   ```
   # Realtek 8211CU Wifi AC USB (Tenda U9)
   ATTR{idVendor}=="0bda", ATTR{idProduct}=="1a2b", RUN+="usb_modeswitch '/%k'"
   ```   

## Check installed driver with **DKMS** (if installing via **DKMS**):

```
# sudo dkms status
rtl8821CU, 5.4.1, 5.4.51-v7+, armv7l: installed
```
### Monitor mode
Use the tool 'iw', please don't use other tools like 'airmon-ng'
```
iw dev wlan0 set monitor none
```
