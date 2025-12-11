# Removing Blocksi

#### This file is not intended for the purpose of removing Blocksi on a controlled computer but is included, use at your own risk and only after reading all other files

## Details
- The requirements are same as those listed in the README
- There is a heightened chance of facing (a) risk(s), the risks are same as those listed in the README
- The components are the same but require some modification
- The purposes are the same as those listed in the README
- The Proper Folder Structure are same as those listed in the README
  
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
- Blocksi extension directory
- Temp extension directory (created by Blocksi)
- Both can be found in `C:\Users\[YOUR_USERNAME]\AppData\Local\Google\Chrome\User Data\[YOUR_PROFILE]\Extensions`
- Replace `[YOUR_USERNAME]` and `[YOUR_PROFILE]` by the actual path. The profile will be numbered, to find the one you want, double-click -> Accounts -> Avatar Images -> Check
- The name for the Blocksi extension folder is `ghlpmldmjjhmdgmneoaibbegkjjbonbk` while Temp is `Temp`

Edit `Delete_Blocksi.bat` and set real filesystem paths for:
- `Delete_Blocksi.ps1`

If the user wishes they may use the shortcut provided, it is a link to the `.bat` and can be displayed on the desktop or be pinned to the taskbar. Users could use the `ico` file provided as the icon for this file or create their own.

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
## What it Does
The script will watch for the specified files, when they are spotted the script will delete the folders, and anything within the folders, within milliseconds. Chrome will try to reinstall the Blocksi extension but the script will continue on deleting the specified folders. This is brute-forced. When using the Chrome profile, run the script first. This only disables Blocksi on the device the script is run on, while the script is running.

## Note
If you could not find `Temp` disregard.
If you could not find `ghlpmldmjjhmdgmneoaibbegkjjbonbk` look in `C:\Users\[YOUR_USERNAME]\AppData\Local\Google\Chrome\User Data\[YOUR_PROFILE]\Extensions`, replacing the variables as specified above. Afterwards, double-click each extension and look for the logo within the file. If you find the Blocksi logo, you are viewing the Blocksi extension file, replace `ghlpmldmjjhmdgmneoaibbegkjjbonbk` in any section above with what you found to be Blocksi's extension ID.

## Version History
1.0.0 Official Build | Stable Release

# End
