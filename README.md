# mysql_cloudmanaged_databases
* HHA 504 HW 4
* Due 10/1
* OG Instructions below
## Creating databases on Google Cloud Platform (GCP) and Azure 
### GCP
* Login on the [Cloud Console](https://console.cloud.google.com/) and click on the button new project. Name it something relevant to the task at hand.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/f9dcf680-5ad7-4ceb-9b2a-834f7c871f6f)
* Then when on the page to create a new project, make sure to designate the location of the new project as AHI - GCP.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/cad03e37-6540-4a02-b80e-27ef4b204a35)
* Once you have created a new project, make sure you are clicked into the right project. Go to the dashboard and check.
* Then click on the navigation menu in the top left and find the tab that is called SQL near the bottom. Once you have clicked on this tab, you'll be taken to a page and you will see a button that says "Create Instance". CLick on it.
  * Then you will see 3 database engine options. Choose the MySQL option.
  ![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/aa6e3304-6f01-4c3b-916d-9af10041fac1)
  * You will then have to click the button on screen to enable API.
  * After this, keep the database version as MySQL 8,0
  * For the "Choose Cloud SQL edition" choose <ins>Enterprise<ins>.
  * When choosing a preset, choose <ins>Sandbox<ins>.
  * Under "Customize your instance", go to machine configuration and choose <ins>Shared core;1 vCPU, 0.614 GB<ins>
  * At "Connections" choose a Public IP, and add a network. Name the network AllowAll and set it to 0.0.0.0/0 and then click the button to create your instance.

### Azure
* Login on the [Azure Portal](https://azure.microsoft.com/en-us/get-started/azure-portal) and search in the bar for Azure Databased for MySQL. Click on it and then click create.
* You will be brough to a page with two options, choose the Flexible Server.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/fefe900f-cce5-452a-9e1f-b557d0d094a0)
* Choose an existing resource group or create a new one
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/58af9f5d-c6be-4edb-811e-1c4fd59f3995)
* Create a server name relevant to the assignment (mine was hha504-hw4)
* Fill out all required fields
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/0e9f0669-a360-4de4-87df-c7a806223c1d)
* Under Networking make sure to enable public access, and to allow public access under Firewall rules. For the start and end IP address, use 0.0.0.0 and 255.255.255.255
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/0314e284-320b-46e2-9e09-1b4f58275210)
* Then press the button to create.

## Connecting the GCP and Azure Database to MySQL Workbench
### GCP
* To connect your GCP database to MySQL Workbench, find the public IP to your instance you created. This can be found on the dashboard.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/0a33af7b-13a9-47ef-8edc-49be27210e0d)
* Then within your MySQL Workbench, click on the tiny plus sign.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/15866b9a-d6b9-4d75-934e-705d04f55d76)
* A window will pop-up to setup a new connection. Name your connection and in the field for Hostname, input your public IP for the instance. The username should match the username you set up when creating your instance.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/cf64eebd-8e8d-4ce9-9efa-deddf6d99bd9)
* Once you have filled out all the fields, hit the button to test connection. You will be prompted to input a password, which you designated when creating your instance. If you input the correct password, you should see a successful connection message.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/91c35b6e-8ec9-4d58-86c5-10c863da3a29)
* Then you should be able to view this new connection on the homescreen.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/76ef4b63-53df-4c2f-8b35-c5dda26d31b2)

### Azure
* To connect my Azure databased to the MySQL Workbench, I followed the instructions found in this [quickstart guide](https://learn.microsoft.com/en-us/azure/mysql/flexible-server/connect-workbench).
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/eee5526e-935f-455c-ba2c-38c74dbebac3)
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/25566c91-abdd-4045-98bd-def38aef6d93)

