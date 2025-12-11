# Automated-Remover-Blocksi

## Overview

### Purpose
- This utility is designed for users who intent on needing to remove a file or file folder many times using brute-force methods.
- This utility will watch for the specified file(s) and remove them upon their appearance **instantly**.

  ### Flow
  - User runs the `.bat`
  - The `.bat` launches the `.ps1`
  - The `.ps1` actively searches for, and deletes the specified file(s)
  - The `.ps1` quits when user closes or presses `Ctrl+C`

### Components
This utility consists of three components that work together:

1. **`Delete_Blocksi.bat`**  
   A Windows batch launcher that executes the PowerShell script and provides a simple user-facing console wrapper.

2. **`Delete_Blocksi.ps1`**  
   The main PowerShell engine that:
   - Monitors specified directory paths,
   - Logs all detected events,
   - Attempts to delete specified files/folders
   - Writes output to both console and a log file.

3. **`script.log`** 
   A persistent plain-text log file automatically created in the same directory as the PowerShell script.
   It logs all action performed by `Delete_Blocksi.ps1`.

## File-by-File Breakdown

### 1. Delete_Blocksi.bat

#### Purpose
A minimal launcher intended to start the deletion workflow and display live logs from the PowerShell script. By clicking directly. Users could use the `ico` file provided as the icon for this `.bat` file.

#### Contents
```bat
@echo off
title File Remover - Live Logs
echo Looking for files...
echo -----------------------------
powershell -ExecutionPolicy Bypass -NoExit -File "[PATH]"
echo -----------------------------
echo Script stopped. Press any key to exit.
pause >nul
```

#### Behavior
- `@echo off` suppresses command-echoing.
- Sets window title: **"File Remover - Live Logs"**.
- Prints:
  - `"Looking for files..."`
  - `"Script stopped. Press any key to exit."`
- Waits for a keypress before closing.

#### Important Notes
- The `.bat` file **does not actually call the PowerShell script** in its current form.
- The launcher requires for the user to find the file path of the PowerShell and replace `[Path]` with the file path.

### 2. Delete_Blocksi.ps1

#### Purpose
This script performs real-time monitoring and deletion of the files/folders. It logs **every operation**, including detections, failures, timestamps, and deletion outcomes.

#### Variables
```powershell
$BlocksiPath = "[PATH]"
$TempPath    = "[PATH]"
```

These paths must be manually set to valid filesystem locations. The current version for the code supports two files to be deleted. It requires the user to find the file path of the file(s) they wish to delete and replace `[Path]` with the file path. Shall they choose to only delete one file, they may leave the other as-is without deleting anything.

```powershell
$ScriptDir = Split-Path -Parent $MyInvocation.MyCommand.Path
$LogFile   = Join-Path $ScriptDir "script.log"
```
- `$ScriptDir` identifies the folder containing the `.ps1` script.
- `script.log` is created or appended inside the same folder.

#### Logging Function
```powershell
function Log($msg) {
    $timestamp = (Get-Date).ToString("yyyy-MM-dd HH:mm:ss:fff")
    $entry = "[$timestamp] $msg"
    Write-Output $entry
    Add-Content -Path $LogFile -Value $entry
}
```

**Features:**
- Millisecond-resolution timestamps.
- Logs to both console and disk.
- All events, including errors, are written.

#### Script Startup Output
```powershell
Write-Output "Watching for Files... Press Ctrl+C to end Script"
```
This script is intended to run continuously in the background until manually terminated.

### Final Active Logic
```powershell
if (Test-Path $TempPath) {
    Log "Temp detected."
    try {
        Remove-Item -Path $TempPath -Recurse -Force -ErrorAction Stop
        Log "Temp deleted."
    }
    catch {
        Log "Temp removal failed."
    }
}

Start-Sleep -Milliseconds 1
```
**Operational Explanation**
1. Check if the folder defined in `$TempPath` exists.
2. If present:
   - Log `"Temp detected."`
   - Attempt recursive forced deletion.
   - Log `"Temp deleted."` or `"Temp removal failed."`
3. Script sleeps for **1 millisecond** to prevent freezing, implying:
   - This runs in a high-frequency monitoring loop.
   - CPU usage is low, as it will only watch, unless a file that is specified is spotted

### 3. script.log

#### Location
Stored in the **same directory** as `Delete_Blocksi.ps1`:
```
<ps1_directory>\script.log
```

#### Format
Each entry follows:
```
[YYYY-MM-DD HH:MM:SS:fff] <Message>
```

#### Behavior
- Log file is **appended**, never cleared.
- Automatically created if missing.
- Grows continuously while script is running.

## Requirements

### Execution Requirements
- Windows 10 or 11
- PowerShell 5.1 or PowerShell 7+
- Administrator privileges are **not** required if specified file(s) are on the current user's. These files must also be saved to the current user's.
- Internet connection **not** required

### Configuration Required
The following placeholders **must be replaced**:
```
$BlocksiPath = "[PATH]"
$TempPath    = "[PATH]"
```
and
```
powershell -ExecutionPolicy Bypass -NoExit -File "[PATH]"
```

## Risks
- The system will touch any file paths specified
- It could accidentally delete wanted/needed files if specified by user
- Use at your own risk
  
## Running the Tool
- Un-ZIP the file
- These files shall be kept in the same folder when used
- Use Notepad
  1. Ctrl+R
  2. Notepad.exe
  3. Press enter
  4. Ctrl+O
  5. Navigate to directory containing file(s)
  6. Click on file or Shift+Click to select multiple files
  7. Press enter
  8. Edit files
- The file will run if the PowerShell window appears 

### Step 1 — Configure Paths
Edit `Delete_Blocksi.ps1` and set real filesystem paths for:
- Installation directory to monitor
- Any temporary file directory to monitor

Edit `Delete_Blocksi.bat` and set real filesystem paths for:
- `Delete_Blocksi.ps1`

### Step 2 — Run the Script
Open PowerShell and run:
```powershell
powershell -ExecutionPolicy Bypass -File "[PATH]"
```
The path shall be the file path for the `Delete_Blocksi.ps1` file. Users could also run by double-clicking the `.bat` file.

### Step 3 — Observe Logs
The console will show timestamps and logs.  
To view, open the log in Notepad using the instructions provided above, or double-click.
Detailed logs accumulate in:
```
script.log
```

## Proper Folder Structure
...
|
Delete-Blocksi-Windows11
│
├── README.md
├── Delete_Blocksi.ps1
├── Delete_Blocksi.bat
└── script.log (created automatically)
      
## Version History
1.0.0 Official Build | Stable Release

# End
