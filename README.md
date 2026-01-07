
# Automated DISA STIG Compliance & Remediation for Windows 11

## ⚙️ Environment and Tooling

- **Platform:** Microsoft Azure – Windows 11 Pro Virtual Machine  
- **Security Scanning:** Tenable.io / Nessus  
- **Automation & Scripting:** PowerShell (v7 and above)  
- **Benchmark:** DISA STIG for Windows 11 (v2, Release 4)

---

## 📘 Project Description

### Understanding DISA STIG
**DISA Security Technical Implementation Guides (STIGs)** are hardening benchmarks issued by the Defense Information Systems Agency. They define mandatory security configurations for systems used within U.S. Department of Defense–aligned environments.

### Purpose of STIG Enforcement
Implementing STIG controls significantly strengthens system security by enforcing restrictive and well-tested configuration standards. This reduces exposure to common attack vectors and misconfigurations.

### Consequences of Failing Compliance
- **Increased Attack Surface:** Misconfigured systems are more susceptible to privilege escalation, credential compromise, and post-exploitation techniques.
- **Compliance Gaps:** Failure to meet STIG requirements can result in unsuccessful audits tied to frameworks such as NIST 800-53, CMMC, or FISMA.
- **Business & Mission Impact:** Persistent non-compliance may cause loss of ATO, financial repercussions, or disqualification from government-related work.

---

## 🔄 End-to-End Process: Assessment to Remediation

This project follows a structured approach to building a secure test environment, assessing its compliance posture, and automating remediation actions.

### Stage 1: VM Deployment & Initial Configuration
1. **Virtual Machine Creation:** Provision a Windows 11 Pro VM through the Azure Portal.
2. **Connectivity Preparation:** For initial authenticated scanning, the Windows Defender Firewall is *temporarily* disabled. Azure NSG rules are adjusted to allow scanner communication.

### Stage 2: Compliance Scanning with Tenable
1. Access the Tenable.io console.
2. Go to **Scans** → **New Scan** → **Advanced Network Scan**.
3. **Scan Settings:**
   - **Scanner Engine:** Assign the appropriate local or cloud scanner (e.g., `LOCAL-SCAN-ENGINE-01`).
   - **Scan Target:** Specify the VM’s private IP address.
   - **Authentication:** Under *Credentials*, configure Windows administrative credentials to enable authenticated checks.
4. **Compliance Profile Selection:**
   - Open the *Compliance* section.
   - Add **DISA STIG – Windows 11 v2r4**.
5. **Performance Tuning (Optional):**
   - Disable unrelated vulnerability plugins.
   - Enable **Policy Compliance** only.
   - Limit checks to **Windows Compliance** to reduce scan duration.
6. **Run Scan:** Execute the scan to capture the initial compliance baseline.

### Stage 3: Findings Review & Automated Fixes
1. **Result Evaluation:** Examine failed STIG controls, prioritizing high and medium severity findings.
2. **Automation:** Apply remediation using custom PowerShell scripts mapped to specific STIG IDs.
3. **Validation:** Re-scan the system to verify remediation success and confirm compliance improvements.

## 📊 Baseline Scan Results

<img width="1915" height="719" alt="image" src="https://github.com/user-attachments/assets/86384c75-c250-45ac-9f90-fbda12c6c72b" />


## 🛡️ STIG Controls Selected for Automated Remediation

The STIG findings listed below were flagged as high-priority compliance failures during the initial scan and were remediated using PowerShell-based automation.

| STIG ID | Control Description | Severity | Security Rationale |
| :--- | :--- | :--- | :--- |
| [**WN11-CC-000038**](https://github.com/aycasanli8/Windows-11-Pro-STIGS-Implementation/blob/main/WN11-CC-000038.md) | Disable WDigest authentication | High | Eliminates the risk of plaintext credentials being cached in LSASS, reducing exposure to credential dumping tools such as Mimikatz. |
| [**WN11-CC-000326**](https://github.com/aycasanli8/Windows-11-Pro-STIGS-Implementation/blob/main/WN11-CC-000326.md) | Enable PowerShell Script Block Logging | High | Improves detection of malicious or suspicious PowerShell execution, including living-off-the-land attack techniques. |
| [**WN11-CC-000327**](https://github.com/aycasanli8/Windows-11-Pro-STIGS-Implementation/blob/main/WN11-CC-000327.md) | Enable PowerShell Transcription | High | Captures full PowerShell session transcripts, providing valuable forensic evidence during investigations. |
| [**WN11-CC-000345**](https://github.com/aycasanli8/Windows-11-Pro-STIGS-Implementation/blob/main/WN11-CC-000345.md) | Disable Basic authentication for WinRM | High | Protects against credential exposure by blocking insecure authentication methods commonly abused for lateral movement. |
| [**WN11-CC-000350**](https://github.com/aycasanli8/Windows-11-Pro-STIGS-Implementation/blob/main/WN11-CC-000350.md) | Enforce encrypted WinRM traffic | High | Prevents interception or manipulation of remote management sessions over the network. |
| [**WN11-SO-000120**](https://github.com/aycasanli8/Windows-11-Pro-STIGS-Implementation/blob/main/WN11-SO-000120.md) | Require SMB server packet signing | High | Mitigates SMB relay and man-in-the-middle attacks by enforcing message integrity. |
| [**WN11-SO-000100**](https://github.com/aycasanli8/Windows-11-Pro-STIGS-Implementation/blob/main/WN11-SO-000100.md) | Require SMB client packet signing | High | Strengthens SMB communications by ensuring authentication and integrity on the client side. |
| [**WN11-CC-000270**](https://github.com/aycasanli8/Windows-11-Pro-STIGS-Implementation/blob/main/WN11-CC-000270.md) | Prevent RDP client from saving passwords | Medium | Reduces the risk of credential harvesting following host compromise. |
| [**WN11-CC-000280**](https://github.com/aycasanli8/Windows-11-Pro-STIGS-Implementation/blob/main/WN11-CC-000280.md) | Force RDP to always prompt for credentials | Medium | Ensures credentials are manually entered, preventing reuse by automated malware. |
| [**WN11-CC-000310**](https://github.com/aycasanli8/Windows-11-Pro-STIGS-Implementation/blob/main/WN11-CC-000310.md) | Restrict users from altering installation settings | Medium | Prevents configuration changes that could be leveraged to bypass security controls or weaken system posture. |

