
# K8 3 Node Cluster
This Project involves 3 Assignments as listed below:
1.	Test #1: Write a bash script to Create Kuberenetes cluster using kubeadm
2.	Test #2: Kubernetes skill assessment
3.	Test #3: Create Jenkins pipeline to build sample Docker image


# Test1: Write a bash script to Create Kuberenetes cluster using kubeadm

#Step1: Create 3 EC2 Instances of type t2.medium (1) for Master and t2.micro (2)

#Step2: Create K8_master.sh, k8_worker1.sh and k8_worker2.sh files in master and worker servers respectively and add the contents present in the shared files. Also give execute rights to the files.

#Step3: Execute the below commands to start installing Kube-adm in all the servers:

Master
./K8_master.sh

worker
./k8_worker1.sh
./k8_worker2.sh

#Step4: Join the cluster: Copy the output from master node and execute it as sudo in the worker nodes

#Step5: on successful installation execute the below command to verify the cluster on master node

kubectl get nodes

# Test 2: Kubernetes Skill Assessment

##Step1: Create a directory named Jenkins and create two files named jenkinsdeployment.yaml and Jenkinsservice.yaml and add the contents present in the shared files

##Step2: Run the below command to create a namespce

kubectl create namespace jenkins

##Step3: Run the below command to verify the namespace

kubectl get namespaces

#Step4: Execute the below commands to create the deployment as well the service of type ClusterIP and verify the same

kubectl create -f jenkinsdeployment.yaml -n jenkins

kubectl get deployments -n jenkins

kubectl create -f Jenkinsservice.yaml -n jenkins

kubectl get services -n jenkins

#Step5: Run the below command to port forward and access from local host

kubectl get pods -n jenkins

kubectl -n jenkins port-forward <pod-name> 8080:8080

#Step6: Now edit the service file and change the service type

vi Jenkinsservice.yaml

now change the service type to NodePort 

kubectl create -f Jenkinsservice.yaml -n jenkins

#Step6: Now verify the changed service type

kubectl get services -n jenkins

#Step7: Now access the jenkins from local using the public ip of EC2 and the port shown (30540)
Note: The port range is between 30000 to 32768 (If not specified in the service file it will assign a random port)

#Step8: Open jenkins and use the admin password which can be seen using the below command

kubectl logs <pod-name> -n jenkins

# Test #3: Create Jenkins pipeline to build sample docker image

#Step1: Create a dockerhub and github account

create a public repo in github

#Step2: create a folder and copy all the files inside the folder

#Step3: Install git for windows in your machine

#Step4: Opne the cmd from the folder where all the files exists and run the below commands

echo "# mytestrepo" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/akm-github/mytestrepo.git
git push -u origin main

Note: mytestrepo is a sample name provided

#Step5: Once done use the below commands to add all the files and commit

git add .

git commit -m "Adding all the files"

git status

git push

#Step6: Installing Jenkins on Ubuntu

Note: Before installing jenkins on EC2 ubuntu run the below pre-req commands

sudo apt-get update
sudo apt install openjdk-8-jdk -y

#Step7: Run the below commands to install Jenkins

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

sudo usermod -a -G docker jenkins

#Step8: Once installed Open the Jenkins using the Publicip and port as 8080

#Step9: Get the admin pasword to Login with the credentials Install the suggested plugins

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Note: Provide the details and change the password

#Step10: Configure the groovy script as presnet in the shared file

Note: Make sure the below is configured before the running the build:

Install sshAgent plugin 
Add the credentials for the EC2 servers
Add the credentials for the docker hub Login
Add the credentials for the github repo (Not required if its public)

#Step7: Click on the build now button to satrt the build

#Step:7 Once successful access the same using the below command

http://EC2-Publicip:forwardedPort

or 

the load balancer URL
