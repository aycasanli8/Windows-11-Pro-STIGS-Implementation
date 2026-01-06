
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
