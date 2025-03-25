# Implementing unsigned driver blocking

## 1. Concept and Method

Drivers can be signed and unsigned. Signed drivers are known drivers, cryptographically linked to their publisher, and as such, a bad vector for crime.  
While not foolproof, enabling signed-only drivers on an operating system ensures that if a device runs on your device, its drivers will have a known origin.


## 2. Implementation
Enabling UEFI Secure Boot also ensures that everything is signed and from an even deeper level.

Both Windows and Linux also support signed-only driver modes for their kernels. Below is explained how one activates this on both operating systems.

### 2.1. UEFI Secure Boot
UEFI Secure Boot is a "special" boot mode that one can activate and that most likely is already in all company settings. It forces a chain of trust at boot for all drivers and firmware, meaning it checks all firmware and drivers that are being loaded and enforces signage.

One must notice that Secure Boot goes much further than merely drivers and firmware, but it checks that ALL pre-boot components are signed.
As UEFI is the first active component on a system, running at the firmware level, it has the potential to check all that loads after it and block even the earliest components in the boot chain if they aren't signed.

UEFI is of course an option one has to enable in the BIOS. Every BIOS is different, of course, so the screenshot below is merely an indication.
<div style="text-align: center;">
<img src="https://images.minitool.com/minitool.com/images/uploads/lib/2019/06/secure-boot/secure-boot-1.jpg" alt="Screenshot Of Bios " style="width: 500px; height: 200px; object-fit: contain; display: inline-block;">
</div>
Most of the time, one can find the Secure Boot setting under the headers Boot/Booting and/or Security. The system will also be required to use UEFI booting only.  

> An important detail is dat UEFI Secure Boot only works Pre-Boot! Most BIOSes force the installed OS at boot to only use signed drivers as well, but Secure Boot itself does not enforce it. It merely tries to force the OS it booted to adopt the settings explained below.

### 2.2. OS-Level Solutions:
If one finds oneself in a situation where using UEFI Secure Boot is unavailable, e.g. due to using custom kernels on Linux, some unsigned Windows Drivers for Legacy Devices or just plain old Hardware without a BIOS that supports it, the OSes have a built-in option to enforce signed-only drivers.

Note that both Windows and Linux enforce these measures when Secure Boot is enabled.

#### 2.2.1 Windows

Windows has an option to configure this using its "Advanced Startup Options", but it is done much quicker using an elevated Command Prompt.

1. Open an elevated Command Prompt
```
bcdedit /set testsigning off
bcdedit /set nointegritychecks off

```

Reboot


#### 2.2.2 Linux

In Linux one can enforce signed-only drivers through two measures. Either to activate it in the kernel or to pass a GRUB parameter at boot.  
Below we describe the GRUB method, as it's widely used and allows us to keep our original kernel in place.

Find the GRUB config file. Most times it is located in the directory ```/etc/default/grub```  
1. Find the line beginning with ```GRUB_CMDLINE_LINUX_DEFAULT``` and append ```module.sig_enforce=1```
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash module.sig_enforce=1"
```

2. Reload GRUB  
    - On most distributions, one uses the following command:  
    ```sudo grub2-mkconfig -o /boot/grub2/grub.cfg```

    - But on Debian derivatives, one needs to use:  
    ```sudo update-grub```

3. Reboot


## 3. Conclusion

We have explored the ways that we can utilize to harden our devices against devices utilizing unsigned drivers, making our computing safer and intrusion more capable, but HID like a Rubber Ducky unfortunately **won't** be stopped by this method.

HIDs have been developed to be truly plug and play, using a built-in class driver in every OS and using that to immediately interact with the device.  
When have you ever needed to download a driver before you could use a keyboard?

HID attacks like the Rubber Ducky utilize this trust and plug-and-play nature of HIDs and hereby prevent the need for a driver. Hence, the reason why the methods described above won't stop attacks of this kind.

We concluded, however, that our research in this matter deserved attention, hence why this page is still featured.

#### Sources

USBBlock: https://publications.sba-research.org/publications/201806%20-%20SNeuner%20-%20USBlock.pdf#:~:text=2,it%20is%20compiled%20into%20a