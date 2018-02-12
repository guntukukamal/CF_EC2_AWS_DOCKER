# CF_EC2_AWS_DOCKER
AppNode:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: t2.micro
      ImageId: ami-66506c1c
      KeyName: awscli_CF_practise
      SecurityGroups:
        - !Ref AppNodeSG



This is a demonstration of how to install EC2, Docker, Kubernetes, and Minikube on your AWS platform, using cloudformation, YAML file. For this repo to work you would need to first create an AWS account. In the management console, go to the IAM page, where you will need to create a user. Grant the user admin privilege full access. At the end of this process you will  be granted an access key ID and a secret Access key. Next, go to EC2, click launch instance (go to community AMI on your left pannel), "note the ami-ID you want to use, and for the region", this is important, cos AMI-ID varies by region, in this case our choice is for Ubuntu 16 for us-east-1.

 To deploy CloudFormation (CF) via terminal you will need to enable awscli. I prefer using the bundle because it works across most platforms.


Download this repo by running 

$ git clone https://github.com/CF_AWS

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




