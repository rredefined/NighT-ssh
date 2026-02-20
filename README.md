# NighT-ssh

A simple script to (re)install and configure OpenSSH server for root password login in Ubuntu/Debian containers or VMs.

## Features
- Installs OpenSSH server if not present, or purges and reinstalls if already installed
- Configures `/etc/ssh/sshd_config` for:
  - `PermitRootLogin yes`
  - `PasswordAuthentication yes`
- Removes any conflicting config in `/etc/ssh/sshd_config.d/`
- Prompts to set the root password
- Restarts the SSH service

## Usage
1. Ready to run Script:
   ```sh
   bash <(curl -s https://raw.githubusercontent.com/rredefined/Vm-Manager/main/open-ssh.sh)
   ```
   ```
2. Follow the prompt to set the root password

After running, SSH will be ready for root login with a password.

---

**Credit:** Eirotf
