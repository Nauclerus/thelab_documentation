# Demo 1: Payload Description

This document outlines a sample payload that creates a warning pop-up, simulating a ransom attack, through the use of PowerShell scripting and Windows Forms. The following sections describe the setup, requirements, the working mechanism, and procedures for testing it manually, as well as potential issues that may arise during execution.

## Setup

### Execution Environment:
- **Windows operating system** (preferably Windows 10/11).
- **PowerShell** with administrative privileges (or sufficient permissions to execute PowerShell scripts).

### Script Initialization:
The payload uses PowerShell scripting to execute a GUI pop-up. It starts by launching PowerShell and then creating a Windows Forms application.

This payload can be run from an existing script or a terminal emulator like PowerShell or a batch file.

## Requirements

### Hardware Requirements:
- A PC running Windows OS.

### Software Requirements:
- **PowerShell**: Built-in tool in Windows OS (Windows PowerShell or PowerShell Core).
- **.NET Framework**: Necessary for Windows Forms to function, which is supported by default in most modern versions of Windows.

### Network Requirements:
- No specific network configuration is needed for the payload to work locally. However, network-related warnings are simulated within the payload.

## How it Works

The payload executes a PowerShell script that does the following:

### GUI Creation:
- A custom form is created using `System.Windows.Forms` and displayed in full-screen mode with a black background and red text to simulate an alarming warning.
- A warning message such as "YOU HAVE BEEN HACKED" is displayed prominently on the screen.
- A ransom message prompts the user to pay Bitcoin to a specified address to recover their files.

### Log Simulation:
- A fake log simulation is displayed in a `RichTextBox` in green text on a black background, mimicking the look of a terminal.
- The log is periodically updated every 2 seconds with the message `[INFO] Accessing system files....`

### Sound Alerts:
- The script plays warning and error sounds (e.g., system message beep and error beep) to simulate urgency.

### Error Message Boxes:
- MessageBoxes pop up, warning the user about unauthorized access and network disconnection.

### Process Termination:
- After 10 seconds, the script randomly kills other running processes (except PowerShell) to simulate system disruption, which may affect the system’s functionality.

## Procedure of Testing (Manually)

### Preparation:
- Ensure the test environment is isolated, preferably on a virtual machine (VM) or a non-production system.
- Backup any important data as the script simulates a system crash and can affect system performance.

### Execution:
Insert your USB device in your computer.

Once the script executes, the user will after 20 seconds see a full-screen warning message with an alarming red text. The background processes will start simulating system file access while periodic message boxes will create a sense of urgency. As time progresses, the script will randomly terminate processes, potentially affecting active applications. The combination of sound alerts, pop-ups, and forced disruptions creates a highly immersive simulation of a security breach.

To regain control, the affected user may need to manually terminate the PowerShell process using Task Manager (Ctrl + Shift + Esc) or force a system reboot.

