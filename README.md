# Vulnerability Assessment Lab using Nessus Essentials
Vulnerability Management With Nessus

I set up this lab to get real hands-on experience with vulnerability scanning. Reading about it is one thing — actually running scans, looking at the results, and tracing findings back to misconfigurations is a completely different experience.

The core idea was to run the same target through two scans under different conditions, and see how much the results change. Spoiler: they change a lot.

## 1. Project Overview

This project demonstrates a **basic vulnerability assessment** performed on a Windows machine using Nessus.

The goal of this lab was to:

* Perform a **non-credentialed scan** to simulate an external attacker.
* Perform a **credentialed scan** to identify deeper system vulnerabilities.
* Analyze detected vulnerabilities.
* Apply remediation steps and understand how to reduce risk.

The scan target was a **Microsoft Windows 10 virtual machine** running inside a lab environment.

---

# 2. Lab Environment

| Component                       | Purpose                 |
| ------------------------------- | ----------------------- |
| Nessus Essentials               | Vulnerability scanner   |
| VMware Workstation / VirtualBox | Virtual lab environment |
| Microsoft Windows 10 VM         | Target machine          |

Network configuration was kept inside a **local virtual network** to safely perform scanning.

---

# 3. Initial Vulnerability Scan (Non-Credentialed)

The first scan was performed **without providing system credentials**.
This simulates what an **external attacker** can see from the network.

**Results**

* Total vulnerabilities detected: **18**
* Severity distribution:

  * Medium
  * Low
  * Informational  

<img width="1600" height="900" alt="Win 10 signle host" src="https://github.com/user-attachments/assets/e94dc4c6-6f42-4a12-a4bb-5cb79dd7093e" />  


Explanation:  

The scanner was able to identify open services and configuration issues such as SMB settings and ICMP responses. Since the scan did not have login access, the visibility was limited to externally detectable issues.

---

# 4. Credentialed Vulnerability Scan

Next, a **credentialed scan** was performed by providing valid Windows credentials to Nessus.

This allows the scanner to inspect:

* system configurations
* installed software
* registry settings
* patch levels

**Results**

* Total vulnerabilities detected: **33**  

<img width="1600" height="900" alt="Win Sanning with Credential" src="https://github.com/user-attachments/assets/041d9d39-f7f6-49b9-8667-04433f5bfae2" />  
  
Explanation:  

The credentialed scan identified **more vulnerabilities compared to the initial scan** because the scanner had deeper access to the system configuration.

This demonstrates why **credentialed scanning is recommended in enterprise vulnerability management**.

---

# 5. Vulnerability Analysis

Below are some of the key vulnerabilities identified during the scan.

---

## 5.1 SMB Signing Not Required

Severity: **Medium**
CVSS Score: **5.3**

<img width="1600" height="900" alt="win 10 signle Host - vulu SMB" src="https://github.com/user-attachments/assets/e241f564-7454-4632-b489-91727fdba142" />  

### Explanation

SMB signing is a security feature used to ensure that SMB communication between systems is authenticated and protected.

If SMB signing is not required, attackers may perform **Man-in-the-Middle attacks** and potentially modify network traffic between systems.

### Risk

An attacker on the same network could intercept or manipulate SMB communication.

---

## 5.2 ICMP Timestamp Request Response

Severity: **Low**

### Explanation

The system responds to ICMP timestamp requests, which can allow attackers to gather information about system time and network configuration.

### Risk

Although this vulnerability has a low impact, it can help attackers collect **information useful for reconnaissance**.

---

# 6. Remediation

To reduce the security risks identified in the scan, the following remediation steps were considered.

---

## Fixing SMB Signing Issue

On the Windows system:

1. Open **Local Security Policy**
2. Navigate to:

```
Local Policies → Security Options
```

3. Enable the following policy:

```
Microsoft network server: Digitally sign communications (always)
```

This enforces SMB message signing and prevents certain **Man-in-the-Middle attacks**.

---

## Disabling ICMP Timestamp Responses

ICMP timestamp responses can be restricted using firewall rules or network configuration settings to limit unnecessary information exposure.

---

# 7. Key Learning Outcomes


A few things that clicked doing this that didn't fully make sense just reading about them:

- The credentialed vs. non-credentialed gap is bigger than you'd expect — it's not a minor difference
- Misconfigurations like SMB signing are often more immediately exploitable than missing patches
- CVSS scores give you a starting point, but context determines what actually needs fixing first
- Remediation for common findings is usually simpler than it sounds

---

# 8. Conclusion

This project demonstrates how vulnerability scanning tools like Nessus Essentials can help identify security weaknesses in systems.

By analyzing scan results and applying remediation steps, organizations can **reduce attack surface and improve overall security posture**.


---
