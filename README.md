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

## STM32Cube
Maybe you need/want to use STM32Microelectronics tools. STM32Cube is a combination of software tools and embedded software libraries.

STMicroeletronics downloads are a pain, so if any of the links are broken, got to [their webpage](http://www.st.com) and look for the tools.

### Preliminaries
STMicroelectronics tools need Java 8 and don't work well with OpenJDK which is a problem on Debian 10 (Buster).

```
sudo apt-get install default-jdk openjfx
```

### STM32CubeMX
STM32CubeMX is a quite useful graphical tool that generates initialization code for the ARM family.

Go to [STM32CubeMX download page](https://www.st.com/en/development-tools/stm32cubemx.html) and download the installer.

Go to the dowload folder, unzip and run the installer.

```
cd <download>
unzip en.STM32CubeMX_v5-3-0.zip
./SetupSTM32CubeMX-5.3.0.linux
```

Just follow the install tool. The binary will be in the install folder.

Add the binaries folder to your PATH.

```
export PATH="$PATH:<install_folder>"
```

### STM32CubeProgrammer
STM32CubeProgrammer is a programmer tool which includes the stlink firmware update tool.

```
CAUTION!
Be aware that updating firmware of chinese stlink clones can brick you device (it happened to me and I still could not find a way to make it work again, even after dowgrading to the original firmware...)
```

Go to [STM32CubeProgrammer download page](https://www.st.com/en/development-tools/stm32cubeprog.html) and download the installer.

Go to the download folder, unzip and run the installer.

```
cd <download>
unzip en.stm32cubeprog.unzip
./SetupSTM32CubeProgrammer-2.1.0.linux
```

Follow the install tool. On Debian 10 Buster you will need to manually patch some files because they can't find the correct openJFX JVM.

First, find where openjfx is installed.

```
dpkg -S javafx.properties
```

On my system, it was in `/usr/share/openjfx/lib/`.

Now, edit openJFXScript.sh

```
cd <install_dir>
nano util/openJFXScript.csh
```

and comment the whole file to disable the openJFX check.

Then, edit STM32CubeProgrammer. This is just a wrapper that calls the application. We will add an enviroment variable that will instruct java where to locate the javafx modules and which ones to add.

```
nano bin/STM32CubeProgrammer
```

Insert the following line before the call to STM32CubeProgrammerLauncher, but be sure to use the correct module path for your system.

```
export JDK_JAVA_OPTIONS='--module-path /usr/share/openjfx/lib --add-modules=javafx.base,javafx.controls,javafx.fxml,``javafx.graphics``,``javafx.media``,javafx.swing,javafx.web'
```

Add the binaries folder to your PATH.

```
export PATH="$PATH:<install_folder>/bin"
```

### STM32CubeIDE
This is an IDE which integrates all STM32Cube functionalities in an Eclipse environment.

Download the installer on the [STM32CubeIDE download page](https://www.st.com/en/development-tools/stm32cubeide.html) and get the Debian installer version.

Go to the download folder, unzip and follow the install tool.

```
cd <download>
unzip en.st-stm32cubeide+1.0.2_3566_20190716_0927_amd64.deb_bundle.sh.zip
chmod +x st-stm32cubeide_1.0.2_3566_20190716_0927_amd64.deb_bundle.sh
sudo ./st-stm32cubeide_1.0.2_3566_20190716_0927_amd64.deb_bundle.sh
```

Press `q` to jump the license agreement text, then `y` to accept the terms. It should install the tool and exit.

In my system the tool was installed on `/opt/st/stm32cubeide` and a GNOME shortcut was automagically added.
