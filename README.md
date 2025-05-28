Jenkins CI/CD - Very Simple Step by Step
I'll break this down into the smallest possible steps:
PART 1: CREATE THE PROJECT
Step 1: Create Maven Project

Open Eclipse
File → New → Other
Type "Maven" in search
Select Maven Project → Next
Check "Create a simple project" → Next
Fill these details:

Group Id: com.example
Artifact Id: my-web-app
Packaging: war ← Very Important!
Name: My Web App


Click Finish

Step 2: Create HTML File

Right-click your project → New → File
Browse to: src/main/webapp
File name: index.html
Add this content:

html<!DOCTYPE html>
<html>
<head>
    <title>My Web App</title>
</head>
<body>
    <h1>Welcome to My Web Application!</h1>
    <p>This is working!</p>
</body>
</html>
Step 3: Create WEB-INF Folder

Right-click src/main/webapp → New → Folder
Folder name: WEB-INF

Step 4: Create web.xml File

Right-click WEB-INF folder → New → File
File name: web.xml
Add this content:

xml<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee" version="4.0">
    <display-name>My Web App</display-name>
</web-app>
Step 5: Fix pom.xml File

Open pom.xml
Make sure it looks like this:

xml<project xmlns="http://maven.apache.org/POM/4.0.0">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>my-web-app</artifactId>
    <version>1.0</version>
    <packaging>war</packaging>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.2.3</version>
                <configuration>
                    <webXml>src\main\webapp\WEB-INF\web.xml</webXml>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
Step 6: Test Build

Right-click project → Run As → Maven clean
Right-click project → Run As → Maven install
Check console - should show "BUILD SUCCESS"
Check target folder - should have my-web-app.war file


PART 2: PUT PROJECT ON GITHUB
Step 1: Create GitHub Account

Go to github.com
Click Sign up
Create account

Step 2: Create Repository

Click New repository (green button)
Repository name: my-web-app
Make it Public
Click Create repository
Copy the repository URL (like: https://github.com/username/my-web-app.git)

Step 3: Install Git on Computer

Download Git from git-scm.com
Install with default options
Open Command Prompt
Type: git --version (should show version number)

Step 4: Setup Git in Eclipse

Right-click your project → Team → Share Project
Select Git → Next
Click Create button
Click Finish

Step 5: Add Files to Git

Right-click project → Team → Add to Index
Right-click project → Team → Commit
Type message: "First commit"
Click Commit

Step 6: Push to GitHub

Right-click project → Team → Remote → Push
Click Create Remote
Remote name: origin
Paste your GitHub URL here
Click Next
Branch: master
Click Next → Finish
Enter your GitHub username
Enter your GitHub password (or personal access token)

Step 7: Check GitHub

Go to your GitHub repository page
Refresh page
You should see your project files!


PART 3: INSTALL AND SETUP JENKINS
Step 1: Download Jenkins

Go to jenkins.io
Click Download
Download Windows version
Install with default options

Step 2: Start Jenkins

Open Services (Windows key + R, type services.msc)
Find Jenkins service
Right-click → Start
Open browser → go to http://localhost:8080

Step 3: Setup Jenkins First Time

Find password in: C:\Users\[username]\.jenkins\secrets\initialAdminPassword
Copy password and paste in Jenkins
Click Install suggested plugins
Wait for installation
Create admin user (username, password, email)
Click Save and Continue

Step 4: Install Required Plugins

Manage Jenkins → Manage Plugins
Click Available tab
Search and install these (check the box):

Maven Integration
Deploy to container
Git
GitHub


Click Install without restart

Step 5: Configure Maven in Jenkins

Manage Jenkins → Global Tool Configuration
Find Maven section
Click Add Maven
Name: Maven-3.6
Check Install automatically
Choose latest version from dropdown
Click Save


PART 4: INSTALL AND SETUP TOMCAT
Step 1: Download Tomcat

Go to tomcat.apache.org
Download Tomcat 9 (Windows Service Installer)
Install with default options
Remember the port number (usually 8080)

Step 2: Start Tomcat

Find Tomcat in Windows Services
Right-click → Start
Test: go to http://localhost:8080 in browser
Should see Tomcat welcome page

Step 3: Setup Tomcat Users

Go to Tomcat installation folder (usually C:\Program Files\Apache Software Foundation\Tomcat 9.0)
Open conf folder
Edit tomcat-users.xml file
Add these lines before </tomcat-users>:

xml<user username="admin" password="admin" roles="manager-gui,manager-script"/>

Save file
Restart Tomcat service


PART 5: CREATE JENKINS JOB
Step 1: Create New Job

In Jenkins, click New Item
Job name: MyWebApp-Build
Select Freestyle project
Click OK

Step 2: Configure Source Code

In Source Code Management section:
Select Git
Repository URL: Paste your GitHub URL
Leave credentials empty (for public repo)

Step 3: Configure Build

In Build section:
Click Add build step
Select Invoke top-level Maven targets
Maven Version: Select Maven-3.6
Goals: Type clean install

Step 4: Configure Deployment

In Post-build Actions section:
Click Add post-build action
Select Deploy war/ear to a container
Fill these details:

WAR/EAR files: **/*.war
Context path: myapp
Container: Select Tomcat 9.x
Tomcat URL: http://localhost:8080
Credentials: Click Add → Add admin/admin



Step 5: Save Job

Click Save


PART 6: RUN THE PIPELINE
Step 1: Run Build

Click Build Now
Wait for build to start
Click on build number (like #1)
Click Console Output
Watch the process

Step 2: Check Results
Look for these messages in console:

BUILD SUCCESS (Maven built successfully)
Deploying war (War file deployed to Tomcat)
Finished: SUCCESS (Jenkins job completed)

Step 3: Test Your Application

Open browser
Go to: http://localhost:8080/myapp
You should see: "Welcome to My Web Application!"


TROUBLESHOOTING
If build fails:

Check Console Output for errors
Make sure GitHub URL is correct
Make sure Maven is configured
Make sure Tomcat is running

If deployment fails:

Check Tomcat credentials
Make sure Tomcat URL is correct
Make sure Tomcat is running on correct port

If application doesn't show:

Check if WAR file exists in Tomcat webapps folder
Check Tomcat logs for errors
Try restarting Tomcat

That's it! You now have a complete CI/CD pipeline that:

✅ Takes code from GitHub
✅ Builds it with Maven
✅ Deploys to Tomcat
✅ Makes it accessible via web browser

https://github.com/VarnitRaina/labprog2.git

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
