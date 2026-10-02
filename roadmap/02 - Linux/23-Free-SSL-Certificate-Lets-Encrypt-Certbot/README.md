# Topic 23 — Free SSL Certificate Installation (Let's Encrypt & Certbot)

## Objective
Master HTTPS, SSL/TLS encryption, Public Key Infrastructure (PKI), Let's Encrypt certificate authority, ACME protocol mechanics, Certbot automation, Nginx and Apache integration, HTTP-01 and DNS-01 validation challenges, wildcard certificates, automated renewals, Cloudflare CDN integration, TLS hardening, troubleshooting, and production deployment checklists.

## Complete Module Structure (27 Markdown Files)

1. **`01-SSL-TLS-and-HTTPS-Fundamentals.md`**: Symmetric vs Asymmetric encryption, SSL/TLS handshake lifecycle, PKI, Certificate Authorities (CAs), X.509 structure, HTTPS port 443.
2. **`02-Lets-Encrypt-and-ACME-Protocol-Architecture.md`**: How Let's Encrypt works, Automated Certificate Management Environment (ACME v2), domain validation, rate limits.
3. **`03-Prerequisites-DNS-Records-and-Network-Ports.md`**: Public IP assignment, A/AAAA DNS record configuration, domain propagation testing, port 80/443 reachability.
4. **`04-Firewall-Configuration-UFW-and-Cloud-Security-Groups.md`**: UFW rules (`ufw allow 'Nginx Full'`), iptables, AWS Security Group / GCP firewall ingress configurations.
5. **`05-Certbot-Installation-Methods-Snap-vs-Apt.md`**: EFF recommended Snapd installation, apt package alternatives, certbot python plugins, environment preparation.
6. **`06-Step-by-Step-Nginx-SSL-Setup-with-Certbot.md`**: `certbot --nginx`, server block configuration, automatic HTTP to HTTPS 301 redirection setup.
7. **`07-Step-by-Step-Apache-SSL-Setup-with-Certbot.md`**: `certbot --apache`, virtual host `<VirtualHost *:443>` configuration, `mod_ssl`, `mod_rewrite` setup.
8. **`08-SSL-Certificate-Files-Structure-and-Permissions.md`**: `/etc/letsencrypt/live/`, `fullchain.pem`, `privkey.pem`, `cert.pem`, `chain.pem`, symlink structure, security permissions (`0600`).
9. **`09-Verifying-HTTPS-and-TLS-Handshakes.md`**: Verifying HTTPS via `curl -Iv`, inspecting raw certificates using `openssl s_client -connect`, SSL Labs SSLLabs testing.
10. **`10-Automated-Renewal-Certbot-Timer-and-Cron.md`**: 90-day lifecycle, `certbot.timer`, systemd timer execution, crontab backup, dry run testing (`certbot renew --dry-run`), post-renewal hooks.
11. **`11-Validation-Challenges-HTTP-01-vs-DNS-01.md`**: HTTP-01 challenge (`/.well-known/acme-challenge/`), DNS-01 challenge (TXT records `_acme-challenge.domain.com`), standalone mode.
12. **`12-Wildcard-and-Multi-Domain-SAN-Certificates.md`**: Wildcard certificates (`*.example.com`), Subject Alternative Name (SAN), DNS API plugins (Cloudflare, Route53, DigitalOcean).
13. **`13-Cloudflare-Proxy-Integration-and-SSL-Modes.md`**: Cloudflare CDN proxying (Orange Cloud), SSL modes (Off, Flexible, Full, Full Strict), origin certificates, resolving redirect loops.
14. **`14-SSL-TLS-Troubleshooting-Guide.md`**: Common failure modes (expired certs, `SSL_ERROR_RX_RECORD_TOO_LONG`, mixed content, ACME challenge 404, rate limiting).
15. **`15-Security-Hardening-HSTS-TLS13-and-Ciphers.md`**: TLS 1.2/1.3 protocol restrictions, Diffie-Hellman parameters, HTTP Strict Transport Security (HSTS), OCSP Stapling, Mozilla SSL Configuration Generator.
16. **`16-Hands-On-Lab-01-Nginx-Certbot-HTTP01.md`**: Step-by-step hands-on lab: Installing Let's Encrypt SSL on Nginx web server using HTTP-01 challenge.
17. **`17-Hands-On-Lab-02-Apache-Certbot-Setup.md`**: Step-by-step hands-on lab: Installing Let's Encrypt SSL on Apache HTTP server.
18. **`18-Hands-On-Lab-03-Wildcard-DNS01-Challenge.md`**: Step-by-step hands-on lab: Obtaining Wildcard certificate via Cloudflare DNS API plugin.
19. **`19-Hands-On-Labs-04-to-10-Renewal-Hardening-Cloudflare.md`**: Step-by-step hands-on labs 4-10: Automated renewal hooks, HSTS setup, OCSP stapling, Cloudflare Full Strict mode setup.
20. **`20-Scenario-Based-Troubleshooting-Drills.md`**: 7 production outage scenario simulations with root cause analysis and resolution steps.
21. **`21-DevOps-SSL-Interview-Questions-and-Answers.md`**: Senior DevOps / Systems Engineer interview questions and technical answers on SSL/TLS and ACME.
22. **`22-MCQs-and-Diagnostic-Quizzes.md`**: Multiple-choice diagnostic questions with answer keys and detailed explanations.
23. **`23-Quick-Revision-Notes.md`**: Essential rules, cheat notes, file paths, and key parameters.
24. **`24-Complete-Certbot-Command-Cheat-Sheet.md`**: Complete command syntax lookup table for Certbot CLI options and flags.
25. **`25-Production-SSL-Deployment-Checklist.md`**: 25-point production checklist for SSL/TLS deployment readiness.
26. **`README.md`**: Module overview and navigation index.
27. **`SOURCE.md`**: Preservation of original documentation and reference material.
