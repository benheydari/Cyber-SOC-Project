# SSH Brute Force Detection with Microsoft Sentinel

A comprehensive guide to detecting and responding to SSH brute force attacks using Microsoft Sentinel and Linux virtual machines.

---

## Table of Contents
- [Objective](#objective)
- [What You'll Learn](#what-youll-learn)
- [Prerequisites](#prerequisites)
- [Infrastructure Setup](#infrastructure-setup)
- [Attack Simulation](#attack-simulation)
- [Creating Detection Rules](#creating-detection-rules)
- [Automated Response](#automated-response)
- [Validation and Testing](#validation-and-testing)
- [Understanding the Results](#understanding-the-results)
- [Troubleshooting](#troubleshooting)

---

## Objective

### What is SSH Brute Force?
SSH (Secure Shell) brute force attacks are one of the most common methods attackers use to gain unauthorized access to Linux systems. In these attacks, malicious actors attempt to guess usernames and passwords by trying thousands or even millions of combinations until they find valid credentials.

**Real-world impact:**
- Compromised servers can be used for cryptocurrency mining
- Attackers can steal sensitive data
- Systems can be used as launch points for other attacks (lateral movement)
- Ransomware deployment

### Project Goal
In this project, you will:
1. **Simulate** a real-world SSH brute force attack in a safe, controlled environment
2. **Detect** the attack by analyzing authentication logs using Microsoft Sentinel
3. **Create automated alerts** that trigger when suspicious activity is detected
4. **Configure automated responses** to handle security incidents efficiently

This hands-on experience demonstrates critical skills for SOC (Security Operations Center) analysts and cyber security professionals.

---

## What You'll Learn

By completing this project, you will gain practical experience with:
- **SIEM Configuration**: Setting up Microsoft Sentinel for security monitoring
- **Log Analysis**: Understanding Linux authentication logs (Syslog)
- **KQL (Kusto Query Language)**: Writing queries to detect security threats
- **Incident Response**: Creating automated workflows to handle security alerts
- **Attack Simulation**: Safely recreating real-world attack scenarios
- **Security Operations**: Following the full incident detection and response lifecycle

---

## Prerequisites

Before starting, ensure you have:

### Required
- **Azure Account** with an active subscription (free trial works)
- **Microsoft Sentinel Workspace** already configured
- **Linux Virtual Machine** (Ubuntu or similar) with:
  - SSH server installed and running
  - Log forwarding configured to send logs to Sentinel
  - Network connectivity (able to receive SSH connections)
- **A second machine** (can be Windows, Linux, or Mac) to simulate attacks from

### Knowledge Prerequisites
- Basic understanding of SSH
- Familiarity with command-line interfaces
- Basic Azure portal navigation
- Understanding of what SIEM (Security Information and Event Management) systems do

### Optional (Helpful)
- Understanding of networking concepts (IP addresses, ports)
- Familiarity with Linux log files
- Basic knowledge of authentication mechanisms

---

## Infrastructure Setup

### Architecture Overview
┌─────────────────┐         ┌──────────────────┐         ┌─────────────────────┐
│   Attacker VM   │────SSH──>│   Target Linux   │────Logs─>│ Microsoft Sentinel  │
│  (Your PC or    │  Failed │      VM          │         │   (Azure Cloud)     │
│  another VM)    │ Attempts│  (Ubuntu/CentOS) │         │                     │
└─────────────────┘         └──────────────────┘         └─────────────────────┘
                                     │                              │
                                     │                              │
                                     └──────Analytics Rules─────────┘
                                                 │
                                                 ▼
                                          ┌─────────────┐
                                          │  Incidents  │
                                          │  & Alerts   │
                                          └─────────────┘
### Component Breakdown

#### 1. **Target Linux VM**
- **Purpose**: The system being attacked (victim)
- **Role**: Receives SSH login attempts and generates authentication logs
- **Requirements**:
  - SSH server (OpenSSH) running on port 22
  - Azure Monitor Agent (AMA) or Log Analytics agent installed
  - Syslog daemon configured to forward auth logs

**To verify SSH is running:**
```bash
sudo systemctl status ssh
# Should show "active (running)"
```

**To check if logs are being generated:**
```bash
sudo tail -f /var/log/auth.log
# OR on some systems:
sudo tail -f /var/log/secure
```

#### 2. **Attacker Machine**
- **Purpose**: Simulate malicious SSH brute force attempts
- **Role**: Generates failed login attempts to trigger detection
- **Requirements**:
  - SSH client installed (built-in on Linux/Mac, available on Windows)
  - Network connectivity to target VM
  - Ability to run command-line tools

**To test connectivity before attack:**
```bash
ping <target-vm-ip>
# Should receive replies
```

#### 3. **Microsoft Sentinel Workspace**
- **Purpose**: Centralized security monitoring and incident response platform
- **Role**: 
  - Collects logs from Linux VM
  - Analyzes logs for suspicious patterns
  - Creates incidents when threats are detected
  - Triggers automated responses
  
**Log Flow:**
```
Linux VM → Syslog → Azure Monitor Agent → Log Analytics Workspace → Sentinel Analytics → Incidents
```

### Network Configuration

**Important networking notes:**

1. **Firewall Rules**: Ensure your Linux VM's Network Security Group (NSG) in Azure allows:
   - Port 22 (SSH) inbound from your attacker machine's IP
   - Outbound connectivity to Azure Monitor

2. **IP Addressing**: 
   - Note your Linux VM's IP address: `ip addr show` or `hostname -I`
   - This is what you'll use for SSH attempts
   - Example: `192.168.1.109` (private network) or `20.x.x.x` (Azure public IP)

3. **DNS Resolution** (optional but helpful):
   - You can use IP addresses directly
   - No need for domain names in this lab

---

## Attack Simulation

### Understanding the Attack

In a real SSH brute force attack:
- Attackers use automated tools (like Hydra, Medusa, or custom scripts)
- Thousands of username/password combinations are tried per minute
- Attacks often come from botnets (multiple IPs) to avoid detection
- Common usernames targeted: root, admin, user, test, ubuntu

**For our lab, we'll manually simulate this** to understand the detection mechanism.

### Step-by-Step Attack Execution

#### Step 1: Identify Your Target

From your attacker machine (Windows PC, another VM, or even Mac), identify the target:

```bash
# Find the target VM's IP address
# On the Linux VM, run:
ip addr show
# or
hostname -I
```

Example output: `192.168.1.109`

#### Step 2: Verify SSH Connectivity

Before attempting the attack, verify you can reach the SSH service:

```bash
# Test connection (from attacker machine)
ssh admin@192.168.1.109

# You should see:
# "admin@192.168.1.109's password:"
# This confirms SSH is reachable
```

**If connection fails:**
- Check if SSH service is running on target: `sudo systemctl status ssh`
- Verify network connectivity: `ping 192.168.1.109`
- Check Azure NSG rules allow port 22
- Verify correct IP address

#### Step 3: Execute Failed Login Attempts

**Manual Method (Recommended for Learning):**

Run this command **5-10 times**, entering wrong passwords each time:

```bash
ssh admin@192.168.1.109
```

**Example interaction:**
```
C:\> ssh admin@192.168.1.109
admin@192.168.1.109's password: [type: wrongpassword1]
Permission denied, please try again.

C:\> ssh admin@192.168.1.109
admin@192.168.1.109's password: [type: wrongpassword2]
Permission denied, please try again.

C:\> ssh admin@192.168.1.109
admin@192.168.1.109's password: [type: wrongpassword3]
Permission denied, please try again.

# Repeat 5-10 times
```

**Important timing notes:**
- Space out attempts by a few seconds (mimics human attacker)
- Don't need to wait between each attempt (bots don't wait)
- **For faster testing**, try at least 5 attempts within 5 minutes (to match our detection query time window)

#### Step 4: What's Happening Behind the Scenes

Every failed SSH attempt creates a log entry on the Linux VM:

**On the Linux VM, you can watch logs in real-time:**
```bash
sudo tail -f /var/log/auth.log
# OR
sudo journalctl -u ssh -f
```

**Example log entries generated:**
```
Mar 11 10:30:15 ubuntu sshd[12345]: Failed password for admin from 192.168.1.100 port 54321 ssh2
Mar 11 10:30:22 ubuntu sshd[12346]: Failed password for admin from 192.168.1.100 port 54322 ssh2
Mar 11 10:30:28 ubuntu sshd[12347]: Failed password for admin from 192.168.1.100 port 54323 ssh2
```

**Key information captured:**
- Timestamp: `Mar 11 10:30:15`
- Service: `sshd` (SSH daemon)
- Event type: `Failed password`
- Username attempted: `admin`
- Source IP: `192.168.1.100` (attacker's IP)
- Source port: `54321`

These logs are forwarded to Azure Sentinel where our detection query will analyze them.


### Expected Results After Attack

After completing 5-10 failed SSH attempts:
- ✅ Failed authentication events logged on Linux VM
- ✅ Logs forwarded to Azure Log Analytics workspace
- ✅ Data available for querying in Sentinel (may take 1-5 minutes)
- ✅ Ready to create detection rule

**Verification:** Check if logs reached Sentinel by running this query in Logs:
```kql
Syslog
| where TimeGenerated > ago(15m)
| where SyslogMessage contains "Failed password"
| project TimeGenerated, Computer, SyslogMessage
```

---

## Creating Detection Rules

### Understanding Analytics Rules

**Analytics Rules** in Microsoft Sentinel are automated queries that:
1. Run on a schedule (e.g., every 5 minutes)
2. Search for specific threat indicators in your logs
3. Create alerts/incidents when suspicious activity is found
4. Can trigger automated responses (playbooks)

**Why we need them:**
- Manual log review is impossible at scale (millions of events per day)
- Real-time detection enables faster response
- Consistent detection logic (no human error)
- Automated incident creation for SOC team

### Step 1: Navigate to Analytics Rules

1. Open **Azure Portal** (portal.azure.com)
2. Go to **Microsoft Sentinel**
3. Select your workspace
4. In the left menu, click **Analytics**
5. Click **+ Create** → **Scheduled query rule**

<img width="940" height="524" alt="image" src="https://github.com/user-attachments/assets/1273cdf7-24a1-4192-8fbe-baf27ae424e6" />


---

### Step 2: Configure Rule Details

**General Tab:**

Fill in the following fields:

| Field | Value | Purpose |
|-------|-------|---------|
| **Name** | `SSH Brute Force Detection` | Identifies this rule in incidents |
| **Description** | `Detects multiple failed SSH login attempts from the same source IP within a 5-minute window, indicating a potential brute force attack` | Explains what the rule does |
| **Severity** | `Medium` | Indicates threat level (Low/Medium/High/Critical) |
| **MITRE ATT&CK** | `Credential Access - Brute Force (T1110)` | Maps to attack framework |
| **Status** | `Enabled` | Rule actively runs queries |

**Why Medium severity?**
- Not Critical: No confirmed breach (just attempts)
- Not Low: Indicates active attack targeting your systems
- Medium: Requires investigation but not immediate emergency

**MITRE ATT&CK Explanation:**
The MITRE ATT&CK framework is a knowledge base of adversary tactics. Credential Access (TA0006) → Brute Force (T1110) describes exactly what we're detecting.

<img width="940" height="454" alt="image" src="https://github.com/user-attachments/assets/b212ec56-c940-46c8-8aa9-e891307b3151" />


---

### Step 3: Configure the Detection Query

**Set rule logic Tab:**

This is the heart of your detection. Paste the following KQL query:

```kql
Syslog
| where SyslogMessage contains "Failed password"
| parse SyslogMessage with * "from " IPAddress " port" *
| summarize Attempts=count() by IPAddress, Computer, bin(TimeGenerated, 5m)
| where Attempts >= 5
```

**Query Breakdown (line by line):**

```kql
Syslog
```
- **What it does:** Selects the Syslog table (where Linux authentication logs are stored)
- **Why:** This is where SSH login attempts are recorded

```kql
| where SyslogMessage contains "Failed password"
```
- **What it does:** Filters to only events containing "Failed password"
- **Why:** We only care about failed attempts (successful logins are normal)
- **Example match:** `"Failed password for admin from 192.168.1.100 port 54321 ssh2"`

```kql
| parse SyslogMessage with * "from " IPAddress " port" *
```
- **What it does:** Extracts the attacker's IP address from the log message
- **How:** Looks for text after "from " and before " port"
- **Result:** Creates a new column called `IPAddress` with value `192.168.1.100`

**Visual example:**
```
Before: "Failed password for admin from 192.168.1.100 port 54321"
After:  IPAddress = "192.168.1.100"
```

```kql
| summarize Attempts=count() by IPAddress, Computer, bin(TimeGenerated, 5m)
```
- **What it does:** Counts failed attempts per IP address, per computer, in 5-minute time windows
- **Why:** Brute force attacks have many attempts in short time periods
- **bin(TimeGenerated, 5m):** Groups events into 5-minute buckets

**Example result:**
| IPAddress | Computer | TimeGenerated | Attempts |
|-----------|----------|---------------|----------|
| 192.168.1.100 | ubuntu-vm | 2026-03-11 10:30:00 | 8 |
| 192.168.1.50 | ubuntu-vm | 2026-03-11 10:25:00 | 2 |

```kql
| where Attempts >= 5
```
- **What it does:** Only alerts when 5 or more failed attempts detected
- **Why:** Reduces false positives (people occasionally mistype passwords)
- **Threshold rationale:** 5 attempts in 5 minutes is abnormal for legitimate users

**You can adjust this threshold:**
- Higher (e.g., 10): Fewer false positives, might miss slower attacks
- Lower (e.g., 3): More sensitive, might alert on legitimate typos

---

### Step 4: Configure Query Scheduling

**Query scheduling Tab:**

| Setting | Value | Explanation |
|---------|-------|-------------|
| **Run query every** | `5 minutes` | How often the query executes |
| **Lookup data from the last** | `5 minutes` | Time range of data to analyze |

**Why these values?**

**Run every 5 minutes:**
- Balances detection speed vs. system load
- Detects attacks within 5 minutes of occurrence
- More frequent = higher Azure costs
- For production, 5-15 minutes is typical

**Lookup last 5 minutes:**
- Matches our time window in the query (bin 5m)
- Prevents duplicate alerts
- Ensures we don't miss events between runs

**Important timing concept:**
```
10:00 - Query runs, checks 9:55-10:00 data
10:05 - Query runs, checks 10:00-10:05 data (new events only)
10:10 - Query runs, checks 10:05-10:10 data
```

<img width="940" height="396" alt="image" src="https://github.com/user-attachments/assets/f7acd3e1-f208-4834-a1ef-7fc00b3d7679" />

---

### Step 5: Set Alert Threshold

**Alert threshold Tab:**

| Setting | Value |
|---------|-------|
| **Generate alert when number of query results** | `Is greater than` |
| **Threshold** | `0` |

**What this means:**
- If the query returns **any results** (1 or more), create an alert
- Since our query already filters for `Attempts >= 5`, any result IS a threat

**Alert suppression:** Leave disabled for this lab
- In production, you might suppress duplicate alerts for same IP within 1 hour
- Prevents alert fatigue

---

### Step 6: Configure Incident Settings

**Incident settings Tab:**

**CRITICAL:** This step is commonly missed!

✅ **Enable:** `Create incidents from alerts generated by this analytics rule`

<img width="940" height="241" alt="image" src="https://github.com/user-attachments/assets/3fbe1efd-6add-4fa8-a6da-2d8b83b8ca1a" />



### Step 7: Configure Automated Response

**Automated response Tab:**

**Important Change (as of June 2023):**
You can **no longer add playbooks directly** in the analytics rule creation wizard. 

Instead, we'll create an **Automation Rule** after the analytics rule is created.

**For now:** 
- Leave this tab empty
- Click **Next**
- We'll set up automation in the next section

**What are automation rules?**
- Automation rules run when specific conditions are met
- Examples: When alert severity = High, when incident created, when incident status changes
- Actions: Assign to analyst, change severity, run playbook, add tags

---

### Step 8: Review and Create

**Review + create Tab:**

1. **Review all settings** - ensure everything is correct:
   - Query logic
   - Schedule (5 minutes / 5 minutes)
   - Incident creation enabled ✅

<img width="940" height="460" alt="image" src="https://github.com/user-attachments/assets/a3afad52-b7ef-44f6-ac4b-f569efdea4a5" />

   
2. **Click "Validation passed"** - Sentinel checks for errors

<img width="940" height="504" alt="image" src="https://github.com/user-attachments/assets/2a0f71ff-f825-4d8c-9093-e031d9d4024d" />

3. **Click "Create"** - Rule is now active!

<img width="940" height="531" alt="image" src="https://github.com/user-attachments/assets/318f52d8-71da-41c1-b562-82515294aad6" />


**What happens next:**
- Rule begins running every 5 minutes
- Analyzing Syslog data for failed SSH attempts
- Creating incidents when threshold is met

**Verification:**
- Go to **Analytics** → **Active rules**
- You should see "SSH Brute Force Detection" with status **Enabled**

---

## Validation and Testing

### Triggering the Detection

Now let's verify everything works end-to-end!

#### Step 1: Execute Another Attack

From your attacker machine, run **at least 5-10 failed SSH attempts**:

```bash
ssh admin@192.168.1.109
# Wrong password

ssh admin@192.168.1.109
# Wrong password

# Repeat 5-10 times within a few minutes
```

**Timing is important:**
- All attempts should be within a 5-minute window
- This ensures they're grouped together in the query

#### Step 2: Wait for Processing

**Timeline of events:**

```
Time 0:00 - You execute 8 failed SSH attempts
Time 0:01 - Logs generated on Linux VM
Time 0:02 - Logs forwarded to Azure Log Analytics
Time 0:03 - Logs available in Sentinel
Time 0:05 - Analytics rule runs (scheduled every 5 minutes)
Time 0:05 - Alert generated (query found 8 attempts from same IP)
Time 0:05 - Incident created
Time 0:05 - Automation rule triggers
Time 0:05 - Incident assigned to you
```

**Total time: 3-7 minutes** from attack to incident creation

**Why the delay?**
- Log ingestion: 1-3 minutes
- Query schedule: up to 5 minutes
- This is normal for log-based detection

**Real-time vs. near-real-time:**
- Real-time: Instant (not possible with log aggregation)
- Near-real-time: 3-5 minutes (what we have)
- Batch: Hours or daily (legacy SIEM approach)

#### Step 3: Check for Incidents

**Navigate to incidents:**

1. Go to **Microsoft Sentinel** → **Incidents**
2. Look for: `SSH Brute Force Detection`

**What you should see:**

| Field | Expected Value |
|-------|----------------|
| **Title** | SSH Brute Force Detection |
| **Severity** | Medium |
| **Status** | Active |
| **Owner** | Your name (if automation worked) |
| **Created time** | Within last 10 minutes |

**Click on the incident** to see details:

#### Step 4: Investigate the Incident

**Incident details page shows:**

**Top section:**
- Incident number (e.g., #1234)
- Timeline
- MITRE ATT&CK tactics
- Related entities

**Entities section:**
- **IP Address:** 192.168.1.100 (attacker)
- **Host:** ubuntu-vm (target)
- **Account:** admin (username attempted)

**Evidence section:**
- Logs that triggered the alert
- Number of failed attempts
- Time range of attack

**Timeline:**
- Visual representation of when attempts occurred
- Shows clustering of events (brute force pattern)

#### Step 5: View the Detection Query Results

**In the incident:**

1. Click **View full details**
2. Scroll to **Alerts**
3. Click on the alert
4. Click **View query results**

**You should see:**

| IPAddress | Computer | TimeGenerated | Attempts |
|-----------|----------|---------------|----------|
| 192.168.1.100 | ubuntu-vm | 2026-03-11T10:30:00Z | 8 |

**This confirms:**
- ✅ Attack was detected
- ✅ Attacker IP identified: 192.168.1.100
- ✅ Number of attempts: 8
- ✅ Time window: 5-minute bin

---

## Understanding the Results

### What Just Happened? (Complete Flow)

Let's trace the complete detection and response flow:

**1. Attack Phase**
```
Attacker → SSH to 192.168.1.109 → Wrong password entered → Connection rejected
```

**2. Logging Phase**
```
Linux sshd → Writes to /var/log/auth.log → "Failed password for admin from 192.168.1.100"
```

**3. Collection Phase**
```
Azure Monitor Agent → Reads auth.log → Forwards to Log Analytics → Stored in Syslog table
```

**4. Detection Phase**
```
Analytics rule runs → KQL query executes → Finds 8 attempts from 192.168.1.100 → Threshold exceeded (>5)
```

**5. Alert Phase**
```
Query returns results → Alert generated → Mapped to MITRE ATT&CK T1110
```

**6. Incident Phase**
```
Alert → Incident created → Entities identified (IP, Host, Account) → Incident ID assigned
```

**7. Automation Phase**
```
Automation rule triggered → Incident assigned to SOC analyst → Status set to Active
```

**8. Response Phase** (What happens next in real SOC)
```
Analyst investigates → Validates threat → Takes action (block IP, reset passwords, etc.)
```

---


## Conclusion

Congratulations! You've successfully:

- Built a complete security monitoring solution
- Detected a real attack pattern
- Automated incident response
- Gained practical SOC analyst experience

