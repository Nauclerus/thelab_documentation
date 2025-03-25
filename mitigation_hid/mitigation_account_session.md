# Implementing User/Account/Session controls

## 1. Methods

As is widely known in the Cybersecurity space, if an attacker infiltrates in an environment, he is only as powerful as the credentials he acquires.  
This is why it is very important to, whenever an attacker manages to breach into a network or device, that he has no way of escalating his privileges to a higher level.  

We know this, and Windows and Linux have standard measures to prevent a normal user from elevating its permissions. But HID devices manage to, at least on the Windows side, skip a few hurdles that allow a Rubber Ducky or similar device to gain access to an elevated command prompt.

One could argue that even allowing a user to access a prompt/shell, elevated or not, is not required for most applications and, as such, should also be blocked.

Below, we'll guide you step by step on how to harden your Windows and Linux accounts, hopefully preventing a further escalation should User Education fail.



## 2. Windows

### 2.1 Password Lock elevated CMD

By default, Windows doesn't ask a user for a password when initiating an elevated prompt on a single user device, it merely asks if you consent. This is easily bypassed, as an HID attack can emulate the key presses or mouse movement needed to accept the prompt.

It is as such paramount that one make Windows at least ask for a password to stop any HID attacks without it knowing at least Admin credentials.

This is accomplished in the following way:

1. ***Domain Group Policy (Using Domain Controller)***  
    Ensure you are working in the correct OU, where the devices one wants to configure are located.

    ```
    Win + R -> gpmc.msc -> Computer Configuration -> Policies -> Windows Settings -> Security Settings -> Security Options
    ```
    1.2. Find the policy “User Account Control: Behavior of the elevation prompt for administrators in Admin Approval Mode.”.
    <div style="text-align: center;">
    <img src="https://www.syxsense.com/syxsense-securityarticles/assets/images/cis_adminapproval1.png" alt="Screenshot of selected Policy" style="width: 500px; height: 300px; object-fit: contain; display: inline-block;">
    </div>
    1.3. Double-click the Policy and select in the dropdown "Prompt for credentials."
    <div style="text-align: center;">
    <img src="https://www.top-password.com/blog/wp-content/uploads/2019/05/bypass-uac-password-for-administrators.png" alt="Screenshot of UAC Window" style="width: 300px; height: 300px; object-fit: cover; display: inline-block;">
    </div>
    This has now changed a standard admin account to also be asked for a password when he/she wants to access an elevated Command Prompt.

    1.4. Now repeat these last steps for this policy: "User Account Control: Behavior of the Elevation Prompt for Standard Users."

    Normally, standard users that are part of a Domain already have this setting set on this option, but it never hurts to check.
 
    1.5. Now update the policies, and everything should be configured. If one want to open an elevated prompt now, admin credentials are needed.

2. ***Local Group Policy (On Windows Pro/Server)***  
    With this section we assume a lone Windows Pro device that is not being administered by Domain Services. It only has one user, and as such, only one account that also has Admin rights.  
    This is the situation for many home users.
    
    ```
    Win + R -> gpedit.msc -> Computer Configuration -> Windows Settings -> Security Settings -> Local Policies -> Security Options 
    ```
    2.2. Find the policy “User Account Control: Behavior of the elevation prompt for administrators in Admin Approval Mode.”.
    <div style="text-align: center;">
    <img src="https://www.syxsense.com/syxsense-securityarticles/assets/images/cis_adminapproval1.png" alt="Screenshot of selected Policy" style="width: 500px; height: 300px; object-fit: contain; display: inline-block;">
    </div>
    2.3. Double-click the Policy and select in the dropdown "Prompt for credentials."
    <div style="text-align: center;">
    <img src="https://www.top-password.com/blog/wp-content/uploads/2019/05/bypass-uac-password-for-administrators.png" alt="Screenshot of UAC Window" style="width: 300px; height: 300px; object-fit: cover; display: inline-block;">
    </div>

    1.4. Now repeat these last steps for this policy: "User Account Control: Admin Approval Mode for the Built-in Administrator Account."

    This ensures that also the built-in Admin needs to insert a password.

3. **Local Group Policy (On Windows Home)**  
    Windows Home typically doesn't allow the manipulation of Local Group Policies, as it doesn't come with the Group Policy Editor required to do so.
    There's alternative open-source editors available though to enable the same functionality. An example of these is PolicyPlus:
    https://github.com/Fleex255/PolicyPlus
    
    One installs this 3rd-party editor and follows the same steps as the Pro version.



### 2.2 Disabling access to Command prompt entirely (optional .bat file block)
Of course, we realize that the command prompt is a tool utilized by IT personell almost exclusively.   
Most User, Professional or Home Users, have no need for the Command Prompt, and instead it poses a threat vector through which attackers can wrangle control from their unassuming hands.
We propose as such that for people that have no need for it, Command Prompt is disabled entirely, preventing an attacker's primary tool from being used.

We describe how to do this step-by-step below:

