# pi-lirc
Steps for configuring raspberry-pi

```bash
apt update && apt upgrade -y
apt install lirc
cd /etc/lirc/lircd.conf.d/
wget -O lircd_celsius.conf https://github.com/mattjm/Fujitsu_IR/raw/master/lircd_celcius.conf  # for Fujitsu AC
```

Configuration for configuring pi at boot (kernel modules);
```
dtoverlay=gpio-ir,gpio_pin=18,gpio_pull=true
dtoverlay=gpio-ir-tx,gpio_pin=27
```

At this point, you can reboot to configure the GPIO pins.
The next part is to mofidy the `lirc_options.conf` file to use predictive device name and kernel.

```
driver          = devinput
#driver          = default
#device          = auto
device          = /dev/lirc0
```
In theory, shouldn't need to modify the device nor the driver, but I've found it might help.

The reason why this is theory only, is that we will use the following command ; 
```
irsend SEND_ONCE fujitsu_heat_ac heat-high-24.5C
```
which should be using the right device right away, but in the case it doesn't find the right 
transmitter GPIO, just find out which lirc device is it (`dmesg |grep -i ir`, rc0 is lirc0).

