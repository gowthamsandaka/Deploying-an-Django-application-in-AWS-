# Deploying-an-Django-application-in-AWS-



Video Link:  https://drive.google.com/file/d/1LEOgSYbWxBvZ3RLJn5wTMcOzPOqbLf2n/view?usp=drive_link


The main objective of this project is to build a full stack Django application and deployment of a scalable Django application based on the CPU usage the application could be auto scaled and provide high availability of computation. Application would be auto scaled by using threshold-based algorithm. We could observe and verify this by monitoring CPU utilization in parallel to running EC2 instances using Cloudwatch.

please click the link to watch the video

URL: 

Design Description
We have selected static django application from GitHub to deploy in the Amazon Web Services. For that, an 
EC2 instance of ubuntu AMI is launched and connected to the server. To deploy the application in ubuntu 
server, Nginx Web server and Gunicorn WSGI are configured. After deploying the Django application, it is auto 
scaled by adding the scaling policies that configures the metric average CPU utilization of the application and 
takes the action satisfying scaling policies.
Implementation 
To deploy an application, at first an Ec2 instance should be launched on the amazon web services.
To launch an Ec2 instance
1. Select the Ec2 services on Aws console and then launch an instance.
2. Select ubuntu server image from the Amazon Machine Images (AMI).
3. Select the default VPC and subnets.
4. Set a security group for the instance with proper inbound rules.
5. Create a key pair and download the pem file.
6. Connect the EC2 instance to the server using putty.
Deploying the Django application on the Amazon Web Services Ec2 instance
Django provides the sever that facilitates to run the Django applications on the local host.one of the most likely 
used services to host Django applications is Amazon services.
Certain set of applications are required to host Django application.
1. Firstly, we need to update our server.
 sudo apt-get update
 sudo apt-get upgrade
2. Make sure that python is installed on the connected server.
3. Create a virtual environment for the application.
sudo apt-get install python3-venv
python3 -m venv env
4. Activate the created environment.
source enc/bin/activate
5. Install Django on the sever using pip.
pip3 install django 
6. Pulling our django application from the GitHub.
As our django application is static which is used for the deployment based, we prepared our django application 
ready for production and pushed it into the GitHub.
Our Django application in GitHub: https://github.com/gowthamsandaka/django_test.git
To clone the django application from GitHub:
git clone https://github.com/gowthamsandaka/django_test.git
Configuration of Nginx and Gunicorn
1. For the faster performance we will use Nginx web server.
 To install Nginx 
sudo apt-get install -y nginx 
2. We will use Gunicorn to link the django application and Nginx web server. For that install gunicorn 
using pip.
Pip3 install gunicorn
3. To configure gunicorn and Nginx and run the application always on background, supervisor should be 
installed. 
 sudo apt-get install supervisor 
4. Create the configuration for the supervisor in a directory /etc/supervisor/conf.d/
 Reach that directory.
cd /etc/supervisor/conf.d
sudo touch gunicorn.conf
5. Open the created configuration file and write the below code in it.
sudo nano gunicorn.conf 
6. We need to create a log directory that we have given in the configuration file.
sudo mkdir/var/log/gunicorn
7. Make supervisor to read the created configuration file.
sudo supervisorctl reread
8. Make supervisor to start gunicorn.
sudo supervisorctl update
9. To check the status of the supervisor, run
sudo supervisorctl status
10. Now, configure the Nginx server. For that we need to reach the directory /etc/nginx/sites-available, 
where the entire Nginx configuration is saved.
cd /etc/nginx/sites-available
11. Create a configuration file for the django application and write in it.
sudo touch django.conf
sudo nano django.conf
12. To test the configuration 
 cd/etc/nginx/sites-available
13. Enable the created configuration, to make nginx read it.
sudo ln django.conf /etc/nginx/sites-enabled/
14. Finally, restart the Nginx server using the command:
sudo service nginx restart
15. visit the website with the IP address of the instance.
Autoscaling of the deployed django application in AWS:
Step1: create an image for the instance in which application is deployed.
Step2: create an application load balancer for the application:
• In the configuration settings, select default VPC and subnets for that VPC.
• Configure the security group with HTTP, SSH inbound rules.
• Create a target group for the load balancer and then launch the application load balancer.
Step3: create a launch configuration for the AMI created from the instance.
• Choose an AMI we created before from the AMIs list.
• Select the t2.micro instance type.
• Select the security group from the list of security groups created before or create a new one.
• Select the key pair and create the launch configuration.
Step4: create an autoscaling group using the created launch configuration.
• Select the launch configuration we created before.
• Select the default VPC and the subnets created for that default VPC.
• Attach the load balancer we created before to the auto scaling group.
• Configure the group size and add the scaling policies.
• In this we can mention the desired, minimum, maximum capacities of the group.
• After successful creation of auto scaling group, the stated number of desired instances get launched and 
the application is deployed in them, that appears in the activity history of the auto scaling group.
• In the step scaling policy, select the metric CPU utilization and set the threshold value for the average 
CPU utilization, add the alarm for the metric, add the action to take and create the step scaling policy.
• Create an alarm that triggers when CPU utilization reaches the threshold value.
• Create a dashboard in the amazon cloud watch and add CPU utilization metric to the dashboard for 
monitoring.
• Stress the CPU using apache bench below command which increases the CPU utilization:
 ab -n 500000 -c 5 ip-172-31-1-208.ec2.internal/index.html 
Validation
• Static Django application is deployed in the ubuntu instance server and the application is accessed from 
the public IPV4 address of ubuntu instance (ec2-3-84-35-84.compute-1.amazonaws.com)
• Auto scaling group with desired capacity 2 is launched with target tracking policy that tracks Average 
CPU utilization metric and deployed the application in that 2 instances using AMI created from the 
ubuntu instance.
Static Django application is deployed in the instances created and can be accessed from the public 
IPV4 address of the instance. (ec2-107-22-132-15.compute-1.amazonaws.com)
 
• The CPU utilization is increased with increased stress using apache bench and can be monitored in the 
amazon cloud watch dashboard.
• An instance is added after increasing the stress on CPU which reached the threshold value of CPU 
utilization 30, specified in the step scaling policy satisfying Scalability with respect to computation.
Results
• The static django application has pulled from the GitHub and deployed in the ubuntu instance server
which is launched in the Amazon Web Services by configuring the Nginx web server, Gunicorn WSGI 
which increased the performance and made the application to run in the background as shown in the 
validation. 
• The deployed django application is auto scaled by adding the scaling policies with 2 desired instances 
which configured with the threshold value of the average CPU utilization of the ubuntu server in which 
the static Django application is deployed.
• Increased stress also increased CPU utilization, which after reaching the threshold value triggered the 
alarm and acted satisfying the step scaling policy, added 1 instance in the dashboard.
• Thus, the Django application deployed in the Amazon Web Services satisfied the requirements 
scalability and high availability with respect to computation.
