# Building a CI/CD Pipeline for a Java Based Application
Welcome! this project is designed to construct a CI/CD pipeline for a Java Based Application "Pet Clinic". To achieve this, we'll leverage the capabilities of modern DevOps tools such as AWS, Jenkins, Docker, Trivy, Git, Github, Sonarqube, Apache Maven, and Sonarqube. Let's embark on this learning journey together!

<h3> Overview</h3>
  <ul>
    <li>[Section 1]: AWS EC2 Virtual Server</li>
    <li>[Section 2]: Jenkins Server </li>
    <li>[Section 3]: Apache Maven </li>
    <li>[Section 4]: Git and Github </li>
    <li>[Section 5]: Ansible </li>
    <li>[Section 6]: Docker </li>
    <li>[Section 7]: Kubernetes </li>
    <li>References</li>
  </ul>

<h3>[Section 1]: AWS EC2 Virtual Server</h3>
This section is dedicated to the creation of an AWS EC2 Virtual Server instance. As part of this project, we will be setting up three EC2 instances for Jenkins, Ansible, and Kubernetes. We can revisit this module in "[Section 2]: Jenkins", “[Section 5]: Ansible" and "[Section 7]: Kubernetes" sections.


  Step 1. Set up an EC2 instance

    Sign in to AWS Console: Log in to your AWS Management Console.
    Navigate to EC2 Dashboard: Go to the EC2 Dashboard by selecting “Services” in the top menu and then choosing “EC2” under the Compute section.
    Launch Instance: Click on the “Launch Instance” button to start the instance creation process.
    Choose an Amazon Machine Image (AMI): Selecta machine with the Free tier eligible tag. For example, you can choose Amazon Linux.
    Choose an Instance Type: In the “Choose Instance Type” step, select t2.micro as your instance type. Proceed by clicking “Next: Configure Instance Details.”
        For “Storage,” click “Add New Volume” and set the size to 8GB.
    Configure Security Group:
        Choose an existing security group or create a new one.
        Ensure the security group has the necessary inbound/outbound rules to allow access as required.
    Review and Launch: Review the configuration details. Ensure everything is set as desired.
    Select Key Pair:
        Select “Create a key pair” and choose the key pair from the drop down.
        Acknowledge that you have access to the selected private key file.
        Click “Launch Instances” to create the instance.
    Access the EC2 Instance: Once the instance is launched, you can access it using the key pair and the instance’s public IP or DNS.
    
 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/EC2.png?raw=true) <br>
 
  Step 2. Access the instance remotely <br>
  Install MobaXterm to access the EC2 Instance remotely
  
 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/moba.png?raw=true) <br>
  
<h3>[Section 2]: Jenkins Server</h3> 
Jenkins is a free and open source automation server. It helps automate the parts of software development related to building, testing, and deploying, facilitating continuous integration and continuous delivery. 


Step 1. Install Jenkins
                            
    Update Package Repositories: sudo yum update -y
    Add Jenkins Repository: sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
      Import Jenkins Key: sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
      Install Java: sudo amazon-linux-extras install java-openjdk11 -y
      Install Jenkins: sudo yum install jenkins -y
      Install fontconfig: sudo yum install fontconfig java-11-openjdk -y
      Start Jenkins Service:  sudo systemctl start jenkins
                              sudo systemctl status jenkins
      Access Jenkins: Jenkins web interface is available at http://your_server_ip:8080. Open this URL in your web browser.
      Unlock Jenkins: sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  
Step 2. Rename the Hostname

      cd /etc/
      vi hostname

Step 3. Reboot the server

      init 6
 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/macielo-bumalay-patch-1/img/2.png?raw=true) <br>
  The hostname has been changed! <br>
     ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/macielo-bumalay-patch-1/img/3.png?raw=true) 

Step 4. Create a shell script to easily access the Jenkins

    #!/bin/bash
        java -jar /path/to/jenkins.war
        
Step 5. Provide executable permissions to run Jenkins

    chmod +x script.sh
    
Step 6. Let’s run the  script

    ./script.sh
    
Step 7. Now copy the public IP address of ec2 and paste it into the browser

    <Ec2-ip:8080>
  In this project I use port `8082` instead of `8080`
   ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/macielo-bumalay-patch-1/img/4.png?raw=true) 

Step 8. Provide the below command for the Administrator password

      sudo cat /var/lib/jenkins/secrets/initialAdminPassword
 
Step 9. `Install suggested plugins` ----> `Create a user Account` ----> `Click on save and continue`

  ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/macielo-bumalay-patch-1/img/7.png?raw=true) 

   
  Step 1. `` <br>
  Step 2. `` <br>
  Step 3. `` <br>
  Step 5. `` <br>
  Step 6. `` <br>
  Step 7. `` <br>
  Step 8. `` <br>

<h3>[Section 3]: Apache Maven</h3>
Apache Maven is a widely used build automation and project management tool in software development. It streamlines the process of compiling, testing, and packaging code by managing project dependencies and providing a consistent build lifecycle.


Step 1. Go to `/opt` directory 

    cd /opt/
    
Step 2. Use this command to download Apache Maven

    sudo wget https://dlcdn.apache.org/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz
To find the latest official Apache Maven release, use the link https://maven.apache.org/download.cgi.    


Step 3. Use this command, to extract Apache Maven from the downloaded archive

    sudo tar -xvzf apache-maven-3.9.4-bin.tar.gz
    
Step 4. Now copy the public IP address of ec2 and paste it into the browser

    <Ec2-ip:8080>
    
Step 4. Now copy the public IP address of ec2 and paste it into the browser

    <Ec2-ip:8080>

Step 4. Now copy the public IP address of ec2 and paste it into the browser

    <Ec2-ip:8080>

Step 4. Now copy the public IP address of ec2 and paste it into the browser

    <Ec2-ip:8080>

Step 4. Now copy the public IP address of ec2 and paste it into the browser

    <Ec2-ip:8080>

<h3>[Section 4]: Git and Github</h3>
Git is software for tracking changes in any set of files, usually used for coordinating work among programmers collaboratively developing source code during software development. 

<h3>[Section 5]: Ansible </h3>
Ansible is an open-source software provisioning, configuration management, and application-deployment tool enabling infrastructure as code. 

<h3>[Section 6]: Docker </h3> 
Docker is a set of platform as a service products that use OS-level virtualization to deliver software in packages called containers.

<h3>[Section 7]: Kubernetes </h3> 
Kubernetes is an open-source container-orchestration system for automating computer application deployment, scaling, and management.




<h3>References: </h3> 
https://hackernoon.com/building-a-cicd-pipeline-with-aws-k8s-docker-ansible-git-github-apache-maven-and-jenkins
<br>
https://youtu.be/5_s7EmZWz78?si=spLgduE-B2lGRfmU
