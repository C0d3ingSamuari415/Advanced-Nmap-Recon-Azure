# Advanced-Nmap-Recon-Azure
Deep-dive Nmap recon against an Azure VM: service/version detection, aggressive scan, full 1–65535 port sweep, SSL/NTLM enumeration, and cloud OS fingerprint analysis. Includes threat modeling and SOC interpretation of exposed RDP and DNS services. 

1. Service & OS Detection Scan (nmap -sV -O)
   Screenshot showing the initial Nmap service and OS detection scan. This output identifies open ports, service versions, and OS fingerprint data. It confirms that RDP (3389/tcp) is exposed and provides early indicators of the underlying Windows Server environment.

2. Aggressive Scan (nmap -A)
   Screenshot of the aggressive Nmap scan including script results, SSL certificate details, NTLM information, and traceroute. This scan reveals deeper host metadata such as the RDP certificate, Windows Server version (10.0.20348), and network path, which are valuable for SOC-level reconnaissance.

3. Full Port Sweep (nmap -p- -T4)
   Screenshot showing the full 1–65535 port sweep. This confirms that only ports 53/tcp (DNS) and 3389/tcp (RDP) are open, with all other ports filtered by Azure’s network controls. This validates the VM’s external attack surface and supports the risk assessment.

4. NTLM & Certificate Enumeration (from -A scan)
   Screenshot highlighting NTLM domain/computer names and RDP certificate details extracted during the aggressive scan. This information helps identify the host OS, validate service exposure, and understand how attackers might fingerprint the system.

5. OS Fingerprint Mismatch (Cloud Artifact)
   Screenshot showing Nmap’s OS fingerprint confusion, where the scan reports Linux-like signatures despite the host being Windows Server. This demonstrates how cloud virtualization layers can distort OS detection and why service banners are more reliable.
