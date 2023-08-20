# Welcome to my tutorial ✨

Let's dive in and discover the secrets of AWS. 🌊 <br>
I hope you benefit and enjoy it.🙏 <br> <br>

# Steps 👣💫
## **1.** Sign in to your AWS accounte.
Use this link                    👉 [AWS](https://signin.aws.amazon.com/signin?redirect_uri=https%3A%2F%2Fconsole.aws.amazon.com%2Fconsole%2Fhome%3FhashArgs%3D%2523%26isauthcode%3Dtrue%26nc2%3Dh_ct%26src%3Dheader-signin%26state%3DhashArgsFromTB_eu-north-1_4b312e400b236363&client_id=arn%3Aaws%3Asignin%3A%3A%3Aconsole%2Fcanvas&forceMobileApp=0&code_challenge=D-5cOHc39_nTlE2-pGonns9_T7n5y5NzWEpzZhds6qk&code_challenge_method=SHA-256) <br>

## 2. Creat Auto Scaling Group 👥

![1](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/6ef7b93b-78e3-4152-819b-35c67b1ce904)
Then choose the name of the name of the Auto Scaling group.📃 <br> <br>

## 3. Creat a luanch template 📑
![Screenshot from 2023-08-20 15-48-06](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/2ade27cc-3f4a-46f2-b28e-ea1790b04f55) <br> <br>


## 4. Write the name and the description of the template 📝

![Screenshot from 2023-08-20 11-20-10](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/c7b4f25f-1443-4ae9-8e51-0ca7f819c662) <br><br>

## 5. This step is the same of creating instance, in my project I used this temaplte, you may have to change something in it. 😁
![Screenshot from 2023-08-20 11-22-49](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/c9f5841a-f879-44ca-9f0d-33468cf4705a)<br><br>
![Screenshot from 2023-08-20 11-23-06](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/279aa9c7-cbad-4dd9-ba00-3e4d90876773)<br><br>
![Screenshot from 2023-08-20 11-23-40](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/5f23f186-bf0e-4aab-a6fd-8c1aa39cdbcf)<br><br>

### Then go here if you want to add a script for the instances.💁🏻‍♀️
![Screenshot from 2023-08-20 11-23-59](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/4afe7a99-081e-42a3-a967-16ad9f8e40f3)<br><br>
### Write your script here 👇
![Screenshot from 2023-08-20 11-24-36](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/308582b3-f87d-4f04-aced-257ca9ca2488) <br><br>

## Here is my script with explanation for it:📜 
```bash
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

### Here is your luanch template ready😁
![Screenshot from 2023-08-20 11-24-54](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/52b08fea-35fb-4627-a1e0-2572a0c2d990) <br><br>

## 6. Now we are ready to creat an Auto scalling group 👥

![Screenshot from 2023-08-20 11-26-53](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/bfcd33cc-9f2d-49c3-a93a-3e9c901e47cc)<br><br>

## 7. Choose your tamplate and the latest version for it 🌟
![Screenshot from 2023-08-20 11-27-59](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/64e0c78f-4f77-4607-83aa-ef4acf449939) <br><br>

## 8. Choose the Availability Zones, it's recommended to choose as possible as you can 🌍
![Screenshot from 2023-08-20 11-28-16](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/913214a1-4278-4e64-a41e-2c8e51038be9)<br><br>

## Next 🔥
![Screenshot from 2023-08-20 11-28-34](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/bd1f90fe-6008-4437-a2e1-fc7d810cf0cf)<br><br>


## Your Auto Scaling Group is ready!🔥👥 Now let's move to creat load balancer. <br><br>

## 9. Creat load balancer 📦🔀 
If you have already a load balancer you can use it.
![Screenshot from 2023-08-20 11-28-57](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/7be57bde-2aba-43f6-b774-a52e7172c3dc)<br><br>

## 10. Choose the load balncer type, name and scehme 🫧
![Screenshot from 2023-08-20 11-31-53](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/2f8efb94-ed67-4e05-9490-a9c6974a4832)<br><br>

## 11. Now take a look in what you have chosen 👀
![Screenshot from 2023-08-20 11-33-56](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/052500a7-75c3-4f20-8bb5-c1d2d706bcc6)<br><br>

## 12. Now creat a target group🎀
![Screenshot from 2023-08-20 11-36-20](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/5b8b99ab-e7d7-4712-a1a6-61756d1da1b0)<br><br>

## 13. Choose the taget group name🚀
![Screenshot from 2023-08-20 11-45-46](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/5976b8fd-5c1e-4e97-9a7c-250b2f3783b2)<br><br>

## 14. Enable the health checks and choose the health period💫
![Screenshot from 2023-08-20 11-46-20](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/8ef007d2-8300-4ddd-9157-7c15df80ba39) <br><br>

##  Let's move to next steps 🔥
![Screenshot from 2023-08-20 11-46-31](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/41bdb410-fc70-4f7f-9e88-79d5647a4cdf)  <br><br>

## 15. CHoose the Group size, accourding to your application.💻
It's recommended to have mimimam 2 instances, because if one fail there will be other one.
![Screenshot from 2023-08-20 11-46-50](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/78b87f41-6704-4e35-aee8-da19d0309627) <br><br>

![Screenshot from 2023-08-20 11-47-56](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/d1717f9d-4a8b-4a8f-980a-25c944c20ab7)<br><br>

## Now you'll have sothing like this 💁🏻‍♀️
![Screenshot from 2023-08-20 11-49-37](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/cdf476c1-9ac4-4bb7-85cc-6fb5645b6a14)<br><br>

##  Your instances will be created automatichly 🍃
![Screenshot from 2023-08-20 11-51-02](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/17782655-b246-48f9-ad48-bee24ecc895a)<br><br>

## Check up your load balancer 👩🏻‍💻
You can copy this url, and make requests to toye instances.
![Screenshot from 2023-08-20 11-57-06](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/cd9fe4fb-02dc-4ac4-bc01-38852a36629c)<br><br>

## Go to yuor target group to check the health of your instances.📊
![Screenshot from 2023-08-20 11-59-36](https://github.com/SarahAbuirmeileh/AWS-Deployment-Tutorial/assets/127017088/30f8e884-3eaf-4c5e-89b4-d40d93a6187b) <br><br>

# Congratulation you are done!💪🔥
#### You have created an Auto Scalling Group, load balncer and deploy your application.👏
