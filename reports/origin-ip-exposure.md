VULNERABILITY REPORT

Title:
Non-Cloudflare IPs Allowed to Access Origin Server

Severity:
High

Affected Asset:
Origin IP: https://13.229.62.191/
Frontend Domain: https://aixblock.io/

Description:
During the initial reconnaissance and asset assessment, it was discovered that the origin server of aixblock.io is directly accessible via the IP address 13.229.62.191, bypassing Cloudflare's protections.
While the application is fronted by Cloudflare for enhanced security (reverse proxy, DDoS protection, WAF, etc.), the origin server does not enforce IP whitelisting and thus accepts direct traffic from non-Cloudflare IPs.
This direct exposure renders Cloudflare's protection ineffective, leaving the origin server vulnerable to:
• Unfiltered and direct malicious traffic
• WAF bypass attacks
• Potential DDoS attacks directly targeting the origin
• Exploitation of server-level misconfigurations or vulnerabilities

Steps to Reproduce:

Visit the direct IP address: https://13.229.62.191/
 
Observe that the application is accessible directly without Cloudflare headers or protections.
Image

Compare with the domain https://aixblock.io which routes through Cloudflare and enforces WAF/security headers.
Image

Optionally inspect headers using tools like curl or a proxy (Burp Suite) to confirm the absence of Cloudflare routing in the IP version.
 
Impact:
• Cloudflare's WAF, DDoS mitigation, and access control mechanisms are completely bypassed.
• The origin server is exposed to:
o Application-layer attacks (e.g., XSS, SQLi) without being filtered.
o Denial of Service (DoS) attacks targeting the server directly.
o Automated attack tools that can evade Cloudflare detection.
• Reduces the effectiveness of CDN-based security and caching.
• Puts sensitive infrastructure at risk of exploitation and service disruption.

Suggested Remediation:
To secure the origin infrastructure and enforce Cloudflare protections:

Restrict direct access to the origin IP (13.229.62.191) by:
o Setting up firewall rules to only allow inbound traffic from Cloudflare’s IP ranges.
o Blocking all other incoming traffic at the network level (e.g., GCP firewall rules or host-based firewalls).
Implement Authenticated Origin Pulls:
o Enable mutual TLS between Cloudflare and your origin to ensure only Cloudflare can communicate with it.
Avoid exposing public IPs of your origin servers where possible, or implement security controls to handle them properly.
Regularly audit DNS and IP exposures to ensure all assets route only through expected security layers.

References:
• Cloudflare: Restricting Access to Your Origin
• Cloudflare Blog: Bypassing WAFs via Origin IP
