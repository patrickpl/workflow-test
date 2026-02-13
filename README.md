# workflow-test

Repository for testing GitHub Actions workflows with Signal notifications for security issues and CI/CD health monitoring.

## Features

This repository includes automated Signal messenger notifications for:

- **Push Events**: Get notified when code is pushed to main/master branches
- **Issue Events**: Track when issues are opened, updated, or closed
- **Security Alerts**:
  - Code scanning alerts (vulnerabilities detected in code)
  - Secret scanning alerts (exposed secrets/credentials)
- **CI/CD Health**: Monitor workflow run status (success, failure, cancelled)

## Setup Instructions

### 1. Enable CI/CD Health Monitoring (Optional)

To receive notifications for workflow status:

1. Edit `.github/workflows/signal-notifications.yml`
2. Uncomment the `workflow_run` section
3. Replace the example workflow names with your actual workflow names
4. Commit and push the changes

Example:
```yaml
workflow_run:
  workflows: ["CI", "Tests", "Deploy"]  # Your workflow names
  types: [completed]
```

Note: The `workflow_run` trigger only works on the default branch (main/master).

### 2. Enable Security Alerts (Optional)

To receive notifications for security issues:

1. Go to repository Settings → Code security and analysis
2. Enable "Code scanning" with CodeQL or another tool
3. Enable "Secret scanning"
4. Enable "Secret scanning push protection" (recommended)

## Notification Examples

### Security Alert - Code Scanning
```
🚨 Security Alert: Code Scanning!

⚠️ Severity: high
📝 Rule: SQL injection vulnerability
📁 File: src/database.js
🔗 Link: https://github.com/...
```

### Security Alert - Secret Detection
```
🔐 Security Alert: Secret Detected!

🔑 Secret Type: GitHub Personal Access Token
📁 Location: config/secrets.yml
🔗 Link: https://github.com/...
⚠️ Action Required: Review and revoke the exposed secret immediately!
```

### CI/CD Health - Failure
```
❌ CI/CD Failed!

📦 Workflow: Run Tests
👤 Triggered by: username
🔗 Link: https://github.com/...
⚠️ Action Required: Check the workflow logs for details
```

### CI/CD Health - Success
```
✅ CI/CD Success!

📦 Workflow: Deploy to Production
👤 Triggered by: username
🔗 Link: https://github.com/...
```

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Security Features](https://docs.github.com/en/code-security)