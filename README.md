 
STEPS TO RUN THE STACK
===================

### Build ECR and Push Sample APP
  >- aws cloudformation create-stack --stack-name snt-ecr-repo --template-body file://ecr.yaml
### clone source code, build, tag  and push to ECR
   >- git clone https://github.com/capitalterefe/node-app.git
   >- $(aws ecr get-login --no-include-email --region us-east-1)
   >- docker build -t snt-nodejs .
   >- docker tag snt-nodejs:latest <accountid>.dkr.ecr.us-east-1.amazonaws.com/snt-nodejs:latest
   >- docker push <accountid>.dkr.ecr.us-east-1.amazonaws.com/snt-nodejs:latest
### Build VPC and Two Subnets in different AZ
  >- aws cloudformation create-stack --stack-name snt-vpc-subs  --template-body file://snt-vpc-subs.yml
### Build a load Balancer
 >- aws cloudformation create-stack --stack-name snt-loadBalancer-stack --template-body file://snt-loadbalancer.yml
### Build ECS Cluster
 >- aws cloudformation create-stack --stack-name snt-service  --template-body file://service.yaml  --capabilities CAPABILITY_NAMED_IAM --parameters ParameterKey=Listener,ParameterValue=[ListnerARN]
