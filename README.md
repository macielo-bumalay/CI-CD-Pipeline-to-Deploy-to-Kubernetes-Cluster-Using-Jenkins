# Building a CI/CD Pipeline for a Java Based Application
Welcome! this project is designed to construct a CI/CD pipeline for a Java Based Application "Pet Clinic". To achieve this, we'll leverage the capabilities of modern DevOps tools. Let's embark on this learning journey together!

![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/banner.png?raw=true) 
                                                                                                     
<h3>[Section 1]: Set up an EC2 instance and Install Jenkins </h3>
                                                                                              

  `Sign in to AWS Console:` Log in to your AWS Management Console.

 `Navigate to EC2 Dashboard:` Go to the EC2 Dashboard by selecting “Services” in the top menu and then choosing “EC2” under the Compute section.
  
 `Launch Instance:` Click on the “Launch Instance” button to start the instance creation process.
    
 `Choose an Amazon Machine Image (AMI):` Select a machine for example, you can choose Ubuntu.
   
 `Choose an Instance Type:` Select t2.large as your instance type. Proceed by clicking “Next: Configure Instance Details.”
  
 `For Storage:` I set the size to 29GB.
 
 `Configure Security Group:` Choose an existing security group or create a new one. Ensure the security group has the necessary inbound/outbound rules to allow access as required.
 
 `Review and Launch:` Review the configuration details. Ensure everything is set as desired.
 
 `Select Key Pair:`
        Select “Create a key pair” and choose the key pair from the drop down.
        Acknowledge that you have access to the selected private key file.



 In the "Advance details" ----> "User data" section enter this script to automatically install Jenkins
       
        #!/bin/bash
          sudo apt update -y
          sudo touch /etc/apt/keyrings/adoptium.asc
          sudo wget -O /etc/apt/keyrings/adoptium.asc https://packages.adoptium.net/artifactory/api/gpg/key/public
          echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
          sudo apt update -y
          sudo apt install temurin-17-jdk -y
          /usr/bin/java --version
          curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
                            /usr/share/keyrings/jenkins-keyring.asc > /dev/null
          echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
                            https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
                                        /etc/apt/sources.list.d/jenkins.list > /dev/null
          sudo apt-get update -y
          sudo apt-get install jenkins -y
          sudo systemctl start jenkins
          sudo systemctl status jenkins
          
Click “Launch Instances” to create the instance.


![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P1.png?raw=true) <br>

  
 `Access the instance remotely:`  Install MobaXterm to access the EC2 Instance remotely
  
 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/moba.png?raw=true) <br>
     
  `Access the EC2 Instance: ` Enter Instance's IP address on your browser

     <Ec2-ip>

<h3>[Section 2]: Create an Executable Scripts </h3>

  <h4>Step 1. Install Docker</h4>
  
  Create a script  `vi docker.sh`
  
        sudo apt-get update
        sudo apt-get install docker.io -y
        sudo usermod -aG docker ubuntu
        newgrp docker
        sudo chmod 777 /var/run/docker.sock 
        docker ps

  Change file permission and run it
  
        chmod 700 docker.sh
        ./docker.sh
        sudo systemctl enable docker
        docker ps
  
        
  <h4>Step 2. Create a Sonarqube container</h4>

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P2.png?raw=true) 

 
     docker run -d --name sonar -p 9000:9000 sonarqube:lts-community

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P3.png?raw=true) 

   
  <h4>Step 3. Install Trivy</h4>
  
  Create a script  `vi trivy.sh`
   
      sudo apt-get install wget apt-transport-https gnupg lsb-release -y
      wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
      echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.lis 
      sudo apt-get update
      sudo apt-get install trivy -y

  Change file permission and run it
  
        chmod 700 trivy.sh
        ./trivy.sh

