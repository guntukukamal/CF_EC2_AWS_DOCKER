This is a demonstration of how to install EC2, Docker, Kubernetes, and Minikube on your AWS platform, using cloudformation, YAML file. For this repo to work you would need to first create an AWS account. In the management console, go to the IAM page, where you will need to create a user. Grant the user admin privilege full access. At the end of this process you will  be granted an access key ID and a secret Access key. Next, go to EC2, click launch instance (go to community AMI on your left pannel), "note the ami-ID you want to use, and for the region", this is important, cos AMI-ID varies by region, in this case our choice is for Ubuntu 16 for us-east-1.

 To deploy CloudFormation (CF) via terminal you will need to enable awscli. I prefer using the bundle because it works across most platforms.


Download this repo by running 

$ git clone https://github.com/CF_EC2_AWS_DOCKER.git

$ curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
$ unzip awscli-bundle.zip
This is assuming python minimum version 2.7 is installed on your system

$ sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws

after installing awscli you can test it by entering the access ID and key you got from the IAM, to setup your terminal give the profile a name for example "osetutorial" as shown below:

$ aws configure --profile osetutorial
AWS Access Key ID [None]: A............
AWS Secret Access Key [None]: A............................
chose the region thats you want to use, for example

Default region name [None]: us-east-1
Default output format [None]: 

the terminal is now setup to run your stack

$ cd EC2_DOCKER_Kernetes.....
then

you will need to download the ssh keypair from the ec2.
create one called awscli_CF_practise dowload and copy it to your present working directory, because you will need it to later ssh, then:
$ chmod 400 awscli_CF_practise.pem
$ aws cloudformation create-stack --stack-name practise-demo --template-body file://$PWD/ubuntucf.yaml --profile osepractise

running this code will deploy EC2 with a security group with access to ssh and http.

you can verify the run process on the AWS console, clicking on the cloudformation page, a stack named practise-demo would be delpoyed, you can follow the steps by clicking on the stack name on the console and then events.
When its finished, it will show you stack completed

you can verify this by ssh into the ec2 instance.

$ ssh -i "awscli_CF_practise.pem" ubuntu@ec2-52-34-45-106.compute-1.amazonaws.com

enter yes to the next prompt.
enter
$ docker ps
this shows docker has not been installed
$ which docker 
$ exit

you can check details of the stack entering the following 

$ aws cloudformation describe-stack-resources --stack-name practise-demo --profile osepractise

this will show you status of the stack in json format.

you can delete the stack by enetering the following 

$ aws cloudformation delete-stack --stack-name  practise-demo --profile osepractise

To install docker on EC2 use:
$ aws cloudformation create-stack --stack-name practise-demo --template-body file://$PWD/ubuntucf_docker.yaml --profile osepractise

you can confirm the installation with:
$ docker ps
this shows docker has not been installed
$ which docker 

while to completely install kubernetes to work in hand with docker 
run the line 

$ aws cloudformation create-stack --stack-name practise-demo --template-body file://$PWD/cf_stack.yaml --profile osepractise

this will install, git, virtualbox, on the ubuntu image, which enables you to easily install minikube, and kubernetes on the ec2 instance, 
It will later deploy a stateless application on the kubernetes cluster using php redis-master and redis slave for backend, and 
a guestbook for frontend. you can confirm this installation with: 

$ docker ps


in my case 


CONTAINER ID        IMAGE                                    COMMAND             CREATED              STATUS              PORTS               
NAMES

fbbf192f9530        kube-build:build-2dcc171db0-5-v1.9.3-2   "make cross"        About a minute ago   Up About a minute                       kube-build-2dcc171db0-5-v1.9.3-2

The issue with this deployment is 2 folds: the instance is not engineered propagate kubernetes clusters, and even if one deploy an auto-scaling script to this deployment, it wont function as ECS, furthermore, the size of the instance, as well as its default cpu cannot properlly handle the deployments. hence there is need of using the right compute platform to sclae and deploy kubernetes. 
please check out github.com/osaosemwen/cloudapplication. this is carried out first on Google Cloud platform, the next would be on AWS, then Azure...


with that you can test your application been deploy to cloud. Please never use this in production... this is only to show you one of the simple things you can do with cloudformation. 
To deploy some complex structure with AWS cloudformation please check github.com/osaosemwen/.... tobe continued.
