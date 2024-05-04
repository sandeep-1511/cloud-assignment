# cloud
STEP-1
In step-1 i have created EKS cluster using Cloud formation 
#### Pre-requisites: 
  - an EC2 Instance 

#### AWS EKS Setup 
1. Setup kubectl   
   a. Download kubectl
   b. Grant execution permissions to kubectl executable   
   c. Move kubectl onto /usr/local/bin   
   d. Test that your kubectl installation was successful    
   ```sh 
   curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
   chmod +x ./kubectl
   mv ./kubectl /usr/local/bin 
   kubectl version --short --client
   ```
 Setup eksctl   
   a. Download and extract the latest release   
   b. Move the extracted binary to /usr/local/bin   
   c. Test that your eksclt installation was successful   
   ```sh
   curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
   sudo mv /tmp/eksctl /usr/local/bin
   eksctl version
   ```
3. Create an IAM Role and attache it to EC2 instance    
   `Note: create IAM user with programmatic access if your bootstrap system is outside of AWS`   
   IAM user should have access to   
   IAM   
   EC2   
   VPC    
   CloudFormation
4. Setup AWs Cli
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

5. I have provided the cloud formation templates usem them we can create eks cluster
 we need enter into our directory where our cloud formation templates are there 
   ./create-eks-stack.sh
using above command we can start installing EKS cluster
6.Once the cluster is created  Validate your cluster using by creating by checking nodes .
   ```sh 
   kubectl get nodes
  aws eks update-kubeconfig --region region-code --name my-cluster
7.If we everything working properly now we need to create our hello world application.
  we need to get in application directory where our files are present and in order to build the application we need to create Dockerfile.
once Dockerfile is created we need to start building the docker image 
    docker build -t hello-world .
once we get docker image we must ensure that we need to test by creating docker container
       docker container run -dt -p 80:3000 hello-world
once the container is created check with that port number and ip address in browser we can access the application .If every thing goes well
8. Now we need to push our docker image to ecr repo . first we need to create repository in ECR 
     aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <your-account-number>.dkr.ecr.<your-region>.amazonaws.com

docker tag <local-image>:<tag> <your-account-number>.dkr.ecr.<your-region>.amazonaws.com/<repository-name>:<tag>

docker push <your-account-number>.dkr.ecr.<your-region>.amazonaws.com/<repository-name>:<tag>

By using above commands we can push our Docker image to Ecr repo
9. 
