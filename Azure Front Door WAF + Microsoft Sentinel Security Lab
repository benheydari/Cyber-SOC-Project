# Azure Front Door WAF + Microsoft Sentinel Security Lab

A complete hands-on lab demonstrating web application protection using Azure Front Door WAF and threat detection with Microsoft Sentinel.

[![Azure](https://img.shields.io/badge/Azure-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)](https://portal.azure.com)
[![Security](https://img.shields.io/badge/Security-Lab-red?style=for-the-badge)]()
[![WAF](https://img.shields.io/badge/WAF-Protection-green?style=for-the-badge)]()

---

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Learning Objectives](#learning-objectives)
- [Cost Considerations](#cost-considerations)
- [Lab Setup](#lab-setup)
- [Testing & Validation](#testing--validation)
- [Threat Detection](#threat-detection)
- [Troubleshooting](#troubleshooting)
- [Cleanup](#cleanup)
- [Skills Demonstrated](#skills-demonstrated)

---

## 🎯 Overview

### What You'll Build

In this lab, you will deploy a complete web application security stack in Microsoft Azure that includes:

1. **Web Application** - A simple backend application hosted on Azure App Service
2. **Azure Front Door** - Global entry point with built-in CDN capabilities
3. **Web Application Firewall (WAF)** - OWASP-based protection against web attacks
4. **Microsoft Sentinel** - Security Information and Event Management (SIEM) for threat detection
5. **Log Analytics** - Centralized log collection and analysis

### Real-World Use Case

This architecture mirrors how enterprises protect public-facing web applications:
- **E-commerce sites** protecting against SQL injection and XSS attacks
- **Banking portals** defending against credential stuffing
- **SaaS applications** preventing DDoS and bot attacks
- **Corporate websites** blocking malicious traffic before it reaches origin servers

### Why This Matters

- **73% of web applications** have at least one serious vulnerability
- **WAFs block 99%** of automated attacks before they reach your application
- **Average cost of a data breach**: $4.45 million (IBM 2023)
- **Mean time to detect**: 207 days without proper SIEM (Ponemon Institute)

This lab teaches you to **prevent, detect, and respond** to web application attacks.

---

## 🏗️ Architecture

### Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        Internet Users                            │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             │ HTTPS Requests
                             ▼
                    ┌─────────────────┐
                    │  Azure Front    │
                    │  Door (Global)  │
                    │                 │
                    │  ┌───────────┐  │
                    │  │    WAF    │  │ ◄── Blocks malicious traffic
                    │  │  (OWASP)  │  │
                    │  └───────────┘  │
                    └────────┬────────┘
                             │
                    ┌────────┴────────┐
                    │                 │
            Legitimate Traffic    Malicious Traffic
                    │                 │
                    │                 ▼
                    │         ┌──────────────┐
                    │         │   Blocked    │
                    │         │  (403 Error) │
                    │         └──────┬───────┘
                    │                │
                    ▼                │
            ┌──────────────┐         │
            │  Origin      │         │
            │  Group       │         │
            └──────┬───────┘         │
                   │                 │
                   ▼                 │
           ┌──────────────┐          │
           │  Azure App   │          │
           │  Service     │          │
           │  (Web App)   │          │
           └──────────────┘          │
                                     │
                   ┌─────────────────┴──────────────────┐
                   │          Diagnostic Logs           │
                   └─────────────────┬──────────────────┘
                                     │
                                     ▼
                            ┌─────────────────┐
                            │ Log Analytics   │
                            │   Workspace     │
                            └────────┬────────┘
                                     │
                                     ▼
                            ┌─────────────────┐
                            │   Microsoft     │
                            │   Sentinel      │
                            │   (SIEM)        │
                            └─────────────────┘
                                     │
                                     ▼
                            ┌─────────────────┐
                            │  Alerts &       │
                            │  Incidents      │
                            └─────────────────┘
```

### Traffic Flow

**Legitimate User Request:**
```
1. User navigates to: https://yourname-waf.azurefd.net
2. DNS resolves to nearest Front Door Point of Presence (PoP)
3. Request hits WAF → Passes inspection (no threats detected)
4. Front Door routes to Origin Group
5. Request forwarded to App Service
6. Response returns through same path
7. CDN caches static content for faster future requests
```

**Malicious Attack Request:**
```
1. Attacker sends: https://yourname-waf.azurefd.net/?id=1' OR '1'='1
2. Request hits WAF → Inspected against OWASP rules
3. WAF detects SQL injection pattern
4. Request BLOCKED with 403 Forbidden
5. Attack logged to Log Analytics
6. Sentinel creates security incident
7. SOC analyst investigates
```

---

## ✅ Prerequisites

### Required

- **Azure Subscription** with the following permissions:
  - Create Resource Groups
  - Deploy App Services
  - Configure Front Door (Premium tier)
  - Access to Microsoft Sentinel
  - Create Log Analytics Workspaces

- **Cost awareness**:
  - This lab uses **paid resources** (see Cost Considerations section)
  - Budget: ~$50-100 for a few hours of testing
  - **Remember to delete resources after completion**

### Recommended Knowledge

- Basic understanding of:
  - HTTP/HTTPS protocols
  - Web application basics (URLs, parameters, cookies)
  - Common web attacks (XSS, SQL injection)
  - Azure portal navigation

### Optional

- **Azure CLI** or **PowerShell** (for automation)
- **Burp Suite** or **OWASP ZAP** (for advanced attack testing)
- **Postman** or **curl** (for API testing)

---

## 🎓 Learning Objectives

By completing this lab, you will:

### Technical Skills

✅ Deploy and configure Azure Front Door with WAF  
✅ Understand WAF rule sets and OWASP protection  
✅ Configure diagnostic logging for security events  
✅ Write KQL (Kusto Query Language) queries for log analysis  
✅ Set up Microsoft Sentinel for SIEM operations  
✅ Create detection rules and security alerts  
✅ Simulate real-world web attacks safely  
✅ Analyze attack patterns and create incident response playbooks  

### Security Concepts

✅ Defense in depth architecture  
✅ Web Application Firewall (WAF) vs. Network Firewall  
✅ OWASP Top 10 vulnerabilities  
✅ Detection vs. Prevention mode  
✅ Security logging and monitoring best practices  
✅ Incident response workflow  

### Cloud Architecture

✅ Global load balancing and CDN concepts  
✅ Origin protection strategies  
✅ Azure resource organization (Resource Groups)  
✅ Cost optimization in security architecture  

---

## 💰 Cost Considerations

### Estimated Costs (USD per hour)

| Resource | Tier | Hourly Cost | Daily Cost |
|----------|------|-------------|------------|
| Azure Front Door | Premium | ~$0.35/hr | ~$8.40/day |
| App Service | F1 (Free) | $0 | $0 |
| Log Analytics | Pay-as-you-go | ~$0.10/hr | ~$2.40/day |
| Microsoft Sentinel | Pay-as-you-go | ~$0.15/hr | ~$3.60/day |
| **Total Estimated** | | **~$0.60/hr** | **~$14.40/day** |

### Cost Optimization Tips

1. **Use Free Tier where possible:**
   - App Service: F1 (Free) tier is sufficient
   - Log Analytics: First 5GB/month is free

2. **Delete resources immediately after testing:**
   - Front Door Premium is the most expensive component
   - Even stopped resources can incur costs

3. **Limit log retention:**
   - Set Log Analytics retention to 30 days (minimum)
   - Archive older logs to cheaper storage

4. **Test during business hours:**
   - Complete the lab in 2-3 hours
   - Total cost: ~$1.20-1.80

### 🚨 CRITICAL: Remember to Clean Up

**Forgetting to delete resources can result in significant charges!**
- See [Cleanup Section](#cleanup) for step-by-step deletion guide
- Set a calendar reminder to delete resources
- Use Azure Cost Alerts to monitor spending

---

## 🚀 Lab Setup

---

## Step 1: Create Resource Group

### What is a Resource Group?

A Resource Group is a **logical container** for Azure resources. Think of it like a folder that groups related resources together for:
- **Organization**: Keep related resources together
- **Access Control**: Apply permissions at the group level
- **Cost Management**: Track spending by resource group
- **Lifecycle Management**: Delete all resources at once

### Instructions

1. **Navigate to Azure Portal:**
   - Go to [https://portal.azure.com](https://portal.azure.com)
   - Sign in with your Azure account

2. **Create Resource Group:**
   - Click **"Create a resource"** (top left)
   - Search for **"Resource Group"**
   - Click **Create**

3. **Configure Resource Group:**
   
   | Setting | Value | Why? |
   |---------|-------|------|
   | **Subscription** | Your subscription | Billing account |
   | **Resource group name** | `rg-waf-lab` | Clear, descriptive naming |
   | **Region** | `East US` or closest to you | Reduce latency |

4. **Review + Create:**
   - Click **"Review + create"**
   - Click **"Create"**
   - Wait for deployment (5-10 seconds)

### Validation

✅ **Success criteria:**
- Resource group appears in your resource list
- Status shows "Succeeded"
- You can open the resource group (currently empty)

### Naming Convention

We use `rg-waf-lab` following Azure naming best practices:
- `rg-` = Resource Group prefix
- `waf-lab` = Descriptive purpose

**Example naming patterns:**
```
rg-<workload>-<environment>-<region>

Examples:
rg-waf-prod-eastus
rg-app-dev-westeurope
rg-sentinel-test-southeastasia
```

---

## Step 2: Create Web App (Backend Origin)

### What is Azure App Service?

Azure App Service is a **Platform-as-a-Service (PaaS)** for hosting web applications:
- No need to manage servers or VMs
- Automatic scaling, patching, and maintenance
- Built-in CI/CD integration
- Support for multiple languages (.NET, Node.js, Python, Java, PHP)

### Why Do We Need This?

In our architecture, the App Service is the **origin** (backend) that:
- Hosts our actual web application
- Processes legitimate requests that pass through WAF
- Sits behind Front Door (not directly exposed to internet)

### Instructions

1. **Navigate to App Services:**
   - In Azure Portal, click **"Create a resource"**
   - Search for **"Web App"**
   - Click **Create**

2. **Configure Basics Tab:**

   | Setting | Value | Explanation |
   |---------|-------|-------------|
   | **Subscription** | Your subscription | Same as Resource Group |
   | **Resource Group** | `rg-waf-lab` | Use existing RG |
   | **Name** | `yourname-waf-demo` | Must be globally unique |
   | **Publish** | `Code` | We're deploying application code |
   | **Runtime stack** | `.NET 8 (LTS)` | Choose any - doesn't matter for this lab |
   | **Operating System** | `Windows` | Preference - either works |
   | **Region** | `East US` (same as RG) | Keep resources in same region |
   
   **Pricing Plan:**
   | Setting | Value | Why? |
   |---------|-------|------|
   | **Pricing plan** | Click "Explore pricing plans" | |
   | **Select** | `F1 (Free)` | **Zero cost** - perfect for testing |

   **F1 Tier Limitations:**
   - 1 GB disk space
   - 60 minutes CPU time per day
   - 1 GB RAM
   - **Perfect for our lab** - we're just testing WAF, not performance

3. **Skip Other Tabs:**
   - **Deployment**: Skip (not needed)
   - **Networking**: Skip (we'll configure Front Door later)
   - **Monitoring**: Skip (we'll use Sentinel)

4. **Review + Create:**
   - Click **"Review + create"**
   - Verify settings
   - Click **"Create"**
   - Wait for deployment (2-3 minutes)

### Post-Deployment Steps

1. **Navigate to Your App Service:**
   - Go to Resource Group `rg-waf-lab`
   - Click on your app service (`yourname-waf-demo`)

2. **Get the Default URL:**
   - Look for **"Default domain"** in the Overview page
   - Example: `https://yourname-waf-demo.azurewebsites.net`
   - **Copy this URL** - you'll need it later!

3. **Test Direct Access:**
   - Click on the default domain URL
   - You should see a default Azure App Service page

   **Expected Result:**
   ```
   Your App Service is up and running.
   ```

   Or a placeholder page with Azure branding.

### Understanding the Default Page

**What you're seeing:**
- Azure's default landing page (since we haven't deployed custom code)
- This proves the App Service is running and accessible

**For this lab:**
- We don't need to deploy actual application code
- The default page is sufficient for testing WAF protection
- The goal is protecting **any** web app, not building one

### Troubleshooting

**Problem:** "The name is already taken"
- **Solution:** App Service names must be globally unique
- Try: `yourname-waf-demo-20260419` (add date)
- Or: `initials-waf-demo` (use your initials)

**Problem:** "Cannot access default domain"
- **Solution:** Wait 2-3 minutes after deployment
- The DNS propagation takes time
- Try accessing from an incognito/private browser window

**Problem:** "404 Not Found"
- **Solution:** This is OK! Some runtime stacks show 404 by default
- The app is still running - proceed to next steps

---

## Step 3: Create Azure Front Door (Premium)

### What is Azure Front Door?

Azure Front Door is a **global, scalable entry point** for web applications that provides:

**Key Capabilities:**
- **Global Load Balancing**: Routes users to nearest healthy endpoint
- **CDN (Content Delivery Network)**: Caches content at edge locations worldwide
- **WAF (Web Application Firewall)**: Protects against OWASP Top 10 attacks
- **SSL/TLS Termination**: Handles HTTPS encryption/decryption
- **Traffic Routing**: Intelligent request routing based on rules

**Analogy:**
Think of Front Door as the **security guard and concierge** at a hotel:
- Checks IDs (WAF inspecting requests)
- Routes guests to the right room (load balancing to backends)
- Keeps records of who enters (diagnostic logs)
- Blocks troublemakers (malicious traffic)

### Standard vs. Premium Tier

| Feature | Standard | Premium | Why We Need Premium |
|---------|----------|---------|---------------------|
| **Basic Routing** | ✅ | ✅ | Both have this |
| **CDN** | ✅ | ✅ | Both have this |
| **WAF - Basic Rules** | ✅ | ✅ | Both have this |
| **WAF - OWASP Managed Rules** | ❌ | ✅ | **Critical for our lab** |
| **Private Link to Origin** | ❌ | ✅ | Enhanced security |
| **Advanced WAF Features** | ❌ | ✅ | Bot protection, custom rules |

**For this lab:** We need **Premium** for OWASP 3.2 rule set protection.

### Instructions

1. **Navigate to Front Door:**
   - In Azure Portal, click **"Create a resource"**
   - Search for **"Front Door and CDN profiles"**
   - Click **Create**

2. **Choose Offering:**
   - You'll see comparison of different offerings
   - Select: **"Azure Front Door"**
   - Click **Continue**

3. **Configure Basics:**

   | Setting | Value | Notes |
   |---------|-------|-------|
   | **Subscription** | Your subscription | Same as before |
   | **Resource Group** | `rg-waf-lab` | Use existing |
   | **Name** | `fd-waf-lab` | Front Door profile name |
   | **Tier** | **Premium** | ⚠️ CRITICAL - Must be Premium |
   | **Endpoint name** | `yourname-waf` | Subdomain for azurefd.net |
   | **Origin type** | `App Services` | Our backend |

4. **Configure Origin:**
   
   In the same creation wizard:

   **Origin Details:**
   | Setting | Value | Explanation |
   |---------|-------|-------------|
   | **Name** | `app-service-origin` | Friendly name for backend |
   | **Origin type** | `App Services` | Type of backend |
   | **Host name** | Select your App Service | From dropdown |
   | | `yourname-waf-demo.azurewebsites.net` | Should auto-populate |
   | **Origin host header** | Same as host name | Forward original host header |
   | **HTTP port** | `80` | Default HTTP |
   | **HTTPS port** | `443` | Default HTTPS |
   | **Priority** | `1` | Highest priority (lower = higher) |
   | **Weight** | `1000` | Load balancing weight |

   **What is Origin Host Header?**
   - The `Host:` HTTP header sent to backend
   - App Service needs this to route to correct app
   - Must match your App Service hostname

5. **Configure Route:**

   | Setting | Value | Purpose |
   |---------|-------|---------|
   | **Name** | `default-route` | Route identifier |
   | **Domains** | Your endpoint domain | Auto-selected |
   | **Patterns to match** | `/*` | Match all paths |
   | **Accepted protocols** | `HTTP and HTTPS` | Allow both |
   | **Redirect** | `Redirect all traffic to HTTPS` | Force encryption |
   | **Origin group** | Your origin group | Backend destination |
   | **Origin path** | (leave empty) | No path rewriting |
   | **Forwarding protocol** | `HTTPS only` | Secure backend communication |
   | **Caching** | `Enabled` | Speed up responses |
   | **Query string caching** | `Ignore query strings` | Default caching behavior |

6. **Configure Security (WAF):**

   - Click **"+ Add a policy"** under Security
   
   **WAF Policy Settings:**
   | Setting | Value | Why? |
   |---------|-------|------|
   | **Name** | `wafpolicy001` | Alphanumeric only |
   | **Policy mode** | `Prevention` | Block malicious traffic |
   | **Policy state** | `Enabled` | Activate protection |

   **Managed Rules:**
   - Click **"+ Add managed rule set"**
   - Select: **"Default rule set (DRS) 2.1"** - Latest OWASP rules
   - **Rule set version**: `2.1` (latest)
   - Leave all rules enabled (default)

   **What you just enabled:**
   - ✅ SQL Injection protection
   - ✅ Cross-Site Scripting (XSS) protection
   - ✅ Remote Code Execution prevention
   - ✅ Local File Inclusion blocking
   - ✅ Protocol attack protection
   - ✅ Session fixation prevention

7. **Review + Create:**
   - Click **"Review + create"**
   - Verify all settings
   - Click **"Create"**
   - **Wait time: 10-15 minutes** (Front Door is global - takes time to propagate)

### Post-Deployment Validation

1. **Check Deployment Status:**
   - Go to Resource Group `rg-waf-lab`
   - You should see:
     - Front Door profile (`fd-waf-lab`)
     - WAF policy (`wafpolicy001`)

2. **Get Your Front Door Endpoint URL:**
   - Open Front Door profile
   - Go to **"Overview"** → **"Endpoints"**
   - Copy endpoint URL: `https://yourname-waf.azurefd.net`

3. **Test Basic Connectivity:**
   - Open browser (or incognito window)
   - Navigate to: `https://yourname-waf.azurefd.net`
   
   **Expected Result:**
   - Same default page you saw from App Service direct access
   - But now traffic is flowing through Front Door + WAF
   - Check browser address bar - URL should be `azurefd.net` domain

4. **Verify HTTPS Redirect:**
   - Try: `http://yourname-waf.azurefd.net` (HTTP not HTTPS)
   - Should automatically redirect to `https://`
   - This proves your route is working correctly

### Understanding What Just Happened

**Request Flow:**
```
Your Browser
    ↓
https://yourname-waf.azurefd.net (Front Door endpoint)
    ↓
Front Door receives request at nearest PoP (Point of Presence)
    ↓
WAF inspects request against OWASP rules
    ↓ (Clean traffic)
Route forwards to Origin Group
    ↓
Origin: yourname-waf-demo.azurewebsites.net
    ↓
App Service processes request
    ↓
Response returns through Front Door
    ↓
Your Browser receives response
```

### Troubleshooting

**Problem:** "Endpoint URL returns 404 or error"
- **Solution:** Front Door is still deploying globally (can take 15-20 minutes)
- Wait 5 minutes, then try again
- Clear browser cache (Ctrl+F5)
- Try incognito/private window

**Problem:** "Cannot find endpoint URL"
- **Solution:** Go to Front Door profile → Overview → Endpoint hostname
- Copy the full URL including `https://`

**Problem:** "Still seeing App Service URL in browser"
- **Solution:** Make sure you're using the `azurefd.net` URL, not `azurewebsites.net`
- The azurewebsites.net URL still works (direct access) - we'll lock this down later

**Problem:** "SSL Certificate warning"
- **Solution:** Wait for Front Door provisioning to complete
- Front Door automatically provisions SSL certificates
- Can take 10-15 minutes

---

## Step 4: Enable Diagnostic Logging

### Why Logging is Critical

**Logging is the foundation of security monitoring:**
- 🔍 **Visibility**: Can't defend what you can't see
- 📊 **Analysis**: Understand attack patterns
- 🚨 **Alerting**: Detect threats in real-time
- 📝 **Compliance**: Required for regulations (PCI-DSS, GDPR, etc.)
- 🔬 **Forensics**: Investigate incidents after the fact

**Without logs:**
- Attackers operate in the dark
- No evidence of compromise
- Cannot tune WAF rules
- Blind to successful attacks

### What We're Logging

Azure Front Door generates several log categories:

| Log Category | What It Contains | Why We Need It |
|--------------|------------------|----------------|
| **FrontDoorAccessLog** | Every request (legitimate + blocked) | Traffic analysis, debugging |
| **FrontDoorHealthProbeLog** | Backend health checks | Monitor origin availability |
| **FrontDoorWebApplicationFirewallLog** | **WAF actions & matched rules** | **🎯 This is what we need!** |

For this lab, we focus on **FrontDoorWebApplicationFirewallLog** - it shows:
- Which requests were blocked
- Why they were blocked (which OWASP rule matched)
- Attack patterns and sources
- Rule performance and false positives

### Instructions

1. **Navigate to Front Door:**
   - Go to Resource Group `rg-waf-lab`
   - Click on your Front Door profile (`fd-waf-lab`)

2. **Open Diagnostic Settings:**
   - In the left menu, scroll down to **"Monitoring"** section
   - Click **"Diagnostic settings"**
   - Click **"+ Add diagnostic setting"**

3. **Configure Diagnostic Setting:**

   | Setting | Value | Purpose |
   |---------|-------|---------|
   | **Name** | `waf-logs-to-sentinel` | Descriptive name |
   
   **Logs - Select Categories:**
   - ✅ **FrontDoorWebApplicationFirewallLog** ← **Critical!**
   - ✅ **FrontDoorAccessLog** ← Optional but useful
   - ⬜ **FrontDoorHealthProbeLog** ← Skip (not needed for this lab)

   **Destination details:**
   - ✅ **Send to Log Analytics workspace**
   - **Subscription**: Your subscription
   - **Log Analytics workspace**: We'll create this next!

4. **Create Log Analytics Workspace:**
   
   Since we don't have one yet, click **"Create new"**:

   | Setting | Value | Notes |
   |---------|-------|-------|
   | **Name** | `law-waf-lab` | Log Analytics Workspace name |
   | **Resource Group** | `rg-waf-lab` | Same RG |
   | **Location** | `East US` | Same region as other resources |
   | **Pricing tier** | `Pay-as-you-go` | ~$2.30/GB after 5GB free |

   Click **OK** to create.

5. **Finalize Diagnostic Settings:**
   - Select the newly created workspace: `law-waf-lab`
   - Click **Save**
   - Wait for confirmation (5-10 seconds)

### Validation

✅ **Success indicators:**
- Diagnostic setting appears in the list with status "Enabled"
- You can see the workspace name: `law-waf-lab`
- Log categories show checkmarks

### Understanding Log Flow

**How data flows:**
```
Front Door receives request
    ↓
WAF inspects and blocks malicious request
    ↓
Front Door generates log entry:
  - Timestamp: 2026-04-19T10:30:45Z
  - Action: Block
  - Rule: SQL Injection - 942100
  - Client IP: 203.0.113.45
  - Request: /?id=1' OR '1'='1
    ↓
Log sent to Log Analytics Workspace (law-waf-lab)
    ↓
Stored in AzureDiagnostics table
    ↓
Sentinel queries this table for threats
    ↓
Alerts created for security incidents
```

**Latency:**
- **Log ingestion delay**: 2-5 minutes (typical)
- **Near real-time**: Most logs appear within 5 minutes
- **Batch processing**: Some logs batched for efficiency

### Sample Log Entry

Here's what a WAF log entry looks like:

```json
{
  "time": "2026-04-19T10:30:45.123Z",
  "resourceId": "/subscriptions/.../fd-waf-lab",
  "category": "FrontdoorWebApplicationFirewallLog",
  "properties": {
    "clientIP": "203.0.113.45",
    "clientPort": "54321",
    "requestUri": "https://yourname-waf.azurefd.net/?id=1' OR '1'='1",
    "ruleName": "SQLInjection-942100",
    "action": "Block",
    "policyName": "wafpolicy001",
    "matchedData": "1' OR '1'='1"
  }
}
```

**Key fields:**
- `action`: `Block`, `Allow`, `Log`
- `ruleName`: Which OWASP rule triggered
- `clientIP`: Attacker's IP address
- `matchedData`: The malicious payload detected

### Cost Considerations

**Log Analytics Pricing:**
- First **5 GB/month**: FREE
- After 5 GB: **$2.30/GB** (Pay-as-you-go tier)

**Expected volume for this lab:**
- ~10-50 MB for a few hours of testing
- Well within the free tier
- No charges expected

**Cost optimization:**
- Set retention to 30 days (minimum)
- Delete workspace after lab completion
- Archive old logs to cheaper storage if needed

### Troubleshooting

**Problem:** "Cannot find Diagnostic settings"
- **Solution:** Make sure you're in the Front Door profile (not Resource Group)
- Left menu → Monitoring section → Diagnostic settings

**Problem:** "No logs appearing after 10 minutes"
- **Solution:** 
  1. Generate some traffic (visit your Front Door URL)
  2. Wait 5 minutes for ingestion
  3. Check if diagnostic setting is actually "Enabled"
  4. Verify workspace is in same region

**Problem:** "Create new workspace button doesn't work"
- **Solution:** Create workspace separately first:
  - Create resource → Log Analytics Workspace
  - Then return to diagnostic settings and select existing workspace

---

## Step 5: Enable Microsoft Sentinel

### What is Microsoft Sentinel?

Microsoft Sentinel is a **cloud-native SIEM (Security Information and Event Management)** and **SOAR (Security Orchestration, Automation, and Response)** solution.

**In simple terms:**
- **SIEM**: Collects and analyzes security data from all sources
- **SOAR**: Automates responses to security incidents

**Think of Sentinel as your SOC (Security Operations Center) in the cloud:**
- 24/7 monitoring of security events
- AI-powered threat detection
- Automated incident investigation
- Playbooks for automated response
- Centralized view of your security posture

### Why We Need Sentinel

**Log Analytics alone gives us:**
- ✅ Log storage
- ✅ Query capabilities (KQL)
- ✅ Basic visualization

**Sentinel adds:**
- ✅ **Security-specific analytics** (threat detection rules)
- ✅ **Incident management** (case tracking, assignment)
- ✅ **Threat intelligence integration**
- ✅ **Investigation graphs** (visualize attack chains)
- ✅ **Automated responses** (playbooks)
- ✅ **MITRE ATT&CK mapping**

**For our lab:**
We use Sentinel to **detect patterns** of WAF blocks and **create security incidents** for investigation.

### Instructions

1. **Navigate to Microsoft Sentinel:**
   - In Azure Portal search bar, type **"Microsoft Sentinel"**
   - Click **Microsoft Sentinel**

2. **Add Sentinel to Workspace:**
   - Click **"+ Create"** (top left)
   - You'll see a list of available Log Analytics workspaces
   - Find and select: **`law-waf-lab`**
   - Click **"Add"**

3. **Wait for Provisioning:**
   - Sentinel setup takes 2-3 minutes
   - You'll see a notification when complete
   - Status will change from "Adding..." to "Active"

4. **Verify Sentinel is Active:**
   - Refresh the Sentinel page
   - Your workspace (`law-waf-lab`) should appear in the list
   - Click on the workspace to open Sentinel

### Exploring Sentinel Interface

After opening Sentinel, you'll see several sections:

**Left Navigation Menu:**

| Section | Purpose | For This Lab |
|---------|---------|--------------|
| **Overview** | Dashboard showing incidents, alerts | Main view for monitoring |
| **Incidents** | Security incidents requiring investigation | Where we'll create and view incidents |
| **Analytics** | Detection rules and alerts | **We'll create WAF detection rules here** |
| **Hunting** | Proactive threat hunting queries | Advanced use (optional) |
| **Notebooks** | Jupyter notebooks for analysis | Advanced use (optional) |
| **Automation** | Playbooks and automated responses | Advanced use (optional) |
| **Logs** | Direct access to Log Analytics | **Query WAF logs here** |

**For this lab, we'll primarily use:**
1. **Logs** - Query WAF events
2. **Analytics** - Create detection rules
3. **Incidents** - View generated alerts

### Initial Configuration

**After enabling Sentinel, configure basic settings:**

1. **Navigate to Settings:**
   - Click **"Settings"** (gear icon, bottom left)

2. **Workspace Settings:**
   - **Data retention**: 30 days (default, keep this)
   - **Daily cap**: No limit (for this lab)

3. **Pricing:**
   - **Tier**: Pay-as-you-go
   - **Cost**: ~$2/GB after 10GB free (first month)
   
   **For this lab:** 
   - We'll generate <100 MB of data
   - Well within free tier
   - No charges expected

### Understanding Sentinel Architecture

**How Sentinel Works:**

```
Data Sources (Front Door WAF logs)
    ↓
Log Analytics Workspace (law-waf-lab)
    ↓
Sentinel Analytics Rules
    ↓ (Pattern matching)
Incidents Generated
    ↓
SOC Analyst Investigation
    ↓
Automated Response (Playbooks)
```

**In our specific case:**

```
Front Door WAF blocks SQL injection
    ↓
Log sent to Log Analytics
    ↓
Sentinel Analytics Rule:
  "If >5 WAF blocks from same IP in 5 minutes"
    ↓
Incident Created:
  "Possible Web Application Attack"
    ↓
Analyst investigates:
  - Who: Source IP
  - What: Attack type (SQL injection)
  - When: Timeline
  - How many: Count of attempts
    ↓
Response:
  - Block IP at firewall
  - Report to abuse contacts
  - Create threat intelligence
```

### Validation

✅ **Success criteria:**
- Sentinel workspace shows status "Active"
- You can access Sentinel dashboard
- Overview page loads (may be empty - that's OK)
- Logs section accessible

### Sample Queries to Test

Once Sentinel is enabled, test that data is flowing:

1. **Navigate to Logs:**
   - In Sentinel, click **"Logs"** (left menu)
   - Close any popup tutorials

2. **Run a test query:**

```kql
AzureDiagnostics
| where TimeGenerated > ago(1h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| take 10
```

**Expected Result (at this point):**
- Likely no results yet (we haven't generated attacks)
- Query should run without errors
- This proves the connection works

**If you get results:**
- You may see some background traffic
- Or previous test requests
- Check the `action_s` column for `Block` vs `Allow`

### Cost Considerations

**Sentinel Pricing:**
- **First 10 GB/month**: FREE (first 31 days)
- **After 10 GB**: $2.50/GB

**Our lab usage:**
- ~50-100 MB total
- Well within free tier
- **Expected cost: $0**

**Long-term consideration:**
- If you keep this running, costs accumulate daily
- **DELETE resources after completing lab**

### Troubleshooting

**Problem:** "Cannot find Microsoft Sentinel in search"
- **Solution:** Ensure you have the right permissions
- Need: Security Reader or Contributor on subscription
- Try searching "Sentinel" instead of "Microsoft Sentinel"

**Problem:** "Cannot add Sentinel to workspace"
- **Solution:** 
  - Verify workspace exists: `law-waf-lab`
  - Ensure workspace is in a supported region
  - Check you have Contributor role on workspace

**Problem:** "Sentinel enabled but Overview page is blank"
- **Solution:** This is normal for new Sentinel instances
- No incidents yet = blank dashboard
- Will populate after we create detection rules

**Problem:** "Logs section shows permission error"
- **Solution:** 
  - Need Log Analytics Reader role minimum
  - Go to workspace → Access Control (IAM) → Add role

---

## Step 6: Query WAF Logs with KQL

### What is KQL?

**KQL (Kusto Query Language)** is a powerful query language for analyzing log data in Azure.

**Think of KQL like SQL, but specifically designed for:**
- Time-series data (logs, events, metrics)
- Large-scale data analysis
- Security event investigation
- Real-time analytics

**Why KQL matters for Security:**
- **Threat Hunting**: Find hidden attacks in massive log volumes
- **Incident Investigation**: Trace attack timelines
- **Detection Engineering**: Build custom alerts
- **Compliance Reporting**: Generate audit reports

### KQL Basics (Quick Tutorial)

**Structure of a KQL Query:**

```kql
TableName                    ← Data source
| where Condition             ← Filter rows
| summarize Aggregation       ← Group and count
| project Columns             ← Select specific fields
| sort by Column              ← Order results
| take N                      ← Limit results
```

**Example:**
```kql
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize Count=count() by clientIP_s
| sort by Count desc
| take 10
```

**Translation:**
- Take `AzureDiagnostics` table
- Keep only WAF logs
- Keep only blocked requests
- Count blocks per IP address
- Sort by most blocks first
- Show top 10 IPs

### Essential KQL Commands

| Operator | Purpose | Example |
|----------|---------|---------|
| `where` | Filter rows | `where action_s == "Block"` |
| `summarize` | Aggregate data | `summarize count() by IP` |
| `project` | Select columns | `project TimeGenerated, IP, Action` |
| `extend` | Add calculated column | `extend Hour = format_datetime(TimeGenerated, 'HH')` |
| `sort` / `order` | Sort results | `sort by TimeGenerated desc` |
| `take` / `limit` | Limit rows | `take 100` |
| `join` | Combine tables | `join kind=inner OtherTable on CommonField` |
| `parse` | Extract from strings | `parse requestUri_s with * "id=" SQLI_Attempt` |

---

### Query 1: View All WAF Logs (Last Hour)

**Purpose:** Verify logs are flowing

```kql
AzureDiagnostics
| where TimeGenerated > ago(1h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| project TimeGenerated, clientIP_s, requestUri_s, action_s, ruleName_s
| sort by TimeGenerated desc
```

**Expected Output:**

| TimeGenerated | clientIP_s | requestUri_s | action_s | ruleName_s |
|---------------|------------|--------------|----------|------------|
| 2026-04-19 10:35:22 | 203.0.113.45 | /?id=1' | Block | SQLi-942100 |
| 2026-04-19 10:34:15 | 203.0.113.45 | /admin | Allow | - |

**If no results:**
- Wait 5 minutes (log ingestion delay)
- Generate some traffic (visit your Front Door URL)
- Try expanding time range: `ago(24h)`

---

### Query 2: Show Only Blocked Requests

**Purpose:** Focus on attacks

```kql
AzureDiagnostics
| where TimeGenerated > ago(1h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| project TimeGenerated, clientIP_s, requestUri_s, ruleName_s, policyName_s
| sort by TimeGenerated desc
```

**What this shows:**
- Only malicious traffic that WAF stopped
- Which rules caught the attacks
- Which IPs are attacking
- Attack timestamps

---

### Query 3: Count Blocks by IP Address

**Purpose:** Find most aggressive attackers

```kql
AzureDiagnostics
| where TimeGenerated > ago(24h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize AttackCount=count() by clientIP_s
| sort by AttackCount desc
| take 10
```

**Expected Output:**

| clientIP_s | AttackCount |
|------------|-------------|
| 203.0.113.45 | 15 |
| 198.51.100.23 | 8 |
| 192.0.2.150 | 3 |

**Interpretation:**
- IP `203.0.113.45` tried 15 attacks in 24 hours
- High attack count = persistent attacker or automated scanning

---

### Query 4: Attacks by Type (OWASP Rule)

**Purpose:** Understand which attacks are being attempted

```kql
AzureDiagnostics
| where TimeGenerated > ago(24h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize Count=count() by ruleName_s
| sort by Count desc
```

**Expected Output:**

| ruleName_s | Count |
|------------|-------|
| SQLInjection-942100 | 25 |
| XSS-941100 | 12 |
| RCE-932100 | 5 |

**OWASP Rule Codes:**
- **94xxxx**: SQL Injection
- **93xxxx**: Remote Code Execution (RCE)
- **94xxxx**: Cross-Site Scripting (XSS)
- **92xxxx**: Protocol Attacks
- **93xxxx**: Local File Inclusion (LFI)

---

### Query 5: Attack Timeline (Hourly)

**Purpose:** Identify attack patterns over time

```kql
AzureDiagnostics
| where TimeGenerated > ago(24h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize AttackCount=count() by bin(TimeGenerated, 1h)
| render timechart
```

**What this shows:**
- Hourly attack volume
- Visual timeline chart
- Helps identify attack campaigns or scanning patterns

**Interpretation:**
- Spike at 2 AM = automated scanning (when admins asleep)
- Steady low-level = opportunistic attacks
- Sudden spike = coordinated attack or vulnerability announced

---

### Query 6: Detailed Attack Analysis

**Purpose:** Deep dive into specific attacks

```kql
AzureDiagnostics
| where TimeGenerated > ago(1h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| extend AttackType = case(
    ruleName_s contains "SQLi", "SQL Injection",
    ruleName_s contains "XSS", "Cross-Site Scripting",
    ruleName_s contains "RCE", "Remote Code Execution",
    ruleName_s contains "LFI", "Local File Inclusion",
    "Other"
)
| project 
    Time = format_datetime(TimeGenerated, 'yyyy-MM-dd HH:mm:ss'),
    SourceIP = clientIP_s,
    AttackType,
    MaliciousURL = requestUri_s,
    TriggeredRule = ruleName_s,
    WAFPolicy = policyName_s
| sort by Time desc
```

**Expected Output:**

| Time | SourceIP | AttackType | MaliciousURL | TriggeredRule |
|------|----------|------------|--------------|---------------|
| 2026-04-19 10:35:22 | 203.0.113.45 | SQL Injection | /?id=1' OR '1'='1 | SQLi-942100 |
| 2026-04-19 10:34:10 | 203.0.113.45 | XSS | /?q=<script>alert(1)</script> | XSS-941100 |

---

### Query 7: Geographic Attack Distribution

**Purpose:** Where are attacks coming from?

**Note:** This requires additional geo-location data. For basic version:

```kql
AzureDiagnostics
| where TimeGenerated > ago(24h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize Attacks=count() by clientIP_s
| extend IPClass = case(
    clientIP_s startswith "10.", "Private",
    clientIP_s startswith "192.168.", "Private",
    clientIP_s startswith "172.16.", "Private",
    "Public"
)
| summarize Count=count() by IPClass
```

---

### Query 8: Success vs. Blocked Ratio

**Purpose:** Understand WAF effectiveness

```kql
AzureDiagnostics
| where TimeGenerated > ago(24h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| summarize 
    Total = count(),
    Blocked = countif(action_s == "Block"),
    Allowed = countif(action_s == "Allow")
| extend BlockRate = round((todouble(Blocked) / Total) * 100, 2)
| project Total, Allowed, Blocked, BlockRatePercent = strcat(BlockRate, "%")
```

**Expected Output:**

| Total | Allowed | Blocked | BlockRatePercent |
|-------|---------|---------|------------------|
| 1000 | 950 | 50 | 5.00% |

**Interpretation:**
- 5% block rate = WAF catching ~50 malicious requests per 1000
- Very high block rate (>50%) = either under heavy attack OR false positives
- Very low block rate (<1%) = good (mostly legitimate traffic)

---

### Advanced Query: Attack Campaign Detection

**Purpose:** Detect coordinated attacks

```kql
AzureDiagnostics
| where TimeGenerated > ago(1h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize 
    AttackCount = count(),
    DistinctRules = dcount(ruleName_s),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated),
    URLs = make_set(requestUri_s, 5)
    by clientIP_s
| where AttackCount > 5  // More than 5 attempts
| extend Duration = LastSeen - FirstSeen
| project 
    AttackerIP = clientIP_s,
    TotalAttempts = AttackCount,
    DifferentAttackTypes = DistinctRules,
    AttackDuration = Duration,
    FirstAttempt = FirstSeen,
    LastAttempt = LastSeen,
    SampleURLs = URLs
| sort by TotalAttempts desc
```

**This identifies:**
- Persistent attackers (>5 attempts)
- Variety of attack methods (distinct rules)
- Campaign duration
- Sample attack payloads

---

### Tips for Writing KQL Queries

**Best Practices:**

1. **Always filter by time first:**
   ```kql
   | where TimeGenerated > ago(1h)  // Reduces data scanned
   ```

2. **Filter early in the query:**
   ```kql
   // Good:
   | where Category == "WAF"
   | where action_s == "Block"
   
   // Bad:
   | project ...
   | where action_s == "Block"  // Too late
   ```

3. **Use `project` to limit columns:**
   ```kql
   | project TimeGenerated, IP, Action  // Only what you need
   ```

4. **Comment your queries:**
   ```kql
   // Find SQL injection attacks in last 24 hours
   AzureDiagnostics
   | where TimeGenerated > ago(24h)
   | where ruleName_s contains "SQLi"
   ```

5. **Test incrementally:**
   - Start simple
   - Add filters one at a time
   - Verify results at each step

---

### Common KQL Mistakes

❌ **Using `=` instead of `==`:**
```kql
where action_s = "Block"  // WRONG
where action_s == "Block" // CORRECT
```

❌ **Forgetting quotes on strings:**
```kql
where action_s == Block   // WRONG
where action_s == "Block" // CORRECT
```

❌ **Case sensitivity issues:**
```kql
where action_s == "block" // Might not match if logs use "Block"
where action_s =~ "block" // Case-insensitive comparison
```

❌ **Not limiting results:**
```kql
AzureDiagnostics
| where Category == "WAF"
// Returns millions of rows - slow!

// Better:
| where TimeGenerated > ago(1h)
| take 100
```

---

### Saving Queries

**To save a query for reuse:**

1. Write your query in Logs section
2. Click **"Save"** (top toolbar)
3. Choose:
   - **Save as query**: Save to your queries
   - **Save as function**: Create reusable function
   - **Pin to dashboard**: Add to visualization

**Example saved query names:**
- `WAF-BlockedAttacks-Last24h`
- `WAF-TopAttackers`
- `WAF-SQLInjectionDetection`

---

### Practice Exercises

Try writing queries for these scenarios:

**Exercise 1:** Show all attacks in the last hour, sorted by most recent

<details>
<summary>Click to see solution</summary>

```kql
AzureDiagnostics
| where TimeGenerated > ago(1h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| sort by TimeGenerated desc
```
</details>

**Exercise 2:** Count how many unique IP addresses attacked today

<details>
<summary>Click to see solution</summary>

```kql
AzureDiagnostics
| where TimeGenerated > ago(1d)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize UniqueAttackers = dcount(clientIP_s)
```
</details>

**Exercise 3:** Find all XSS attacks in the last 24 hours

<details>
<summary>Click to see solution</summary>

```kql
AzureDiagnostics
| where TimeGenerated > ago(24h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| where ruleName_s contains "XSS"
| project TimeGenerated, clientIP_s, requestUri_s
```
</details>

---

## Step 7: Create Analytics Rule in Sentinel

### What are Analytics Rules?

**Analytics Rules** are the "brains" of Microsoft Sentinel - they automatically detect threats by:
- Running queries on a schedule
- Looking for specific attack patterns
- Creating incidents when threats are found
- Alerting security teams

**Think of them as:**
- Automated threat hunters that never sleep
- Security detectors running 24/7
- Rules that say "If X happens, alert me"

**Example:**
*"If the same IP address triggers WAF blocks more than 5 times in 5 minutes, create a high-severity incident because this is likely an automated attack."*

### Types of Analytics Rules

| Type | How It Works | Use Case |
|------|--------------|----------|
| **Scheduled** | Runs query every X minutes | Most common - time-based detection |
| **Microsoft Security** | Alerts from other Microsoft tools | Azure Defender, Cloud App Security |
| **Fusion** | ML-based correlation | Advanced persistent threats |
| **Anomaly** | Machine learning detects unusual behavior | Baseline deviations |

**For this lab:** We'll create a **Scheduled** query rule.

---

### Our Detection Rule Logic

**What we want to detect:**
- Web application attacks (WAF blocks)
- From the same source IP
- Multiple attempts in short time
- Indicates automated scanning or persistent attacker

**Rule in plain English:**
```
IF:
  - More than 5 WAF blocks
  - From the same IP address
  - Within a 5-minute window
THEN:
  - Create incident
  - Severity: Medium
  - Alert SOC team
```

---

### Instructions: Create Analytics Rule

#### **Step 1: Navigate to Analytics**

1. **Open Microsoft Sentinel:**
   - Go to Microsoft Sentinel
   - Click on your workspace (`law-waf-lab`)

2. **Open Analytics:**
   - In the left menu, click **"Analytics"**
   - You'll see the analytics rules dashboard

3. **Create New Rule:**
   - Click **"+ Create"** (top left)
   - Select **"Scheduled query rule"**

---

#### **Step 2: General Tab**

Configure basic rule information:

| Setting | Value | Explanation |
|---------|-------|-------------|
| **Name** | `High Volume WAF Blocks - Possible Attack` | Descriptive, action-oriented |
| **Description** | `Detects multiple WAF blocks from the same IP in a short time, indicating possible web application attack or automated scanning.` | Explains what and why |
| **Tactics** | Select: `Initial Access`, `Credential Access` | MITRE ATT&CK mapping |
| **Severity** | `Medium` | Not critical (WAF blocked it) but needs investigation |
| **Status** | `Enabled` | Rule actively runs |

**Why these tactics?**
- **Initial Access (TA0001)**: Attacker trying to gain entry
- **Credential Access (TA0006)**: May be trying SQL injection to steal credentials

**Severity Reasoning:**
- **Low**: Would be for single failed attempts
- **Medium**: Multiple attempts = persistent threat ✓
- **High**: Would be if attacks succeeded
- **Critical**: Would be if data was exfiltrated

Click **Next: Set rule logic**

---

#### **Step 3: Set Rule Logic**

**Rule Query:**

Paste this KQL query:

```kql
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize 
    AttackCount = count(),
    AttackTypes = make_set(ruleName_s),
    SampleURLs = make_set(requestUri_s, 3),
    FirstAttempt = min(TimeGenerated),
    LastAttempt = max(TimeGenerated)
    by clientIP_s, bin(TimeGenerated, 5m)
| where AttackCount > 5
| extend 
    Duration = LastAttempt - FirstAttempt,
    UniqueAttackTypes = array_length(AttackTypes)
| project 
    TimeGenerated,
    AttackerIP = clientIP_s,
    TotalAttempts = AttackCount,
    DifferentAttacks = UniqueAttackTypes,
    AttackDuration = Duration,
    RulesTriggered = AttackTypes,
    ExampleURLs = SampleURLs
```

**Query Breakdown:**

```kql
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
```
→ Get WAF logs only

```kql
| where action_s == "Block"
```
→ Only blocked requests (attacks)

```kql
| summarize 
    AttackCount = count(),
    AttackTypes = make_set(ruleName_s),
    ...
    by clientIP_s, bin(TimeGenerated, 5m)
```
→ Group by IP address and 5-minute time windows  
→ Count attacks, collect rule names, collect sample URLs

```kql
| where AttackCount > 5
```
→ **Only alert if >5 attacks in the time window**  
→ This is our threshold

```kql
| extend Duration = LastAttempt - FirstAttempt
```
→ Calculate how long the attack lasted

**Entity Mapping:**

Scroll down to **"Entity mapping"** section:

Click **"+ Add new entity"**

| Entity Type | Identifier | Value |
|-------------|------------|-------|
| **IP** | Address | `AttackerIP` |

**Why entity mapping?**
- Enables Sentinel to track the IP address across incidents
- Allows you to see all activity from this IP
- Powers the investigation graph

**Alert enrichment:**

Scroll to **"Alert enrichment"** section:

| Setting | Value |
|---------|-------|
| **Alert Name Format** | Leave default |
| **Alert Description Format** | Leave default |
| **Custom Details** | Add these key-value pairs: |

Click **"+ Add custom details"**:

| Key | Value |
|-----|-------|
| `TotalAttempts` | `TotalAttempts` |
| `DifferentAttacks` | `DifferentAttacks` |
| `AttackDuration` | `AttackDuration` |

**This adds context to alerts** - SOC analysts see attack details immediately.

Click **Next: Incident settings**

---

#### **Step 4: Incident Settings**

Configure how incidents are created:

| Setting | Value | Explanation |
|---------|-------|-------------|
| **Create incidents from alerts** | ✅ Enabled | **Critical - must enable!** |
| **Alert grouping** | `Disabled` | Each detection = separate incident (simpler for lab) |
| **Re-open closed incidents** | ✅ Enabled | If attack continues, re-open case |
| **Lookback period** | `5 hours` | Consider events from last 5 hours for grouping |

**Alert Grouping Options (for reference):**
- **Disabled**: Each alert = separate incident (what we chose)
- **Group by entity**: All alerts for same IP = one incident
- **Group by alert**: All alerts from this rule = one incident

**For production:** You'd likely group by entity (IP address)  
**For this lab:** Disabled makes it easier to see how detection works

Click **Next: Automated response**

---

#### **Step 5: Automated Response**

**Automated response** allows you to run Logic Apps (playbooks) when incidents are created.

**Common playbook actions:**
- Send email to SOC team
- Post to Microsoft Teams channel
- Block IP in firewall automatically
- Enrich incident with threat intelligence
- Create ticket in ServiceNow/Jira

**For this lab:**
- Skip automated response (we'll investigate manually)
- Click **Next: Review**

**In production, you might create playbooks like:**

```
When incident created:
  1. Lookup IP in threat intelligence
  2. Check if IP is in known bot list
  3. If malicious AND >10 attacks:
      - Block IP at NSG level
      - Send Teams notification
      - Create Jira ticket
  4. If first-time attacker:
      - Just send email alert
```

---

#### **Step 6: Review + Create**

**Review your configuration:**

- ✅ Rule name: Clear and descriptive
- ✅ Query: Detects >5 blocks per IP per 5 minutes
- ✅ Severity: Medium
- ✅ Entity mapping: IP address
- ✅ Incidents: Enabled
- ✅ Status: Enabled

**Validation checks:**
- **Query syntax**: Verify query runs without errors
- **Test query**: Optional - click "View query results" to test
- **Expected frequency**: Rule will run every 5 minutes

Click **Create**

---

### Post-Creation Validation

**Verify Rule is Active:**

1. **Navigate to Analytics → Active rules**
2. Find your rule: `High Volume WAF Blocks - Possible Attack`
3. Check status: **Enabled** with green checkmark

**Rule details to verify:**

| Property | Expected Value |
|----------|----------------|
| **Status** | Enabled |
| **Severity** | Medium |
| **Last modified** | Current timestamp |
| **Next run** | Within 5 minutes |

---

### Understanding Rule Execution

**How the rule works:**

```
Every 5 minutes:
    ↓
Sentinel runs your KQL query
    ↓
Looks at last 5 minutes of WAF logs
    ↓
Finds IPs with >5 blocks
    ↓
For each matching IP:
        ↓
    Creates incident
        ↓
    Assigns severity: Medium
        ↓
    Maps entities (IP address)
        ↓
    Adds custom details (attack count, types)
        ↓
SOC dashboard shows new incident
```

**Timeline example:**

```
10:00 AM - Rule runs, finds nothing
10:05 AM - Rule runs, finds nothing
10:10 AM - You simulate attacks
10:10-10:12 AM - 8 SQL injection attempts from 203.0.113.45
10:15 AM - Rule runs, detects 8 blocks from 203.0.113.45
10:15 AM - Incident created!
10:16 AM - Appears in Sentinel → Incidents
```

---

### Analytics Rule Best Practices

**Threshold Tuning:**

Our rule uses `> 5` blocks in 5 minutes. Why?

- **Too low (>1)**: Alert fatigue - too many false positives
- **Too high (>50)**: Miss slow, persistent attacks
- **Just right (>5)**: Catches automated scanning without noise

**Tune based on your environment:**
- High-traffic site: Increase to >10
- Internal app: Decrease to >3
- E-commerce site: >5 is good baseline

**Time Window:**

We use 5-minute bins. Why?

- **Too short (1 min)**: Might miss attack spread over time
- **Too long (1 hour)**: Delayed detection
- **5 minutes**: Balance between speed and context

**Query Performance:**

Our query is efficient because:
- ✅ Filters by `Category` first (reduces data)
- ✅ Uses time binning (`bin(TimeGenerated, 5m)`)
- ✅ Aggregates instead of returning all rows
- ✅ Filters after aggregation (`where AttackCount > 5`)

**Inefficient query example (don't do this):**
```kql
AzureDiagnostics  // Gets ALL diagnostics
| where action_s == "Block"  // Scans everything first
| take 1000000  // Returns too many rows
```

---

### Creating Additional Detection Rules

**Suggested additional rules to create:**

**Rule 2: SQL Injection Attack Campaign**
```kql
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| where ruleName_s contains "SQLi"
| summarize Count = count() by clientIP_s, bin(TimeGenerated, 1h)
| where Count > 3
```
- **Purpose**: Specifically detect SQL injection attempts
- **Threshold**: >3 per hour (lower because SQL injection is high-risk)

**Rule 3: Distributed Attack (Multiple IPs)**
```kql
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize UniqueIPs = dcount(clientIP_s) by bin(TimeGenerated, 5m)
| where UniqueIPs > 10
```
- **Purpose**: Detect DDoS or botnet attacks
- **Indicator**: Many different IPs attacking at once

**Rule 4: After-Hours Attacks**
```kql
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| extend Hour = datetime_part("hour", TimeGenerated)
| where Hour >= 22 or Hour <= 6  // 10 PM to 6 AM
| summarize Count = count() by clientIP_s
| where Count > 2
```
- **Purpose**: Attacks during off-hours (when SOC is understaffed)
- **Assumption**: Less legitimate traffic at night

---

### Troubleshooting Analytics Rules

**Problem:** "Rule created but no incidents appearing"

**Possible causes:**

1. **No attacks generated yet**
   - Solution: Simulate attacks (see Testing section)
   - Wait 5 minutes for next rule execution

2. **Threshold too high**
   - Solution: Lower threshold temporarily to test
   - Change `where AttackCount > 5` to `> 1`

3. **Query has no results**
   - Solution: Test query in Logs section first
   - Verify WAF logs are flowing

4. **Incident creation disabled**
   - Solution: Edit rule → Incident settings → Enable

**Problem:** "Too many incidents (alert fatigue)"

**Solutions:**

1. **Increase threshold:**
   ```kql
   | where AttackCount > 10  // Was 5
   ```

2. **Enable alert grouping:**
   - Edit rule → Incident settings
   - Group by entity (IP address)
   - All alerts for same IP = one incident

3. **Add more filters:**
   ```kql
   | where AttackCount > 5
   | where UniqueAttackTypes > 2  // Multiple attack types
   ```

**Problem:** "Rule runs but returns errors"

**Check:**
- Query syntax (paste in Logs section to test)
- Field names match actual log fields
- Time ranges are valid

---

### Monitoring Rule Performance

**Check rule execution history:**

1. Navigate to **Analytics → Active rules**
2. Click on your rule
3. View **"Recent executions"** tab

**Metrics to monitor:**

| Metric | What It Means | Good Value |
|--------|---------------|------------|
| **Execution time** | How long query takes | <30 seconds |
| **Results returned** | How many matches | Depends on activity |
| **Errors** | Query failures | 0 |
| **Incidents created** | Alerts generated | Varies |

**Optimization tips:**

- If execution time >1 minute, optimize query
- If 0 results always, threshold may be too high
- If hundreds of incidents, threshold too low

---

### Summary: What We Built

✅ **Detection rule** that automatically finds web attacks  
✅ **Threshold-based** detection (>5 blocks per IP per 5 min)  
✅ **Incident creation** for SOC investigation  
✅ **Entity mapping** to track attacker IPs  
✅ **Custom details** for rich alert context  
✅ **MITRE ATT&CK mapping** for threat categorization  

**Next step:** Simulate attacks to test detection!

---

## Step 8: Simulate Web Application Attacks

### ⚠️ Important Legal & Ethical Notice

**CRITICAL - READ BEFORE PROCEEDING:**

✅ **ALLOWED:**
- Attacking **your own** Azure resources (this lab)
- Testing **your own** applications
- Authorized penetration testing

❌ **ILLEGAL:**
- Attacking others' systems without permission
- Using these techniques on production systems you don't own
- Scanning/attacking websites without authorization

**Legal consequences:**
- Federal prison time (Computer Fraud and Abuse Act)
- Heavy fines
- Civil lawsuits
- Career-ending criminal record

**For this lab:**
- You're attacking YOUR OWN Front Door endpoint
- You OWN the Azure subscription
- This is YOUR infrastructure
- **Completely legal and safe**

---

### Why Simulate Attacks?

**Testing validates that:**
- ✅ WAF is actually blocking attacks (not just configured)
- ✅ Logs are being generated
- ✅ Sentinel detection rules work
- ✅ Incidents are created correctly
- ✅ You can investigate attacks

**Think of this as:**
- Fire drill for your security system
- Proof that your defenses work
- Practice for real incident response

---

### Attack Testing Methods

**Method 1: Browser (Easiest)**
- Just visit URLs with malicious payloads
- Good for beginners
- Limited attack variety

**Method 2: curl/Postman (Better)**
- Command-line testing
- More control over requests
- Can script repeated attacks

**Method 3: OWASP ZAP / Burp Suite (Advanced)**
- Automated attack scanning
- Full penetration testing capabilities
- Overkill for this lab

**For this lab:** We'll use **Method 1 (Browser)** and **Method 2 (curl)**

---

### Test Attack Payloads

Here are safe attack payloads to test WAF protection:

#### **1. SQL Injection Attacks**

**What they target:**
- Database queries
- Try to extract data or bypass authentication

**Test payloads:**

```
Basic SQL Injection:
https://yourname-waf.azurefd.net/?id=1' OR '1'='1

Union-Based SQL Injection:
https://yourname-waf.azurefd.net/?id=1' UNION SELECT NULL--

Time-Based Blind SQL Injection:
https://yourname-waf.azurefd.net/?id=1' AND SLEEP(5)--

Boolean-Based SQL Injection:
https://yourname-waf.azurefd.net/?id=1' AND '1'='1

Comment Injection:
https://yourname-waf.azurefd.net/?id=1'--

Stacked Queries:
https://yourname-waf.azurefd.net/?id=1'; DROP TABLE users--
```

**Expected Result:** All should return **403 Forbidden**

---

#### **2. Cross-Site Scripting (XSS) Attacks**

**What they target:**
- Inject malicious JavaScript
- Steal cookies, session tokens
- Deface websites

**Test payloads:**

```
Basic XSS:
https://yourname-waf.azurefd.net/?q=<script>alert('XSS')</script>

Image Tag XSS:
https://yourname-waf.azurefd.net/?q=<img src=x onerror=alert(1)>

Event Handler XSS:
https://yourname-waf.azurefd.net/?q=<body onload=alert('XSS')>

SVG XSS:
https://yourname-waf.azurefd.net/?q=<svg/onload=alert(1)>

Encoded XSS:
https://yourname-waf.azurefd.net/?q=%3Cscript%3Ealert(1)%3C/script%3E

DOM-Based XSS:
https://yourname-waf.azurefd.net/?q=<iframe src=javascript:alert(1)>
```

**Expected Result:** All should return **403 Forbidden**

---

#### **3. Path Traversal / Local File Inclusion**

**What they target:**
- Read sensitive files on server
- Access configuration files, passwords

**Test payloads:**

```
Basic Path Traversal:
https://yourname-waf.azurefd.net/?file=../../../etc/passwd

Windows Path Traversal:
https://yourname-waf.azurefd.net/?file=..\..\..\windows\system32\config\sam

Encoded Path Traversal:
https://yourname-waf.azurefd.net/?file=%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd

Null Byte Injection:
https://yourname-waf.azurefd.net/?file=../../../etc/passwd%00.jpg

Double Encoding:
https://yourname-waf.azurefd.net/?file=%252e%252e%252fetc%252fpasswd
```

**Expected Result:** All should return **403 Forbidden**

---

#### **4. Remote Code Execution (RCE) Attempts**

**What they target:**
- Execute system commands
- Gain shell access

**Test payloads:**

```
Basic Command Injection:
https://yourname-waf.azurefd.net/?cmd=;ls -la

Windows Command Injection:
https://yourname-waf.azurefd.net/?cmd=| dir

PHP Code Injection:
https://yourname-waf.azurefd.net/?cmd=<?php system('whoami'); ?>

Bash Command:
https://yourname-waf.azurefd.net/?cmd=`whoami`

PowerShell:
https://yourname-waf.azurefd.net/?cmd=$(Get-Process)
```

**Expected Result:** All should return **403 Forbidden**

---

### Step-by-Step Attack Simulation

#### **Exercise 1: Basic Attack Test (Browser)**

**Goal:** Verify WAF blocks simple SQL injection

**Steps:**

1. **Open browser** (incognito/private window recommended)

2. **Copy this URL** (replace `yourname` with your actual endpoint):
   ```
   https://yourname-waf.azurefd.net/?id=1' OR '1'='1
   ```

3. **Paste into address bar** and press Enter

4. **Expected Result:**
   ```
   403 Forbidden
   
   The request has been blocked by Web Application Firewall
   Request ID: 0abc1234-5678-90de-fghi-jklmnop12345
   ```

5. **What just happened:**
   - Front Door received your request
   - WAF inspected the URL parameter `?id=1' OR '1'='1`
   - OWASP rule detected SQL injection pattern
   - Request blocked before reaching App Service
   - Block logged to Log Analytics

**✅ Success Criteria:**
- You see 403 error page
- Request is blocked (not 200 OK)
- Error mentions "Web Application Firewall"

---

#### **Exercise 2: Multiple Attack Types (Browser)**

**Goal:** Trigger multiple different rules

**Steps:**

Try each of these URLs one at a time (wait 10 seconds between each):

1. **SQL Injection:**
   ```
   https://yourname-waf.azurefd.net/?id=1' UNION SELECT NULL--
   ```

2. **XSS:**
   ```
   https://yourname-waf.azurefd.net/?q=<script>alert(1)</script>
   ```

3. **Path Traversal:**
   ```
   https://yourname-waf.azurefd.net/?file=../../../etc/passwd
   ```

4. **Command Injection:**
   ```
   https://yourname-waf.azurefd.net/?cmd=;ls
   ```

5. **XSS (Image Tag):**
   ```
   https://yourname-waf.azurefd.net/?q=<img src=x onerror=alert(1)>
   ```

**Expected:** All 5 should return **403 Forbidden**

**Different error IDs:** Each request gets unique Request ID for tracking

---

#### **Exercise 3: Automated Attack Simulation (curl)**

**Goal:** Simulate attacker's automated scanning

**Prerequisites:**
- Windows: Use PowerShell or WSL
- Mac/Linux: Use Terminal
- Tool: `curl` (pre-installed on most systems)

**Attack Script:**

**For PowerShell (Windows):**

```powershell
# SQL Injection attacks
$endpoint = "https://yourname-waf.azurefd.net"  # Replace with your endpoint

Write-Host "Simulating SQL Injection attack campaign..." -ForegroundColor Yellow

$attacks = @(
    "?id=1' OR '1'='1",
    "?id=1' UNION SELECT NULL--",
    "?id=1' AND SLEEP(5)--",
    "?id=1'; DROP TABLE users--",
    "?id=1' AND '1'='1",
    "?user=admin' OR '1'='1'--",
    "?id=1' UNION SELECT username, password FROM users--"
)

foreach ($attack in $attacks) {
    $url = $endpoint + $attack
    Write-Host "Attacking: $url" -ForegroundColor Red
    
    try {
        $response = Invoke-WebRequest -Uri $url -UseBasicParsing
        Write-Host "Response: $($response.StatusCode)" -ForegroundColor Green
    } catch {
        Write-Host "Blocked: 403 Forbidden" -ForegroundColor Cyan
    }
    
    Start-Sleep -Seconds 2  # Wait 2 seconds between attacks
}

Write-Host "Attack simulation complete!" -ForegroundColor Yellow
```

**For Bash (Linux/Mac):**

```bash
#!/bin/bash

endpoint="https://yourname-waf.azurefd.net"  # Replace with your endpoint

echo "Simulating SQL Injection attack campaign..."

attacks=(
    "?id=1' OR '1'='1"
    "?id=1' UNION SELECT NULL--"
    "?id=1' AND SLEEP(5)--"
    "?id=1'; DROP TABLE users--"
    "?id=1' AND '1'='1"
    "?user=admin' OR '1'='1'--"
    "?id=1' UNION SELECT username, password FROM users--"
)

for attack in "${attacks[@]}"
do
    url="${endpoint}${attack}"
    echo "Attacking: $url"
    
    status=$(curl -s -o /dev/null -w "%{http_code}" "$url")
    
    if [ "$status" = "403" ]; then
        echo "Blocked: 403 Forbidden"
    else
        echo "Response: $status"
    fi
    
    sleep 2  # Wait 2 seconds between attacks
done

echo "Attack simulation complete!"
```

**How to run:**

**PowerShell:**
1. Save script as `attack-simulation.ps1`
2. Edit line 2 with your Front Door URL
3. Run: `.\attack-simulation.ps1`

**Bash:**
1. Save script as `attack-simulation.sh`
2. Edit line 3 with your Front Door URL
3. Make executable: `chmod +x attack-simulation.sh`
4. Run: `./attack-simulation.sh`

**Expected Output:**
```
Simulating SQL Injection attack campaign...
Attacking: https://yourname-waf.azurefd.net/?id=1' OR '1'='1
Blocked: 403 Forbidden
Attacking: https://yourname-waf.azurefd.net/?id=1' UNION SELECT NULL--
Blocked: 403 Forbidden
Attacking: https://yourname-waf.azurefd.net/?id=1' AND SLEEP(5)--
Blocked: 403 Forbidden
...
Attack simulation complete!
```

---

#### **Exercise 4: Mixed Attack Campaign**

**Goal:** Simulate realistic attacker behavior (multiple attack types)

**PowerShell Script:**

```powershell
$endpoint = "https://yourname-waf.azurefd.net"

Write-Host "Simulating mixed attack campaign..." -ForegroundColor Yellow

$attacks = @(
    # SQL Injection
    @{Type="SQLi"; Payload="?id=1' OR '1'='1"},
    @{Type="SQLi"; Payload="?id=1' UNION SELECT NULL--"},
    
    # XSS
    @{Type="XSS"; Payload="?q=<script>alert(1)</script>"},
    @{Type="XSS"; Payload="?q=<img src=x onerror=alert(1)>"},
    
    # Path Traversal
    @{Type="LFI"; Payload="?file=../../../etc/passwd"},
    @{Type="LFI"; Payload="?file=..\..\..\windows\system32\config\sam"},
    
    # Command Injection
    @{Type="RCE"; Payload="?cmd=;ls -la"},
    @{Type="RCE"; Payload="?cmd=| dir"}
)

foreach ($attack in $attacks) {
    $url = $endpoint + $attack.Payload
    Write-Host "[$($attack.Type)] $url" -ForegroundColor Red
    
    try {
        Invoke-WebRequest -Uri $url -UseBasicParsing | Out-Null
        Write-Host "  Response: Allowed (Unexpected!)" -ForegroundColor Red
    } catch {
        Write-Host "  Response: Blocked ✓" -ForegroundColor Green
    }
    
    Start-Sleep -Seconds 2
}

Write-Host "`nAttack campaign complete!" -ForegroundColor Yellow
Write-Host "Check Sentinel for incidents in 5-10 minutes." -ForegroundColor Cyan
```

**This simulates:**
- Attacker trying multiple exploit types
- Realistic scanning behavior
- Varied attack vectors

---

### Validation: Verify Attacks Were Logged

**Wait 5 minutes** after running attacks, then check logs.

**Navigate to Sentinel → Logs:**

**Query 1: Verify attacks were logged**

```kql
AzureDiagnostics
| where TimeGenerated > ago(15m)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| project TimeGenerated, clientIP_s, requestUri_s, ruleName_s
| sort by TimeGenerated desc
```

**Expected output:**
You should see your recent attacks listed with:
- Your IP address
- The malicious URLs you tested
- Which OWASP rules caught them

**Query 2: Count your attacks**

```kql
AzureDiagnostics
| where TimeGenerated > ago(15m)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize Count = count()
```

**Expected:** Should match number of attacks you simulated (e.g., 7-8)

---

### Troubleshooting Attack Tests

**Problem:** "Attacks return 200 OK instead of 403"

**Possible causes:**

1. **WAF in Detection mode instead of Prevention**
   - Solution: Go to WAF policy → Policy settings
   - Change mode to "Prevention"
   - Wait 5 minutes for propagation

2. **WAF policy not attached to Front Door**
   - Solution: Front Door → Security → Verify policy attached
   - Re-attach if needed

3. **Using wrong URL**
   - Solution: Make sure using `azurefd.net` domain, not `azurewebsites.net`
   - Direct App Service access bypasses WAF

**Problem:** "403 errors but no logs in Sentinel"

**Possible causes:**

1. **Log ingestion delay**
   - Solution: Wait 5-10 minutes
   - Normal delay for logs to appear

2. **Diagnostic logs not enabled**
   - Solution: Front Door → Diagnostic settings
   - Verify "FrontdoorWebApplicationFirewallLog" enabled

3. **Logs going to wrong workspace**
   - Solution: Check diagnostic settings destination
   - Should be `law-waf-lab`

**Problem:** "Cannot run curl/PowerShell scripts"

**Solutions:**

1. **PowerShell execution policy:**
   ```powershell
   Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
   ```

2. **curl not found:**
   - Windows: Use PowerShell's `Invoke-WebRequest` instead
   - Mac/Linux: curl should be pre-installed

3. **Syntax errors:**
   - Copy script exactly as shown
   - Watch for quote character issues (use straight quotes, not smart quotes)

---

### Best Practices for Attack Testing

**DO:**
✅ Test against your own resources only  
✅ Document what you're testing and why  
✅ Use realistic but safe payloads  
✅ Verify blocks are logged  
✅ Space out attacks (realistic timing)  

**DON'T:**
❌ Test against production systems without approval  
❌ Run DDoS-level attack volumes (thousands of requests/second)  
❌ Share attack scripts without context  
❌ Test during business-critical hours  
❌ Use actual exploit code that could cause harm  

**For this lab:**
- 5-10 attacks every few minutes is plenty
- Demonstrates WAF effectiveness
- Generates enough logs for Sentinel
- Won't trigger Azure rate limits

---

### Understanding Attack Results

**What each attack teaches:**

**SQL Injection blocked:**
- ✅ Database is protected from unauthorized queries
- ✅ Can't extract data through URL manipulation
- ✅ Authentication bypass prevented

**XSS blocked:**
- ✅ Can't inject malicious JavaScript
- ✅ Session hijacking prevented
- ✅ Website defacement blocked

**Path Traversal blocked:**
- ✅ Can't read sensitive server files
- ✅ Configuration files protected
- ✅ Source code not accessible

**Command Injection blocked:**
- ✅ Can't execute system commands
- ✅ Server compromise prevented
- ✅ Lateral movement blocked

---

### Next Steps

After simulating attacks:

1. **Wait 10-15 minutes** for:
   - Logs to fully ingest
   - Sentinel analytics rule to run
   - Incidents to be created

2. **Check Sentinel → Incidents:**
   - Should see incident: "High Volume WAF Blocks - Possible Attack"
   - Severity: Medium
   - Entity: Your IP address

3. **Investigate the incident:**
   - See next section for full investigation walkthrough

---

## Step 9: Investigate Security Incidents

### What is Incident Investigation?

**Incident Investigation** is the process of:
1. **Analyzing** security alerts to determine if they're real threats
2. **Understanding** what happened (who, what, when, where, why)
3. **Determining scope** (how many systems affected)
4. **Deciding response** (contain, remediate, or dismiss)
5. **Documenting findings** for compliance and lessons learned

**Think of it like detective work:**
- Incident = crime scene
- Logs = evidence
- Sentinel = forensic lab
- You = detective solving the case

---

### Accessing Incidents in Sentinel

**Step 1: Navigate to Incidents**

1. Open **Microsoft Sentinel**
2. Click on your workspace (`law-waf-lab`)
3. In left menu, click **"Incidents"**

**Step 2: Find Your Incident**

You should see an incident similar to:

```
🔶 High Volume WAF Blocks - Possible Attack
Severity: Medium
Status: New
Time: [Timestamp of your attacks]
Entities: 1 IP address
```

**If you don't see an incident:**
- Wait 5-10 more minutes (analytics rule runs every 5 minutes)
- Verify you generated >5 attacks in 5-minute window
- Check Analytics rule is enabled
- Verify attacks were logged (run KQL query)

---

### Understanding the Incident Card

**Incident Overview:**

| Field | Example Value | What It Means |
|-------|---------------|---------------|
| **Title** | High Volume WAF Blocks - Possible Attack | Name from analytics rule |
| **Severity** | Medium | Impact level (Low/Medium/High/Critical) |
| **Status** | New | Investigation state |
| **Owner** | Unassigned | Who's investigating |
| **Incident ID** | #1234 | Unique identifier |
| **Created** | 2026-04-19 10:35 | When detected |
| **Entities** | 1 IP | Related entities discovered |
| **Alerts** | 1 | Number of underlying alerts |
| **Tactics** | Initial Access | MITRE ATT&CK mapping |

---

### Opening and Analyzing an Incident

**Step 1: Open Full Details**

Click on the incident to open full investigation view.

**You'll see several tabs:**

---

#### **Tab 1: Overview**

**Left Panel - Incident Information:**

```
Incident: #1234
Created: 2026-04-19 10:35:22
Status: New
Severity: Medium
Owner: Unassigned

Description:
Detects multiple WAF blocks from the same IP in a short time, 
indicating possible web application attack or automated scanning.

MITRE ATT&CK:
• Initial Access (TA0001)
• Credential Access (TA0006)
```

**Right Panel - Timeline:**

Shows when alerts fired and when incident was created.

**Entities Section:**

```
📍 IP Address
   203.0.113.45 (Your IP from attacks)
   
   Related Activities:
   • 8 alerts associated
   • First seen: 10:30:00
   • Last seen: 10:35:00
```

**Custom Details (from our analytics rule):**

```
TotalAttempts: 8
DifferentAttacks: 3
AttackDuration: 00:05:00
```

**This tells us:**
- 8 blocked attacks
- 3 different OWASP rules triggered
- Attack lasted 5 minutes

---

#### **Tab 2: Alerts**

Shows the underlying detection that triggered this incident.

**Alert Details:**

```
Alert: High Volume WAF Blocks - Possible Attack
Time: 2026-04-19 10:35:22
Severity: Medium

Query Results:
┌─────────────────────┬──────────────┬──────────┬─────────────┐
│ AttackerIP          │ TotalAttempts│ Different│ RulesTriggered│
│                     │              │ Attacks  │              │
├─────────────────────┼──────────────┼──────────┼─────────────┤
│ 203.0.113.45        │ 8            │ 3        │ SQLi-942100, │
│                     │              │          │ XSS-941100,  │
│                     │              │          │ LFI-930100   │
└─────────────────────┴──────────────┴──────────┴─────────────┘

Example URLs:
• /?id=1' OR '1'='1
• /?q=<script>alert(1)</script>
• /?file=../../../etc/passwd
```

**This gives us:**
- Exact attack count
- Which rules were triggered
- Sample malicious requests

---

#### **Tab 3: Investigation**

**Investigation Graph** - Visual representation of the attack:

```
        [IP: 203.0.113.45]
               ↓
        [WAF Policy: wafpolicy001]
               ↓
     ┌─────────┴──────────┐
     ↓                    ↓
[SQLi Attack]      [XSS Attack]
 (5 attempts)       (3 attempts)
```

**Interactive features:**
- Click on IP to see all related activity
- Click on attack types to drill down
- Explore connections

**Add to Investigation:**

You can manually add related entities:
- Other IPs that might be related
- User accounts (if applicable)
- Other incidents from same campaign

---

### Investigation Checklist

**For each incident, answer these questions:**

#### **1. WHO attacked?**

```kql
// Find attacker details
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where clientIP_s == "203.0.113.45"  // The IP from incident
| summarize 
    TotalRequests = count(),
    FirstSeen = min(TimeGenerated),
    LastSeen = max(TimeGenerated),
    UniqueURLs = dcount(requestUri_s)
```

**Document:**
- IP Address: 203.0.113.45
- Geo-location: [Use IP lookup tools like ipinfo.io]
- ISP/Organization: [Whois lookup]
- Known malicious? [Check VirusTotal, AbuseIPDB]

---

#### **2. WHAT did they attack?**

```kql
// List all malicious requests
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where clientIP_s == "203.0.113.45"
| where action_s == "Block"
| project TimeGenerated, requestUri_s, ruleName_s, matchVariableName_s
| sort by TimeGenerated asc
```

**Analyze:**
- Attack types (SQLi, XSS, LFI, RCE)
- Which URLs targeted
- Parameters attacked
- Attack sophistication

**Example output:**
```
Time: 10:30:00  URL: /?id=1' OR '1'='1          Rule: SQLi-942100
Time: 10:30:15  URL: /?id=1' UNION SELECT--     Rule: SQLi-942100
Time: 10:31:00  URL: /?q=<script>alert(1)</script>  Rule: XSS-941100
Time: 10:32:00  URL: /?file=../../../etc/passwd     Rule: LFI-930100
```

**Pattern recognition:**
- Sequential SQLi attempts = testing different payloads
- Then XSS = broad scanning
- Then LFI = escalating to file access
- Suggests: Automated scanning tool (not manual)

---

#### **3. WHEN did the attack occur?**

```kql
// Attack timeline
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where clientIP_s == "203.0.113.45"
| where action_s == "Block"
| summarize Count = count() by bin(TimeGenerated, 1m)
| render timechart
```

**Timing analysis:**
- **Duration:** 10:30 AM - 10:35 AM (5 minutes)
- **Pattern:** Steady rate (not burst)
- **Time of day:** Business hours (vs. night = more suspicious)

---

#### **4. WHERE did they attack from?**

**External Investigation:**

1. **IP Geolocation:**
   - Visit: https://ipinfo.io/203.0.113.45
   - Note: Country, City, ISP

2. **Reputation Check:**
   - Visit: https://www.abuseipdb.com/check/203.0.113.45
   - Check if IP is known malicious

3. **VirusTotal:**
   - Visit: https://www.virustotal.com/gui/ip-address/203.0.113.45
   - See if flagged by threat intelligence

**For this lab:**
- It's YOUR IP (you simulated the attacks)
- In real scenario, you'd check if IP is:
  - Tor exit node
  - VPN provider
  - Known botnet
  - Residential ISP (less suspicious)
  - Cloud provider (could be compromised server)

---

#### **5. WHY / HOW did they find us?**

**Possible scenarios:**

**Scenario A: Opportunistic Scanning**
- Automated bot crawling internet
- Found your Front Door endpoint
- Tried common exploits
- **Likelihood:** High (most common)

**Scenario B: Targeted Attack**
- Attacker specifically chose your site
- Reconnaissance performed
- Multiple attack vectors tested
- **Likelihood:** Low (unless high-profile target)

**Scenario C: Vulnerability Scanner**
- Legitimate security scanner
- Pentest or bug bounty program
- **Likelihood:** Check if IP belongs to known scanner (Qualys, Rapid7)

**For our lab:**
- Scenario D: Security Testing (us!)

---

#### **6. WHAT was the impact?**

**Impact Assessment:**

```kql
// Check if any attacks succeeded
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where clientIP_s == "203.0.113.45"
| summarize 
    Blocked = countif(action_s == "Block"),
    Allowed = countif(action_s == "Allow")
```

**Expected for our lab:**
- Blocked: 8
- Allowed: 0
- **Impact: NONE** - All attacks blocked!

**Questions to answer:**
- ✅ Did WAF block all attacks? → Yes
- ✅ Did any reach backend? → No
- ✅ Data exfiltration? → No
- ✅ System compromise? → No
- ✅ Required actions? → Monitor, potentially block IP

---

### Incident Classification

**Based on investigation, classify the incident:**

| Classification | Criteria | Action |
|----------------|----------|--------|
| **True Positive** | Real attack, correctly detected | Investigate & respond |
| **False Positive** | Legitimate activity flagged as attack | Tune rule, close incident |
| **Benign Positive** | Real scan but authorized (pentest) | Document, close incident |
| **Informational** | Interesting but not actionable | Close, adjust severity |

**For our lab incident:**
- **Classification:** True Positive (intentional test)
- **Severity:** Correctly Medium
- **Action:** Document and close (no real threat)

---

### Taking Action on Incidents

**Available Actions:**

#### **1. Change Status**

- **New** → **Active** (when you start investigating)
- **Active** → **Closed** (when resolved)

**Status meanings:**
- **New:** Just created, not yet looked at
- **Active:** Under investigation
- **Closed:** Investigation complete

#### **2. Assign Owner**

- Click **"Assign to me"** to take ownership
- Or assign to another analyst
- Tracks who's responsible

#### **3. Change Severity**

- Upgrade: Medium → High (if more serious than initially thought)
- Downgrade: Medium → Low (if less serious)

**For our lab:** Medium is appropriate (attacks blocked, no damage)

#### **4. Add Comments**

Document your investigation:

```
Investigation Notes:
- Analyzed 8 blocked attack attempts
- Verified all from IP 203.0.113.45
- Attack types: SQL injection (5), XSS (2), LFI (1)
- Impact: None - all attacks blocked by WAF
- External check: IP is residential ISP, not known malicious
- Assessment: Opportunistic scanning, automated tool
- Recommendation: Monitor, no immediate action required
- Closing as resolved
```

#### **5. Create Tasks**

Assign follow-up actions:
- Block IP at firewall level
- Update threat intelligence
- Notify ISP abuse contact
- Review WAF rules for tuning

#### **6. Close Incident**

**Classification options:**
- **True Positive - Suspicious Activity:** Real threat, correctly detected ✅
- **Benign Positive - Suspicious but Expected:** Authorized activity
- **False Positive - Incorrect Alert Logic:** Not actually malicious
- **Undetermined:** Not enough info to classify

**For our lab:**
- Select: **"True Positive - Suspicious Activity"**
- Add closure comment documenting findings
- Click **"Close"**

---

### Real-World Response Actions

**In production environment, you might:**

#### **Immediate Actions (During Attack):**

1. **Block at network level:**
   ```
   Azure NSG rule: Deny 203.0.113.45
   Or
   Add to WAF custom deny list
   ```

2. **Rate limiting:**
   - If attacks continue from different IPs
   - Implement CAPTCHA challenges
   - Geo-blocking if from specific countries

3. **Escalate if:**
   - >100 attacks per minute (DDoS)
   - Attacks from many IPs (distributed)
   - Any attacks succeeded (urgent!)

#### **Post-Incident Actions:**

1. **Threat intelligence:**
   - Report IP to AbuseIPDB
   - Share with ISP abuse contacts
   - Add to internal blocklist

2. **Detection tuning:**
   - Review if rules need adjustment
   - Too sensitive? (False positives)
   - Too lenient? (Missed attacks)

3. **Application hardening:**
   - Even though WAF blocked, fix underlying vulnerabilities
   - Parameterize SQL queries
   - Sanitize user input
   - Update frameworks

4. **Reporting:**
   - Document for compliance (PCI-DSS requires this)
   - Executive summary if significant
   - Trend analysis (monthly reports)

---

### Investigation Best Practices

**DO:**
✅ Document everything in comments  
✅ Check external threat intelligence  
✅ Verify impact before escalating  
✅ Close incidents with clear classification  
✅ Learn from each investigation  

**DON'T:**
❌ Ignore "low severity" incidents  
❌ Close without investigation  
❌ Assume blocked = no impact  
❌ Skip documenting your findings  
❌ Forget to check for related incidents  

---

### Sample Investigation Report

**Incident Investigation Summary**

```markdown
# Incident #1234: High Volume WAF Blocks

## Summary
Multiple web application attacks blocked by Azure Front Door WAF 
from a single source IP address over 5-minute period.

## Timeline
- 10:30:00 - First attack detected (SQL injection)
- 10:30:15 - Continued SQL injection attempts (varied payloads)
- 10:31:00 - Pivot to XSS attacks
- 10:32:00 - Local File Inclusion attempts
- 10:35:00 - Attacks ceased
- 10:35:22 - Incident auto-created by Sentinel

## Attacker Profile
- IP Address: 203.0.113.45
- Geolocation: [Your location]
- ISP: [Your ISP]
- Reputation: No prior malicious activity (AbuseIPDB clean)
- Assessment: Likely automated security scanner

## Attack Details
- Total attempts: 8
- Attack types:
  - SQL Injection: 5 attempts (942100)
  - Cross-Site Scripting: 2 attempts (941100)
  - Local File Inclusion: 1 attempt (930100)
- Target: Main application endpoint
- Method: URL parameter injection

## Impact Assessment
- ✅ All attacks blocked by WAF (100% block rate)
- ✅ No traffic reached backend App Service
- ✅ No data exfiltration
- ✅ No system compromise
- ✅ No customer impact

## Response Actions
1. Verified WAF protection effective
2. Confirmed logs properly captured
3. No immediate blocking required (attacks ceased)
4. Recommended: Monitor for repeat attempts from same IP

## Root Cause
- Public endpoint discoverable via search engines
- No authentication on public pages (expected/normal)
- Automated scanner found endpoint and tested common exploits

## Recommendations
1. Continue monitoring for similar patterns
2. Consider implementing rate limiting for >10 requests/minute
3. Review application code for SQL injection vulnerabilities 
   (defense in depth - don't rely solely on WAF)
4. Quarterly review of WAF rules and false positive rate

## Closure
- Classification: True Positive - Suspicious Activity
- Severity: Medium (appropriate)
- Status: Closed
- Owner: [Your name]
- Date: 2026-04-19 11:00
```

---

### Advanced Investigation Techniques

#### **Technique 1: Correlation with Other Incidents**

**Check if this IP attacked other systems:**

```kql
// Search all security logs for this IP
search "203.0.113.45"
| where TimeGenerated > ago(30d)
| summarize count() by $table, Category
```

**Might find:**
- Same IP in Azure AD sign-in logs (credential attacks)
- Same IP in other WAF policies (multi-target campaign)
- Related IPs (same /24 subnet)

#### **Technique 2: User-Agent Analysis**

```kql
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where clientIP_s == "203.0.113.45"
| distinct userAgent_s
```

**User-Agent tells you:**
- Automated tool: `sqlmap/1.4`, `Nikto/2.1.6`, `Acunetix`
- Browser: `Mozilla/5.0...` (possibly manual)
- Script: `python-requests/2.28.0`

#### **Technique 3: Attack Velocity**

```kql
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where clientIP_s == "203.0.113.45"
| where action_s == "Block"
| summarize Count = count() by bin(TimeGenerated, 10s)
| extend RequestsPerSecond = Count / 10.0
| summarize AvgRPS = avg(RequestsPerSecond), MaxRPS = max(RequestsPerSecond)
```

**Interpretation:**
- <1 req/sec: Manual testing or slow scanner
- 1-10 req/sec: Moderate automated scanning
- >10 req/sec: Aggressive scanning or attack
- >100 req/sec: DDoS or high-speed exploitation

---

### Next Steps After Investigation

**After closing this incident:**

1. **Create detection improvements:**
   - Did rule catch attack appropriately?
   - Too sensitive? (Adjust threshold)
   - Missed anything? (Add new rules)

2. **Update runbooks:**
   - Document investigation steps
   - Create playbooks for similar incidents
   - Share learnings with team

3. **Trend analysis:**
   - Monthly: How many attacks blocked?
   - Which attack types most common?
   - Are attacks increasing?

4. **Compliance reporting:**
   - Export incident details for audits
   - Required for PCI-DSS, SOC 2, ISO 27001

---

## Summary: Investigation Complete! ✅

**What you learned:**
- How to navigate Sentinel incident interface
- Investigation methodology (who, what, when, where, why, how)
- Using KQL for deep-dive analysis
- External threat intelligence lookups
- Incident classification and closure
- Documenting findings
- Real-world response actions

**Incident Resolution:**
- ✅ Investigated thoroughly
- ✅ Verified WAF protection worked
- ✅ Confirmed no impact
- ✅ Documented findings
- ✅ Closed with proper classification

**In production:**
- This same process for every security incident
- More complex investigations for successful attacks
- Escalation to incident response team when needed
- Integration with SOAR playbooks for automation

---

Now let's move to the final steps: cleanup and project documentation!

---

## 🧹 Cleanup

### ⚠️ CRITICAL: Delete Resources to Avoid Charges

**Azure Front Door Premium** costs approximately **$8-10 per day** even if idle!

Forgetting to delete resources is the #1 mistake in cloud labs.

---

### Quick Cleanup (Delete Everything)

**Easiest method: Delete the entire Resource Group**

This deletes ALL resources at once: Front Door, App Service, WAF Policy, Log Analytics, Sentinel.

**Steps:**

1. **Navigate to Resource Groups:**
   - Azure Portal → Resource Groups
   - Click on `rg-waf-lab`

2. **Delete Resource Group:**
   - Click **"Delete resource group"** (top toolbar)
   - Type the resource group name to confirm: `rg-waf-lab`
   - Click **"Delete"**
   - Wait 5-10 minutes for deletion to complete

3. **Verify Deletion:**
   - Resource group should disappear from list
   - Go to "All resources" - should see none from this lab

✅ **Done!** All resources deleted, no further charges.

---

### Detailed Cleanup (Delete Individually)

**If you want to keep some resources, delete in this order:**

#### **1. Delete Front Door** (Highest cost - delete first!)

```
Resource Groups → rg-waf-lab → fd-waf-lab
→ Delete
→ Confirm
```

**Wait:** 5 minutes for deletion

#### **2. Delete WAF Policy**

```
Resource Groups → rg-waf-lab → wafpolicy001
→ Delete
→ Confirm
```

#### **3. Disable Sentinel** (Must do before deleting workspace)

```
Microsoft Sentinel → law-waf-lab
→ Settings (gear icon)
→ Remove workspace
→ Confirm
```

#### **4. Delete Log Analytics Workspace**

```
Log Analytics Workspaces → law-waf-lab
→ Delete
→ Confirm
```

**Note:** Soft-delete by default (recoverable for 14 days)

#### **5. Delete App Service**

```
App Services → yourname-waf-demo
→ Delete
→ Confirm
```

**If you created App Service Plan:**
```
App Service Plans → [your plan name]
→ Delete
```

#### **6. Delete Resource Group** (If empty)

```
Resource Groups → rg-waf-lab
→ Delete resource group
→ Type name to confirm
→ Delete
```

---

### Verification Checklist

After deletion, verify nothing remains:

**Check 1: Resource Groups**
```
Azure Portal → Resource Groups
→ Should NOT see: rg-waf-lab
```

**Check 2: All Resources**
```
Azure Portal → All Resources
→ Filter by "waf-lab" or your names
→ Should return: 0 results
```

**Check 3: Cost Analysis**
```
Azure Portal → Subscriptions → Cost Analysis
→ Look for charges from these resources
→ Should stop accruing after today
```

**Check 4: Activity Log**
```
Azure Portal → Activity Log
→ Filter: Last hour
→ Look for "Delete" operations
→ Should see deletion confirmations
```

---

### Cost Verification

**After 24-48 hours**, check your billing:

```
Azure Portal → Cost Management + Billing
→ Cost analysis
→ Group by: Resource
→ Filter: Last 7 days
```

**Expected costs for this lab:**
- **Front Door Premium**: $0.35/hour × [hours running] = ~$1-3
- **App Service F1**: $0 (free tier)
- **Log Analytics**: $0 (under 5GB free tier)
- **Sentinel**: $0 (under 10GB free first 31 days)

**Total expected:** $1-5 depending on how long you ran the lab

**If charges are higher:**
- Check if resources were truly deleted
- Verify no other resources created (snapshots, etc.)
- Review itemized billing

---

### What If I Forget to Delete?

**Scenario:** You forget and leave resources running for 30 days.

**Estimated charges:**
- Front Door Premium: ~$250-300/month
- Other resources: ~$50/month
- **Total:** ~$300-350

**Mitigation:**

1. **Set up Azure Budget Alerts:**
   ```
   Cost Management → Budgets
   → Create budget
   → Set threshold: $10
   → Alert when 80% consumed
   ```

2. **Use Azure Cost Alerts:**
   ```
   Cost Management → Cost alerts
   → Create alert
   → Notify when daily spend >$1
   ```

3. **Schedule calendar reminder:**
   - Set alarm for lab completion day
   - "Delete Azure WAF Lab Resources"

---

### Cleanup Troubleshooting

**Problem:** "Cannot delete resource group - resources still exist"

**Solution:**
- Delete resources individually first
- Then delete empty resource group
- Some resources have protection locks

**Problem:** "Front Door deletion failing"

**Solution:**
- First remove WAF policy association
- Then delete Front Door
- Then delete WAF policy

**Problem:** "Sentinel workspace won't delete"

**Solution:**
- First disable Sentinel (remove from workspace)
- Wait 5 minutes
- Then delete Log Analytics workspace

**Problem:** "Resources show as deleted but still incurring charges"

**Solution:**
- Some resources have soft-delete (recoverable for 14 days)
- Check "Show deleted resources" in portal
- Permanently delete if needed
- Allow 24 hours for billing to reflect deletion

---

## 📊 Skills Demonstrated

**By completing this lab, you've demonstrated:**

### **Cloud Security Engineering**
✅ Deployed Azure Front Door with WAF  
✅ Configured OWASP-based web application protection  
✅ Implemented defense-in-depth architecture  
✅ Secured public-facing applications  

### **SIEM Operations**
✅ Configured Log Analytics workspace  
✅ Enabled diagnostic logging  
✅ Deployed Microsoft Sentinel  
✅ Created custom analytics rules  
✅ Investigated security incidents  

### **Threat Detection**
✅ Wrote KQL queries for threat hunting  
✅ Analyzed attack patterns  
✅ Identified malicious activity  
✅ Classified security incidents  

### **Incident Response**
✅ Followed investigation methodology  
✅ Documented security findings  
✅ Made containment decisions  
✅ Recommended remediation actions  

### **Attack Simulation**
✅ Safely simulated web application attacks  
✅ Tested SQL injection, XSS, path traversal  
✅ Validated security controls  
✅ Generated realistic security events  

---

## 🎯 Interview Talking Points

**When discussing this project in interviews:**

### **Project Summary (30 seconds)**

*"I built a complete web application security stack in Azure that demonstrates defense-in-depth. I deployed a web app protected by Azure Front Door WAF with OWASP rules, configured Microsoft Sentinel for SIEM operations, and created custom detection rules using KQL. I then simulated real-world attacks like SQL injection and XSS to validate that the WAF blocked them, generated security incidents in Sentinel, and performed full incident investigations. This project showcases my ability to design security architectures, write detection logic, and respond to threats."*

### **Technical Depth Questions**

**Q: "How does Azure Front Door WAF differ from a traditional firewall?"**

*"A traditional network firewall works at layers 3-4 of the OSI model, filtering based on IP addresses and ports. Azure Front Door WAF operates at layer 7 (application layer), inspecting HTTP/HTTPS traffic for web-specific attacks like SQL injection and XSS. It understands the application protocol and can detect attacks hidden in URL parameters, headers, or POST data that a network firewall would miss. WAF is specifically designed for web application protection with rule sets like OWASP Top 10, while network firewalls focus on network segmentation and port-based access control."*

**Q: "Walk me through your incident investigation process."**

*"I follow a structured approach: First, I identify WHO - checking the source IP, geo-location, and reputation. Then WHAT - analyzing attack types, payloads, and which OWASP rules triggered. Then WHEN - building a timeline to understand attack duration and patterns. WHERE - checking if the IP is known malicious using threat intelligence. WHY - determining if it's opportunistic scanning vs. targeted. And HOW - assessing impact by verifying all attacks were blocked. I document everything in Sentinel incident comments and classify based on whether it's a true positive, false positive, or benign activity before closing with appropriate recommendations."*

**Q: "How would you tune WAF rules to reduce false positives?"**

*"I'd start by analyzing blocked requests to identify patterns. If legitimate traffic is being blocked, I'd check which specific OWASP rule is triggering. Options include: creating exclusion rules for specific URL paths or parameters, adjusting the anomaly scoring threshold, switching problematic rules to detection mode instead of prevention, or creating custom allow rules for known good patterns. I'd test changes in a staging environment first and monitor for a week before applying to production. The key is balancing security with usability - you don't want to block attacks, but you also can't break the application for legitimate users."*

---

## 🔗 Additional Resources

### **Microsoft Documentation**

- [Azure Front Door Documentation](https://docs.microsoft.com/azure/frontdoor/)
- [Azure WAF Documentation](https://docs.microsoft.com/azure/web-application-firewall/)
- [Microsoft Sentinel Documentation](https://docs.microsoft.com/azure/sentinel/)
- [KQL Reference](https://docs.microsoft.com/azure/data-explorer/kusto/query/)

### **OWASP Resources**

- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [OWASP ModSecurity Core Rule Set](https://owasp.org/www-project-modsecurity-core-rule-set/)

### **Security Frameworks**

- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework)
- [CIS Controls](https://www.cisecurity.org/controls/)

### **Practice & Learning**

- [Microsoft Learn - Sentinel Learning Path](https://docs.microsoft.com/learn/paths/security-ops-sentinel/)
- [KQL Training](https://docs.microsoft.com/azure/data-explorer/kusto/query/tutorial)
- [OWASP WebGoat](https://owasp.org/www-project-webgoat/) - Practice identifying vulnerabilities
- [TryHackMe - Web Security](https://tryhackme.com/paths) - Hands-on labs

---

## 📝 License & Disclaimer

**Educational Purpose:**
This lab is designed for learning and demonstration purposes in controlled Azure environments you own.

**Disclaimer:**
- Only test against your own resources
- Never attack systems you don't own
- Follow all applicable laws and regulations
- Cloud costs are your responsibility
- No warranty or guarantee provided

**Legal:**
This project and its documentation are provided "as-is" for educational purposes.

---

## 📧 Questions or Improvements?

**Found an issue or have suggestions?**
- Open an issue on GitHub
- Submit a pull request with improvements
- Share your experience and learnings

---

## 🎓 Certificate of Completion

**You've successfully completed the Azure WAF + Sentinel Lab!**

**Skills Acquired:**
✅ Cloud Security Architecture  
✅ Web Application Firewall Configuration  
✅ SIEM Deployment & Management  
✅ KQL Query Language  
✅ Threat Detection & Hunting  
✅ Security Incident Investigation  
✅ Attack Simulation & Validation  

**Next Steps:**
1. Add this project to your portfolio/resume
2. Share on LinkedIn with screenshots
3. Build on this foundation with advanced scenarios
4. Consider pursuing Microsoft Security certifications (SC-200, SC-300)

**Recommended certifications:**
- Microsoft Certified: Security Operations Analyst Associate (SC-200)
- Microsoft Certified: Azure Security Engineer Associate (AZ-500)
- Certified Cloud Security Professional (CCSP)

---

**Project completed!** 🎉

**Date:** [Your completion date]  
**Author:** [Your name]  
**GitHub:** [Your GitHub profile]

---

## Appendix: Quick Reference

### **Common KQL Queries**

```kql
// All WAF blocks in last hour
AzureDiagnostics
| where TimeGenerated > ago(1h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"

// Top attacking IPs
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize Count=count() by clientIP_s
| sort by Count desc
| take 10

// Attack types distribution
AzureDiagnostics
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize Count=count() by ruleName_s
| sort by Count desc

// Hourly attack trend
AzureDiagnostics
| where TimeGenerated > ago(24h)
| where Category == "FrontdoorWebApplicationFirewallLog"
| where action_s == "Block"
| summarize Count=count() by bin(TimeGenerated, 1h)
| render timechart
```

### **OWASP Rule ID Reference**

| Rule ID Range | Attack Type |
|---------------|-------------|
| 920xxx | Protocol Enforcement |
| 921xxx | Protocol Attack |
| 930xxx | Local File Inclusion (LFI) |
| 931xxx | Remote File Inclusion (RFI) |
| 932xxx | Remote Code Execution (RCE) |
| 933xxx | PHP Injection |
| 941xxx | Cross-Site Scripting (XSS) |
| 942xxx | SQL Injection |
| 943xxx | Session Fixation |

### **Incident Severity Guidelines**

| Severity | Criteria | Example |
|----------|----------|---------|
| **Critical** | Active compromise, data exfiltration | Successful ransomware, database breach |
| **High** | High likelihood of compromise | Successful exploitation attempt |
| **Medium** | Blocked attacks, potential threats | Multiple WAF blocks, scanning activity |
| **Low** | Informational, low risk | Single failed login, low-volume scanning |

---

**End of Lab Guide**
