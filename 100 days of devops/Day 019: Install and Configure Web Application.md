🔹 Step 1: Login to App Server 3

From jump_host:

ssh tony@app03


(Username may vary in your lab; use the correct one.)

🔹 Step 2: Install Apache (httpd)
sudo yum install -y httpd


Verify:

httpd -v

🔹 Step 3: Configure Apache to listen on port 8088

Edit Apache config:

sudo vi /etc/httpd/conf/httpd.conf

Change this line:
Listen 80

To:
Listen 8088


Save and exit.

🔹 Step 4: Copy website backups from jump_host

From jump_host, copy directories to app server 3:

scp -r /home/thor/beta tony@app03:/tmp/
scp -r /home/thor/apps tony@app03:/tmp/

🔹 Step 5: Place websites in Apache document root

On app server 3:

sudo mv /tmp/beta /var/www/html/
sudo mv /tmp/apps /var/www/html/


Set proper permissions:

sudo chown -R apache:apache /var/www/html/beta /var/www/html/apps
sudo chmod -R 755 /var/www/html/beta /var/www/html/apps



sudo vi /etc/httpd/conf/httpd