<h4>Step 4. Setup Jenkins and Install Plugins</h4>

  `Access the Jenkins Server: ` Enter Instance's IP address and 8080 as port

     <Ec2-ip:8080>
 
 But first we need to unlock it by providing an Admin Password. 


 Go back to your terminal and enter this command `cat /var/lib/jenkins/secrets/initialAdminPassword`
     
 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-2/blob/main/img/a12.png?raw=true) 

 Now, click on `Install suggested Plugins`

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-2/blob/main/img/a14.png?raw=true) 

  Wait for it to complete.
 
   ![alt text](https://github.com/macielo-bumalay/DevOps-Project-2/blob/main/img/a15.png?raw=true) 

  Create an Admin user account 
 
   ![alt text](https://github.com/macielo-bumalay/DevOps-Project-2/blob/main/img/a16.png?raw=true) 


Your now at the homepage of Jenkins, Click on Manage Jenkins

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-2/blob/main/img/a17.png?raw=true) 

Install below plugins


1 → Eclipse Temurin Installer

2 → SonarQube Scanner 


Configure Java & Maven in Global Tool Configuration:   Go to `Manage Jenkins` --> `Tools` --> `Install JDK and Maven3` --> `Click on Apply and Save`

<h3> Create a Job </h3>

You can name it as `Real-World CI-CD`, click on Pipeline and proceed.


 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P4.png?raw=true) 

<h3>[Section 3]:  Create a Pipeline Scripts </h3>
 <h3> </h3>

Go to `Pipeline` --> `Pipeline Syntax` --> `Steps` --> `Checkout from Version Control` https://github.com/Aj7Ay/amazon-eks-jenkins-terraform-aj7.git
--> `Generate Pipeline Script`

Enter this in Pipeline Script

          pipeline {
              agent any 
              tools{
                  jdk 'jdk11'
                  maven 'maven3'
              }
               stages{
                       stage ('checkout scm'){
                          steps {checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Aj7Ay/amazon-eks-jenkins-terraform-aj7.git']])
                          }
                        }
                        stage("Compile"){
                            steps{
                                sh "mvn clean compile"
                            }
                        }
                 }
              }
  
  
You will see the stage view like this

![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P5.png?raw=true) 

 <h3> Configure Sonar Server </h3>

   `Access the Sonar Server: ` Enter Instance's IP address and 9000 as port

     <Ec2-ip:9000>

 Click on `Administration` --> `Security` --> `Users` --> Click on `Tokens and Update Token`  --> Give it a name  --> and click on `Generate Token`
 Click on Update Token

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P6.png?raw=true) 

 <h4>Here what it looks like</h4>

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P7.png?raw=true) 

  We will configure different tools that we install using Plugins in >>>  `Global Tool Configuration`

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P8.png?raw=true) 

  <h5>Install sonar-scanner in tools</h5>

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P10.png?raw=true) 

 <h4> Go to Dashboard --> Real-World CI-CD --> Pipeline Syntax --> Steps</h4>

  ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P11.png?raw=true) 


 <h4>Lets goto our Pipeline and add Sonar-qube Stage in our Pipeline Script</h4>



        pipeline {
            agent any 
            tools{
                jdk 'jdk11'
                maven 'maven3'
            }
             stages{
                     stage ('checkout scm'){
                        steps {checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Aj7Ay/amazon-eks-jenkins-terraform-aj7.git']])
                        }
            }
            stage("Compile"){
                steps{
                    sh "mvn clean compile"
                }
            }
            stage("Sonarqube Analysis"){
               steps{
                   script{withSonarQubeEnv(credentialsId: 'Sonar-token') {
                       sh "mvn sonar:sonar"
                      }
                   }
               }    
            }
            
            }
     }
}

<h4>To see the report, you can goto Sonarqube Server and goto >>> Projects.  Here is what it looks like </h4>

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P12.png?raw=true) 

<h3>[Section 4]: Install OWASP Dependency Check Plugins</h3>


Goto Dashboard >>> Manage Jenkins >>> Plugins >>> OWASP Dependency-Check. Click on it and install without restart.

Next is to configure it in the Configuration Tool.  Click on Apply and Save

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P13.png?raw=true) 

Add this stage to the Pipeline


     stage("Build"){
              steps{
                  sh " mvn clean install"
              }
          }
        
          stage("OWASP Dependency Check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./ ' , odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }


<h4> Here's how the Pipeline looks like </h4>


    pipeline {
    agent any 
    
    tools{
        jdk 'jdk11'
        maven 'maven3'
    }
    
    stages{
        
        stage ('checkout scm'){
                        steps {checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Aj7Ay/amazon-eks-jenkins-terraform-aj7.git']])
               }
            }
        
        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }
        
         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                script {
                withSonarQubeEnv(credentialsId: 'Sonar-token') {
                sh 'mvn sonar:sonar'
                }
              }
            }
        }
      
        stage("Build"){
            steps{
                sh " mvn clean install"
            }
        }
        
          stage("OWASP Dependency Check"){
            steps{
                dependencyCheck additionalArguments: '--scan ./ ' , odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
    }
}



