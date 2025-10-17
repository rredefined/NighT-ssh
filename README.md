#!/bin/bash

set -e

echo "=== SSH Server Setup Script ==="

# Check if openssh-server is installed
if dpkg -l | grep -q openssh-server; then
  echo "OpenSSH server is already installed. Purging and reinstalling..."
  apt-get purge -y openssh-server
  apt-get autoremove -y
fi

echo "Installing OpenSSH server..."
apt-get update
apt-get install -y openssh-server

echo "Configuring SSH for root login and password authentication..."

# Backup old config
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak.$(date +%s)

# Remove any existing settings for PermitRootLogin or PasswordAuthentication
sed -i '/^PermitRootLogin/d' /etc/ssh/sshd_config
sed -i '/^PasswordAuthentication/d' /etc/ssh/sshd_config

# Add correct settings at the end
echo "PermitRootLogin yes" >> /etc/ssh/sshd_config
echo "PasswordAuthentication yes" >> /etc/ssh/sshd_config

# Optionally, remove any overrides in /etc/ssh/sshd_config.d/
if [ -d /etc/ssh/sshd_config.d ]; then
  echo "Removing overrides in /etc/ssh/sshd_config.d/ ..."
  rm -f /etc/ssh/sshd_config.d/*
fi

echo "Restarting SSH service..."
systemctl restart ssh || service ssh restart

echo "Setting root password..."
passwd

echo "=== SSH server is ready. Root login with password is enabled. ==="