1. ***Domain Group Policy (Using Domain Controller)***  
    Ensure you are working in the correct OU, where the devices one wants to configure are located.

    ```
    Win + R -> gpmc.msc -> User Configuration -> Policies -> Administrative Templates -> System
    ```
    1.2. Find the policy "Prevent access to the command prompt.”
    <div style="text-align: center;">
    <img src="https://www.online-tech-tips.com/wp-content/uploads/2011/03/Prevent-Access-to-the-Windows-7-Command-Prompt_thumb.png" alt="Screenshot of Policy List" style="width: 300px; height: 300px; object-fit: contain; display: inline-block;">
    </div>
    1.3. Double-click the Policy and select Enable. If wanted, one can also block the running of Windows .bat files here with the extra option.
    <div style="text-align: center;">
    <img src="https://www.maketecheasier.com/assets/uploads/2015/02/disable-command-prompt-enable.png" alt="Screenshot of Run utility" style="width: 600px; height: 300px; object-fit: contain; display: inline-block;">
    </div>
    
    This has now stopped any users upon which this Policy has effect to not be able to use the Command prompt.
 
    1.4. Now update the policies, and everything should be configured. If one wants to open an elevated prompt now, they are met with an error message.
    <div style="text-align: center;">
    <img src="https://cdn.appuals.com/wp-content/uploads/2024/10/fab04930-c041-4a30-8492-a2de782687e5.png" alt="Screenshot of Error Message" style="width: 600px; height: 300px; object-fit: contain; display: inline-block;">
    </div>

2. ***Local Group Policy (On Windows Pro/Server)***  
    With this section, we assume a lone Windows Pro device that is not being administered by Domain Services. It only has one user, and as such, only one account that also has Admin rights.  
    This is the situation for many home users.
    
    ```
    Win + R -> gpedit.msc -> -> User Configuration -> Policies -> Administrative Templates -> System
    ```
    2.2. Find the policy "Prevent access to the command prompt.”
    <div style="text-align: center;">
    <img src="https://www.online-tech-tips.com/wp-content/uploads/2011/03/Prevent-Access-to-the-Windows-7-Command-Prompt_thumb.png" alt="Screenshot of Policy List" style="width: 300px; height: 300px; object-fit: contain; display: inline-block;">
    </div>
    2.3. Double-click the Policy and select Enable. If wanted, one can also block the running of Windows .bat files here with the extra option.
    <div style="text-align: center;">
    <img src="https://www.maketecheasier.com/assets/uploads/2015/02/disable-command-prompt-enable.png" alt="Screenshot of Run utility" style="width: 600px; height: 300px; object-fit: contain; display: inline-block;">
    </div>
    
    This has now stopped any users upon which this Policy has effect to not be able to use the Command prompt.
 
    2.4. Now update the policies, and everything should be configured. If one want to open an elevated prompt now, they are met with an error message
    <div style="text-align: center;">
    <img src="https://cdn.appuals.com/wp-content/uploads/2024/10/fab04930-c041-4a30-8492-a2de782687e5.png" alt="Screenshot of Error Message" style="width: 600px; height: 300px; object-fit: contain; display: inline-block;">
    </div>
    

3. **Local Group Policy (On Windows Home)**  
    Windows Home typically doesn't allow the manipulation of Local Group Policies, as it doesn't come with the Group Policy Editor required to do so.
    There's alternative open-source editors available though to enable the same functionality. An example of these is PolicyPlus:
    https://github.com/Fleex255/PolicyPlus
    
    One installs this 3rd-party editor and follows the same steps as the Pro verion.



## 3. Linux
Linux is a whole other beast than Windows. By default, it forces password authentication, even for those accounts with sudo rights, and if a user is not part of the sudo group, they can forget elevated shells altogether. 

Of course there are also a few simple ways to prevent users from accessing ANY shell, they are listed below, step-by-step:

1. Changing the login shell
    A login shell is the shell a user is greeted by when it first opens a shell. Linux employs the "nologin" shell to block users.  
    One accomplishes this through the following command:
    ```sudo usermod -s /sbin/nologin <username>```

2. Changing the SSH config to deny remote logins
    If shell access is still desired, but SSH connections are not, one could edit the SSHD config and exclude the needed users.
    ```
    sudo nano /etc/ssh/sshd_config

    DenyUsers <username>
    ```

    Now restart the daemon to load the changes.  
    ```sudo systemctl restart sshd```


## 4. Conclusion

Command Prompt/Shell access is only a tiny fraction of tools that an attacker can utilize, but it certainly is one of the most powerful ones. Preventing access to this Swiss army knife for users that don't have a need for it, prevents that even if said users ever introduced a malicious HID device into the network, the device on there has no way of elevating its permissions, preventing a larger security risk.

This does work for HID devices, as said devices have payloads that follow a set of keystroke injections. Now if a device would try to summon a command prompt, it would get stuck.

> Note that this list of measures is non-exhaustive, Powershell for example is not being blocked by these measures taking above, neither is the Group Policy Editors themselves. Way more restrictions are possible with file permissions within the Windows and Linux ecosystem, but conveying the point about account-based restrictions was the primary scope of this page.