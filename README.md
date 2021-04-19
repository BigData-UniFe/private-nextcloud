# 💻📱 Private Nextcloud

## ⚙️ Sign up for a DDNS service

The service will be available through the internet thanks to a [DDNS service](https://en.wikipedia.org/wiki/Dynamic_DNS). A recommended one is [No-IP](https://www.noip.com/), that has been used and proved working during tests.
Steps are easy:

- Create an account and a hostname in [No-IP](https://www.noip.com/) site
- In almost every home router, a dynamic ddns service can be configured; just follow the home router configuration page, nothing more than filling some details (DDNS provider, username, password, hostname)
- Enable port forwarding on the router for ports 80 and 443
- Check on No-IP site if an hostname update has been detected to make sure everything is working fine

That's it, DDNS should be now configured and ready to go!

## 🚀 Deploy with Ansible

### 🛠️ Installation

```bash
# Install pip3
sudo apt install python3-pip

# Install ansible
pip3 install ansible

# Install required plugins
ansible-galaxy collection install community.docker
ansible-galaxy collection install community.kubernetes

# Optionally install following package if ansible will log to remote/local machine using a password instead of a key
sudo apt install sshpass
```

### 🗒️ Configuration

Head to `inventory`:

- `host_vars/node00.yaml`: insert correct ansible user (the user used by ssh) and host (ip address of remote machine or localhost)
- `group_vars/all.yaml`:
  - Choose between `ansible_ssh_private_key_file` or `ansible_ssh_pass` and give the correct path for the key or password
  - Edit `nextcloud_host` to match your FQDN
  - Edit `nextcloud_user` with the admin username for Nextcloud
  - Remember to change the default password `password` for Nextcloud at the first login
  - Optionally edit `microk8s_version` (required >= `1.21/stable`)
  - Optionally edit `cert_manager_version` (required >= `v1.3.0`)

### ▶️ Execute playbooks

Install and deploy docker or kubernetes version by choosing the tag between `docker` or `microk8s`:

```bash
ansible-playbook site.yaml --tags "[docker or microk8s]"
```

Uninstall and undeploy what `site.yaml` did:

```bash
ansible-playbook site-rollback.yaml --tags "[docker or microk8s]"
```
