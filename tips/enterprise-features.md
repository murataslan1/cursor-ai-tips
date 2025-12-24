# 🏢 Enterprise Features Guide

> **December 2025**: Cursor achieves enterprise maturity with Service Accounts, SOC 2 certification, and advanced security controls.

## Overview

Cursor's enterprise capabilities have evolved from "developer tool" to "software factory platform." Key additions:

| Feature | Impact |
|:--------|:-------|
| **Service Accounts** | Enables headless automation |
| **SOC 2 Certification** | Enterprise compliance ready |
| **Enforcement Hooks** | Programmatic security controls |
| **Linux Sandboxing** | Container-friendly deployments |
| **Billing Groups** | Cost allocation by team |

---

## Service Accounts (Non-Human Identities)

> 🆕 **Released December 18, 2025**

Service Accounts are machine identities that can invoke Cursor agents without human presence.

### Capabilities

| Capability | Description |
|:-----------|:------------|
| **API Invocation** | Trigger code generation via API |
| **CI/CD Integration** | Automated PR review, linting, docs |
| **Credential Rotation** | Survives employee turnover |
| **Rate Limit Isolation** | Separate quotas from human users |

### Use Cases

```
CI/CD Pipeline:
├── PR opened
├── Service Account invokes Cursor
├── Agent reviews code, suggests fixes
├── Agent updates documentation
├── Human reviews final changes
└── Merge

Nightly Maintenance:
├── 2 AM: Service Account starts
├── Agent updates dependencies
├── Agent runs security scan
├── Agent opens PR for review
└── Human reviews in morning
```

### Setup

1. **Create Service Account**
   ```
   Cursor Dashboard → Enterprise → Service Accounts → Create
   ```

2. **Generate API Key**
   ```
   Service Account → API Keys → Generate
   ```

3. **Configure CI/CD**
   ```yaml
   # GitHub Actions example
   env:
     CURSOR_SERVICE_KEY: ${{ secrets.CURSOR_SERVICE_KEY }}
   ```

### Pricing

Service Accounts consume from Enterprise pool credits. No per-seat charge.

---

## SOC 2 Certification

Cursor has achieved **SOC 2 Type II** certification, covering:

- Security controls
- Data handling
- Availability guarantees
- Processing integrity
- Confidentiality

### What This Means

| Requirement | Cursor Status |
|:------------|:--------------|
| Data encryption at rest | ✅ |
| Data encryption in transit | ✅ |
| Access logging | ✅ |
| Incident response | ✅ |
| Vendor management | ✅ |

### Requesting Compliance Docs

Enterprise customers can request SOC 2 reports from their account manager.

---

## Enforcement Hooks

Programmatic controls that run before AI actions:

### Pre-Prompt Hooks

Runs **before** AI sees the prompt:

```javascript
// .cursor/hooks/pre-prompt.js
module.exports = async (prompt, context) => {
  // Block sensitive data patterns
  if (prompt.includes('API_KEY') || prompt.includes('password')) {
    return {
      blocked: true,
      reason: 'Prompt contains sensitive data patterns'
    };
  }
  
  return { blocked: false };
};
```

### Pre-Write Hooks

Runs **before** code is written to disk:

```javascript
// .cursor/hooks/pre-write.js
module.exports = async (filePath, content) => {
  // Block writing to critical paths
  if (filePath.includes('/etc/') || filePath.includes('C:\\Windows\\')) {
    return {
      blocked: true,
      reason: 'Cannot write to system directories'
    };
  }
  
  // Block dangerous patterns
  if (content.includes('rm -rf /')) {
    return {
      blocked: true,
      reason: 'Dangerous command detected'
    };
  }
  
  return { blocked: false };
};
```

### Configuration

```json
// .cursor/settings.json
{
  "security": {
    "enforcement_hooks": true,
    "hooks_path": ".cursor/hooks/",
    "block_on_hook_error": true
  }
}
```

---

## Linux Sandboxing

Agent commands now run in isolated sandboxes on Linux:

### Features

| Feature | Description |
|:--------|:------------|
| **Filesystem Isolation** | Agent can't access host files outside project |
| **Network Restrictions** | Optional network isolation |
| **Resource Limits** | CPU/memory limits configurable |
| **Container Support** | Works in Docker, Kubernetes |

### Configuration

```json
{
  "security": {
    "sandbox_mode": "strict",
    "sandbox_config": {
      "allowed_paths": ["/project", "/tmp"],
      "network": "host",
      "max_memory": "2G",
      "max_cpu": "2"
    }
  }
}
```

---

## Billing Groups

Allocate AI costs by team or project:

### Setup

1. **Create Billing Groups**
   ```
   Dashboard → Enterprise → Billing Groups → Create
   ```

2. **Assign Users**
   ```
   Billing Group → Members → Add Users
   ```

3. **Set Budgets**
   ```
   Billing Group → Budget → Set Monthly Limit
   ```

### Reporting

```
Dashboard → Insights → Cost Breakdown by Group
```

---

## Security: Known Vulnerabilities

### RCE via Workspace Trust (Patched)

**Vulnerability**: Malicious repos could execute code via `tasks.json` when opened, even with Workspace Trust disabled.

**Status**: Patched in Cursor 2.3+

**Mitigation**:
1. Update to Cursor 2.3+
2. Review `tasks.json` and `.vscode/` in cloned repos
3. Enable pre-prompt/pre-write hooks

### Best Practices

```
✅ Enable enforcement hooks
✅ Use read-only MCP configurations for production databases
✅ Audit .mdc files in untrusted repos
✅ Use Service Accounts with minimal permissions
✅ Set billing group budgets
❌ Don't trust arbitrary repos without review
❌ Don't give Service Accounts admin access
```

---

## Enterprise Deployment Checklist

```
Pre-Deployment:
[ ] SOC 2 report reviewed
[ ] Data handling policies approved
[ ] Network requirements documented
[ ] SSO/SAML configured

Security:
[ ] Enforcement hooks enabled
[ ] Sandbox mode configured
[ ] Audit logging enabled
[ ] Incident response plan documented

Operations:
[ ] Service Accounts created for CI/CD
[ ] Billing Groups configured
[ ] Usage alerts set
[ ] Support escalation path defined

Training:
[ ] Security best practices documented
[ ] Model usage guidelines published
[ ] Vibe coding policies established
[ ] Review workflow defined
```

---

## Enterprise vs Pro Comparison

| Feature | Pro ($20/mo) | Enterprise |
|:--------|:-------------|:-----------|
| Service Accounts | ❌ | ✅ |
| SOC 2 Report | ❌ | ✅ |
| Enforcement Hooks | ❌ | ✅ |
| Billing Groups | ❌ | ✅ |
| SSO/SAML | ❌ | ✅ |
| Dedicated Support | ❌ | ✅ |
| Custom Model Access | Limited | ✅ |

---

## Related Guides

- [Security Concerns](security-concerns.md) - General security practices
- [Cursor 2.3 Features](cursor-23-features.md) - Latest platform updates
- [MCP Integration](mcp-integration.md) - External integrations

---

*Last updated: December 24, 2025*