## MySQL Workbench for Database Interaction
* For my SQL script, as a base I copied over the [example week4 code](https://github.com/hantswilliams/HHA_504_2023/blob/main/WK4/code/1_1_create.sql)
* I then modified this code to be for employees instead of patients, changing some names and making the patients table an employees table, and the demographics table a contact_info table.
* For my third table, I chose to create an employee tasks table
```
CREATE TABLE employee_tasks (
    task_id INT PRIMARY KEY AUTO_INCREMENT,
    employee_id INT,
    task_description TEXT,
    task_due_date DATE,
    FOREIGN KEY (employee_id) REFERENCES employees(employee_id)
);
```
* So overall within the query window within MySQL Workbench, this is what my code looked like
```
CREATE DATABASE employment;

USE employment;

CREATE TABLE employees (
    employee_id INT PRIMARY KEY AUTO_INCREMENT,
    SSN VARCHAR(9) UNIQUE NOT NULL
);

CREATE TABLE contact_info (
    contact_id INT PRIMARY KEY AUTO_INCREMENT,
    employee_id INT UNIQUE,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    date_of_birth DATE,
    address TEXT,
    phone_number VARCHAR(15),
    email VARCHAR(100),
    FOREIGN KEY (employee_id) REFERENCES employees(employee_id)
);

CREATE TABLE employee_tasks (
    task_id INT PRIMARY KEY AUTO_INCREMENT,
    employee_id INT,
    task_description TEXT,
    task_due_date DATE,
    FOREIGN KEY (employee_id) REFERENCES employees(employee_id)
);
```
* Then to run the code, I highlighted each section and hit the lighting bolt button to run it
 ![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/fe1c4325-b3ae-42da-8007-71c36c3e703c)
* I then used the command below, replacing the word employees with the different titles of my various tables to obtain a preview of what the tables looked like. After each one, hitting the lightning bolt again.
```
SELECT *
FROM employees;
```
* To create the ERD, I navigated to the Database button in the options ribbon and chose the button "Reverse Engineer", also accesisble by hitting CTRL+R
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/1c70e289-580d-4adb-abc4-973295945507)
* You will then be taken to a window that asks you to choose a stored connection (I chose my GCP and then did the same process for my Azure afterwards). You may have to re-input the password again.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/f0676117-e35e-4d1b-8245-53bb465fc90b)
* Then you can keep clicking the next button as the program goes through the process of Connect to DBMS, Select Schemas, Retrieve Objects, Select Objects, Reverse Engineer and Results.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/dac31a8b-fc1f-452a-a541-47a5f3176311)
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/aec09ee3-420c-4e68-92c8-efcc1b1bfd20)
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/3b686729-482b-4206-96ff-6d10231d2e8d)
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/c83cf4b5-cb45-47e1-b964-f1362dda9327)
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/4ab5cbc1-d6e3-4a6e-92cf-a9f5484f4ee1)
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/ebc6fe83-04bb-4801-861d-a2a2bf1557a1)
* On your results page, scroll around to find your generated ERD. The program automatically creates relationships but they may not be correct (one-to-one, or one-to-many). If you need to change the relationship type, follow the steps in the image below.
![image](https://github.com/meglee67/mysql_cloudmanaged_databases/assets/123908362/0a80ab53-a7f0-44dd-81b3-7ebb00cefdad)
<br>

## **Week 4 Homework Assignment: MySQL on Cloud Platforms - Azure and GCP**

### **Objective**:
This assignment focuses on MySQL, and exploring its implementation on leading cloud platforms: Azure and GCP. By the end, you'll gain hands-on experience in setting up MySQL on these platforms, using MySQL Workbench to design, manage, and interact with your databases, and optionally connecting to your database using Python to retrieve data.

### **Instructions**:

#### **1. MySQL Setup on Azure and GCP**:
- Spin up an instance of MySQL on GCP and Azure. Be sure to use the lowest cost options. We only went through GCP in class, but Azure is similar. My instructions for Azure are on Lecture Slide 63 of wk4_managed.pptx, but here is the information also.
  - Azure: 
    - Azure Database for MySQL
    - Deployment option: Flexible, 
    - Tier: Burstable
    - Compute: B1S [$6.21 p/month] or B1MS [$12.41 p/month]
  - GCP 
    - DB-standard-1 (vCPU 1, RAM 3.75gb)
    - 10gb storage
    - No backups [$9.37 month]
- Make sure that the **instance is properly configued to allow inbound traffic** from the world wide web. In GCP, we needed to configure a network that allowed any ip (`0.0.0.0/0`) - there is a similar configuration option that you need to find in Azure. 

#### **2. MySQL Workbench for Database Interaction**:
- Install and configure MySQL Workbench on your local machine.
- Connect MySQL Workbench to the MySQL instances you set up in step 1.
- Based off my provided SQL scripts in the WK4/code folder, generate an Entity-Relationship Diagram (ERD) for your databases using MySQL Workbench. You must create at least one new table, with at least 3 fields, that contains a primary key and a foreign key that connects it to another table. You can create the same data in both instances on Azure and GCP. 

#### **3. OPTIONAL BUT HIGHLY RECOMMENDED: Python Script for Database Interaction**:
- Write a Python script that connects to your MySQL database. Please use my script as a example in WK4/code/python_connectionExample.py 
- Then using pandas, create some fake dumy data for each of the tables in your databases. Push the data into them. 
- Then retrieve data from one of the tables and store it in a Pandas DataFrame.
- Document the process, including the libraries used, the connection method, and any challenges faced.
- If you do this step, be sure to use a .ENV file to store your connection keys, and .gitignore to avoid sharing the .ENV file with github

#### **4. Documenting Code Errors**:
- If you encounter any errors or challenges in any step, it's vital to document them meticulously.
  - Take screenshots or create screencasts/recordings showcasing the issue.
  - Describe the steps leading up to the error and any troubleshooting or modifications you attempted.
  - Elucidate what you believe to be the root cause, even if you're uncertain. This helps in understanding your approach.
  - Remember that facing and resolving errors is a significant part of the learning journey. Thorough documentation is key.

### **Submission**:
- Create a new GitHub repository named `mysql_cloudmanaged_databases` in your GitHub account.
- Populate your repository with:
  - Your MySQL setup documentation for both Azure and GCP.
  - A copy of your ERD generated with MySQL Workbench.
  - SQL commands executed and their outcomes.
  - **A screen shot showing a successlly run command against your database in MySQL Work Bench**
  - Optionally, your Python scripts and a Jupyter notebook detailing your database interaction.
  - Screenshots, videos, or any other visual aids showcasing your application, especially any documented errors.
  - A comprehensive README.md file, detailing:
    - The setup process on both Azure and GCP.
    - Your experience with MySQL Workbench, including the ERD creation and database interactions.
    - Optionally, your Python to database interaction approach and findings.
    - Documented code errors and your troubleshooting attempts.
    - Any other important documentation or guidelines.
- Submit the link to your repository as your assignment submission.
- Ensure your repository is public for accessibility during review.

**Tip**:  **BE SURE TO USE .ENV FILE IN YOUR REPO AND .GITIGNORE TO AVOID SHARING YOUR SECURITY KEYS IF YOU DO THE OPTIONAL PART OF THIS ASSIGNMENT**. Prioritize security! Properly secure your MySQL instances and never expose sensitive credentials or data.
