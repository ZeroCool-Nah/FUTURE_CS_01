Phishing Attack Analysis: Email Authentication Headers
This documentation provides a technical breakdown of email security protocols (SPF, DKIM, and DMARC) used to identify and mitigate phishing attempts, based on the comparative analysis of legitimate and malicious email headers.
1. Overview of the Analysis
The analysis utilizes the "Show Original" feature of a webmail client to inspect the metadata of incoming messages. By comparing a legitimate service email with a spoofed "Security Alert," we can identify the technical failures that trigger phishing warnings.
   * Legitimate Case: The email from studocu.com aligns with all security protocols, resulting in a "PASS" status.
# [Legitimate Email Sample](./IMAGE3.jpg)
 * Phishing Case: The email from googl-service-com.com fails all primary authentication checks, leading to a high-risk warning banner.
 # [Phishing Email Sample](./IMAGE4.png)
2. Technical Explanation of Authentication Protocols
SPF (Sender Policy Framework)
SPF is a DNS-based mechanism that allows a domain owner to publish a list of authorized IP addresses allowed to send email on their behalf. When an email is received, the mail server checks if the sender's IP matches the list in the DNS record. In the phishing example, SPF failed because the IP 47.189.215.11 was not authorized to send mail for the domain it claimed to represent.
DKIM (DomainKeys Identified Mail)
DKIM adds a cryptographic signature to the email header. The receiving server uses a public key found in the sender's DNS records to verify the signature. This ensures the email's "From" address is authentic and the message content hasn't been altered. The phishing attempt failed DKIM because the signature was either missing or did not cryptographically match the spoofed domain googl-service-com.com.
DMARC (Domain-based Message Authentication, Reporting, and Conformance)
DMARC is a policy layer that sits on top of SPF and DKIM. It tells the receiving server how to handle messages that fail authentication (e.g., "None," "Quarantine," or "Reject"). Because both SPF and DKIM failed in the malicious example, DMARC also failed, which triggered the automated "likely phishing" warning for the user.
3. Indicators of a Phishing Attack
 * Look-alike Domain Spoofing: The attacker used googl-service-com.com. While it contains the word "google," the actual top-level domain and structure are not owned by Google.
 * Urgency and Social Engineering: The subject line "A critical security alert for your account" is designed to create a sense of panic, pushing the user to click a link without verifying the sender.
 * Authentication Metadata: The most definitive proof is the "FAIL" status for SPF, DKIM, and DMARC found within the message's technical details.
4. Ways of Prevention
Technical Mitigation
 * Check Headers: Always use the "Show Original" or "View Headers" tool to verify that the "PASS" status is present for all three protocols before trusting a security alert.
 * Implement Strict DMARC Policies: Organizations should set their DMARC policy to p=reject to prevent unauthorized senders from reaching any user inboxes.
 * Email Security Gateways: Use advanced filtering that automatically flags look-alike domains and suspicious IP ranges.
User Best Practices
 * Manual URL Verification: Hover over links to see the destination URL. If the domain looks suspicious or doesn't match the official service website, do not click.
 * Enable Multi-Factor Authentication (MFA): MFA provides a critical safety net. Even if a user falls for a phishing attack and provides their password, the attacker still cannot access the account without the second factor.
 * Official Channels: If you receive a "Critical Alert," navigate directly to the official website (e.g., https://www.google.com/search?q=google.com) through your browser instead of clicking the link provided in the email.
Would you like me to help you write a script to automate the extraction of these headers for future analysis?
