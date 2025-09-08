# Rallly Self-Hosting Example

This repository contains all the necessary information and files to self-host your own instance of Rallly. Rallly is an open-source scheduling and collaboration tool designed to make organizing events and meetings easier.

## How to Use

Instructions for using this repo are available in the [self-hosting docs](https://support.rallly.co/self-hosting/docker-compose).

## Links

- [Source code](https://github.com/lukevella/rallly)

# How to Host Rally in the Cloud

 - Launch an EC2 instance. I usually use Amazon Linux 2023, but Ubuntu is also fine. It's preferable that your instance type is t2.small
 - Install docker and its dependencies as well as docker compose
 ```
sudo yum update -y
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo systemctl enable --now docker

sudo usermod -aG docker ec2-user
newgrp docker
#log out and log in for the group to take effect

#Verify installations
docker run hello-world
docker compose version

```
- Clone the repo to your ec2 instance using git or curl
```
git clone https://github.com/lukevella/rallly-selfhosted.git
cd rallly-selfhosted

curl -L https://github.com/lukevella/rallly-selfhosted/archive/master.tar.gz | tar -xz
cd rallly-selfhosted-main
```
 - Setup up the required config in the config.env file. Ensure the database url is correct. Generate a secret key using the commands below.
```
openssl rand -base64 32
```
 - Set SECRET_PASSWORD tov your secret key. Set NEXT_PUBLIC_BASE_URL the public url of your EC2 instance.
 - Next, complete email configurations and set up an smtp server. For ALLOWED_EMAILS, add emails which will be used to create accounts in your instance of rally. For NO_REPLY_MAIL, and SUPPORT_MAIL, add the email that will be used to send mails to the allowed emails (authentication codes).
 - In setting up my smtp server, I used my gmail account. First, you'll need to enable two-factor authentication. Second, generate an gmail app password on your gmail/google account.
<img width="1116" height="452" alt="image" src="https://github.com/user-attachments/assets/9bfc180b-b5b7-4ad6-9224-17122d697590" />
Your smtp configurations should look like the above.


Finally, start the server using
```
docker-compose up -d
```
Now, you can access rally via your public ip address

<img width="1200" height="750" alt="image" src="https://github.com/user-attachments/assets/84bd9fd1-9f79-43d5-9642-cce60ee5dba4" />
Next, enter the mail in your allowed mails. Then you'll receive a 6-digit authentication code. Use it to log into Rally.

<img width="1200" height="657" alt="image" src="https://github.com/user-attachments/assets/82eb5e23-6a2f-4f17-9ba4-7dea2244b89e" />

Rally is now working perfectly in the cloud

