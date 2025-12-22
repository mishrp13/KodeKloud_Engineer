Below is a clean, exam-ready, step-by-step solution that will make the WordPress app show:

✅ “App is able to connect to the database using user kodekloud_aim”

This is written exactly for a KodeKloud / Stratos Datacenter environment.

✅ STEP-BY-STEP IMPLEMENTATION
🔹 A. Install Apache, PHP & dependencies (on ALL App Hosts)

Run on each app server (e.g. stapp01, stapp02, stapp03):

sudo yum install -y httpd php php-mysqlnd php-cli php-common


Enable and start Apache:

sudo systemctl start httpd
sudo systemctl enable httpd

🔹 B. Configure Apache to listen on port 6000 (ALL App Hosts)

Edit Apache config:

sudo sed -i 's/Listen 80/Listen 6000/' /etc/httpd/conf/httpd.conf


Restart Apache:

sudo systemctl restart httpd


Verify:

ss -tulnp | grep httpd


✔ Should show :6000

🔹 C. Install & configure MariaDB (DB Server only)

On DB Server:

sudo yum install -y mariadb-server


Start and enable MariaDB:

sudo systemctl start mariadb
sudo systemctl enable mariadb

🔹 D. Create database & user (DB Server)

Login to MySQL:

sudo mysql -u root


Run the following SQL exactly:

CREATE DATABASE kodekloud_db5;
CREATE USER 'kodekloud_aim'@'%' IDENTIFIED BY 'ksH85UJjhb';
GRANT ALL PRIVILEGES ON kodekloud_db5.* TO 'kodekloud_aim'@'%';
FLUSH PRIVILEGES;
EXIT;

🔹 E. Allow DB access (CRITICAL STEP)
1️⃣ SELinux (DB connections from Apache)

On ALL app hosts:

sudo setsebool -P httpd_can_network_connect_db on

2️⃣ Firewall (DB Server)
sudo firewall-cmd --permanent --add-service=mysql
sudo firewall-cmd --reload

🔹 F. Configure WordPress database connection

On any app host, edit WordPress config:

sudo vi /var/www/html/wp-config.php


Set these values:

define('DB_NAME', 'kodekloud_db5');
define('DB_USER', 'kodekloud_aim');
define('DB_PASSWORD', 'ksH85UJjhb');
define('DB_HOST', '<DB_SERVER_HOSTNAME>');


⚠️ Use DB server hostname, NOT localhost
(example: stdb01, db, etc.)

🔹 G. Final checks (MANDATORY)
1️⃣ Test DB login from app server
mysql -u kodekloud_aim -p -h <DB_SERVER_HOSTNAME> kodekloud_db5


✔ Must connect successfully

2️⃣ Restart Apache
sudo systemctl restart httpd

🔹 H. Verify via Load Balancer

Click App button on top bar

Open LBR URL

You should see:

✅ “App is able to connect to the database using user kodekloud_aim”
