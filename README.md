## Symfony_or_Drupal_Instance_with_optional_RDS_instance.template

To automate the creation of AWS resources for Symfony or Drupal websites

### IMPORTANT INFORMATION

This template creates an Amazon EC2 instance and an Amazon RDS DB instance. You will be billed for the AWS resources used if you create a stack from this template.

This template has been created based on my needs and has to be customized for your usage. 
The elements I recommend to change are identified by these symbols: << >> in the template. 

### What this template do

It creates an AWS environment ready for a Drupal or Symfony project with a database hosted on AWS RDS.  

It creates these resources: 

- 1 EC2 instance

   AMI : in parameter (tested with Amazon Linux AMI)  
   Type : in parameter  
   Tags : in parameter  
   Key : provide the name of the Amazon key pair (in parameter)  
   Termination protection : true (attention! it means the stack can't be deleted before disabling the protection)  
   IAM role : start the instance with a specific IAM role  
   Packages : an update is performed and the packages for Drupal or Symfony projects are installed (depending on parameter)   
   Web server : Apache is configured and we make sure it's running  
   php.ini : set the date.timezone  
   Monitoring : install the Amazon monitoring scripts and set the cron job to send Disk space and Memory data to CloudWatch

- 1 Security group for the EC2 instance

   Port 22 : open only to CIDR in parameter (ExternalIP)  
   Port 80 : open only to CIDR in parameter (ExternalIP)

- 4 CloudWatch alarms for the EC2 instance

	Disk space available  
	Memory utilization  
	Status checks  
	CPU utilization

- 1 RDS DB instance (optional)

   Engine : MySQL  
   Type : db.t1.micro  
   Storage : 5G  
   MultiAZ : false  
   MasterUser : root  
   MasterPassword : in parameter

- 1 Security group for the RDS DB instance (optional)

   The EC2 instance is authorized  
   The CIDR (parameter ExternalIP) is authorized

- 2 CloudWatch alarms for the RDS instance

   CPU Utilization  
   Freeable Space


### What this template doesn't do

- It doesn't install a database server on the EC2 instance 

- It doesn't create a database on the RDS DB instance (it just creates the instance, you have to create your databases afterwards)

- It doesn't upload your code (the Drupal code should be in `/var/www/<projectName>/htdocs/`, the Symfony code should be in `/var/www/<projectName>/web/`)

- It doesn't create the IAM role

- It doesn't create the SNS topic (used for CloudWatch alarms)

- It doesn't check the length of the master password for the DB instance

