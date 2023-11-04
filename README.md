# HCI-ESP32
Micropython C module to access the VHCI layer in the ESP 32.   
Creates a module HCI_ESP32   

```
>>> from HCI_ESP32 import *
>>> bt = HCI()
>>> bt.readable()
False
>>> bt.send_raw(b'\x01\x03\x0C\x00') 
True
>>> bt.readable()
True
>>> bt.receive_raw()
b'\x04\x0e\x04\x05\x03\x0c\x00'
>>> bt.send_raw(b'\x01\x01\x10\x00') 
True
>>> bt.receive_raw()
b'\x04\x0e\x0c\x05\x01\x10\x00\x08\x0e\x03\x08`\x00\x0e\x03'
```



# References   
Micropython instructions: https://github.com/micropython/micropython/blob/master/ports/esp32/README.md    
usbipd-win instructions: https://devblogs.microsoft.com/commandline/connecting-usb-devices-to-wsl/


# Build instructions (WSL on Windows)
Install USBIPD (provide access to ESP32 over USB into WSL) from  https://github.com/dorssel/usbipd-win/releases    
Presumes WSL already active on your Windows machine
 
***Windows***
```
cd "c:\Program Files\usbpid-win"
usbipd wsl list
usbipd wsl attach --busid <busid>

# Install Ubuntu in WSL

wsl --install Ubuntu
[provide username and password]
```

***WSL***
```
# Then you are in WSL

sudo bash
apt update
apt upgrade


# USB access for Windows
apt install linux-tools-virtual hwdata
update-alternatives --install /usr/local/bin/usbip usbip `ls /usr/lib/linux-tools/*/usbip | tail -n1` 20

# ESP IDF installation
apt install python3.8-venv
git clone -b v5.0.2 --recursive https://github.com/espressif/esp-idf.git
cd esp-idf

./install.sh esp32
source export.sh

# Micropython installation
cd ~
git clone https://github.com/micropython/micropython
cd micropython

apt install cmake

make -C mpy-cross

cd ports/esp32
make submodules
make

# Clone this repo into micropython codebase

cd ~
git clone https://github.com/paulhamsh/HCI-ESP32.git
cd HCI-ESP32

mkdir ~/micropython/userc
cp -r HCI-ESP32 ~/micropython/userc

cd ~/micropython/ports/esp32
make USER_C_MODULES=~/micropython/userc/HCI-ESP32/micropython.cmake


# Install to ESP32
make erase
make deploy
```

***Windows***
```
# Close USB access in Windows so can see ESP32
Windows:
usbipd wsl detach --busid <busid>
```

