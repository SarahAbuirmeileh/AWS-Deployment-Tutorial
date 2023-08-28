<br> <h1 align="center"><b>Welcome to my tutorial </b><img src="https://media.giphy.com/media/hvRJCLFzcasrR4ia7z/giphy.gif" width="35"> </h1> 

<br> <h3 align="center"><b> Let's dive in and discover the secrets of AWS. 🌊 <br>
I hope you benefit and enjoy it.🙏  <br>  </h1> 

<br> <h2 align="center"><b> From Zero to AWS Hero:🔥<br>Mastering Auto Scaling and Load Balancing for Seamless Deployments✨ <br> <br> </h1>  <br> <br>

## Table of Contents:
### 1. Prerequisites
### 2. Steps
**I. Creating an Auto Scaling Group**
  
  * Signing in to your AWS account
  * Creating an Auto Scaling Group
  * Creating a Launch Template
  * Configuring the Launch Template
  * Adding Startup Script to Instances
    
  
  **II. Setting Up Load Balancers**
  
  * Creating a Load Balancer
  * Configuring Load Balancer Settings
  * Creating a Target Group
  * Configuring Health Checks
  * Setting Group Size
  

### 3. Conclusion

### 4. Final Thoughts <br> <br>

## Prerequisites:📝
Before we dive into the deployment process, ensure you have the following:

1. An AWS account
2. Your application code was uploaded to GitHub in a public repo.
3. Familiarity with basic AWS concepts <br><br>

## Steps:👣💫 
### I. Creating an Auto Scaling Group:👥
**1. Signing in to your AWS account:** <br>   Begin by signing in to your AWS account from this link 👉[AWS]. <be>


**2. Set general settings:** 🔗 <br>
 Select a region that is geographically closer to your target audience. In my case I chose London. <br><br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/d75e8420-9a37-4159-9a5e-c744048bbcbd) <br>


3. In the search par: 🔎 <br>
write EC2 , then choose EC2 service<br><br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/0be56efa-bbe6-43b9-9a9c-8b0f80c6c3eb)<br>


4. Creating an Auto Scaling Group:👥<br>
4.1 From the right side choose: Auto Scalling Group<br><br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/1901f07d-affc-46d8-a0a2-2bdf96b96b3d)<br><br>
  4.2 Click on Creat Auto Scaling Group:<br><br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/02552627-6a6d-40f8-b23b-ec295ac901fa)<br><br>
  4.3 Choose a unique name for your Auto Scaling group.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/ecbe4e38-3e06-4012-82eb-0905082ae33a)<br><br>
  4.4 Create a Launch Template:<br>
You can choose Launch Template if you have one, now we will create new one. The Launch Template contains instance configuration details.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/a9eeb096-1232-41f5-aed7-69cccff183ea)<br><br>
  4.5 Provide a descriptive name and useful description for your template.📜<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/056cfa3b-737e-4d51-9e0e-caf99093916f)<br><br>
4.6 Walkthrough on creating an optimal launch template to ensure smooth application deployment. I use this template, you may have to change something in it.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/d35ee939-78ee-4e1e-a9ea-d77d08be5942)<br><br>
Use a key pair to securely connect to your instance. Ensure that you have access to the selected key pair before you launch the instance.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/715d831e-a3d8-4ba1-b5e5-26c93d117c3d)<br><br>
You can create Security group and add rules to allow specific traffic to reach your instance.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/73ed0bff-be0d-4cd0-95e6-2dbeb936a88c)<br><br>
Scroll down to reach **Advanced details**<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/756fa6cb-82a3-47a9-9c56-30abfa831326)<br><br>
Scroll down to reach **User data**.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/c5f7e5f4-722d-4ff2-8407-8be9e32f6c9e)<br><br>
Write your script in the box.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/254d97af-45dd-4723-b263-37ef6c42409e)<br><br>
### Here is my script with explanation for it: 📝<br>
This script is for my Express JS project, you can write any script you want, but you have to make sure the script is good, you can test it in alone instance.
#### Your script may includes steps like:
* Updating and upgrading packages
* Installing Node.js and other dependencies
* Creating user accounts for security
* Downloading and deploying your app <br><br>
```ash
#!/bin/sh
set -e

sudo apt update
sudo apt upgrade -y

# install nodejs repo to download the latest version then the node js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install nodejs jq curl -y

# create app and github users, to run the application from them, without root, for security purposes
sudo useradd --system --create-home --shell /usr/sbin/nologin app
sudo useradd -g app --no-create-home --no-user-group --home-dir /home/app --shell /bin/bash github
sudo usermod --append --groups app github

# deploy the app

# Write the github acoount and repo in a variable
repo="SarahAbuirmeileh/Book-App"

# Creat a download URL to use it later
# Here we want to download a release which is ready to run.
download_url=$(curl "https://api.github.com/repos/$repo/releases/latest" | jq --raw-output '.assets[0].browser_download_url')

# Download the service file to run it as systemd service
curl -O "https://raw.githubusercontent.com/$repo/main/infrastructure/app.service"

# Adding the file to systemd and enable it, to run whenever the instance start
sudo mv app.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable app.service

# Creat a repository for the application, and download the app there, then dcompress the app and install the npm 
sudo -u app sh -c "mkdir -p /home/app/app && cd /home/app/app && curl -LO $download_url  && tar xzvf app.tar.gz  && npm install --omit=dev"

sudo reboot
# Evrything is done !!!
```
<br><br> 
Create the template. <br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/eebcc717-8054-412b-aab9-76966cb3b0aa)<br><br>
4.7 Move back to the Auto Scaling group and choose your template.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/230fe4f0-19d8-4f05-91c0-8689b059f933)<br><br>
**Note:** If your template didn’t appear, you may need to refresh the template.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/66a24f9e-02f0-48f2-8d69-e7bf347ffa6c)<br><br>
4.8 Choose the latest version (optional), then click next.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/f54a8ae3-ea90-4988-8335-3b228848ca87)<br><br>
4.9 Choose the Availability Zones<br>
* It’s recommended to choose as possible as you can
* The number of Availability Zones may be different from one region to another.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/01b9bb66-9069-43da-a75b-d050373fe4f2)<br><br>
Now let’s move to the next part.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/b585ef8e-6210-4502-9267-82a6ec52cc28)<br><br><br>
                                                                                                                                     
