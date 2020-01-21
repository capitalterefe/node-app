 1 #Build ECR and Push sample App 
  2 aws cloudformation create-stack --stack-name snt-ecr-repo --template-body file://ecr.yaml
  3 #clone source code, build, tag  and push to ECR
  4 git clone https://github.com/capitalterefe/node-app.git
  5 docker build -t node-app .
  6 docker tag node-app:latest <account_id>.dkr.ecr.<region>.amazonaws.com/node-app:latest
  7 docker push <account_id>.dkr.ecr.<region>.amazonaws.com/node-app:latest
  8 #Build VPC and Two Subnets in different AZ
  9 aws cloudformation create-stack --stack-name snt-vpc-subs  --template-body file://snt-vpc-subs.yml
 10 #Build a load Balancer
 11 aws cloudformation create-stack --stack-name snt-loadBalancer-stack --template-body file://snt-loadbalancer.yml
 12 #Build ECS Cluster
 13 aws cloudformation create-stack --stack-name snt-service  --template-body file://service.yaml  --capabilities CAPABILITY_    NAMED_IAM --parameters ParameterKey=Listener,ParameterValue=[ListnerARN]
~                                                                                   
