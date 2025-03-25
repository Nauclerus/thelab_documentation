# Blocking Malicious HID Hardware: Strategies and Methods

Malicious Human Interface Devices (HIDs) such as Rubber Ducky devices pose a unique security risk. These devices mimic trusted input peripherals (like keyboards or mice) and can inject pre-programmed keystrokes, bypassing traditional defenses. This essay discusses methods to block these threats, from physical prevention techniques to sophisticated software measures, arranged from the most to the least obtrusive.

---

## 1. Physical Access Controls

The first line of defense is preventing unauthorized physical access to hardware ports and devices. Educating End Users is also in this category.  
Physical security measures include:

### 1.1 Port Locking and Blocking

- **USB Port Locks**: Using physical locks that block USB ports can effectively prevent the insertion of unauthorized devices. These locks are inexpensive and can be installed in public or shared environments. These are simply keyed "plugs," only removable with a specialized tool. Visual examples of lock and "key" are provided below.
<div style="text-align: center;">
<img src="https://connectivitycenter.com/wp-content/uploads/2020/09/Smart-Keeper-USB-Port-Lock-Professional.jpg" alt="Example of a physical USB lock" style="width: 300px; height: 300px; object-fit: contain; display: inline-block;">
<img src="https://media.s-bol.com/Bv1MBMAklEO2/no2797/550x550.jpg" alt="Example of a physical USB lock key" style="width: 300px; height: 300px; object-fit: contain; display: inline-block;">
</div>

- **Physical Barriers**: In high-security environments, port blockers can be integrated into device casings or used in enclosures that only allow specific connections. This is generally limited to static setups, where a desktop or a server enclosure can be locked away, and static peripherals are provided.

### 1.2 Controlled Environments

- **Secure Zones**: Limiting physical access to areas where sensitive systems are located is critical. This can be achieved through biometric access, key card systems, or other physical security measures. This is mostly reserved for servers and other mission-critical equipment.
- **Device Security Policies**: Enforcing policies that restrict the use of personal USB drives and external devices in controlled environments helps minimize the risk of malicious HID hardware introduction. We are strictly speaking of physical security policies. This would entail searching employees upon entry into the workplace, and as such would be reserved for companies with an already strict policy regimen.

### 1.3 Educating users on Malicious hardware
 
- **Educating Users**: Training staff to recognize suspicious devices (such as unfamiliar USB dongles or cables) adds a layer of vigilance. This is the most important step to reduce the risk of staff unknowingly bringing a risk to a network and compromising an otherwise secure environment.

---

## 2. Software-Based Blocking Techniques

When physical measures are insufficient or impractical, software-based approaches provide security controls that can be tailored. These methods can be tailored to strike a balance between security and user convenience.

### 2.1 Device Whitelisting and Blacklisting

- **Whitelisting**: Implement device whitelisting policies that only allow pre-approved HIDs to connect to systems. The operating system or specialized security software can enforce this by checking device IDs or digital signatures. 
- **Blacklisting**: Conversely, blacklisting known malicious devices (or device classes) can help prevent their operation. However, this approach is generally less effective than whitelisting because it requires a constantly updated database of malicious IDs.
- **Methods**: There are multiple methods through which one could block a HID device from executing its payload immediately upon insertion. The [White/Blacklisting](./mitigation_hid/mitigation_white_black_listing.md) article will explore some of the paths one could take.

### 2.2 Driver-Level Controls and Firmware Policies

- **Digital Signature Verification**: Require that drivers or devices be digitally signed. Modern operating systems can enforce this, rejecting devices that have not been authenticated by a trusted certificate authority. This is since a few years by standard enforced on Windows

- **Methods**: The different operating systems configure this differently, of course. In the [Driver/Firmware control](./mitigation_hid/mitigation_driver_level_control.md) article, we look at the different operating systems and see if they are effective in warding off automated HID devices.

### 2.3 User Account Control and Session Management

- **Limited Privileges**: Ensure that user accounts have limited privileges. Even if a malicious device is connected, the damage can be mitigated by restricting what actions the device-triggered commands can perform. This can be applied in a spectrum, as there are a wide range of tools one can take away from regular users to prevent unwanted execution of scripts and or malware.
- **Session Isolation**: Using virtual desktops or sandboxing can prevent the system-wide impact of an attack. If a malicious device injects commands in a sandboxed environment, the broader network or operating system remains unaffected.

- **Methods**: We have broken these down in a separate [Account and Session control](./mitigation_hid/mitigation_account_session.md) article, herein we go into greater detail.

### 2.4 Endpoint Security Solutions

- **Antivirus and Anti-Malware Tools**: Use endpoint security software that includes device control modules. These modules can detect suspicious behavior typical of a malicious HID and alert administrators, or automatically disable the device.
- **Methods**: We have broken these down in a separate [Endpoint Security](./mitigation_hid/mitigation_endpoint_security_software.md) article, here we go into greater detail.

---

## 3. Balancing Security and Usability

When deploying security measures, it is crucial to balance robust protection with user convenience:

- **Very Obtrusive Measures**: Physical port locks and complete USB disabling offer strong protection but can severely limit legitimate device use.
- **Moderately Obtrusive Measures**: Device whitelisting, driver-level controls, and OS policies strike a balance by allowing necessary peripherals while blocking unauthorized ones.
- **Least Obtrusive Measures**: Monitoring tools and endpoint security solutions work transparently in the background. They allow regular operations with minimal disruption but require ongoing maintenance and vigilance.

---