II. Setting Up Load Balancers📦<br>
Fine-tune Load Balancer settings to optimize traffic distribution and improve availability.<br> <br>
1.1 Creat a Load Balancer and choose the type of it.<br>
You can choose ‘Attach to an existing load balancer’ if you already have one, now we’ll creat new one.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/5ba35d4a-25b9-47f9-a5a0-65770c03c366)<br> <br>
1.2 Write the name of your Load Balancer and choose the scheme.<br>
Internet-facing is designed to distribute incoming traffic from the internet to multiple targets.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/a7aadb0b-0e26-45ff-9895-a09d0e1e9a9d)<br><br>
1.3 Creat target group<br>
Configure a Target Group to manage routing traffic to instances effectively.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/4f505c49-0562-403e-b3af-5ef168b091a4)<br><br>
Choose the target group name<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/4285cce7-0f4c-4652-ba79-434c3e045e9c)<br><br>
1.4 Let the VPC Lattice integration options to the default value and enable the Turn on Elastic Load Balancing health checks<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/1bd2c3b3-853a-40ae-9f41-5520421d4512)<br><br>
1.5 Choose the Health check grace period according to your requirements.<br>
Set up health checks to monitor the instances’ status and replace unhealthy instances automatically.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/3d7ab1f3-5e17-41d4-a4c5-0989cf849bab)<br><br>
1.6 Choose the Group size : Desired, Minimum, Maximum.<br>
* Determine the appropriate group size based on your application’s needs and ensure high availability.
* It’s recommended to have a minimum of 2 instances because if one fails there will be another one.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/dbed0778-31bf-4dd0-af5b-f4d13b571a33)<br><br>
1.7 Enable instance scale-in protection (optional)<br>
You can choose Scaling policies<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/9dee3a8b-976f-4d27-bc1d-77699cc4629e)<br><br>
1.8 You have to see something like this 😁<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/6a7f25c2-3bca-41b5-b012-645d06e11d60)<br><br>
1.9 If you go to the EC2 instances you’ll see the your desired number of your instances is generated automatchly.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/147e9026-70df-4b96-ae05-c3cfd4dddcec)<br><br>
5.10 Go to your target group to check the health of your instances.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/5c7b4809-8a16-4594-b22d-89fa7476adf2)<br><br>
5.11 If you want to delete this work, you should start by deleting your Auto Scaling group.<br>
![image](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/a73c980c-315a-4dca-8a11-e533b9fa3dd9)<br><br><br><br>


## Conclusion:📌
Congratulations! By following this comprehensive guide, you’ve successfully learned how to deploy your application using AWS Auto Scaling Groups and Load Balancers. This process ensures high availability, fault tolerance, and efficient scaling for your applications. Now, you’re ready to take your AWS deployment skills to the next level and build robust, scalable applications.<br><br>

## Final Thoughts:📃
Deploying applications on AWS can seem daunting, but with this step-by-step guide, you’re equipped with the knowledge needed to navigate the process seamlessly. Remember, practice makes perfect. Feel free to experiment with different settings and configurations to tailor the deployment to your specific project’s needs.
