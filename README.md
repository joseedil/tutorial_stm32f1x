# tutorial_stm32f1x
All instructions are for Debian 10 (Buster). Should work with some adaptation on other systems.

# Tools

## Stlink
The open source version of the STMicroelectronics stlink tools are currently unmaintained, but still quite useful. OpenOCD should suffice to flash programs and debug.

First, install dependencies.

```
sudo apt-get install build-essential cmake libusb-1.0-0 libusb-1.0-0-dev
```

Clone repository and compile:

```
git clone https://github.com/texane/stlink.git
cd Stlink
make release
cd build/Release; sudo make isntall
sudo ldconfig
```

You need to copy the udev rules:

```
sudo cp ../../etc/udev/rules.d/* /etc/udev/rules.d
sudo udevadm control --reload-rules
udevadm trigger
```

Finally, you need to add your user to the `stlink` group:

```
sudo groupadd stlink
sudo useradd <user> stlink
```

## OpenOCD
OpenOCD is a debugger tool.

```
git clone https://github.com/ntfreak/openocd.git
cd openocd
git submodule init
git submodule update
sudo apt-get install libtool pkg-config texinfo libusb-dev libusb-1.0.0-dev libftdi-dev autoconf
autoreconf -i
./configure --prefix=/opt/openocd
```

Compile and install:

```
make -j 8
sudo make install
sudo ln -s /opt/openocd/bin/openocd /usr/local/bin/openocd 
```
