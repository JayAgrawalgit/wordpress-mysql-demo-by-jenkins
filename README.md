# WordPress MySQL deployed by Jenkins
Hello, I am using Ubuntu 20.04.3 LTS For my base machine.  

## Architecture
* Step 1: Install Jenkins in minikube
* Step 2: Write helm chart for WordPress and MySQL
* Step 3: Deploy WordPress and MySQL charts in minikube through Jenkins Pipeline.

## Prerequisites
* Kubernetes Install on our local machine with minikube in the form of VM
* Helm install

## Process
We are doing this project in a different namespace called `demo`.
```bash
kubectl create namespace demo # for all steps we are using the same namespace
``` 

### Step 1: Install Jenkins in minikube Jenkins 
1. You can refer to Jenkins's official documentation for installation. [Link](https://github.com/jenkinsci/helm-charts/blob/main/charts/jenkins/README.md)

### Step 2: Writing helm chart 

### Step 3: Create a job in Jenkins

#### For running MySQL and WordPress in a single pipeline

1. First go to the new item and create a new pipeline job with any name. (recommended name `demo`)  
2. Then go in the last in the pipeline section there in Definition select `Pipeline script for SCM`.  
3. In SCM select `git` and in `Repository URL` pest *`https://github.com/JayAgrawalgit/wordpress-mysql-deploy-by-jenkins.git`* or HTTPS link of this repo which is present in the top green code button.  
4. In `Branches to build` enter `*/main`.  
5. In `Script Path` enter `Jenkinsfile`.  
6. `Apply` and `Save`.  
7. Now Build the Project

#### For running MySQL and WordPress in 2 different pipelines.  
1. First go to the new item and create a new pipeline job with any name. (recommended name `MySQL-pipeline`)  
2. Then go in the last in the pipeline section there in Definition select `Pipeline script for SCM`.  
3. In SCM select `git` and in `Repository URL` pest *`https://github.com/JayAgrawalgit/wordpress-mysql-deploy-by-jenkins.git`* or HTTPS link of this repo which is present in the top green code button.  
4. In `Branches to build` enter `*/main`.  
5. In `Script Path` enter `Jenkinsfile1` for MySQL.  
6. `Apply` and `Save`.  
7. go again to the new item on the dashboard and create a new pipeline job with any name. (recommended name `WordPress-pipeline`)  
8. go in the last in the pipeline section there in Definition select `Pipeline script for SCM`.  
9. In SCM select `git` and in `Repository URL` pest *`https://github.com/JayAgrawalgit/wordpress-mysql-deploy-by-jenkins.git`* or HTTPS link of this repo which is present in the top green code button.  
10. In `Branches to build` enter `*/main`.  
11. In `Script Path` enter `Jenkinsfile2` for MySQL.  
12. `Apply` and `Save`.  
13. Now Build the Project name `MySQL-pipeline` and if It will run `SUCCESS` then build the project with the name `WordPress-pipeline`.

### If you encountered the below error in running the pipeline Then: 
1. `failed to query with labels: secrets are forbidden: User "system:serviceaccount:demo:default" cannot list resource "secrets" in API group "" in the namespace "demo"`  
then run 
> Given command is `Stricktly Not recommended in production environment`, This is only for practice purpose.  
```bash
kubectl create clusterrolebinding permissive-binding --clusterrole=cluster-admin --user=admin --user=kubelet --group=system:serviceaccounts:demo
```
where,  
`permissive-binding` is the name of `clusterrolebinding`.  
you can use any name over here.

#### ThankYou
