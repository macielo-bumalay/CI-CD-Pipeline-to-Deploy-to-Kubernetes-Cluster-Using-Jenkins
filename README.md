# Building a CI/CD Pipeline with AWS, Jenkins, Docker, Ansible, Git, Github, Apache Maven, and Kubernetes
Welcome! this project is designed to construct a CI/CD pipeline from scratch. To achieve this, we'll leverage the capabilities of modern DevOps tools such as AWS, Jenkins, Docker, Ansible, Git, Github, Apache Maven, and Kubernetes. Let's embark on this learning journey together!

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


  Step 1. `Set up an EC2 instance` 

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
 
  Step 2. `Access the instance remotely` <br>
  Install MobaXterm 
  
 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/moba.png?raw=true) <br>
  
<h3>[Section 2]: Jenkins Server</h3> 
Jenkins is a free and open source automation server. It helps automate the parts of software development related to building, testing, and deploying, facilitating continuous integration and continuous delivery. 

Step 1. `Install Jenkins`
                            
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
  

 
  Step 3. `` <br>
  Step 4. `` <br>
  Step 5. `` <br>
  Step 6. `` <br>
  Step 7. `` <br>
  Step 8. `` <br>

<h3>[Section 3]: Apache Maven</h3>

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
