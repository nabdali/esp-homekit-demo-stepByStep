# esp-homekit-demo-stepByStep
Tutorial step by step esp homekit demo (tested on OSx 10.13.2)

Inspired by [esp-homekit-demo](https://github.com/maximkulkin/esp-homekit-demo)

1. Clone this repo https://github.com/maximkulkin/esp-homekit-demo
  ```shell
  git clone https://github.com/maximkulkin/esp-homekit-demo
  ```
2. Initialize and sync all submodules (recursively):
  ```shell
  git submodule update --init --recursive
  ```
3. Copy wifi.h.sample -> wifi.h and edit it with your WiFi SSID and password (used by esp).
4. Install [esp-open-sdk](https://github.com/pfalcon/esp-open-sdk) :
    - Install esp-open-sdk requirement tools
    versions advisable :
        - Homebrew 1.5.2
        - Python 2.7.10
        - Pip 9.0.1
    ```shell
    $ brew tap homebrew/dupes
    $ brew install binutils coreutils automake wget gawk libtool help2man gperf gnu-sed --with-default-names grep python
    $ export PATH="/usr/local/opt/gnu-sed/libexec/gnubin:$PATH"
    $ pip install esptool
    ```
    
    - In addition to the development tools MacOS needs a case-sensitive filesystem.
    You might need to create a virtual disk and build esp-open-sdk on it:
    ```bash
    $ sudo hdiutil create ~/Documents/case-sensitive.dmg -volname "case-sensitive" -size 10g -fs "Case-sensitive HFS+"
    $ sudo hdiutil mount ~/Documents/case-sensitive.dmg
    $ cd /Volumes/case-sensitive
    ```
    
    - Be sure to clone recursively in volumes mounted previously:
    ```shell
    $ git clone --recursive https://github.com/pfalcon/esp-open-sdk.git
    ```
    
    - build it with
    ```shell
    $ make toolchain esptool libhal STANDALONE=n
    ```
    
    - Then edit your PATH and add the generated toolchain bin directory (add it on your .bach_profile):
    ```shell
    $ export PATH="/Volumes/case-sensitive/esp-open-sdk/xtensa-lx106-elf/bin:$PATH"
    ```
    
5. Install [esp-open-rtos](https://github.com/SuperHouse/esp-open-rtos):

    - Clone recursively:    
    ```shell
    $ git clone --recursive https://github.com/Superhouse/esp-open-rtos.git
    ```
    
    - And set SDK_PATH environment variable pointing to it (add it on your .bach_profile):  
    ```shell
    $ export SDK_PATH="/TO/FOLDER/esp-open-rtos/"
    ```
6. Test build:
    ```shell
    $ make -C examples/led all
    ```
    If you have any problems with esptool.py file, you must review installation and version of python, pip, esptool and serial.
    If the problems persist, install serial (python librarie)
    ```shell
    $ pip install serial
    ```
7. You’ll need to install a driver for the USB -> UART adapter:
    - Download and install this [driver](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)
    - Connect the ESP and you should be able to see the /dev/tty.SLAB_USBtoUART/ device on your machine
    ```shell 
    $ ls -l /dev/tty.SLAB_USBtoUART/
    ```
    result `crw-rw-rw- 1 root wheel 18, 4 Jul 13 19:46 /dev/tty.SLAB_USBtoUART`
    - You can also verify the kernel module is loaded via the kextstat command:
    ```shell 
    $ kextstat | grep -i silabs
    ```
    result `358 0 0xffffff7f83374000 0x6000 0x6000 com.silabs.driver.CP210xVCPDriver (4.10.11)`

    - Set ESPPORT environment variable pointing to USB device your ESP8266 is attachedto (assuming your device is at /dev/tty.SLAB_USBtoUART):
    ```shell
    $ export ESPPORT=/dev/tty.SLAB_USBtoUART
    ```
8. Upload firmware to ESP:
    ```shell
        make -C examples/led test
    ```
      or
    ```shell
        make -C examples/led flash
        make -C examples/led monitor
    ```
9. Pair your Iphone with ESP :
    - [Home Kit manuel](https://support.apple.com/en-us/HT204893)
  

  
