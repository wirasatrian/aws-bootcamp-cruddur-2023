# Week 0 — Billing and Architecture

## Technical Task Todo

### Create an Admin user dan test to access this account

I logged in using the AWS free-tier root account I created earlier. Following [security considerations **Ashish Rajan**](https://www.youtube.com/watch?v=4EMWBYVggQI&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=15) :
  1. Enable MFA for root user using google authenticator
  2. I have created an IAM user before, so I use this user as an admin user

   ![Existing IAM Admin user](assets/IAM-Admin-User.png)
  
  4. Logout from the root user and login using the admin user
  5. Enable MFA for admin user

  ![Enable MFA for Admin user](assets/Enable-MFA-for-admin-user.png)
  
### Generate AWS Credential

I added new AWS credential for use on this bootcamp

  ![AWS Credential](assets/AWS-credential.png)
  

### Install AWS CLI

I installing AWS CLI on Gitpod. First, point to my repo on Github, then click the Gitpod button.
Create new Terminal with bash prompt, go to the week-0 branch.
Refer to [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html), I follow these steps from the command line to install the AWS CLI :

  ![Command Line to Install AWS CLI](assets/Install-AWS-CLI.png)
  
AWS CLI installed succesfully !

  ![AWS CLI](assets/Proof-of-AWS-CLI-installed.png)
  

 


