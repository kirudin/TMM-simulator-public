[PUBLIC_REPO_README.md](https://github.com/user-attachments/files/29675920/PUBLIC_REPO_README.md)
# TMM Simulator

Private-source binary release page for TMM Simulator.

This public repository is used only for distributing packaged executables.  
It does not include the source code, numerical kernels, material-library implementation, or research-development history.

## Download

Open the [Releases](https://github.com/kirudin/TMM-simulator-public/releases) page and download the ZIP file for your operating system.

- macOS Apple Silicon: `TMM-Simulator-Darwin-arm64-v...zip`
- Windows 64-bit: `TMM-Simulator-Windows-AMD64-v...zip`

## How To Run

### macOS

1. Download the macOS ZIP file.
2. Unzip it.
3. Open the extracted folder.
4. Double-click `Launch TMM Simulator.command`.
5. Keep the launcher window open while using the app.

If macOS blocks the launcher, right-click `Launch TMM Simulator.command`, choose `Open`, then confirm.

### Windows

1. Download the Windows ZIP file.
2. Unzip it.
3. Open the extracted folder.
4. Double-click the TMM Simulator executable.

If Windows SmartScreen appears, choose `More info` and then `Run anyway`.

## What The App Does

TMM Simulator is a local browser-based research tool for optical multilayer simulations.

Main workspaces:

- Dispersion map
- Isofrequency contour
- E-field profile
- Material library

The app runs locally on your computer. It opens a browser interface, usually at a local address such as:

```text
http://127.0.0.1:8502/
```

No source-code installation is required for the binary release.

## User Manual

See `USER_MANUAL.md` in the private development repository or request the latest user manual from the maintainer.

## Notes

- The released ZIP files are OS-specific.
- macOS and Windows builds are generated separately.
- User-saved materials and stack presets are stored in the user's local app-data folder.
- This public repository is not intended for issue-level source review or source installation.

## Maintainer

Hwi Je Woo  
ARON Lab  
hjwoo.aron@gmail.com