<h4>The stage view would look like this</h4>

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P14.png?raw=true) 

 <h4>A graph will also be generated in the status</h4>

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P15.png?raw=true) 



 <h3>[Section 5]:  Docker Image Build and Push</h3>

 Goto Dashboard >>> Manage Plugins >>> Available plugins >>> Search for Docker and install these plugins
  
 
 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P16.png?raw=true) 

 Now configure it in the Configuration Tool.  Click on Apply and Save

   ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P19.png?raw=true)


  Add your DockerHub Username and Password under Global Credentials

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P18.png?raw=true) 

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P17.png?raw=true) 

  Add this stage in Pipeline Script

  
        stage("Build and Push to DockerHub"){
            steps{
                script{
                    withDockerRegistry(credentialsId: 'docker', toolName: "docker") {
                        sh "docker build -t petclinic1 ."
                        sh "docker tag petclinic1 macielo/pet-clinic123:latest "
                        sh "docker push macielo/pet-clinic123:latest "
                    }
                }

  Here is the stage view after the stage Build and Push to DockerHub has been added
  
  Now go to your CLI and enter this command

     docker images 

  This should appear in your screen.

  
 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P22.png?raw=true) 


 Log in to your Dockerhub account, you will see a new image is created

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P21.png?raw=true) 

 <h3>[Section 6]:  Deploy image using Docker</h3>

 
Add this stage to the Pipeline


       stage("Trivy"){
                steps{
                    sh "trivy image macielo/pet-clinic123:latest"
                }
            }
            stage("Deploy to Container"){
                steps{
                    sh " docker run -d --name pet1 -p 8082:8080 macielo/pet-clinic123:latest "
                }
            }
    
  You will see the Stage View like this

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P23.png?raw=true) 


 In your CLI and enter this command that will show a list of the currently running Docker containers on your system.

     docker ps 

  This should appear in your screen.

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P26.png?raw=true) 

  `Access the container: ` Enter Instance's IP address and 8082 as port

     <Ec2-ip:8082>


  Tada! here is your fully deployed Java Based Application
 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P24.png?raw=true) 

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P25.png?raw=true) 


 <h3>[Section 6]:  Terminate the AWS EC2 Instance</h3>
 
  Lastly, do not forget to terminate the AWS EC2 Instance.

 Goto EC2 Dashboard >>> Instances >>> Select the instance you want to terminate >>> Click instance state >>> STOP



Here is the complete Pipeline Script


      pipeline {
        agent any 
        
        tools{
            jdk 'jdk11'
            maven 'maven3'
        }
        
        stages{
            
            stage ('checkout scm'){
                            steps {checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Aj7Ay/amazon-eks-jenkins-terraform-aj7.git']])
                   }
                }
            
            stage("Compile"){
                steps{
                    sh "mvn clean compile"
                }
            }
            
             stage("Test Cases"){
                steps{
                    sh "mvn test"
                }
            }
            stage("Sonarqube Analysis "){
                steps{
                    script {
                    withSonarQubeEnv(credentialsId: 'Sonar-token') {
                    sh 'mvn sonar:sonar'
                    }
                  }
                }
            }
          
            stage("Build"){
                steps{
                    sh " mvn clean install"
                }
            }
            
            stage("OWASP Dependency Check"){
                steps{
                    dependencyCheck additionalArguments: '--scan ./ ' , odcInstallation: 'DP-Check'
                    dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                }
            }
            stage("Build and Push to DockerHub"){
                steps{
                    script{
                        withDockerRegistry(credentialsId: 'docker', toolName: "docker") {
                            sh "docker build -t petclinic1 ."
                            sh "docker tag petclinic1 macielo/pet-clinic123:latest "
                            sh "docker push macielo/pet-clinic123:latest "
                        }
                    }
                }
            }
            stage("Trivy"){
                steps{
                    sh "trivy image macielo/pet-clinic123:latest"
                }
            }
            stage("Deploy to Container"){
                steps{
                    sh " docker run -d --name pet1 -p 8082:8080 macielo/pet-clinic123:latest "
                }
            }
        }
    }


 

 Reference: https://www.youtube.com/watch?v=Rj9oQHC12c4


