# Router-Hack

⚠️ **Disclaimer**:  
This project is for educational purposes only. I am **not responsible** if you brick your router during the process.

---

## Overview
This repository contains a Python tool (`nokia-router-cfg-tool.py`) and instructions for unlocking hidden settings on certain Nokia routers.  
It allows you to:
- Export and decrypt router config files  
- Enable Telnet/SSH access  
- Gain root access via terminal  
- Unlock ISP-restricted settings  

---

## Requirements
- A Nokia router (tested on Airtel routers)  
- Windows PC with [Python](https://www.python.org/downloads/) installed  
- [MobaXterm Portable](https://mobaxterm.mobatek.net/download.html)  

---

## Steps

### 1. Export Router Config
1. Go to `192.168.1.1` in your browser.  
   - Default username: `admin`  
   - Default password: `admin`  
2. Navigate to **Maintenance → Backup and Restore → Export Config File**.  
3. Save it as `config.cfg` on your desktop.

---

### 2. Setup Environment
1. Install Python.  
2. Place `nokia-router-cfg-tool.py` on your desktop.  
3. Open `cmd` and run:
   ```bash
   python C:\Users\XXXXX\Desktop\nokia-router-cfg-tool.py
````

*(replace `XXXXX` with your Windows username)*

---

### 3. Decrypt and Unpack Config

1. Decrypt key:

   ```bash
   python nokia-router-cfg-tool.py -d OYdLWUVDdKQTPaCIeTqniA==
   ```
2. Unpack config:

   ```bash
   python nokia-router-cfg-tool.py -u config.cfg
   ```
3. A `.xml` file will be created on your desktop.

---

### 4. Edit Config

1. Open the XML file in any text editor.
2. Search (`Ctrl+F`) for **TelnetSshAccount**.
3. Modify values as below:

   ```xml
   <TelnetSshAccount. n="TelnetSshAccount" t="staticObject">
     <Enable rw="RW" t="boolean" v="True"></Enable>
     <UserName ml="64" rw="RW" t="string" v="admin"></UserName>
     <Password ml="64" rw="RW" t="string" v="OYdLWUVDdKQTPaCIeTqniA==" ealgo="ab"></Password>
   </TelnetSshAccount.>
   ```
4. Save and close the file.

---

### 5. Repack Config

Run:

```bash
python nokia-router-cfg-tool.py -ple config-XXXXXXX-XXXXXX.xml 0x4924ea42
```

A new `.cfg` file will be generated.

---

### 6. Import Config

1. Login to router (`192.168.1.1`).
2. Go to **Maintenance → Backup and Restore**.
3. Import the new `.cfg` file.
4. Wait for the router to reboot.

---

### 7. Enable Telnet & SSH

1. Login again (`192.168.1.1`).
2. Go to **Security → Access Control**.
3. Allow Telnet and SSH for both **WAN** and **LAN**.

---

### 8. Gain Root Access

1. Download and run [MobaXterm Portable](https://mobaxterm.mobatek.net).
2. Start a local terminal.
3. Connect to router:

   ```bash
   telnet 192.168.1.1
   ```

   * Username: `admin`
   * Password: `admin`
4. Run:

   ```bash
   enable
   shell
   ```

   When asked for `password2`, paste this and hit enter:

   ```bash
   '; /bin/sh; #
   ```

   You now have **root access**.

---

### 9. Useful Commands

* Take backup of Airtel settings:

  ```bash
  cfgcli dump
  ritool dump
  ```

  *(Save the output via terminal → Save text)*

* Unlock settings:

  ```bash
  ritool set OperatorID ALCL
  ```

---

### 10. Post-Unlock Notes

* If staying on Airtel Fiber → leave as is.
* If switching ISPs → do a factory reset.

  * New login will be:

    * IP: `192.168.1.254`
    * Username: `AdminGPON`
    * Password: `ALC#FGU`

---

## Author

Tested and documented by **Arsh**.
Successfully unlocked **3 routers** using this method.

---

## License

This project is provided **AS-IS** with no warranty. Educational use only.
