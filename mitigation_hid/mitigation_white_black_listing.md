# Implementing White/Blacklists

## 1: Methods

### 1.1: Based on PID/VID
Basic blocking of unwanted USB devices on Windows can be accomplished by creating a whitelist or blacklist within Group Policies using PIDs/VIDs.

Now one could ask themselves what these acronyms mean: Product ID and Vendor ID describe the exact product and the vendor/manufacturer, respectively.  
This allows us to use these values to check the validity of hardware and filter 
Below are described methods on how to get these IDs and how to use them to establish block lists.
- [Windows PID/VID Blocking](#21-pidvid-blocking)
- [Linux PID/VID Blocking](#31-pidvid-blocking)
- [macOS PID/VID Blocking](#41-pidvid-blocking)



## 2: Windows
>Only Windows Pro and Server by default support many of the methods written below, where a workaround for Windows Home was found during our research, it will be included with instructions.

### 2.1: PID/VID blocking
Below is a step-by-step guide on how to establish PID/VID blocking for USB devices.

1. Get your PIDs/VIDs of the devices you want to allow/block. On Windows one does this through the Device Manager interface.
Win+R -> "devmgmt.msc" -> Select device -> Details -> Hardware Ids

<div style="text-align: center;">
<img src="https://mcci.com/wp-content/uploads/2020/07/win10-run-devmgmt.png" alt="Screenshot of Run utility" style="width: 300px; height: 200px; object-fit: contain; display: inline-block;">
<img src="https://mcci.com/wp-content/uploads/2020/07/win10-devmgmr-768x949.png" alt="Screenshot of Window's Device Manager window" style="width: 300px; height: 200px; object-fit: contain; display: inline-block;">
<img src="https://kb.synology.com/_images/autogen/How_do_I_check_the_PID_VID_of_my_USB_device/2.png" alt="Screenshot of the details page of a device" style="width: 300px; height: 200px; object-fit: contain; display: inline-block;">
</div>

2. Once a list has been compiled of all devices one wishes to allow/block, a new Policy has to be created.  

    2.1. ***Domain Group Policy (Using Domain Controller)***  
    ```
    Win + R -> gpmc.msc -> Computer Configuration -> Administrative Templates -> System -> Device Installation -> Device Installation Restrictions
    ```
    Now one can enable one or both of the following policies:  
        - "Allow installation of devices that match any of these device IDs."  
        - "Prevent installation of devices that match any of these device IDs."
        <div style="text-align: center;">
        <img src="" alt="Screenshot of the policies in GPMC" style="width: 300px; height: 200px; object-fit: contain; display: inline-block;">
        <img src="https://winaero.com/blog/wp-content/uploads/2016/01/Prevent-Driver-Installation.png" alt="Screenshot of policy config in GPMC" style="width: 300px; height: 200px; object-fit: contain; display: inline-block;">
        </div>
    
    Don't forget to update your policies:  
    ```gpupdate /force```  
    
    Now the listed IDs should be rejected by all Domain Enrolled machines.

    2.2. ***Local Group Policy (On Windows Pro/Server)***  
    ```
    Win + R -> gpedit.msc -> Computer Configuration -> Administrative Templates -> System -> Device Installation -> Device Installation Restrictions  
    ```
    Now one can enable one or both of the following policies:  
        - "Allow installation of devices that match any of these device IDs."  
        - "Prevent installation of devices that match any of these device IDs."

    Don't forget to update your policies:  
    ```gpupdate /force```  
    
    2.3. **Local Group Policy (On Windows Home)**  
    Windows Home typically doesn't allow the manipulation of Local Group Policies, as it doesn't come with the Group Policy Editor required to do so.
    There's alternative open-source editors available though to enable the same functionality. 
    https://github.com/Fleex255/PolicyPlus
    




## 3: Linux
As we all know, Linux often provides very granular control over its IO and other processes, this isn't different here.

### 3.1: Temporary blocking
It is possible to completely block any devices that aren't currently connected to the device, using a simple one-liner:
```
# To block all new devices not currently connected
echo "0" | sudo tee /sys/bus/usb/drivers/usb/bind

# To allow all usb connections again
echo "1" | sudo tee /sys/bus/usb/drivers/usb/bind

```

This method is merely a stopgap, a temporary measure one can use to block USB connections to a device.


### 3.2: PID/VID blocking
Like Windows, there's a more permanent solution for regulating devices based on their PIDs and VIDs. This method uses udev rules to prevent or allow certain USB devices.

Below is a step-by-step guide on how to discover your devices' IDs and how to configure the udev rules:

1. Compile a list of the USB devices you want to allow/block. Show is shell output from our test machine running openSUSE Tumbleweed  
    ```
    lsusb
    Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 001 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
    Bus 001 Device 003: ID 138a:003c Validity Sensors, Inc. VFS471 Fingerprint Reader
    Bus 001 Device 004: ID 03f0:371d HP, Inc HP un2430 Mobile Broadband Module
    Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
    Bus 002 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
    Bus 002 Device 003: ID 1bcf:2805 Sunplus Innovation Technology Inc. HP HD Webcam [Fixed]

    ```

    One can see we are working with a HP laptop. We want to block the webcam. We note down the VID and PID, respectively, here: 1bcf:2805.

    If you're ever curious as to what company made what devices, visit https://devicehunt.com/, it holds a decent list of manufacturers and products.

2. Create an udev list to allow/block USB devices.  
    We are going to create a new udev-rule file and specify our IDs in there. A snippet is provided for our demo story here.

    ```
    # Create the new rule file
    sudo nano /etc/udev/rules.d/99-block-usb.rules
    ```

    Based on the method you'd like to use, one can now block specific devices or only allow those we trust:  
    
    2.1. Blocking devices  
    We accomplish this through a simple blocking rule in our rule file.
    ```
    ACTION=="add", SUBSYSTEMS=="usb", ATTRSACTION=="add", ATTR{idVendor}=="1bcf", ATTR{idProduct}=="2805", RUN+="/bin/sh -c 'echo 0 > /sys/bus/usb/devices/$kernel/authorized'"
    ```

    2.2. Allowing only trusted devices
    We will first block all devices and then add those that we trust we will make specific rules for.
    ```
    ACTION=="add", SUBSYSTEM=="usb", ATTR{authorized}="0"
    ```

    Add in a rule to allow the webcam
    ```
    ACTION=="add", SUBSYSTEM=="usb", ATTR{idVendor}=="1bcf", ATTR{idProduct}=="2805", ATTR{authorized}="1"

    ```
      
    Using this method, one can very easily control what devices can connect to a Linux system.

  
## Conclusion

As you will have read above, PID/VID blocking can go a long way in blocking unwanted devices from your PC/network.  
It is, however, vulnerable to the spoofing of these IDs. While the Rubber Ducky and Evil Crow Cable aren't capable of spoofing these, Hak5 and other manufacturers have other, more advanced devices capable of this, and blocking based on these IDs would be ineffective against this.