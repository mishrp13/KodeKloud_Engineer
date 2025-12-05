✅ Step 1: Install iptables

Run on each app host:

For CentOS/RHEL:

sudo yum install iptables iptables-services -y


For Ubuntu/Debian:

sudo apt-get update
sudo apt-get install iptables -y
sudo apt-get install iptables-persistent -y

✅ Step 2: Block port 6000 for everyone except LBR host

Assume the LBR host IP = 172.16.238.14
(Replace with the correct IP in your environment.)

Run these commands on each app host:

Allow LBR host to access port 6000
sudo iptables -A INPUT -p tcp -s 172.16.238.14 --dport 6000 -j ACCEPT

Block port 6000 for everyone else
sudo iptables -A INPUT -p tcp --dport 6000 -j DROP

(Optional but recommended) Allow existing traffic, SSH, loopback, etc.
sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -P INPUT DROP

✅ Step 3: Make rules persistent after reboot
If using CentOS/RHEL:

Save rules:

sudo service iptables save


Enable service:

sudo systemctl enable iptables
sudo systemctl start iptables

If using Ubuntu/Debian:

Save rules:

sudo netfilter-persistent save
sudo netfilter-persistent reload

🎯 Final Expected State

✔ iptables installed on all app hosts
✔ Port 6000 allowed only from LBR host
✔ Port 6000 blocked for all other IPs
✔ Rules remain after reboot
