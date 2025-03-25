# Hardware analysis

# Rubber Ducky

## Introduction

A Rubber Ducky is a USB hardware hacking tool that looks like a regular USB stick but actually functions as an automated attack system. It is a powerful penetration testing tool developed by Hak5. It functions as a Human Interface Device (HID). HID refers to devices that enable interaction between humans and computers. Keyboards, mice, and game controllers are common examples. This tool is widely used in cybersecurity for ethical hacking, red teaming and security awareness training. It can also be used for malicious purposes if misused. 

How it is build  
Rubber Ducky is an appliance designed, manufactured and sold by Hak5, a company that manufactures a multitude of pentesting appliances. They are also the developers of Ducky Script, a structured scripting language that enables programmatically executing keystrokes as part of payloads or simply as part of macros.  

<img src="https://thumbs.img-sprzedajemy.pl/1000x901c/12/8e/14/usb-rubber-ducky-hak5-479670027.jpg" alt="Rubber Ducky" style="width: 300px;">
 
It includes a Type-A USB plug for connection and a 60 MHz 32-bit CPU to run scripts efficiently. Some models feature a replay button for quick re-execution of the last payload, and an LED indicator shows the device's activity. An optional decal can be added for branding purposes.

## Requirements

You will need the Rubber Ducky USB device, which serves as the hardware programmed to execute keystrokes. A micro-SD card is required to store the payload scripts, which must be written in Ducky Script. Since our USB device requires binary code, we will need encoder software to convert the Ducky Script into a binary file. Finally, a target system, such as a computer or any device with an available USB port, is necessary for execution.

## 

## Functionality 

The Rubber Ducky is a USB device that operates by injecting keystrokes into a target system, mimicking a real keyboard. Once inserted into an unsecured system, it executes its payload at incredible speeds, which can include commands to run shell processes, initiate tasks, or more. Because it behaves like a keyboard, it’s difficult to block once inserted, making it a potent tool for attacks. Payloads are programmed using Duck Script, a custom scripting framework originally developed for the Rubber Ducky but now used by other HID tools like Key Croc and O.MG. The device runs on the ATmega32u4 microcontroller and uses the USB HID standard for keystroke emulation. Payloads are compiled into machine code and executed by the firmware, which is open-source, allowing users to modify or extend its functionality for additional features like encryption or performance improvements. The Rubber Ducky works across various operating systems without needing extra drivers and is ideal for penetration testing or automating system tasks due to its plug-and-play functionality and speed.

## Risks

* **Unauthorized Access**: It can prevent unauthorized access by executing commands automatically.  
* **Data theft:** It can automatically steal data and send it to a remote server.  
* **Malware distribution:** It can install appropriate software on systems.  
* **Privilege Escalation:** It can increase control over a system even without passwords.  
* **Network Spread:** It can spread malware to other systems on a network.  
* **Security bypass:** It can bypass security measures such as USB restrictions or password prompts.

## 

## Mitigation

Rubber Duckies are extremely fast in their execution, and this necessitates a predefined set of limitations that have to be set in order to minimize or prevent an attack executed by an HID such as a Rubber Ducky. Ordered from Pervasive to not noticeable, here are a few measures a system administrator can take to harden their fleet of devices: 

1. Disabling all USB ports  
   The only foolproof way to completely eliminate HID-based attacks is to block its interface, either physically or digitally. The measure is, of course, extremely pervasive to users of affected devices, as most peripherals nowadays use USB to communicate with computers. While this could be a stop-gap measure to prevent HID-based attacks for a short-term period, for devices that normal users need to work with, this isn’t a long-term viable option.

2. Device whitelisting  
   One could filter peripherals with a whitelist set in on their network, be it via Windows domain policies or via system configuration, but this would add a lot of overhead for sysadmin teams to adapt these whitelists to all peripherals and keep these updated for their entire fleet.

3. Restricting regular User accounts  
   Typical users have no need for shells or any Shell-interaction program like Windows’ Run. Disabling access to those systems is a first easy step to prevent a rubber ducky from executing anything at all. This is not foolproof, however. In select enterprises, System Administrators aren’t the only ones with Admin rights (ex. C-Suite). This could leave an attack vector open for HIDs to compromise an entire network.    
Disabling shell-access is not foolproof, as the Rubber Ducky still has access to all the same resources as the user has, which could already involve files and/or important systems.

# 

# Evil Crow

## Introduction

The Evil Crow cable is a BadUSB device with an embedded microcontroller. Depending on the cable’s version, it has different possibilities and a different microcontroller. The differences will be summarized in a table under the functionality rubric.

How it is build

