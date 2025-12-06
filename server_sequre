##############################################################################################################
#create shh keys
ssh-keygen -t ed25519 -C "comment"

##############################################################################################################
#copy keys to server
ssh-copy-id root@server_ip

mkdir -p  ~/.ssh
chmod 700 ~/.ssh
echo "key" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
sudo vim /etc/ssh/sshd_config
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
PasswordAuthentication no
PermitRootLogin no
ChallengeResponseAuthentication no
UsePAM no
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
sudo systemctl restart sshd

##############################################################################################################
#Firewall (UFW) and Fail2Ban
sudo apt update
sudo apt install ufw -y
sudo ufw status verbose
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable

sudo apt install fail2ban -y
sudo systemctl status fail2ban
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
vim /etc/fail2ban/jail.local
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
[DEFAULT]
bantime = 36000
findtime = 600
maxretry = 5
backend = systemd
banaction = iptables-multiport
banaction_allports = iptables-allports
protocol = tcp
ignoreip = 127.0.0.1/8 ::1
[sshd]
enabled = true
port = 22
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
sudo systemctl restart fail2ban
sudo systemctl status fail2ban
sudo fail2ban-client status
sudo fail2ban-client status sshd

##############################################################################################################
#auto-updates
sudo apt update && sudo apt upgrade -y
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades

##############################################################################################################
#add admin user , no use root
sudo adduser --ingroup admin admin
sudo usermode -aG sudo admin
mkdir -p /home/admin/.ssh
cp /root/.ssh/authorized_keys /home/admin/.ssh/
chmod 700 /home/admin/.ssh
chmod 600 /home/admin/.ssh/authorized_keys
chown -R admin:admin /home/admin/.ssh
su - admin

