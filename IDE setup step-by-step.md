# Arduino IDE setup

## Introduction
The purpose of this file is to explain how to setup the IDE(s) for both the evil crow cable and rubber ducky as there might be some differences in the complete setup.

### Setup for Windows

#### Basic requirements and initial installations
1. Download Arduino IDE: https://www.arduino.cc/en/Main/Software
2. Install the micronucleus drivers: https://raw.githubusercontent.com/digistump/DigistumpArduino/master/tools/micronucleus-2.0a4-win.zip
3. Extract the file you downloaded in step 2 to any folder.
4. Run install.exe in the folder you just extracted the zip to.
5. Download the update to the micronucleus: https://github.com/micronucleus/micronucleus
6. Extract the file you downloaded in step 5 to any folder.
7. Install Zadig from the following link https://zadig.akeo.ie/
8. Open Zadig and select the micronucleus.cfg from the folder you extracted in step 5. It can be found in the windows_driver_install folder.
9. Once the cfg file is selected, choose libusb-win32 (vx.x.x.x) version. This will most likely change the button from Install Driver to Install WCID Driver, you need to change it back to Install Driver and click that button
10. Download the Digispark USB Driver from the following link: https://sourceforge.net/projects/digistump/files/Digistump1.5Addons-v092.zip/download
11. Extract the zip file to Arduino/libraries.
12. Run Arduino IDE
13. Click File and then Preferences
14. Add the JSON URL to the Additional Boards Manager text box: https://raw.githubusercontent.com/digistump/arduino-boards-index/master/package_digistump_index.json
15. Click OK
16. Click Tools > Board > Board Manager
17. Search and install: Digistump AVR Boards
18. Restart Arduino IDE

#### Setup for Linux

> Note: I ran into issues finalizing this and never got it to work in the end, do with the information below what you want.

##### Basic requirements and initial installations
1. Download and Install Arduino IDE
2. Ensure you have the needed libraries, a Ubuntu example: ```sudo apt install libusb-dev && sudo apt install lib32stdc++6```
3. Download the following .ZIP. It contains the library files for DigisparkKeyboard:
    ```https://github.com/ernesto-xload/DigisparkKeyboard/archive/refs/heads/master.zip```
4. Import this file in the Arduino IDE: Sketch > Include Library >  Add .ZIP Library
5. Add the following link to the Additional Board Manager: ```https://raw.githubusercontent.com/digistump/arduino-boards-index/master/package_digistump_index.json```
6. Add them to the ABM: File > Preferences > Additional Board Manager URLs.
7. Clone the Micronucleus repository and compile the firmware
    ```
    git clone https://github.com/micronucleus/micronucleus.git
    cd micronucleus/firmware
    make
    ```
8. Add the correct UDEV rule to allow non-root users to write to a USB device
    ```
    sudo tee /etc/udev/rules.d/49-micronucleus.rules <<EOF
    # Micronucleus Bootloader
    SUBSYSTEM=="usb", ATTRS{idVendor}=="16d0", ATTRS{idProduct}=="0753", MODE="0666"
    EOF
    ```
9. Reload the UDEV rules
    ```
    sudo udevadm control --reload-rules
    sudo udevadm trigger
    ```