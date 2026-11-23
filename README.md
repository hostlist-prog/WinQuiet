<div align="center">

# WinQuiet

**A small, reversible privacy and declutter tool for Windows, with everyday utilities in the system tray.**

![Version](https://img.shields.io/badge/version-1.0.0-7FB69A)
![Platform](https://img.shields.io/badge/platform-Windows%2010%20%7C%2011-0078D4)
![PowerShell](https://img.shields.io/badge/PowerShell-5.1-5391FE)
![.NET Framework](https://img.shields.io/badge/.NET%20Framework-4.8-512BD4)
![License: MIT](https://img.shields.io/badge/license-MIT-green)

• [Features](#features)
• [Installation](#installation)
• [Usage](#usage)
• [License](#license)

</div>

---

## Overview

WinQuiet turns off Windows telemetry, AI assistants, news feeds, advertising, and preinstalled apps from a single window. It records the original state before each change, so any item can be reverted later.

It also includes a set of optional utilities, such as a system monitor and clipboard history. Modules you do not need can be hidden and stop running in the background.

> [!WARNING]
> WinQuiet changes system policies, services, scheduled tasks, and installed apps. Version 1.0.0 is an early release, so try it in a virtual machine or back up your system first.

## Features

### System cleanup

WinQuiet includes 31 individually selectable tweaks:

| Category | Items | What it covers |
| --- | :---: | --- |
| Telemetry and privacy | 8 | Diagnostic services and tasks, advertising ID, activity history, feedback prompts, input data collection, error reporting |
| AI assistants | 7 | Copilot, Recall, Click to Do, Settings agent, AI in Paint and Notepad, Bing in Start search |
| News, ads, and suggestions | 5 | Widgets, search highlights, suggested apps, lock screen tips, Start and File Explorer recommendations |
| Microsoft Edge | 2 | Diagnostic data, shopping assistant, Copilot sidebar, new tab news feed |
| Preinstalled apps | 5 | News, Weather, promotional apps, personal Teams, Phone Link, Xbox apps |
| Minimal interface | 4 | Cleaner taskbar, practical File Explorer defaults, classic context menu, no mouse acceleration |

Each tweak has a risk level:

| Level | Items | Included in |
| --- | :---: | --- |
| Safe | 21 | Recommended and Minimal presets |
| Moderate | 7 | Minimal preset |
| Advanced | 3 | Manual selection only |

WinQuiet deliberately does **not** modify Microsoft Defender, SmartScreen, Windows Update, Microsoft Store, WebView2, drivers, or system files.

### Utilities

| Module | Description |
| --- | --- |
| Monitor | Live CPU, memory, system disk, network, and uptime |
| Clipboard | In-memory text history with search, plain-text conversion, and URL tracking removal |
| Startup | Enable or disable startup entries without deleting them |
| Ports | List listening TCP ports and stop non-system processes |
| Temporary files | Remove files older than 24 hours from temp and crash dump folders |
| Notes | Autosaved local scratchpad |
| Quick actions | Keep awake, theme switch, lock screen, empty Recycle Bin, app updates |

## Requirements

- Windows 10 or Windows 11
- Administrator account
- Windows PowerShell 5.1 (included with Windows)
- .NET Framework 4.8 (included with Windows 10 version 1903 and later)

## Installation

1. Get `WinQuiet.exe` from the release archive.
2. Run it and accept the User Account Control prompt.

The executable extracts its embedded scripts to `C:\ProgramData\WinQuiet`, which only administrators can modify.

### From source

Copy the whole project folder to a Windows PC and run `Start.bat`. In this mode, WinQuiet stores its data in a `data` folder next to the scripts.

## Usage

1. Open WinQuiet from your own administrator account.
2. Choose a preset: **Recommended** or **Minimal**.
3. Review the selected items and adjust them if needed.
4. Click **Apply**.
5. Restart File Explorer or Windows if a change has not taken effect.

Closing the window keeps WinQuiet running in the system tray. Use the tray icon menu to exit.

### Keyboard shortcuts

| Shortcut | Action |
| --- | --- |
| <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>V</kbd> | Open clipboard history |
| <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>P</kbd> | Remove formatting from clipboard text |
| <kbd>Ctrl</kbd> + <kbd>Alt</kbd> + <kbd>U</kbd> | Remove tracking parameters from a copied URL |

## Command line

Run these commands from the project folder. Commands that change the system require an elevated terminal.

```powershell
# List all tweaks
powershell -ExecutionPolicy Bypass -File .\WinQuiet.ps1 -List

# Show the current status
powershell -ExecutionPolicy Bypass -File .\WinQuiet.ps1 -Status

# Preview a preset without changing anything
powershell -ExecutionPolicy Bypass -File .\WinQuiet.ps1 -Apply recommended -DryRun

# Apply a preset or specific tweaks
powershell -ExecutionPolicy Bypass -File .\WinQuiet.ps1 -Apply minimal
powershell -ExecutionPolicy Bypass -File .\WinQuiet.ps1 -Apply ai.copilot,ai.recall

# Revert specific tweaks or everything
powershell -ExecutionPolicy Bypass -File .\WinQuiet.ps1 -Undo ai.copilot
powershell -ExecutionPolicy Bypass -File .\WinQuiet.ps1 -Undo all
```

| Parameter | Description |
| --- | --- |
| `-List` | Print the tweak catalog |
| `-Status` | Print the state of each tweak |
| `-Apply <items>` | Apply `recommended`, `minimal`, or tweak IDs |
| `-Undo <items>` | Revert `all`, a preset, or tweak IDs |
| `-DryRun` | With `-Apply`, show planned actions only |
| `-Tray` | Start the interface minimized to the tray |

## Reverting changes

Before changing anything, WinQuiet requests a System Restore point and saves the original values to `state.json`:

| Launch method | Location |
| --- | --- |
| Executable | `C:\ProgramData\WinQuiet\data\state.json` |
| From source | `data\state.json` |

> [!CAUTION]
> Do not delete `state.json` while you may still need to revert changes.

To revert, select the items and click **Revert selected**, or use `-Undo all`.

Removed apps are restored if their package files are still on the system. Otherwise, reinstall them from Microsoft Store.

## Privacy

WinQuiet has no accounts, analytics, or telemetry, and it does not send your data anywhere. Clipboard history stays in memory and is cleared on exit. Content marked as excluded by password managers is skipped.

The **app updates** quick action runs `winget upgrade` or opens Microsoft Store, and those tools use the network.

## Troubleshooting

| Problem | Solution |
| --- | --- |
| SmartScreen blocks the executable | Verify the checksum, then select **More info** and **Run anyway** |
| Changes are blocked for another administrator | Sign in to the account you want to configure, then run WinQuiet there |
| A tweak shows as partially applied | A Windows update may have reset it, or it may not apply to your edition. Apply it again |
| A shortcut does not work | Another app may already use it. Check `winquiet.log` |
| The app fails to start | Check `winquiet.log` in the `data` folder |

## Known limitations

- Major Windows updates may restore some settings.
- Some Microsoft policies only apply to specific Windows editions and builds.
- On Home and Pro, Windows does not allow required diagnostic data to be fully disabled by policy.
- Apps are removed for the current user only and may be reinstalled by Windows.

### Project structure

```text
.
├── WinQuiet.ps1          # Entry point and command-line interface
├── Start.bat             # Launcher for running from source
├── LICENSE               # MIT License
├── src/
│   ├── Catalog.ps1       # Tweak definitions
│   ├── Core.ps1          # Apply, status, and revert engine
│   ├── Gui.ps1           # WPF interface and tray utilities
│   └── Native.ps1        # Windows API interop
└── launcher/             # C# executable launcher
```

## Contributing

Issues and pull requests are welcome. Please follow these rules:

- Keep `.ps1` files in UTF-8 with BOM and CRLF line endings.
- Keep scripts compatible with Windows PowerShell 5.1.
- Keep embedded C# compatible with C# 5.
- Make every new tweak reversible and add it to `src/Catalog.ps1`.
- Do not add tweaks that weaken Windows security or updates.

## License

WinQuiet is released under the [MIT License](LICENSE).

## Disclaimer

WinQuiet is provided "as is", without warranty of any kind. You are responsible for changes made to your system. WinQuiet is not affiliated with or endorsed by Microsoft. Windows, Copilot, Edge, and other product names are trademarks of their respective owners.
