<p align="left">
  <img src="https://img.shields.io/badge/Tool-Nmap-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Cloud-Azure-0089D6?style=for-the-badge" />
  <img src="https://img.shields.io/badge/OS-Windows%20Server%202022-0078D4?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Category-Network%20Security-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Focus-SOC%20Analysis-yellow?style=for-the-badge" />
  <img src="https://img.shields.io/badge/MITRE%20ATT%26CK-Mapped-red?style=for-the-badge" />
</p>


Advanced-Nmap-Recon-Azure:
   Deep‑dive Nmap reconnaissance against an Azure-hosted Windows Server 2022 VM. This project includes service/version detection, aggressive scanning, full 1–65535 port sweep, SSL/NTLM enumeration, and cloud OS fingerprint analysis. Findings are mapped to MITRE ATT&CK and interpreted from a SOC analyst perspective.

Overview:
   This project performs advanced external reconnaissance against an Azure Windows Server 2022 virtual machine using Nmap. The objective is to identify exposed services, analyze risk, understand cloud OS fingerprint inconsistencies, and interpret results through a SOC analyst lens. This simulates the reconnaissance phase of an external threat actor and demonstrates defensive analysis skills.

Scan Commands:
   nmap -sV -O <IP>
   nmap -A <IP>
   nmap -p- -T4 <IP>


Findings (SOC Analysis):
Port 3389/tcp — RDP
Service: Microsoft Terminal Services
Version: Windows Server 2022 (10.0.20348)
Risk: High
Why: Exposes remote access to the internet
MITRE ATT&CK:
   T1133 — External Remote Services
   T1021 — Remote Services
   T1110 — Brute Force
SOC Notes:  
RDP is one of the most commonly attacked services in cloud environments. Attackers frequently attempt brute-force authentication or exploit weak configurations. Monitoring Event IDs 4625, 4624, 4778, and 4779 is essential.

Port 53/tcp — DNS:
Service: Domain (DNS)
Risk: Medium
Why: DNS exposure can enable tunneling or data leakage
MITRE ATT&CK:
   T1071.004 — Application Layer Protocol: DNS
SOC Notes:  
DNS over TCP is uncommon in normal Windows Server configurations. Misconfigurations may allow tunneling or exfiltration. Monitor for abnormal DNS queries or large TXT records.

OS Fingerprint Mismatch (Cloud Artifact):
Nmap reports Linux-like OS signatures
Service banners confirm Windows Server 2022
Insight:  
   Cloud virtualization layers (Azure hypervisor, load balancers, NAT) distort OS fingerprinting. Service banners and         protocol responses provide more reliable host identification.

NTLM & Certificate Enumeration:
Extracted NTLM domain/computer names
Identified Windows Server version (10.0.20348)
RDP certificate details confirm host identity and exposure
Value:  
   This metadata helps attackers profile the system and tailor exploits — and helps defenders understand what information      is externally visible.

Recommendations:
Restrict RDP using Network Security Group (NSG) rules
Enable Just‑In‑Time (JIT) VM access
Disable DNS exposure if not required
Monitor authentication logs for brute-force attempts
Apply Microsoft Defender for Cloud hardening recommendations
Consider placing the VM behind a VPN or Bastion host

Screenshots:

Service & OS Detection Scan (nmap -sV -O)
Screenshot showing the initial Nmap service and OS detection scan. This output identifies open ports, service versions, and OS fingerprint data. It confirms that RDP (3389/tcp) is exposed and provides early indicators of the underlying Windows Server environment.

Aggressive Scan (nmap -A)
Screenshot of the aggressive Nmap scan including script results, SSL certificate details, NTLM information, and traceroute. This scan reveals deeper host metadata such as the RDP certificate, Windows Server version (10.0.20348), and network path, which are valuable for SOC-level reconnaissance.

Full Port Sweep (nmap -p- -T4)
Screenshot showing the full 1–65535 port sweep. This confirms that only ports 53/tcp (DNS) and 3389/tcp (RDP) are open, with all other ports filtered by Azure’s network controls. This validates the VM’s external attack surface and supports the risk assessment.

NTLM & Certificate Enumeration
Screenshot highlighting NTLM domain/computer names and RDP certificate details extracted during the aggressive scan. This information helps identify the host OS, validate service exposure, and understand how attackers might fingerprint the system.

OS Fingerprint Mismatch (Cloud Artifact)
Screenshot showing Nmap’s OS fingerprint confusion, where the scan reports Linux-like signatures despite the host being Windows Server. This demonstrates how cloud virtualization layers can distort OS detection and why service banners are more reliable.
