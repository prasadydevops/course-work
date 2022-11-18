- Create vm in azure.

- Make sure to select these ports for incoming traffic during VM Creation.

  - 80 (http)
  - 443 (https)
  - 22 (ssh)

- Give your ssh public key during vm creation to access it later using SSH connection.

```bash
# Generate ssh keys (Avoid this if you already ssh key pair)
ssh-keygen -o # Generates RSK ssh key pair

# Copy ssh public key to use it during VM Creation for VM access.
cat ~/.ssh/id_rsa.pub
```

- Login to VM

```bash
ssh azureuser@<your_vm_public_ip>
```

- Clone code repo `git clone <ssh_clone_link>`.
- To access repos from azure repos you need to add your public ssh key to it. View public key using command `cat ~/.ssh/id_rsa.pub`

- Install nodejs ([Source](https://github.com/nodesource/distributions#debinstall)).

```bash
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```

- Change CWD to code repo and install node_modules

```
cd <code_repo_directory> # Run from home directory

npm install # Install node modules
```

- Build project code to generate build artifects

```bash
npm run build
```

> This generates dist/ which contains the actual web content that is server to end user.

- Install nginx

```
sudo apt install nginx
```

> NGINX is the web server that takes given content (In this case above dist/ folder) and serves to end user when they visit our website.
> NGINX have multiple features otherthan being a static webserver, like reverse proxy, load balancer, mail proxy and HTTP cache. We will use reverse proxy during backend deployment.

_At this point when you visit to your vm public ip from any web browser it should show nginx's default home page._

- Point our web content (dist/) to nginx webserver

```bash
# ln syntax
#  - Create a symbolic link to a file or directory:
#    ln -s /path/to/file_or_directory path/to/symlink

sudo ln -s ~/<code_repo_directory> /var/www/html
```

_At this point when you visit the VM public IP, you will see our site running on http://<public_ip> . it is not fully functional as some of the modules we use requires site to have an SSL certificate, which is what we do in the next step._

- Install SSL certificate to our site using Let's Encrypt certbot tool. ([source](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal))

```bash
# Ensure that your version of snapd is up to date
sudo snap install core; sudo snap refresh core

# Install Certbot
sudo snap install --classic certbot

# Prepare the Certbot command
sudo ln -s /snap/bin/certbot /usr/bin/certbot

# Choose how you'd like to run Certbot
# This command issues SSL certificate and automatically configures NGINX.
sudo certbot --nginx

```

_👏 Congratulations! your site deployed and running on `https`. Visit https://<vm_public_ip>` and checkout it._
