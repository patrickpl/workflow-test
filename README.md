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

### 1. Deploy Signal API Service

To send Signal notifications, you need to deploy a `signal-cli-rest-api` service:

```bash
docker run \
  --name signal-cli-rest-api \
  --volume "<config_path>:/home/.local/share/signal-cli" \
  --restart unless-stopped \
  --publish 8080:8080 \
  bbernhard/signal-cli-rest-api:latest
```

Replace `<config_path>` with a path on your host for persistent configuration.

### 2. Register Signal Bot Number

Follow the [signal-cli-rest-api documentation](https://github.com/bbernhard/signal-cli-rest-api/blob/master/doc/EXAMPLES.md) to register a phone number for your bot.

### 3. Configure GitHub Secrets

Add the following secrets to your GitHub repository (Settings → Secrets and variables → Actions):

- **`SIGNAL_API_URL`**: URL of your signal-cli-rest-api instance
  - Example: `http://your-server:8080/v2/send`
  
- **`SIGNAL_BOT_NUMBER`**: Phone number of the Signal bot (with country code)
  - Example: `+1234567890`
  
- **`SIGNAL_RECIPIENTS`**: Comma-separated list of recipient phone numbers
  - Example: `+1234567890,+0987654321`

### 4. Enable Security Alerts (Optional)

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

## Troubleshooting

If notifications are not being sent:

1. Check that all three GitHub secrets are configured correctly
2. Verify your Signal API service is running and accessible
3. Check the Actions tab for workflow run logs
4. Ensure your Signal bot number is properly registered

## Resources

- [signal-cli-rest-api](https://github.com/bbernhard/signal-cli-rest-api)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Security Features](https://docs.github.com/en/code-security)