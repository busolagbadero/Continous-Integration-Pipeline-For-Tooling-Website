## INSTALL AND CONFIGURE JENKINS SERVER

Created an AWS EC2 server based on Ubuntu Server 20.04 LTS and named it "Jenkins"

![y1](https://user-images.githubusercontent.com/94229949/187044353-1d84390c-7fac-4c63-84aa-3db6881197e2.png)

Installed JDK since Jenkins is a Java-based application 

![y2](https://user-images.githubusercontent.com/94229949/187044382-3798ab0f-290d-40ab-9c96-61f23bb532e1.png)

![y3](https://user-images.githubusercontent.com/94229949/187044386-c9c521c2-1792-4367-b3b2-01fc1e9c254c.png)

Installed Jenkins using these command '
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins '

Made sure Jenkins is up and running using ' sudo systemctl status jenkins '

![y4](https://user-images.githubusercontent.com/94229949/187044449-04d821ff-1d6a-4ec8-857d-1ea63db3a83f.png)

![y4b](https://user-images.githubusercontent.com/94229949/187044457-fbc1cfbc-eead-4ef6-be68-2cfdfd56b31b.png)

Opened Jenkins Server Port 8080 on AWS Security Group

![y4c](https://user-images.githubusercontent.com/94229949/187044545-8fb9f161-c839-412a-a783-2965629ae6bb.png)

Performed initial Jenkins setup from the browser access http://52.90.43.26:8080
  
  ![y5](https://user-images.githubusercontent.com/94229949/187044605-629e4b23-263a-456f-9e2d-eaa952bf3f1a.png)

Retrieved Password from  'sudo cat /var/lib/jenkins/secrets/initialAdminPassword ' and  'chose install suggested plugins ' to complete installatiion

## Configured Jenkins to retrieve source codes from GitHub using Webhooks

Enabled webhooks in my GitHub repository settings

![y6](https://user-images.githubusercontent.com/94229949/187044821-c4a4c8dd-9b36-42b4-b0f1-b2c08db7a64a.png)

From the Jenkins web console, I clicked  on "New Item" and created a "Freestyle project"

In Jenkins freestyle project . i chose  Git repository, provided  the link to the Tooling GitHub repository and credentials (user/password) so Jenkins can access files in the repository.

![y15](https://user-images.githubusercontent.com/94229949/187045046-ab63c289-17cc-47df-9eda-a469e523588d.png)

I then Clicked the  "Build Now" button and it was succesful

![y7](https://user-images.githubusercontent.com/94229949/187045124-034d84ae-b1bc-43b3-bb7b-af0c4ad4bb67.png)

To Configure triggering the job from GitHub webhook ,i clicked on configured and scrolled down to 'build triggers ' 


![y16](https://user-images.githubusercontent.com/94229949/187045376-ae0ccb91-42c1-4be6-8ce6-aa3df59fefa9.png)

Also Configured  "Post-build Actions" to archive all the files 

![y17](https://user-images.githubusercontent.com/94229949/187045457-fba49a8b-5d12-43bf-8e0b-d55801ef3535.png)

Made some changes in a file in the GitHub repository  and pushed the changes to the master branch.A new build was  launched automatically by webhook

![y9](https://user-images.githubusercontent.com/94229949/187045582-d0deff43-e0e2-40a2-bbb3-88ff1c6a9458.png)

![y10](https://user-images.githubusercontent.com/94229949/187045595-6789c96d-255e-479e-9138-c56548359cef.png)

Checked the artifacts  stored on Jenkins server locally 'sudo ls /var/lib/jenkins/jobs/tooling_github/builds/2/archive/ '

![y11](https://user-images.githubusercontent.com/94229949/187045653-5f6a66bb-63a4-49c4-ab2c-38a0776191ef.png)

## Configured Jenkins to copy files to NFS server via SSH

Installed "Publish Over SSH" plugin on Jenkins

On Dashboard I selected "Manage Jenkins" and chose "Configure System" menu item and then Scrolled down to Publish over SSH plugin configuration section and configured it to be able to connect to  NFS server

Provided a private key , Arbitrary name , Hostname ,Username and Remote directory 

Opened  Jenkins job configuration page and added another one in "Post-build Action" and selected from the drop menu " send build artifacts over ssh " .  

Under the 'transfer set ' i chose to  copy all files and directories – i used **  to copy all and then saved it.

Went ahead to edit file in the GitHub Tooling Repo and Webhook triggered a new job 

![y13](https://user-images.githubusercontent.com/94229949/187047292-b55332dd-f44d-4b2c-ad7e-8c7cc226de3d.png)

To make sure that the files in have been updated , I SSH into the server and ran cat /mnt/apps/README.md

![y14](https://user-images.githubusercontent.com/94229949/187047358-968e5448-3ae2-47a9-8b36-9265c512f9ad.png)











