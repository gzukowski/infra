# VM Hardening

## 1. Updates

```bash
apt update && apt full-upgrade -y
apt install -y unattended-upgrades
dpkg-reconfigure -plow unattended-upgrades
```

## 2. Create user (avoid using root)

```bash
adduser new-user
usermod -aG sudo new-user
```

## 3. Generate SSH key (on local machine)

```bash
ssh-keygen -t ed25519
```

## 4. Authorize key

```bash
ssh-copy-id new-user@VM-IP
```

## 5. SSH hardening

```bash
sudo nano /etc/ssh/sshd_config.d/00-hardening.conf
```

```
PermitRootLogin no
PasswordAuthentication no
KbdInteractiveAuthentication no
PubkeyAuthentication yes
MaxAuthTries 3
LoginGraceTime 30
X11Forwarding no
AllowUsers new-user
```

```bash
sudo sshd -t && sudo systemctl restart ssh
```

## 6. Firewall (SSH only)

```bash
sudo apt install -y ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status verbose
```

## 7. Check open ports

```bash
sudo ss -tulpn
```
