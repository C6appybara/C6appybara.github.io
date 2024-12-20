---
title: "AMSI Bypass. Using MinHook:"
date: 2024-12-04 10:00:00 +0000
categories: [blog, tutorial]
tags: [Malware, Windows, Bypass]
toc: true
---

## Bypassing Amsi Using C & MinHook

Powershell can be a very powerful tool and can be used as a `LOLBin` which is used by attackers to stay stealthy on the victim's machine. The only downside with using powershell for malicious tasks is bloody `AMSI (Anti Malware Scan Interface)`.


## How Amsi Scans and Detects:
**Amsi**, like other antimalware solutions, uses signature based detections to defend against malicious scripts. But Amsi does not use the same database that other installed antimalware vendors use, it uses and maintains its own signature database.

Below is a diagram of `AMSI`'s architecture and how it operates. This along with Microsofts documentation on it's architecture [here](https://learn.microsoft.com/en-us/windows/win32/amsi/how-amsi-helps) can give you a more in depth understanding.
![Amsi Diagram](https://github.com/user-attachments/assets/adcb991b-a123-4c7c-8653-14f686f56890)
As we can see in the diagram, AMSI WinApi's are acquired from `amsi.dll`. This DLL is loaded into the virtual memory of a target process to initiate a scan of a command.

Here are some WinApi's exported from `amsi.dll`:
- **[AmsiOpenSession](https://learn.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiopensession)** - This function will open a session with AMSI to enable scanning and analysis of content for potential threats.
- **[AmsiScanBuffer](https://learn.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiscanbuffer)** - This function will scan a given buffer for malicious code.
- **[AmsiScanString](https://learn.microsoft.com/en-us/windows/win32/api/amsi/nf-amsi-amsiscanstring)** - This function will scan a given string to find possible malicious signatures.
