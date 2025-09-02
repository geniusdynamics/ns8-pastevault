# 🗂️ PasteVault - Secure Text Sharing for NethServer 8

[![License](https://img.shields.io/badge/License-GPL--3.0-blue.svg)](https://opensource.org/licenses/GPL-3.0)
[![NethServer 8](https://img.shields.io/badge/NethServer-8-orange.svg)](https://nethserver.github.io/ns8-core/)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](https://www.docker.com/)

> **PasteVault** is a secure, self-hosted pastebin service for NethServer 8 that allows you to share text snippets, code, and documents with customizable expiration times and access controls.

## ✨ Features

- 🔒 **Secure Sharing** - Host your own pastebin with full control over data
- ⏰ **Expiration Control** - Set custom expiration times for pastes
- 🌐 **Web Interface** - Clean, modern web UI for easy management
- 🔧 **NethServer Integration** - Seamless integration with NS8 ecosystem
- 🛡️ **SSL/TLS Support** - Automatic HTTPS with Let's Encrypt certificates
- 📊 **Admin Dashboard** - Monitor and manage your paste vault
- 🌍 **Multi-language** - Support for multiple languages via Weblate
- 💾 **Backup & Restore** - Built-in backup capabilities

## 🚀 Quick Start

### Installation

Add the PasteVault module to your NethServer 8 cluster:

```bash
add-module ghcr.io/geniusdynamics/pastevault:latest 1
```

**Expected Output:**

```json
{
  "module_id": "pastevault1",
  "image_name": "pastevault",
  "image_url": "ghcr.io/geniusdynamics/pastevault:latest"
}
```

### Configuration

Configure your PasteVault instance (replace `pastevault1` with your instance name):

```bash
api-cli run configure-module --agent module/pastevault1 --data - <<EOF
{
  "host": "pastevault.yourdomain.com",
  "http2https": true,
  "lets_encrypt": true
}
EOF
```

**Configuration Options:**

- `host`: Your fully qualified domain name
- `http2https`: Enable HTTPS redirection (recommended: `true`)
- `lets_encrypt`: Request Let's Encrypt SSL certificate (recommended: `true`)

### Access Your PasteVault

Once configured, access your PasteVault at: `https://pastevault.yourdomain.com`

## 📖 Table of Contents

- [Features](#-features)
- [Quick Start](#-quick-start)
- [Installation](#installation)
- [Configuration](#configuration)
- [Management](#-management)
- [Debugging](#-debugging)
- [Testing](#-testing)
- [Contributing](#-contributing)
- [License](#-license)

## 🔧 Management

### Get Current Configuration

```bash
api-cli run get-configuration --agent module/pastevault1
```

### Update Module

```bash
api-cli run update-module --data '{
  "module_url": "ghcr.io/geniusdynamics/pastevault:latest",
  "instances": ["pastevault1"],
  "force": true
}'
```

### Uninstall

```bash
remove-module --no-preserve pastevault1
```

## 🔍 Debugging

### Environment Variables

Check module environment variables:

```bash
runagent -m pastevault1 env
```

### Access Module Environment

Become the module agent for debugging:

```bash
runagent -m pastevault1
```

### Container Inspection

List running containers:

```bash
runagent -m pastevault1
podman ps
```

Inspect container environment:

```bash
podman exec pastevault-app env
```

Access container shell:

```bash
podman exec -ti pastevault-app sh
```

## 🧪 Testing

Run the test suite using Robot Framework:

```bash
./test-module.sh <NODE_ADDR> ghcr.io/geniusdynamics/pastevault:latest
```

## 🌐 Smarthost Integration

PasteVault automatically discovers and integrates with your centralized smarthost setup:

- **Auto-discovery**: The `bin/discover-smarthost` script keeps settings synchronized
- **Event-driven updates**: Changes to smarthost configuration automatically restart services
- **Redis integration**: Settings are stored and retrieved from Redis keys

## 🎨 UI & Translations

### Supported Languages

PasteVault supports multiple languages through [Weblate](https://hosted.weblate.org/projects/ns8/):

- English (en)
- Spanish (es)
- Italian (it)
- Basque (eu)
- Portuguese (pt)
- Portuguese (Brazil) (pt_BR)
- German (de)

### Translation Setup

1. Add [GitHub Weblate app](https://docs.weblate.org/en/latest/admin/continuous.html#github-setup) to your repository
2. Add your repository to [hosted.weblate.org](https://hosted.weblate.org) or contact a NethServer developer

## 📄 License

This project is licensed under the GPL-3.0 License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

- **Documentation**: [NethServer 8 Docs](https://nethserver.github.io/ns8-core/)
- **Issues**: [GitHub Issues](https://github.com/geniusdynamics/dev/issues)
- **Discussions**: [NethServer Community](https://community.nethserver.org/)

---

**Built with ❤️ for the NethServer 8 ecosystem**
