# SOC Threat Hunting with Microsoft Sentinel

## Objective:
The SOC Threat Hunting with Microsoft Sentinel project documents my participation in the Day 9 Mini Project of the 30-Day MyDFIR Microsoft Challenge. The objective was to build and configure a Microsoft Sentinel environment, perform threat hunting activities, develop KQL queries, create security dashboards, and investigate a phishing incident. Through this project, I aimed to strengthen my SOC analyst skills by working with Microsoft security technologies and simulating real-world security monitoring and incident response activities.

## Project Overview:
This project focuses on deploying and configuring a Microsoft Sentinel environment within Microsoft Azure and using it to investigate security events and phishing-related activity.

## Lab Setup:
<img width="400" height="600" alt="image" src="https://github.com/user-attachments/assets/064808a1-ffdb-4cc8-b125-cb78a1eedc51" />

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
<img width="807" height="455" alt="image" src="https://github.com/user-attachments/assets/b8613e4d-36e6-41a9-baa9-aff1d880788a" />
<img width="800" height="437" alt="image" src="https://github.com/user-attachments/assets/41c15be4-35b3-4c92-9f79-c7c6d9417fb7" />
<img width="804" height="439" alt="image" src="https://github.com/user-attachments/assets/0dad3ae2-c41b-46ce-b9ed-2565f6cf9f0b" />

- Deployed the Microsoft Sentinel Training Lab solution.
<img width="707" height="386" alt="image" src="https://github.com/user-attachments/assets/7a0cae76-8002-49e1-8dd1-79677f96b14d" />

- Connected Microsoft Defender XDR to Microsoft Sentinel.


- Developed KQL queries to: 
   - Identify malicious sender domains
   - Analyze Azure activity logs
   - Investigate error events
<img width="683" height="411" alt="image" src="https://github.com/user-attachments/assets/6b1b4b28-866e-49b2-a63d-1495c551a891" />
<img width="691" height="264" alt="image" src="https://github.com/user-attachments/assets/96517f56-2665-4ca4-8607-930a36b54324" />
<img width="691" height="240" alt="image" src="https://github.com/user-attachments/assets/9e4bc1bc-a288-48d8-a219-7b11f668fe3d" />


- Created a custom Sentinel Workbook containing: 
   - Activity Volume
   - Top 5 Failed User Logins 
   - Malicious Sender Domains
   - Successful Activities
<img width="752" height="335" alt="image" src="https://github.com/user-attachments/assets/c27e33db-3878-4e83-8ef5-f4dbf229e7cf" />
<img width="752" height="215" alt="image" src="https://github.com/user-attachments/assets/8be7fdfa-674a-49cf-9243-3e4438782f4b" />
<img width="750" height="259" alt="image" src="https://github.com/user-attachments/assets/2aa9a5e9-e694-4a6f-99c0-8737cf53db0e" />
<img width="749" height="270" alt="image" src="https://github.com/user-attachments/assets/6e9be7fc-8329-45be-a95d-13b9dafd8166" />

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
- IP: 185.220.101.55
- Domain: sh4repoint-pkwork.xyz
- Recipient: ceo@pkwork.onmicrosoft.com
- Subject: SharePoint: Board meeting documents shared with you
- Attachment: Q4-Board-Meeting-Agenda.docx
- Verdict: Phishing (Confidence: 93)
- Email Authentication: SPF Fail | DKIM None | DMARC Fail
- Delivery Status: Allowed

#### Investigation:
A phishing email was received on 2026‑06‑11 07:17:42 UTC from sharepoint-notify@sh4repoint-pkwork.xyz, originating from the malicious IP 185.220.101.55 (100% abuse on AbuseIPDB). The message included a typosquatted URL (sh4repoint-pkwork.xyz/download/board-agenda) and a Word attachment (Q4-Board-Meeting-Agenda.docx). Email authentication checks failed (SPF/DMARC failed, DKIM none), yet the email was still delivered despite a phishing threat score of 93. An investigation is ongoing to determine whether the link or attachment was accessed and to assess any potential data exposure.

#### Incident Analysis:
- WHO: Malicious sender IP 185.220.101.55 (100% abuse on AbuseIPDB).
- WHAT: Phishing email from sharepoint-notify@sh4repoint-pkwork.xyz containing a typosquatted URL and the attachment Q4‑Board‑Meeting‑Agenda.docx.
- WHEN: Sent on 2026‑06‑11 07:17:42 UTC and delivered despite failing email security checks.
- WHERE: Targeted ceo@pkwork.onmicrosoft.com with subject “SharePoint: Board meeting documents shared with you.”  
- WHY: Likely aimed at credential theft or delivering malware to compromise the CEO’s system.
- HOW: Email passed through because the security gateway was likely not configured to block or quarantine such threats.

#### Recommendations:
- Use Threat Intelligence to verify sender IPs/domains; in this case, 185.220.101.55 was confirmed malicious.
- Delete the identified phishing email and configure email security controls to automatically delete or quarantine similar phishing emails in the future.
- Enable domain similarity protection to detect spoofed or look‑alike domains.
- Search the organization for any other emails from 185.220.101.55 or sh4repoint-pkwork.xyz.
- Conduct targeted phishing awareness training focused on identifying Microsoft 365 and SharePoint impersonation emails, recognizing typosquatted domains, verifying unexpected file-sharing requests, avoiding suspicious links and attachments, promptly reporting phishing attempts, and understanding the risks of credential theft, malware, and business email compromise (BEC).

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
