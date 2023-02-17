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
