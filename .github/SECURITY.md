# Security Policy

## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| main    | :white_check_mark: |

## Reporting a Vulnerability

If you discover a security vulnerability in this project, please report it responsibly.

**Do NOT open a public GitHub issue for security vulnerabilities.**

Instead, please email: **security@smartmur.com**

Include the following in your report:

- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)

## Response Timeline

- **Acknowledgement**: Within 48 hours of receiving the report
- **Initial assessment**: Within 5 business days
- **Fix or mitigation**: Depending on severity, typically within 30 days

## Scope

This policy applies to all code in this repository, including:

- Docker Compose configurations
- Shell scripts (Docker Swarm setup, automation)
- GitHub Actions workflows
- Any credentials or secrets handling

## Best Practices for Contributors

- Never commit secrets, tokens, or passwords — use `.env.example` templates
- Pin GitHub Actions to full SHA hashes, not mutable tags
- Use `StrictHostKeyChecking accept-new` instead of disabling host key checking
- Avoid `chmod 666` on sensitive sockets — use proper group membership
- Do not flush iptables rules — use targeted allow rules instead
