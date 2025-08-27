# Nextcloud Talk HPB Recording Server - Automated Installation

Automated Ansible playbook to deploy the **Nextcloud Talk HPB Recording Server** for **Nextcloud Enterprise** customers.

This script installs, configures, and prepares the recording server to integrate with your existing Nextcloud Enterprise instance and Talk High Performance Backend (HPB).

---

## 📋 Requirements

Before running this playbook, make sure you have:

1. **Nextcloud Server Enterprise** up and running.
2. **Talk HPB already installed and configured** (Signaling server).
3. The following information ready:

   * **Admin Email** → For Let's Encrypt SSL certificate requests
   * **Recording Server URL** → DNS hostname pointing to this server (e.g. `record.yoursite.com`)
   * **Cloud URL** → Your Nextcloud main instance URL (e.g. `cloud.yoursite.com`)
   * **Talk Secret** → The secret configured in your HPB signaling server
   * **Talk Internal Secret** → The internal secret configured in your HPB signaling server
4. **Fresh Ubuntu 24.04 AMD64** installation.
5. **DNS configured** for the Recording Server hostname (must resolve to this server's public IP).
   Example:

   ```
   record.yoursite.com  →  your-server-ip
   ```
6. Root SSH access to the Recording Server.

---

## 📦 Initial Setup

Update packages and install dependencies for running this playbook:

```bash
sudo apt update && sudo apt install -y vim git ansible
```

---

## 🚀 How to Run

1. Clone this repository:

```bash
git clone https://github.com/jbcortezf/nextcloud-talk-recording-enterprise.git
cd nextcloud-talk-recording-enterprise
```

2. Run the playbook as root (it will prompt for configuration values):

```bash
sudo ansible-playbook talk-recording-enterprise.yml
```

3. The playbook will ask for:

   * **Admin Email** → For Let's Encrypt SSL
   * **Recording Server URL** (e.g. `record.yoursite.com`)
   * **Cloud URL** (e.g. `cloud.yoursite.com`)
   * **Talk Secret**
   * **Talk Internal Secret**

4. The script will:

   * Install required system packages (Python, FFmpeg, Pulseaudio, Firefox ESR, Geckodriver, etc.)
   * Create service user and directories
   * Clone the official **Nextcloud Talk Recording Server** repo
   * Build and configure it inside a Python venv
   * Configure `server.conf` with your secrets
   * Setup a **systemd service** for auto-start
   * Configure **nginx** reverse proxy for the Recording Server
   * Request an **SSL certificate** via Let's Encrypt
   * Start the service and validate it is running

5. Once completed, the playbook will display a summary with:

   * Recording Server URL
   * Config file path
   * Data directory
   * Service name

---

## 📑 Configuration on Nextcloud

After installation, go to your **Nextcloud Admin Settings** → **Talk** and configure **Recording**:

* **Recording server URL**:

  ```
  https://record.yoursite.com
  ```

* **Secrets**: Use the same **Talk Secret** and **Talk Internal Secret** provided during the playbook run.

---

## 🛠 Troubleshooting

* Make sure DNS for the Recording Server hostname resolves to the server before running the playbook.
* Ensure ports **80** and **443** are open on your firewall.
* If SSL request fails, check `/var/log/letsencrypt/letsencrypt.log`.
* If Firefox/Geckodriver errors appear, ensure the latest version was installed successfully.
* To debug service issues:

  ```bash
  systemctl status nextcloud-recording
  journalctl -u nextcloud-recording -f
  ```

---

## 📬 Support & Contact

For help, bug reports, or questions, contact:

**João Cortez**
Sales Engineer at Nextcloud GmbH

For help with this script if you're Nextcloud Customer
📧 Professional Email: [joao.cortez@nextcloud.com](mailto:joao.cortez@nextcloud.com)

For any other subjects
📧 Personal Email: [joao@jbcortezf.com](mailto:joao@jbcortezf.com)

---

## ⚠️ Disclaimer

This playbook is intended **only for Nextcloud Enterprise customers** with a valid Talk HPB license and recording entitlement.
Do not use without a valid subscription.
