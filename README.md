#  End-to-End Automated CI/CD Pipeline with GitOps

**Designed and implemented a robust, enterprise-grade CI/CD pipeline to demonstrate mastery of modern DevOps tooling and principles. The pipeline automates the entire software delivery process, integrating best practices for continuous integration, security, and continuous deployment via GitOps.**

<img width="1324" height="661" alt="Screenshot 2025-09-13 at 9 13 35 PM" src="https://github.com/user-attachments/assets/9dc34dda-9008-4c71-8db1-9c6a0eb8c1d1" />

# Step-by-Step Explanation of the Pipeline:

The flow generally moves from left to right, triggered by a developer pushing new code.

 1. *Source Code (The Starting Point):*
     - A developer pushes a code change to a Source Code Repository (like GitHub, GitLab, or Bitbucket).
     - This act is the trigger that starts the entire automated pipeline.
       
 2. *CI (Continuous Integration) Phase - Building & Testing:*
     - *Webhook:* The source code repository sends a notification (a webhook) to Jenkins, informing it that new code is available.
     - *Jenkins (CI Server)*: Jenkins automatically pulls the new code and begins orchestrating the CI process:
        - *Maven*: It uses Maven (a build automation tool for Java) to compile the source code into executable artifacts (like a JAR file) and manage dependencies.
        - *Tests*: It runs the automated test suite (unit tests, integration tests) to verify the code works as expected.
        - *SonarQube*: It sends the code to SonarQube for static code analysis. SonarQube checks code quality, identifies bugs, vulnerabilities, and "code smells," and generates a report.
     - *Exit Report*: This likely represents the final artifact from the CI phase—a successfully built and tested application package (e.g., a Docker image) and a quality report from SonarQube. If any step here fails, the pipeline stops.

 3. *New Image Pushed to DockerHub (Packaging):*
     - Once the code passes all tests and checks, Jenkins (or another tool) packages the application and its environment into a *Docker container image.*
     - This new image is then pushed to a container registry, in this case, *DockerHub.* This makes the new, versioned image available for deployment.

 4. *CD (Continuous Deployment) Phase - Deploying:*
     - *Updates with new image / Image Updater*: This is a crucial modern CD step. A tool (like *Argo CD Image Updater*) detects that a new image has been pushed to DockerHub.
     - *Manifests Repo*: The Image Updater automatically updates the Kubernetes configuration files (YAML manifests) in a *Manifests Repository*. It changes the image tag to point to the new version you just pushed. This declares the desired state of the application in the cluster.

 5. *GitOps Deployment with Argo CD:*
     - *Argo CD* is a *GitOps* tool. It continuously monitors the *Manifests Repository* and compares it to the actual state of the application running in the Kubernetes cluster.
     - It detects that the manifests have been updated (with the new image tag) and automatically synchronizes the cluster, deploying the new version of the application. This is a declarative and automated deployment.


# Implementation Step

 *1. Jenkins Server Setup*
 
   - We have to configure Jenkins, For that We are going to create an EC2 server where Jenkins will be installed. Also, We are going to install some other tools as well such as Sonarqube and Docker so, we need more RAM and CPU. 
   - Configurations: t2.large, Ubuntu22.04, Security Group Port Open- 22, 9000, 8080, 8081, and 8082, Storage: 30GB gp2.

  <img width="1232" height="760" alt="Screenshot 2025-09-13 at 3 03 47 PM" src="https://github.com/user-attachments/assets/ef3295c3-6355-4820-97b8-0ae90602aa3a" />

  <img width="1266" height="522" alt="Screenshot 2025-09-13 at 3 04 30 PM" src="https://github.com/user-attachments/assets/54f626cf-f696-4afb-9343-f5408fbd9eb6" />

  <img width="1227" height="292" alt="Screenshot 2025-09-13 at 3 04 54 PM" src="https://github.com/user-attachments/assets/643e3be2-1cef-4db0-982f-55de9522f924" />



 *2. After creating EC2 Instance, log in to the created machine using ssh.*

   - ssh -i <key-pair> ubuntu@instanceipaddress

   <img width="849" height="140" alt="Screenshot 2025-09-13 at 3 07 01 PM" src="https://github.com/user-attachments/assets/be59b956-f0bc-4cb6-8276-73e6f9998d73" />


 *3. Install Jenkins. Follow the command to install it on your EC2 Instance.*

   *Installing Java* 
   - sudo apt update -y
   - sudo apt install openjdk-17-jre -y
   - java --version

   <img width="819" height="92" alt="Screenshot 2025-09-13 at 3 09 05 PM" src="https://github.com/user-attachments/assets/46e3babe-3aa2-4d78-9e21-a912cb7d0635" />


   *Installing Jenkins*
   - curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

   - sudo apt-get update -y
   - sudo apt-get install jenkins -y

   You can see below Jenkins has been installed.
   
   <img width="1116" height="218" alt="Screenshot 2025-09-13 at 3 10 17 PM" src="https://github.com/user-attachments/assets/e6e7398e-b689-4a83-8700-750018d91d38" />



# Key Responsibilities & Achievements:
 - Architected the pipeline orchestrated by Jenkins, triggered automatically via webhooks upon code commit.
 - Implemented Continuous Integration (CI) using Maven for building and dependency management, followed by comprehensive automated testing.
 - Integrated SonarQube for static code analysis to enforce code quality gates and ensure security vulnerability checks.
 - Automated containerization by building Docker images and pushing versioned images to DockerHub.
 - Implemented GitOps methodology using Argo CD for Continuous Deployment (CD). Argo CD automatically synchronizes and deploys new application versions to a Kubernetes cluster by detecting changes in the declarative manifests repository.
 - The result is a fully automated, self-service pipeline that reduces manual effort, minimizes deployment errors, and provides a clear, auditable trail from code.
