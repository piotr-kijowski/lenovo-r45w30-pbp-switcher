# Lenovo R45w-30 PBP Switcher

Contributors: piotr.kijowski  
Tags: monitor, pbp, ddcci, displayport, hdmi, automation, ultrawide  
Requires at least: Windows 10  
Tested up to: Windows 11  
Requires Software: ControlMyMonitor  
Stable tag: 1.0.0  
License: GPLv3 or later  
License URI: https://www.gnu.org/licenses/gpl-3.0.html

*Quickly toggle Picture-by-Picture (PBP) mode on the Lenovo R45w-30 ultrawide monitor using DDC/CI commands and simple automation scripts.*

## Description 

Lenovo R45w-30 PBP Switcher provides a simple way to control **Picture-by-Picture mode** on the Lenovo R45w-30 monitor without navigating the monitor’s on-screen display (OSD).

The scripts send **VESA DDC/CI commands** directly to the monitor using the ControlMyMonitor utility.

This allows instant switching between:

- Single DisplayPort input
- PBP mode with HDMI on the left and DisplayPort on the right

The scripts can be triggered via:

- Keyboard shortcuts
- Stream Deck buttons
- Automation tools
- Any application capable of launching `.bat` files

This effectively turns the monitor into a **programmable layout switcher**.

## Features

- Enable **PBP mode (HDMI left / DP right)**
- Return to **single DisplayPort input**
- Works with keyboard shortcuts
- Works with Stream Deck
- Uses lightweight ControlMyMonitor utility
- No Lenovo software required
- Instant switching between monitor layouts

## Requirements

- Windows
- Lenovo R45w-30 monitor
- DDC/CI enabled in monitor settings
- ControlMyMonitor utility


Download ControlMyMonitor → [https://www.nirsoft.net/utils/control_my_monitor.html](https://www.nirsoft.net/utils/control_my_monitor.html)

## Installation

1. Download this repository.
2. Download **ControlMyMonitor** from NirSoft.
3. Place `ControlMyMonitor.exe` in the project folder.
4. Ensure **DDC/CI is enabled** in the monitor’s OSD settings.
5. Run the scripts manually or bind them to shortcuts.


Example folder layout:
```
MonitorHotkeys
├── ControlMyMonitor.exe
├── R45w_PBP_HDMI1_left_DP_right.bat
└── R45w_Single_DP.bat
```
> [!NOTE]  
> ControlMyMonitor is include in repository.

## Usage

Run one of the scripts depending on the desired monitor layout.

### Enable PBP Mode (HDMI Left / DP Right) - `R45w_PBP_HDMI1_left_DP_right.bat`

This script:

- Enables multi-picture mode
- Waits for the monitor scaler to switch layouts
- Assigns HDMI1 to the active window

Example code:

```
@echo off
setlocal

set "DIR=%~dp0"
set "CMM=%DIR%ControlMyMonitor.exe"
set "MON=\.\DISPLAY1\Monitor0"

"%CMM%" /SetValue "%MON%" A4 65535
"%CMM%" /SetValue "%MON%" F4 513
"%CMM%" /SetValue "%MON%" F5 257

ping 127.0.0.1 -n 5 >nul

"%CMM%" /SetValue "%MON%" A5 512
ping 127.0.0.1 -n 2 >nul
"%CMM%" /SetValue "%MON%" 60 17

ping 127.0.0.1 -n 2 >nul
"%CMM%" /SetValue "%MON%" A5 512

endlocal
exit /b 0
```

### Disable PBP and Return to DisplayPort - `R45w_Single_DP.bat`

This script disables PBP and restores a single DisplayPort input.

Example code:

```
@echo off
setlocal

set "DIR=%~dp0"
set "CMM=%DIR%ControlMyMonitor.exe"
set "MON=\.\DISPLAY1\Monitor0"

"%CMM%" /SetValue "%MON%" F5 0
"%CMM%" /SetValue "%MON%" F4 0
"%CMM%" /SetValue "%MON%" A4 0

ping 127.0.0.1 -n 4 >nul

"%CMM%" /SetValue "%MON%" A5 0
ping 127.0.0.1 -n 2 >nul

"%CMM%" /SetValue "%MON%" 60 15

endlocal
exit /b 0
```

## Keyboard Shortcut Setup 

Windows allows `.bat` files to be triggered using keyboard shortcuts.

1. Right-click the `.bat` file.
2. Select **Create Shortcut**.
3. Right-click the shortcut → **Properties**.
4. In the **Shortcut key** field, assign a key combination.

Example:

Single DisplayPort: Ctrl + Alt + 1

Enable PBP: Ctrl + Alt + 2


## Stream Deck Setup

These scripts work perfectly with **Elgato Stream Deck**.

Create two buttons.

Button 1 – Enable PBP

> Action: Open
> Target: R45w_PBP_HDMI1_left_DP_right.bat


Button 2 – Disable PBP

> Action: Open
> Target: R45w_Single_DP.bat


Pressing the buttons instantly switches monitor layouts.

## Notes 

- The monitor requires 1–2 seconds to reconfigure its internal scaler when switching layouts.
- Delays in the scripts ensure reliable switching.
- Color presets may differ between PBP and single-input modes depending on monitor picture settings.

## FAQ 


- Does this work with other Lenovo monitors?

Possibly, but the VCP registers used here were discovered specifically for the Lenovo R45w-30.

- Why does the script include delays? 
The monitor scaler requires a short period to apply multi-picture configuration changes.

- Can I trigger this from automation tools? 
Yes. Any tool capable of launching `.bat` files can trigger these scripts.

## Changelog

### 1.0.0 - Initial release.

- PBP enable script (HDMI left / DP right)
- Single DisplayPort restore script
- Keyboard shortcut and Stream Deck support

## License 

This project is licensed under the [GPL v3](https://www.gnu.org/licenses/gpl-3.0.html) or later.

## Disclaimer 

This project is a personal hobby / educational project created primarily for my own use.

It was developed as a workaround for the current Lenovo monitor software, which does not provide a way to assign custom keyboard shortcuts for controlling Picture-by-Picture (PBP) modes on the Lenovo R45w-30.

The scripts included in this repository interact directly with the monitor using DDC/CI commands through the ControlMyMonitor utility. Some of the commands used rely on undocumented or vendor-specific monitor registers discovered through experimentation.

As a result:

- This project is **not affiliated with or endorsed by Lenovo**.
- It may **not work on other monitors or firmware versions**.
- Future monitor firmware updates could change or break the behavior.
- Use of these scripts is **entirely at your own risk**.

These scripts are provided **as-is**, without warranty of any kind.  
Always test carefully and ensure that DDC/CI control is enabled on your monitor before use.