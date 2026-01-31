# Clawdbot Security & Sandboxing Video Series

This folder contains detailed video scripts for a security-focused series on safely running Clawdbot.

## 🛡️ Series Overview

This series covers the risks of running an AI assistant with system access and provides a comprehensive guide to running it safely, from least secure to most secure configurations.

## 📹 Video Parts

| Part | File | Duration | Description |
|------|------|----------|-------------|
| 1 | [01-the-risks.md](01-the-risks.md) | 8-10 min | Understanding the dangers of running AI with shell access |
| 2 | [02-global-install.md](02-global-install.md) | 10 min | Risks of global npm install and basic mitigations |
| 3 | [03-sandboxing-basics.md](03-sandboxing-basics.md) | 12 min | Docker sandboxing fundamentals |
| 4 | [04-security-layers.md](04-security-layers.md) | 12 min | DM pairing, allowlists, tool policies, exec approvals |
| 5 | [05-hardened-setup.md](05-hardened-setup.md) | 10 min | Fully hardened, risk-free configuration |

**Total Estimated Duration:** ~50-55 minutes

## 🎯 Target Audience

- Users who want to understand security implications before installing
- Existing users looking to harden their setup
- Privacy-conscious users who want maximum isolation
- Developers deploying Clawdbot in shared environments

## 📋 Risk Levels Covered

| Level | Configuration | Risk | Use Case |
|-------|--------------|------|----------|
| 🔴 Critical | Open DMs + Full exec + No sandbox | Extreme | Never recommended |
| 🟠 High | Global install + Pairing only | High | Personal trusted use only |
| 🟡 Moderate | Sandboxed non-main + Allowlists | Medium | Family/shared devices |
| 🟢 Low | Full Docker + Strict policies | Low | Public/untrusted channels |
| ✅ Minimal | Headless container + Read-only | Minimal | Maximum security |

## 🔗 Related Documentation

- [Security Guide](../gateway/security.md)
- [Sandboxing](../gateway/sandboxing.md)
- [Exec Approvals](../tools/exec-approvals.md)
- [Elevated Mode](../tools/elevated.md)
