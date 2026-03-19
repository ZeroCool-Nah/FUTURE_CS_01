Email Phishing Analysis: SPF, DKIM, and DMARC
This project demonstrates how to identify phishing attacks by analyzing Google Mail's "Original Message" headers. By comparing a legitimate email with a malicious one, we can see how authentication protocols act as the first line of defense.
1. Technical Explanation of Authentication Headers
The three core protocols used to verify an email's sender are:
SPF (Sender Policy Framework)
Purpose: A DNS record that lists the specific IP addresses authorized to send emails on behalf of a domain.
Analysis: In the phishing example, the SPF failed because the IP 47.189.215.11 was not listed in the authorized DNS records for the domain it was trying to spoof.
DKIM (DomainKeys Identified Mail)
Purpose: Adds a cryptographic signature to the email header. The receiving server uses a public key (found in the sender's DNS) to verify that the email content wasn't tampered with during transit.
Analysis: The phishing email failed DKIM because the signature was either missing or did not match the domain googl-service-com.com.
DMARC (Domain-based Message Authentication, Reporting, and Conformance)
Purpose: A policy that tells the receiving server what to do if SPF or DKIM fails (e.g., "do nothing," "quarantine/spam," or "reject").
Analysis: Since both SPF and DKIM failed, the DMARC check failed, triggering the "likely phishing" warning.
2. Comparative Analysis
|------------------------------------------------------------------------|
| Feature | Legitimate Email (Example 1) | Phishing Email (Example 2)    |
|------------------------------------------------------------------------|
| Domain Name | studocu.com (Authentic) |  googl-service-com.com (Look-alike) |
| SPF Status  |      PASS               |         FAIL                        |
| DKIM Status |     PASS                |         FAIL                        |
| DMARC Status |    PASS                |         FAIL                        |
| Visual Indicators | Standard headers  |    High-visibility warning banner.  |
|-----------------------------------------------------------------------------|
3. How to Identify a Phishing Attack
Check the "From" Address: Look for subtle misspellings. For example, googl-service-com.com is a classic "look-alike" domain designed to mimic google.com.
Inspect Authentication Headers: In Gmail, click the three dots (More) next to the reply button and select "Show original." Check if SPF, DKIM, and DMARC show "PASS."
Analyze the Tone: Phishing often uses "Critical Security Alerts" or "Urgent Action Required" to create panic.
4. Ways of Prevention
For Individuals:
Enable MFA (Multi-Factor Authentication): Even if a phisher steals your password, they cannot access your account without the second factor.
Hover Before Clicking: Hover your mouse over any link to see the actual destination URL in the bottom corner of your browser.
Use Built-in Tools: Pay attention to browser and email client warnings (like the red banner in the second image).
For Organizations/Developers:
Strict DMARC Policy: Set your DMARC record to p=reject to ensure that unauthorized emails are blocked before they reach the inbox.
Email Security Gateways: Implement tools that automatically scan attachments and links for malicious behavior.
Security Awareness Training: Conduct simulated phishing tests to educate users on spotting sophisticated attacks.
5. [Phishing Email Sample](./IMAGE4.png)
6. [Legitimate Email Sample](./IMAGE3.jpg)
