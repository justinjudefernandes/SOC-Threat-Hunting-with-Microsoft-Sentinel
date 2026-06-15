# SOC Threat Hunting with Microsoft Sentinel

## Objective

The SOC Threat Hunting with Microsoft Sentinel project documents my participation in the Day 9 Mini Project of the 30-Day MyDFIR Microsoft Challenge. The objective was to build and configure a Microsoft Sentinel environment, perform threat hunting activities, develop KQL queries, create security dashboards, and investigate a phishing incident. Through this project, I aimed to strengthen my SOC analyst skills by working with Microsoft security technologies and simulating real-world security monitoring and incident response activities.

## Project Overview
This project focuses on deploying and configuring a Microsoft Sentinel environment within Microsoft Azure and using it to investigate security events and phishing-related activity.

### Tools Used:
- Microsoft Azure 
- Microsoft Sentinel 
- Log Analytics Workspace 
- Microsoft Defender XDR 
- Kusto Query Language (KQL) 
- Microsoft Sentinel Workbooks 
- AbuseIPDB

### Skill Developed:
- SIEM deployment and configuration
- Threat hunting
- Log analysis
- KQL query development
- Security monitoring
- Dashboard creation
- Incident investigation
- Phishing analysis

### Key Deliverables:
- Microsoft Sentinel deployment
- Custom workbook/dashboard creation
- Detection rule development
- Threat hunting queries
- Phishing investigation report
- Incident creation and management

## Steps Performed:
- Created Azure resources including: 
   - Resource Group 
   - Log Analytics Workspace 
   - Microsoft Sentinel 
- Deployed the Microsoft Sentinel Training Lab solution.
- Connected Microsoft Defender XDR to Microsoft Sentinel.
- Developed KQL queries to: 
   - Identify malicious sender domains
   - Analyze Azure activity logs
   - Investigate error events
- Created a custom Sentinel Workbook containing: 
   - Activity Volume 
   - Top 5 Failed User Logins 
   - Malicious Sender Domains
   - Successful Activities
- Created a detection rule to identify accounts with excessive failed logons using Event ID 4625.
- Investigated a phishing email that was marked as phishing but still allowed into the environment. 
- Created bookmarks and converted findings into a security incident for further analysis.

### Key Findings:
- A phishing email successfully bypassed email controls despite:
   - SPF failure 
   - DMARC failure 
   - Missing DKIM validation 
   - High threat confidence score 
- The sender domain used typosquatting techniques to impersonate Microsoft SharePoint. 
- The sender IP had a malicious reputation according to threat intelligence sources.

### What I Learned:
- How Microsoft Sentinel integrates with Microsoft Defender XDR.
- How to build effective KQL queries for threat hunting.
- How workbooks improve visibility into security data.
- How detection rules automate security monitoring.
- How phishing campaigns can bypass security controls when policies are not properly configured.
- How bookmarks and incidents support SOC investigations.

### What I Would Do Differently in a Production SOC:
- Enable stricter email filtering and anti-spoofing policies.
- Configure automatic quarantine actions for high-confidence phishing emails.
- Implement domain similarity protection.
- Automate threat intelligence enrichment using Sentinel playbooks.
- Create automated incident response workflows for phishing detections.