The Evil Crow Cable is an open source pentesting project, originally conceived by Joel Serna and Ernesto Sánchez, who first had their prototype working on a plug-in breadboard.  

![First prototype](https://github.com/joelsernamoreno/BadUSB-Cable/raw/master/images/badusb-cable-breadboard.jpg) 

Later, they started developing a PCB with help from open source contributors to incorporate all this in a USB cable enclosure.    

![Size comparison processor](https://github.com/joelsernamoreno/BadUSB-Cable/raw/master/images/first-pcbs-3.jpeg)
![Complete Evil Crow Cable without enclosure](https://github.com/joelsernamoreno/BadUSB-Cable/raw/master/images/evilcrowcable2.jpg)

It utilizes the Attiny85 processor. it is built into a simple PCB that allows it to communicate with a computer like an Arduino would. In the same manner, one can write payloads for it and upload it to the device. Popular software used for this purpose is the Arduino IDE and ESP IDF, as they already have the “push/upload” functionality built-in for their conventional use case.

## Requirements

There are some hardware, software, firmware and payload requirements.   
The need for an evil crow cable itself is clear, the other hardware requirements are a computer or mobile device to deploy the payload on, the device will need either a USB-A or USB-C port depending on the cable’s model, and we will need Wi-Fi or Bluetooth connectivity. Some other hardware options are a secondary device for testing and a power bank to deploy it more discreetly.

In terms of software, people will most likely use Arduino IDE or ESP-IDF, which is used to flash the firmware, one that can utilize different encoders to convert written payloads to binary. Sometimes a web browser is used for web-based control panels, and there is need for a serial monitor (like Putty) to debug and interact with the device, but Rubber Ducky and Evil Crow don’t necessitate this.

Some Evil Crow cables come with the correct firmware, others need to be flashed. The easiest way to know whether it’s necessary to flash the firmware is by going to the official GitHub repository for the correct cable version. A HID payload is necessary to give the cable the ability to do what you want it to do.

## Functionality

| Feature | Evil Crow Cable | Evil Crow Cable Pro | Evil Crow Cable Wind
|---|----|----|----|
| USB type | USB-A & USB-C   |  USB-A, USB-C, Lightning |  USB-A & USB-C  |    
| Microcontroller |  Attiny85  |  RP2040  |  ESP32-S3  |    
| HID (keyboard injection) |  Basic, but more stable then older cable versions  |  Most advanced  |  Faster & more reliable then regular cable  |    
| Wi-Fi Deauthing |  Yes  |  Stronger Antenna (so it's improved) |  Yes  |    
| Wi-Fi Keylogging |  Yes  |  More storage  |  Yes  |    
| Remote control (bluetooth) |  Yes  |  No (less detection chance)  |  No (microcontroller lacks BT functionality)  |    
| Encrypted Payloads |  No  |  Yes  |  No  |    
| Stealth level |  Better HID timing then older cable versions  |  Hard to detect  |  Harder to detect then regular cable due to no BT  |   

Functionalities for the regular Evil Crow Cable itself are the following, made possible by the firmware:

* HID Attacks (acts as a Rubber Ducky)  
* Bad USB Exploits:   
  * can disguise itself as a legitimate keyboard  
  * Supports scripts written in Ducky scripts 
* Wireless control using Bluetooth (more traceable)  
* Cross-Platform compatibility, you don’t need specific drivers, and it appears as a regular HID device on the target  
* Can execute custom payloads  
* There is no onboard storage

## Risks

* **Credential Theft and Data Exfiltration**: can be used to extract saved passwords, credentials, or sensitive files from a victim’s device  
* **Bypassing Security Measures**: antivirus and endpoint protection software often don’t detect HID attacks, making these cables a stealthy attack vector  
* **Indistinguishable from Regular USB Cables**: they look like ordinary charging or data cables, making it easy for someone to unknowingly use a malicious cable  
* **Remote Access and Control**: some versions of malicious cables allow an attacker to remotely execute commands or gain unauthorized access to a system  
* **Malware Distribution**: installs appropriate software on systems to distribute malware

## Mitigation

1. Disabling all USB ports  
   The only foolproof way to completely eliminate HID-based attacks is to block its interface, either physically or digitally. The measure is, of course, extremely pervasive to users of affected devices, as most peripherals nowadays use USB to communicate with computers. While this could be a stop-gap measure to prevent HID-based attacks for a short-term period, for devices that normal users need to work with, this isn’t a long-term viable option.

2. Device whitelisting  
   One could filter peripherals with a whitelist set in on their network, be it via Windows domain policies or via system configuration, but this would add a lot of overhead for sysadmin teams to adapt these whitelists to all peripherals and keep these updated for their entire fleet.

3. Restricting regular User accounts  
   Typical users have no need for shells or any Shell-interaction program like Window’s Run. Disabling access to those systems is a first easy step to prevent them from executing anything at all. This is not foolproof, however. In select enterprises, System Administrators aren’t the only ones with Admin rights (ex. C-Suite). This could leave an attack vector open for His to compromise an entire network.   
   Disabling shell-access is not foolproof for normal Users either, as the HID still has access to all the same resources as the user has, which could already involve vulnerable files and or important systems.

# USB-Protocol

Both tools we explained above function as a USB HID device, more precisely a keyboard.  
There were many standards of the USB cable spec over the years, most notable USB 2.0, 3.0, 3.1, and now with USB-C: 4.0. These cables grew in complexity over the years, allowing for greater and greater throughput of data. The schematics below describe how the cables evolved, mostly more lanes were added for greater data-throughput while maintaining backwards compatibility.

![USB A 2.0 schematic](https://threeboard.dev/documentation/images/hardware/usb/connector_type_a_small.png)

![Newer USB schematic](https://threeboard.dev/documentation/images/hardware/usb/connector_type_c_small.png)

Keyboards in detail aren’t terribly complex devices. Just as our subject matters above, they are simply shells with a microcontroller in them, only they detect actual physical key presses, compared to our subjects that get their “key presses” from the payload.   

![Enumeration Diagram](https://threeboard.dev/documentation/images/hardware/usb/enumeration_sequence.png)

When a USB device is first inserted, it first goes through a process called enumeration.   
First, the host system detects the version of the USB specification of the cable and its rated data speed. For example, between USB 1 and 2, the D+ lane is connected via a pull-up resistor to the Vbus, which runs at 5V. The speed of the connection is indicated by either D+ or D- being pulled up to 3.3V. This allows for cables to be made in the same way, just with cheaper components for low-speed variants, and allows the use of the same interfaces.  
Continuing on enumeration, this merely entails the device getting 7-bit addresses for every endpoint the device has on the USB bus. During this process that’s also visualized in the diagram to the left, it gets its addresses and is set to a certain mode, which will be further elaborated on below.

Most simple USB devices only support one mode of the 4 USB modes.   
Those would be: Control, Bulk, Interrupt and Isochronous. Every USB device has at least one “endpoint” in Control mode (endpoint 0). This is then supplemented by other endpoints that are needed for other functions. Typically, a keyboard will only have one other endpoint, an interrupt endpoint that conveys the key presses. The diagram below shows how the communication between the HID and the Host system works.

![Keypress sequence diagram](https://threeboard.dev/documentation/images/hardware/usb/keypress_sequence.png)

Note that the polling of such a device runs at a theoretical maximum rate of once per millisecond or at a frequency of 1000 Hertz (1ms frame limit), although many devices run at a far lower polling rate. If a device is of USB 2.0 spec or higher, it can support higher polling rates in “High-Speed”, up to 8000 Hertz (by splitting a frame into 125ns frames).   
Assuming the standard USB boot protocol, a USB frame contains 8-bytes of which the first is reserved for modifiers (Ctrl, Shift,...), the second is a designated reserved byte by the protocol. This leaves us with 6 bytes per frame, at a maximum of 8000 Hertz, we conclude that the theoretical maximum that our subject would be able to type is 48000 characters per second. Due to processing constraints on the subject devices this limit is of course not reached. Reportedly Rubber Duckies can reach typing speeds of up to 1000 per second.

# Sources:

*How do USB keyboards work?* (z.d.). Threeboard. https://threeboard.dev/documentation/threeboard/how\_usb\_keyboards\_work.html  
Ostapiuk, R. (z.d.). *USB Transfer Types \- Developer help*. https://developerhelp.microchip.com/xwiki/bin/view/applications/usb/how-it-works/transfer-types/  
Joelsernamoreno. (n.d.). *GitHub \- joelsernamoreno/BadUSB-Cable: BadUSB cable project*. GitHub. https://github.com/joelsernamoreno/BadUSB-Cable  
*Evil Crow cable by AprilBrother on Tindie*. (2021, January 1). Tindie. https://www.tindie.com/products/aprbrother/evil-crow-cable/  
*USB keystroke injection protection*. (n.d.). Google Open Source Blog. https://opensource.googleblog.com/2020/03/usb-keystroke-injection-protection.html  
OPSWAT, Inc. (2024, December 13). *The danger of a USB device and keystroke injection attack \- OPSWAT*. OPSWAT. https://www.opswat.com/blog/the-danger-of-a-usb-device-and-keystroke-injection-attack  
OpenAI. (2023). ChatGPT (version 14 March) \[Large language model\]. https://chat.openai.com/chat