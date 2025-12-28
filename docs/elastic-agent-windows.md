# Elastic Agent Setup (Windows Server 2019)

This guide documents how to install and enroll the Elastic Agent on **Windows Server 2019**, connecting it securely to a **Fleet Server over TLS** using a manually trusted internal Certificate Authority (CA).

This setup simulates real-world enterprise scenarios where agents must verify the server’s identity via a trusted root CA.

---

## Prerequisites

- Windows Server 2019 VM or machine
- Fleet Server running and reachable over HTTPS (`https://fleet-server.lab.local:8220`)
- Enrollment token from Kibana
- `ca.crt` from internal Certificate Authority (used to sign the Fleet Server cert)

---

## Tools Used

- Elastic Agent `.msi` or `.exe` installer (downloaded from Elastic)
- Windows Certificate Management Console
- PowerShell or CMD

---

## Installation Steps

### Step 1: Download Elastic Agent for Windows

Download from [Elastic Downloads]:

---

### Step 2: Import CA Certificate into Windows Root Store

1. Open `certlm.msc` (Local Machine Certificate Manager)
2. Navigate to: **Trusted Root Certification Authorities → Certificates**
3. Right-click → **All Tasks → Import**
4. Select your `ca.crt` file and complete the wizard

---

### Step 3: Install and Enroll Elastic Agent

In PowerShell or CMD, run the following:

```powershell
ElasticAgent.exe install `
  --url=https://fleet-server.lab.local:8220 `
  --enrollment-token=<your-token> `
  --certificate-authorities="C:\Path\To\ca.crt"
```
You should see confirmation that the agent was successfully installed and enrolled.

---

### Step 4: Verify Agent is Running

Open Services and confirm Elastic Agent is listed as Running.
Or run:
```powershell
Get-Service "Elastic Agent"
```

### Verify in Kibana

1. Go to Kibana → Fleet → Agents
2. Confirm your Windows Server appears as Healthy
3. Click into the agent to see details like hostname, IP, integrations, and logs

### Notes

1. The CA must be imported as a Trusted Root on the Windows machine.
2. The hostname in the --url parameter must match the name in the Fleet Server’s certificate.
3. Logs can be viewed via Kibana's Observability → Logs or the Agent tab.

### Screenshots

All steps are documented in:
/images/elastic-agent-windows/

### Related Docs
```bash
Fleet Server Setup
Elastic Agent - Linux
Certificate Authority Setup
```
