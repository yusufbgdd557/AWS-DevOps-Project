# DevOps Project: Setting Up a IDE on AWS EC2 

This project demonstrates how to set up a development environment for Java applications on an AWS EC2 instance using Visual Studio Code (VS Code) and Remote-SSH. The focus is on integrating DevOps practices to streamline the development workflow, including environment setup, code editing, and deployment.

## Prerequisites

### Create IAM User
- An IAM User is an additional user that gets access to your AWS Account's resources. When creating a User, you can specify the level of access it has to your account's resources and services.
- It's important to create IAM Users because the Root User is vulnerable to security breaches that could expose your billing information.
- I created an IAM User with Administrator Access. This means the IAM User can perform all possible actions on all resources in the account.


![image](https://github.com/user-attachments/assets/da518523-d253-4520-acb9-3fa8d6d5f459)



## Project Overview

In this project, you'll learn how to:
- Create and configure an EC2 instance for development.
- Set up secure access to the EC2 instance using SSH.
- Install necessary tools such as Amazon Corretto (Java) and Maven.
- Generate a Java application using Maven.
- Develop and edit Java applications directly on the EC2 instance using VS Code.

By following these steps, you'll gain practical experience in setting up a development environment in the cloud and using remote development tools, a crucial aspect of modern DevOps workflows.

## Steps

### Step 1: Create an EC2 Instance

1. **Log into AWS Management Console**:
   - Go to the EC2 Dashboard

2. **Launch a New Instance**:
   - Click "Launch Instance."
   - Choose **Amazon Linux 2 AMI**.
   - Select **t2.micro** (free tier eligible).
   - Configure instance details, storage, and security group (allow SSH).
   - Create a new key pair and download the `.pem` file.
   - Click "Launch Instances" and wait for the instance to be initialized.

### Step 2: Download the .pem File

Download the key pair file (`.pem`) to your local machine. This file is necessary for SSH access to your EC2 instance.

### Step 3: Set File Permissions

- Run Windows Command Prompt as Administrator.
- Run this command: 
```cmd
icacls C:\path\to\your-key-pair.pem /inheritance:r /grant:r %username%:R
```

### Step 4: Configure VS Code for Remote-SSH

1. **Install VS Code**:
   - Download and install [Visual Studio Code](https://code.visualstudio.com/).

2. **Install Remote - SSH Extension**:
   - Open VS Code.
   - Go to Extensions (Ctrl+Shift+X), search for "Remote - SSH," and install it.

3. **Configure SSH**:
   - Open Command Palette (Ctrl+Shift+P) in VS Code.
   - Select "Remote-SSH: Add New SSH Host."
   - Enter the SSH connection string:
     ```plaintext
     Host 54.165.71.118
       HostName 54.165.71.118
       IdentityFile C:\Users\yusuf\OneDrive\Masaüstü\AWS Studies\My AWS Projects\AWS x DevOps Project\ec2keypair.pem
       User ec2-user
     ```
   - Save the configuration.

### Step 5: Connect to the EC2 Instance

1. **Open VS Code** and use Command Palette (Ctrl+Shift+P).
2. **Select "Remote-SSH: Connect to Host"** and choose your configured host.
3. **Connect** to the EC2 instance.

### Step 6: Install Amazon Corretto and Maven

1. **Open Terminal in VS Code** (connected to your EC2 instance).

2. **Install Amazon Corretto**:
   ```bash
   sudo amazon-linux-extras enable corretto8
   sudo yum install -y java-1.8.0-amazon-corretto-devel
   ```

3. **Install Maven**:
   ```bash
   sudo wget https://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
   sudo sed -i s/\$releasever/6/g /etc/yum.repos.d/epel-apache-maven.repo
   sudo yum install -y apache-maven
   ```

4. **Verify Installation**:
   ```bash
   java -version
   mvn -version
   ```

### Step 7: Generate a Maven Project

1. **Create a New Maven Project**:
   ```bash
   mvn archetype:generate \
     -DgroupId=com.nextwork.app \
     -DartifactId=nextwork-web-project \
     -DarchetypeArtifactId=maven-archetype-webapp \
     -DinteractiveMode=false
   ```

2. **Navigate to Your Project Directory**:
   ```bash
   cd nextwork-web-project
   ```

### Step 8: Edit the `index.jsp` File

1. **Open the Project Folder** in VS Code.

2. **Edit `index.jsp`**:
   - Navigate to `src/main/webapp/index.jsp`.
   - Replace the contents with the following code, replacing `<YOUR NAME>` with your actual name:
     ```html
     <html>
     <body>
     <h2>Hello <YOUR NAME>!</h2>
     <p>This is my NextWork web application working!</p>
     </body>
     </html>
     ```

3. **Save the Changes**.

## Explanation

This project showcases how to leverage AWS EC2 and modern development tools to create a flexible and efficient Java development environment. By setting up an EC2 instance and using VS Code with Remote-SSH, you can develop and manage your applications directly in the cloud. This approach aligns with DevOps principles, enabling continuous development and deployment workflows, improving productivity, and enhancing collaboration.


## Notes

- This project was configured and tested on Amazon Linux 2.
- The `.pem` file should be kept secure and its permissions properly set.
- Ensure that the key pair file path and permissions are correctly configured to avoid connection issues.

********************************************************



# AWS DevOps Project

This project demonstrates how to set up a Java development environment on an AWS EC2 instance using VS Code and Remote-SSH. It involves creating an EC2 instance, configuring it, and developing a Java web application.

## Project Overview

This project involves the following steps:
1. Create an IAM User
2. Launch and Connect to EC2 Instance
3. Set Up Development Environment
4. Generate a Maven Project
5. Edit and Customize the Application

## Steps

### Step 1: Create IAM User
- An IAM User is an additional user that gets access to my AWS Account's resources. When creating a User, I can specify in detail the level of the access it has to my account's resources and services.
- It's important to create IAM Users because the Root User is vulnerable to security breaches that could result in my billing information being accessed without my permission.
- I created an IAM User with Administrator Access. This means my IAM User is allowed to perform all possible actions to all the resources in my account.

### Step 2: Launch and Connect to EC2 Instance
1. **Create a New EC2 Instance:**
   - Log into the AWS Management Console and navigate to the EC2 Dashboard.
   - Launch a new instance with Amazon Linux 2 AMI and choose the free tier eligible t2.micro instance type.
   - Configure instance details, add storage, and configure security groups. Ensure SSH (port 22) is allowed from your IP.

2. **Download Key Pair:**
   - Create and download a new key pair (.pem file). Keep it in a safe place as it is required to access the instance.

3. **Configure Key Pair Permissions:**
   - Open Command Prompt and navigate to the directory with the `.pem` file.
   - Set the correct permissions using the following command:
     ```bash
     icacls MyKeyPair.pem /inheritance:r /grant:r %username%:R
     ```

4. **Connect to EC2 Instance:**
   - Configure the SSH connection using VS Code:
     - Install the Remote-SSH extension in VS Code.
     - Open the SSH configuration file and add the following details:
       ```plaintext
       Host your-ec2-instance
         HostName [EC2-Public-DNS]
         IdentityFile C:\path\to\your\keypair.pem
         User ec2-user
       ```
   - Connect to the instance using the VS Code Remote-SSH extension.

### Step 3: Set Up Development Environment
1. **Install Java and Maven on EC2:**
   - Connect to your EC2 instance and run the following commands to install Amazon Corretto 8 and Apache Maven:
     ```bash
     sudo amazon-linux-extras enable corretto8
     sudo yum install -y java-1.8.0-amazon-corretto-devel
     sudo wget https://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo -O /etc/yum.repos.d/epel-apache-maven.repo
     sudo yum install -y apache-maven
     ```

2. **Verify Installation:**
   - Check the installations with:
     ```bash
     java -version
     mvn -version
     ```

### Step 4: Generate a Maven Project
1. **Create a New Maven Project:**
   - In the terminal on your EC2 instance, generate a new Maven project using:
     ```bash
     mvn archetype:generate \
       -DgroupId=com.nextwork.app \
       -DartifactId=nextwork-web-project \
       -DarchetypeArtifactId=maven-archetype-webapp \
       -DinteractiveMode=false
     ```
   - Navigate to the project directory:
     ```bash
     cd nextwork-web-project
     ```

### Step 5: Edit and Customize the Application
1. **Modify the `index.jsp` File:**
   - Open and edit the `index.jsp` file located at `src/main/webapp/index.jsp` to customize the application. For example:
     ```html
     <html>
     <body>
     <h2>Hello <YOUR NAME>!</h2>
     <p>This is my NextWork web application working!</p>
     </body>
     </html>
     ```
   - Save your changes.

## Conclusion
This project demonstrates setting up a Java development environment on AWS EC2 using VS Code and Remote-SSH, configuring the necessary software, and creating a simple web application with Maven.


