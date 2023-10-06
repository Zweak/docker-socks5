# Docker SOCKS5 Proxy

This repository will allow you to launch your own socks5 proxy server at minimal cost.

## Quickstart

Below are instructions for raising a SOCKS5 proxy. The only requirement is the presence of a [white](https://help.keenetic.net/hc/ru/articles/213965789-What-is-the-difference-between-white-and-gray-IP-address-) IP. If you do not have a personal server, then at the bottom of the page there are instructions for setting up a free server on AWS. You can also use any other cloud server provider.

1. Install Docker engine.
   
   Select instructions for your OS. The entire system was tested on Ubuntu only, but for other OSes it should also work with some modifications to the commands below.
 
   1. [Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce)
   2. [Mac](https://docs.docker.com/docker-for-mac/install/)
   3. [Windows](https://docs.docker.com/docker-for-windows/install/)
   
2. Clone Repo

   ```bash
   git clone https://github.com/elejke/docker-socks5.git
   cd docker-socks5
   ```
   
3. Build a Docker image

   ```bash
   sudo docker build -t socks5 .
   ```
   
   If you want to use a login and password for your server, then correct the corresponding line in [Dockerfile](Dockerfile#L4) to the desired login/password pair.
   
4. Run the Docker image:

   ```bash
   sudo docker run -d -p 80:1080 socks5
   ```
   
   In this case, the proxy server will work on port 80. You can change it to an arbitrary one by changing the corresponding number when starting the Docker container. Please note that the selected port must be open for external access.
   
   If you want to use a login/password for your proxy, you must also add a configuration file to the Docker container, which is done by adding an option at startup:
   
   ```bash
   sudo docker run -d -p 80:1080 -v ${PWD}/sockd.conf:/etc/sockd.conf socks5
   ```
   
   In this case, the login/password specified in step 3 will be used.
   
5. Your proxy server is ready!

   Use your IP address, port specified in step 4 and login/password (if specified) in any application!
   
   For Telegram, the corresponding settings are in: 
   
   1. **iOS**: Settings - Data and Storage - Use Proxy
   2. **Desktop**: Settings - Privacy and Security - Use Proxy
   
## Free server on Amazon [AWS](https://aws.amazon.com)

1. Registering an AWS account with the ability to raise free servers for 12 months:

   1. Create an account at https://aws.amazon.com/free/ by selecting the account type **Personal**. When using free servers, money from the linked card will not be spent, with the exception of $1, which will be withdrawn upon registration.
   2. Log in to the created account using: https://console.aws.amazon.com
   3. Registration may take up to 24 hours to complete and confirmation that resources can be allocated. You can check whether it has ended or not in the console: go to the **Services** tab and click **EC2**

2. To launch a free server on **AWS EC2** go to the **Services > EC2** tab and click **Launch Instance**, on the left side click the checkbox next to **Free tier only** and select an image from the list * *Ubuntu Server 16.04 LTS (HVM), SSD Volume Type - ami-43a15f3e**
    
   1. Select **t2.micro**
   2. At the top of the screen, click **Configure Security Group**, click **Add Rule** and select **HTTP** from the drop-down list (your proxy server will already be configured to use 80)
   3. Click **Review and Launch > Launch**
      
      1. In the window that appears, click Create a new key pair, name it socks5 and click Download Key Pair and Launch Instance.
      2. Congratulations! You have started the server.

3. Go to the **Instances** tab on the left side of the **EC2** console and select the running server, press the **Connect** button and follow the instructions (change the key permissions and connect from the console via **ssh**)