# My AWS final Project

This README details the steps taken to deploy a website on AWS using S3 and GitHub Actions.

## Steps

### 1. Create an account in aws console

Get into AWS console

![Screenshot 1](/src/assets/Screenshots/1..png)

### 2.Create a new bucket in Amazon S3

![Screenshot 2](/src/assets/Screenshots/2.png)

### 3.Enter a unique name for your bucket (e.g., my-portfolio-web).
### 4.Select the region closest to you or that best suits your needs.

![Screenshot 3](/src/assets/Screenshots/3.png)
### 5.Turn off the "Block all public access" option.
### 6.Accept the warnings and click "Create bucket".

![Screenshot 4](/src/assets/Screenshots/4.png)

### 7.Verify its creation
![Screenshot 5](/src/assets/Screenshots/5.png)

### 8.Click on the recent bucket
Go to the "Properties" tab.
Scroll down and select "Static website hosting."
Check the "Use this bucket to host a website" option.
Enter index.html as the index document, and if you have an error document, enter that as well (for example, 404.html).
Save your changes.

![Screenshot 6](/src/assets/Screenshots/6.png)

![Screenshot 7](/src/assets/Screenshots/7.png)

### 9.Configure Bucket Permissions:
Go to the "Permissions" tab in your bucket.
Click "Bucket Policy" and paste the following JSON policy to make the bucket public:

------------------------------
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3::: my-web-portfolio-paul /*"
        }
    ]
}
--------------------------------
![Screenshot 8](/src/assets/Screenshots/8.png)

### 10.Create a Github repository

Create a new private repository or use an existing one.
Add the README.md file and other required files.

![Screenshot 9](/src/assets/Screenshots/9.png)

### 11.Configura GitHub Actions:
Go to the "Actions" tab in your GitHub repository.
Create a new workflow by selecting "Set up a workflow yourself".
Add the .github/workflows/deploy.yml file with the content set for your Angular app.

-----------------------------------------------------
name: Deploy Angular App to S3

on:
  push:
    branches:
      - master  # Asegúrate de usar la rama correcta de tu repositorio

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v2

    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '18'  # Usa una versión de Node.js compatible

    - name: Install dependencies
      run: npm install

    - name: Build Angular app
      run: npm run build --prod  # Este comando debe coincidir con tu script de build en package.json

    - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: us-east-1

    - name: Sync S3 bucket
      run: aws s3 sync ./dist/my-portfolio s3://mi-portafolio-web --delete

![Screenshot 10](/src/assets/Screenshots/10.png)

### 12.Steps to get AWS_ACCESS_KEY_ID y AWS_SECRET_ACCESS_KEY

![Screenshot 11](/src/assets/Screenshots/11.png)

### 13 Set unsername:
Username: Enter a name for the user, such as github-actions-deploy.
Access Type: Check the "Programmatic Access" option to generate a AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.


![Screenshot 12](/src/assets/Screenshots/12.png)

### 14.Configurar Permisos:
Select "Attach existing policies directly."
Find and select the AmazonS3FullAccess policy if you want to give full access to S3. This allows the created user to have permissions to read and write to all S3 buckets.
Note: If you'd like to be more specific, you can create a custom policy to limit access to only the bucket you'll use for your site.


![Screenshot 13](/src/assets/Screenshots/13.png)

### 15. Review and Create User:

Review the selected settings and click "Next: Review."
Click "Create User" to finish 

![Screenshot 14](/src/assets/Screenshots/14.png)

### 16.Obtaining Credentials:
Once the user is created, you will see a confirmation screen with the AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY.
Important: Copy these values and keep them in a safe place. This is the only time AWS will display the AWS_SECRET_ACCESS_KEY.


![Screenshot 15](/src/assets/Screenshots/15.png)
----------------------------------------------------
### 17.Github secret configuration
Accessing your GitHub Repository:

Go to the repository where you want to configure GitHub Actions.
Configure Repository Secrets:

Click on "Settings" for the repository.
In the left sidebar, select "Secrets and variables" and then "Actions."
Click the "New repository secret" button to add a new secret.
Add AWS Secrets:

Name: AWS_ACCESS_KEY_ID
Value: Paste the AWS_ACCESS_KEY_ID you got from the AWS console.
Click "Add secret" to save.
Repeat this process for the AWS_SECRET_ACCESS_KEY:
Name: AWS_SECRET_ACCESS_KEY
Value: Sticks the AWS_SECRET_ACCESS_KEY.
---------------------------------------------------
![Screenshot 16](/src/assets/Screenshots/16.png)

### 18.Workflow test
Commit and push to the configured branch (e.g., main).
Verify that the workflow runs correctly in GitHub Actions.
Once it's complete, verify that your website is properly deployed in the S3 bucket.

![Screenshot 17](/src/assets/Screenshots/17.png)



## Results

The website is now online and working properly!

![Sitio Web en Línea](/src/assets/Screenshots/18.png)

