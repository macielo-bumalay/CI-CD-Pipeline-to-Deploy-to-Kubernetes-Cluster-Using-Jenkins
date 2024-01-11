# Building a CI/CD Pipeline for a Java Based Application
Welcome! this project is designed to construct a CI/CD pipeline for a Java Based Application "Pet Clinic". To achieve this, we'll leverage the capabilities of modern DevOps tools such as AWS, Jenkins, Docker, Trivy, Git, Github, Sonarqube, Apache Maven, and Sonarqube. Let's embark on this learning journey together!

![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/banner.png?raw=true) <br>


  Step 1. Set up an EC2 instance and Install Jenkins

    Sign in to AWS Console: Log in to your AWS Management Console.
    Navigate to EC2 Dashboard: Go to the EC2 Dashboard by selecting “Services” in the top menu and then choosing “EC2” under the Compute section.
    Launch Instance: Click on the “Launch Instance” button to start the instance creation process.
    Choose an Amazon Machine Image (AMI): Selecta machine with the Free tier eligible tag. For example, you can choose Ubuntu.
    Choose an Instance Type: In the “Choose Instance Type” step, select t2.large as your instance type. Proceed by clicking “Next: Configure Instance Details.”
        For “Storage,”  set the size to 29GB.
    Configure Security Group:
        Choose an existing security group or create a new one.
        Ensure the security group has the necessary inbound/outbound rules to allow access as required.
    Review and Launch: Review the configuration details. Ensure everything is set as desired.
    Select Key Pair:
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
    
 Step 2. Access the instance remotely <br>
        Install MobaXterm to access the EC2 Instance remotely
  
 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/moba.png?raw=true) <br>
     
 Step 3. Access the EC2 Instance: 

     <Ec2-ip:8080>

 Step 4. Provide the below command for the Administrator password

      sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/macielo-bumalay-patch-1/img/4.png?raw=true) 
  
Step 5. `Install suggested plugins` ----> `Create a user Account` ----> `Click on save and continue`

  ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/macielo-bumalay-patch-1/img/7.png?raw=true)
  

Step 6. Install Docker

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

      
Create a Sonarqube container

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P2.png?raw=true) 

 
     docker run -d --name sonar -p 9000:9000 sonarqube:lts-community

 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P3.png?raw=true) 

   
Step 7. Install Trivy

Create a script  `vi trivy.sh`
 
    sudo apt-get install wget apt-transport-https gnupg lsb-release -y
    wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
    echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.lis 
    sudo apt-get update
    sudo apt-get install trivy -y

Change file permission and run it

      chmod 700 trivy.sh
      ./trivy.sh


Step 8. Install Plugins

Goto `Manage Jenkins` --> `Plugins` --> `Available Plugins`

Install below plugins


1 → Eclipse Temurin Installer

2 → SonarQube Scanner 

Step 9. Configure Java & Maven in Global Tool Configuration


Go to `Manage Jenkins` --> `Tools` --> `Install JDK and Maven3` --> `Click on Apply and Save`

Step 10. Create a Job

You can name it as `Real-World CI-CD`, click on Pipeline and proceed.


 ![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P4.png?raw=true) 

Step 10. In the Configuration 

`Pipeline` --> `Pipeline Syntax` --> `Steps` --> `Checkout from Version Control`
https://github.com/Aj7Ay/amazon-eks-jenkins-terraform-aj7.git
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
  
  


![alt text](https://github.com/macielo-bumalay/DevOps-Project-1/blob/main/img/P5.png?raw=true) 


Configure Sonar Server

 Click on `Administration` --> `Security` --> `Users` --> Click on `Tokens and Update Token`  --> Give it a name  --> and click on `Generate Token`
 Click on Update Token



 Dashboard --> Real-World CI-CD --> Pipeline Syntax --> Steps






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
            stage("Quality Gate"){
               steps{
                   script{waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                   }  
               }
                
            }
     }
}




Dashboard --> Manage Jenkins --> Tools DP

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
                      stage("Quality Gate"){
                         steps{
                             script{waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                             }  
                         }
                          
                      }
                      stage("OWASP Dependency Check"){
                          steps{
                               dependencyCheck additionalArguments: '--scan ./ --format HTML ', odcInstallation: 'DP-Check'
                               dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
                          }
                      }
                      stage("Build war file"){
                          steps{
                              sh "mvn clean install package"
                          }
                      }
                  }
          
               }


OWASP DP 


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









