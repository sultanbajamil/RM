# 📷 RM: Windows Media Foundation & Virtual Camera Optimizer

**RM** provides a targeted Windows Registry configuration script for the **Windows Media Foundation (WMF)** platform subsystem. It removes the `EnableFrameServerMode` value across both 64-bit and WOW6432Node registry trees.

---

## ❓ What is `EnableFrameServerMode`?

Starting with Windows 10 (Anniversary Update), Microsoft introduced the **Windows Media Foundation Frame Server**. This service routes camera and video capture streams through a unified system process to enable multiple applications to access a single physical camera simultaneously.

### Why Remove It?
While intended to share camera feeds, the Frame Server architecture frequently causes:
1. **Video Freezes and Lag**: Legacy applications, specialized capture devices, and USB webcams may experience significant latency, black screens, or frame freezing.
2. **Virtual Camera Incompatibility**: Virtual camera drivers (such as OBS Virtual Camera or Unity capture) often experience conflicts or fail to hook properly into video streams.
3. **Proctoring and Verification Conflicts**: Automated exam proctoring and facial verification tools often fail to initialize webcam devices under the Frame Server architecture.

By removing the `EnableFrameServerMode` value, Windows falls back to direct capture mode, allowing applications to read directly from the camera driver without intermediate frame processing.

---

## 📁 Repository Contents

```text
RM/
├── disable_virtual_camera.reg   # Windows Registry script to remove Frame Server mode
├── .gitignore                   # System file exclusion rules
└── README.md                    # Project documentation
```

---

## 🚀 How to Apply

### Method 1: Direct File Import (One-Click)
1. Right-click `disable_virtual_camera.reg` and select **Merge** (or double-click the file).
2. Click **Yes** on the User Account Control (UAC) prompt.
3. Click **OK** to confirm the registry update.
4. Restart your camera application or reboot the system for changes to take effect.

### Method 2: PowerShell (Administrator)
Run the following commands in an elevated PowerShell session:
```powershell
# Remove from 64-bit registry path
Remove-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows Media Foundation\Platform" -Name "EnableFrameServerMode" -ErrorAction SilentlyContinue

# Remove from 32-bit WOW64 registry path
Remove-ItemProperty -Path "HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows Media Foundation\Platform" -Name "EnableFrameServerMode" -ErrorAction SilentlyContinue

Write-Host "Frame Server mode successfully disabled." -ForegroundColor Green
```

### Reverting to Default
If you wish to restore the default Windows Media Foundation Frame Server behavior:
```powershell
Set-ItemProperty -Path "HKLM:\SOFTWARE\Microsoft\Windows Media Foundation\Platform" -Name "EnableFrameServerMode" -Value 1 -Type DWord
Set-ItemProperty -Path "HKLM:\SOFTWARE\WOW6432Node\Microsoft\Windows Media Foundation\Platform" -Name "EnableFrameServerMode" -Value 1 -Type DWord
```
