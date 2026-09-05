# Juniper EX3400 Day-0 Automation Scripts (Tera Term)

Automated **Day-0 provisioning**, maintenance, and recovery scripts for **Juniper EX3400 Series Switches** using **Tera Term Macro Language (`.ttl`)** over serial console connection.

---

## 📌 Overview

Deploying new out-of-the-box (OOTB) or redeploying decommissioned network switches typically requires repetitive manual tasks over a serial console (e.g., aborting auto-ZTP, resetting root credentials, checking hardware status, and extracting diagnostic logs).

This project automates standard onboarding procedures to:
- Eliminate human error during initial configuration.
- Accelerate mass staging and bench provisioning in labs, warehouses, and remote sites.
- Standardize diagnostic evidence collection with timestamped session logs.

---

## 📂 Repository Structure

```text
.
└── juniper/
    ├── ex3400_ootb_clear-ztp.ttl       # Aborts ZTP and sets clean baseline configuration
    ├── ex3400_password_recovery.ttl    # Automates root password recovery via console
    └── ex3400_export-log.ttl           # Collects operational states and exports timestamped logs
```

### Script Descriptions

| Script Name | Scope | Description |
| :--- | :--- | :--- |
| **`ex3400_ootb_clear-ztp.ttl`** | **Day-0 Provisioning** | Stops persistent Zero Touch Provisioning (ZTP) processes, clears factory default prompts, and leaves the switch ready for initial template deployment. |
| **`ex3400_password_recovery.ttl`** | **Recovery & Maintenance** | Automates the boot loader sequence into single-user / recovery mode to reset the root password without wiping the existing Junos configuration. |
| **`ex3400_export-log.ttl`** | **Verification & Auditing** | Connects to the switch, runs standard health/diagnostic show commands, and saves output to a structured log file tagged with a date-time stamp (`YYYYMMDD_HHMMSS`). |

---

## ⚙️ Prerequisites

- **OS:** Windows 10 / 11
- **Software:** [Tera Term](https://osdn.net/projects/ttssh2/) (v4.xx or v5.xx)
- **Hardware:**
  - USB-to-Serial Console Cable (RJ-45 or DB9 to USB)
  - Juniper Networks EX3400 Switch

---

## 🚀 Configuration & Customization

Before executing the scripts, open the `.ttl` files with any text editor (e.g., Notepad, VS Code) to match your local setup:

### 1. COM Port & Baud Rate
By default, the connection string targets **COM4** at **9600 baud**:
```ini
; Update '/C=4' to match your assigned COM port in Device Manager
connect '/C=4 /BAUD=9600'
```

### 2. Credentials
Ensure the password variable is sanitized or set according to your environment:
```ini
pass = 'YourSecurePassword'
```

### 3. Log Output Path (in `ex3400_export-log.ttl`)
Specify a generic or local directory to avoid path-not-found errors:
```ini
; Recommended: Use a dedicated logs directory (e.g., C:\Logs\ or D:\NetworkLogs\)
sprintf2 logfile 'C:\Logs\EX3400_Log_%s%s%s_%s%s%s.txt' year month day hour min sec
```

---

## 💻 Usage

### Method 1: Using the Tera Term GUI
1. Connect your console cable from your PC to the switch's CONSOLE port.
2. Open Tera Term.
3. In the top menu, go to **Control** > **Macro**.
4. Select the desired `.ttl` macro (e.g., `ex3400_ootb_clear-ztp.ttl`).

### Method 2: Command Line / Batch Launcher
You can trigger execution via the Windows Command Prompt or a `.bat` shortcut:
```cmd
"C:\Program Files (x86)\teraterm\ttpmacro.exe" "juniper\ex3400_export-log.ttl"
```

---

## 🔒 Security Best Practices

- **Never Commit Real Passwords:** Keep `pass = 'xxx'` or use placeholders before pushing code to version control.
- **Sanitize Local Paths:** Avoid hardcoding local usernames or machine-specific paths (e.g., `C:\Users\<username>\...`) in public repositories.
- **Access Control:** Restrict physical and console access to switches during automated password recovery operations.

---

## 📄 License

This repository is distributed under the [MIT License](LICENSE).
