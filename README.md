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

#### Key Findings:
- A phishing email successfully bypassed email controls despite:
   - SPF failure 
   - DMARC failure 
   - Missing DKIM validation 
   - High threat confidence score 
- The sender domain used typosquatting techniques to impersonate Microsoft SharePoint. 
- The sender IP had a malicious reputation according to threat intelligence sources.

#### What I Learned:
- How Microsoft Sentinel integrates with Microsoft Defender XDR.
- How to build effective KQL queries for threat hunting.
- How workbooks improve visibility into security data.
- How detection rules automate security monitoring.
- How phishing campaigns can bypass security controls when policies are not properly configured.
- How bookmarks and incidents support SOC investigations.

#### What I Would Do Differently in a Production SOC:
- Enable stricter email filtering and anti-spoofing policies.
- Configure automatic quarantine actions for high-confidence phishing emails.
- Implement domain similarity protection.
- Automate threat intelligence enrichment using Sentinel playbooks.
- Create automated incident response workflows for phishing detections.

## Investigation Report:

#### Phishing Email Investigation:
A phishing email impersonating SharePoint was delivered to an executive mailbox despite multiple authentication failures and a phishing verdict.

#### Query Used:
- MailGuard365_Threats_CL
  | where ThreatVerdict contains "phish" and Direction == "Inbound"
  | where Action contains "Allow" and SPFResult == "Fail" and DKIMResult == "None" and DMARCResult == "Fail"
  | sort by ThreatConfidence

#### Indicators Identified:
- Time: 2026-06-11T07:17:42 UTC
- Sender IP: 185.220.101.55
- Send Address: sharepoint-notify@sh4repoint-pkwork.xyz
- Sender Domain: sh4repoint-pkwork.xyz
- Subject: SharePoint: Board meeting documents shared with you
- URL: https://sh4repoint-pkwork.xyz/download/board-agenda
- Threat Confidence: 93
- Recipient Address: ceo@pkwork.onmicrosoft.com
- Recipient Domain: pkwork.onmicrosoft.com
- Attachment: Q4-Board-Meeting-Agenda.docx
- Threat Verdict: Phishing
- Action: Allow
- SPF: Fail
- DKIM: None
- DMARC: Fail

#### Investigation:
- On 2026-06-11 07:17:42 UTC, an email was received from the email ID sharepoint-notify@sh4repoint-pkwork.xyz with the sender IP being 185.220.101.55.
- This email contained a URL https://sh4repoint-pkwork.xyz/download/board-agenda along with a Word document as an attachment Q4-Board-Meeting-Agenda.docx.
- Based on our investigation, the Sender IP 185.220.101.55 was flagged as malicious (100% abuse) on AbuseIPDB and the email was allowed to pass through even though it was flagged as phishing with a threat score of 93.
- On further investigating, we noticed that both SPF and DMARC failed, while DKIM returned a value of none.
- Moreover, the URL is a classic example of ‘typosquatting’ where the word sharepoint is purposely misspelled as sh4repoint.
- A deeper analysis is underway to identify if the link and the attachment were accessed and if so, what extent of data has been compromised.

#### Incident Analysis:
- WHO – Sender Address: 185.220.101.55 – flagged as malicious on AbuseIPDB.
- WHAT – Phishing email sent by sharepoint-notify@sh4repoint-pkwork.xyz with URL https://sh4repoint-pkwork.xyz/download/board-agenda and attachment Q4-Board-Meeting-Agenda.docx
- WHEN: Based on our findings, the email was sent on 2026-06-11 07:17:42 UTC & was allowed to pass through although it failed to meet the email security requirements.
- WHERE: The email was sent to ceo@pkwork.onmicrosoft.com with subject SharePoint: Board meeting documents shared with you.
- WHY: Though the exact intent of this phishing email was not specified, we can quite certainly say that the email was intended to either capture the CEO’s credentials or to download a virus that would compromise the CEO’s system.
- HOW: The email was allowed to pass through since the probably because the email security gateway was not configured to quarantine or block.

#### Recommendations:
- With reliance on Threat Intelligence Platforms, search for the sender IP and domain to see if it was previously flagged as malicious – on this case the sender IP is 185.220.101.55 which was marked by AbuseIPDB as malicious.
- Configure the email security gateway to quarantine or block any incoming emails originating from domains which use the ‘typosquatting’ technique – in this case ‘sh4repoint-pkwork.xyz’.
- Configure anti-spoofing policies to detect domains with the similar names (detect typosquatting) thereby promoting Domain Similarity Protection.
- Search across the organization to see if emails were received by other recipients from either the sender IP 185.220.101.55 or domain ‘sh4repoint-pkwork.xyz’.
- Conduct Phishing campaigns across the organization making employees aware of such instances and how they can avoid falling prey to such social engineering attacks.

## Project Summary:
By completing this project, I can now:
- Deploy and configure Microsoft Sentinel.
- Connect Microsoft Defender XDR data sources.
- Create custom KQL queries for threat hunting.
- Build Sentinel workbooks and dashboards.
- Create detection rules for security monitoring.
- Investigate phishing incidents using log data and threat intelligence.
- Use bookmarks and incidents to support SOC workflows.

## Most Impactful Learning Experience:
The phishing investigation provided the most realistic SOC experience by combining threat hunting, log analysis, threat intelligence validation, incident creation, and remediation recommendations into a single workflow.
