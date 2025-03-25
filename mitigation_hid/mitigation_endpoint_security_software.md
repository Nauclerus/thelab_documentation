# Introducing Endpoint security suites

Most companies, accompanied by a robust security policy, use a form of endpoint security software to protect their devices.
These are mostly standard malware detection tools, monitoring suites, etc.
HIDs however, form a unique challenge: They just perform their task very fast. This makes them less susceptible to the classic malware scanners, as they have nothing to scan, they're just keyboards, mice, etc. to the victim's PC.

There do exist tools and software suites on the market to detect and block HID-based attacks, and in this small article we explore two of the open-source programs that succeed in stopping the HID attacks launched by a Rubber Ducky:

## 1. Duckhunt
As one can imagine, [Duckhunt](https://github.com/pmsosa/duckhunt) was developed with exactly the Rubber Ducky in mind. It is a daemon running in the background of Windows and stops all typing when it detects the typical keystroke injection patterns and/or speed of the Ducky or devices like it. It is currently Windows only.

## 2. UKIP
[UKIP](https://github.com/google/ukip) is a daemon developed for Linux by Google. Similarly as Duck Hunt for Windows, it detects the patterns and speed of the aforementioned devices and blocks their input until further notice by the user.

## Conclusion

There are multiple open-source initiatives available to stop HID-based keystroke injection attacks from happening. Please pay these repositories a visit and, perhaps, give them a star to encourage the development of these projects.

There's more closed-source software available for enterprises.