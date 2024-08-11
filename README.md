# AWS & DevOps Project 1 : Setting Up an IDE on AWS EC2 

**Purpose of the Project:**
This project provides an alternative way to set up a IDE on an EC2 instance using VS Code and Remote-SSH *(since I couldn't access AWS Cloud9 due to access restrictions)*. It offers a flexible cloud-based development solution, aligned with DevOps practices.

## Prerequisites

### Create IAM User
- An IAM User is an additional user that gets access to your AWS Account's resources. When creating a User, you can specify the level of access it has to your account's resources and services.
- It's important to create IAM Users because the Root User is vulnerable to security breaches that could expose your billing information.
- I created an IAM User with Administrator Access. This means the IAM User can perform all possible actions on all resources in the account.



![8888](https://github.com/user-attachments/assets/eff6f9ec-e202-4741-bb36-f990dd9079bd)



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

![ec2](https://github.com/user-attachments/assets/a56cfc16-754a-45fc-99c1-35dd64cb2f0f)

![ec2instance](https://github.com/user-attachments/assets/ac7af1a2-3fd6-47b5-b4f2-ed9674710477)

### Step 2: Download the .pem File

Download the key pair file (`.pem`) to your local machine. This file is necessary for SSH access to your EC2 instance.

### Step 3: Set File Permissions

- Run Windows Command Prompt as Administrator.
- Then run this command: 
```cmd
icacls C:\path\to\your-key-pair.pem /inheritance:r /grant:r %username%:R
```

### Step 4: Install VS Code and Install Remote-SSH

1. **Install VS Code**:
   - Download and install Visual Studio Code.

2. **Open VS Code and Install Remote-SSH Extension:**
   - Install the Remote-SSH extension from the Extensions Marketplace in Visual Studio Code.

3. **Configure the SSH Settings:**
   - Open the SSH configuration file by navigating to `~/.ssh/config` (or create it if it doesn't exist). 
   - Add the following configuration, but **replace the placeholder values with your specific details**:

     ```plaintext
     Host <Your-Host-Name>
       HostName <Your-EC2-Instance-Public-IP>
       IdentityFile <Path-to-Your-.pem-File>
       User ec2-user
     ```

   - **Example:**
     ```plaintext
     Host MyEC2Instance
       HostName 54.165.71.118
       IdentityFile C:\Users\YourUsername\Path\To\Your\ec2keypair.pem
       User ec2-user
     ```
     
![config](https://github.com/user-attachments/assets/a596da2b-186f-4f55-adef-6a1b486e4c85)

   - **Explanation:**
     - `<Your-Host-Name>`: A friendly name for your connection (you can name this whatever you like).
     - `<Your-EC2-Instance-Public-IP>`: The public IP address of your EC2 instance.
     - `<Path-to-Your-.pem-File>`: The full path to where your `.pem` file is located on your local machine.
     - `User ec2-user`: The default username for Amazon Linux 2 instances.

4. **Save The Configuration File:**
   - Save the SSH configuration file and use the Remote-SSH extension in VS Code to connect to your EC2 instance using the `Host` name you defined.


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

### Step 8: Edit and Customize the Application
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


![ec2working](https://github.com/user-attachments/assets/a4a9348a-fcfd-4573-97d8-094e98f3e568)



## Explanation

This project showcases how to leverage AWS EC2 and modern development tools to create a flexible and efficient Java development environment. By setting up an EC2 instance and using VS Code with Remote-SSH, you can develop and manage your applications directly in the cloud. This approach aligns with DevOps principles, enabling continuous development and deployment workflows, improving productivity, and enhancing collaboration.


## Notes

- This project was configured and tested on Amazon Linux 2.
- The `.pem` file should be kept secure and its permissions properly set.
- Ensure that the key pair file path and permissions are correctly configured to avoid connection issues.




