# cloud
#29NGe726DOooYShFJS8NeGqdHJWzf3Wm
#ATOACKTFwStOeF2C07TU7kS_6EJXXfMYDFYTiMOIE0KEdZ0sH4z7iocWq-kflC7ztGnU6BCCC6BC
https://sandeepkonamars-1721303752297.atlassian.net/
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

5. I have provided the CLOUDFORMATION templates usem them we can create eks cluster
 we need enter into our directory where our cloud formation templates are there 
              ./create-eks-stack.sh
using above command we can start installing EKS cluster

6.Once the cluster is created  Validate your cluster using by creating by checking nodes .
   ```sh 
   kubectl get nodes
   aws eks update-kubeconfig --region region-code --name my-cluster

7.If we everything working properly now we need to create our hello world application.
  we need to get in application directory where our files are present and in order to build the application we need to create Dockerfile.
install docker by using below commands

       sudo apt update: sudp apt upgrade -y : sudo apt install docker.io
once Dockerfile is created we need to start building the docker image 

        docker build -t hello-world .
once we get docker image we must ensure that we need to test by creating docker container

        docker container run -dt -p 80:3000 hello-world
once the container is created check with that port number and ip address in browser we can access the application .If every thing goes well.

8. Now we need to push our docker image to ecr repo .first we need to create repository in ECR 

           aws ecr get-login-password --region <your-region> | docker login --username AWS --password-stdin <your-account- 
           number>.dkr.ecr.<your-region>.amazonaws.com

           docker tag hello-world:latest public.ecr.aws/n2y5o7l2/hello-world:latest

       docker push <your-account-number>.dkr.ecr.<your-region>.amazonaws.com/<repository-name>:<tag>

 By using above commands we can push our Docker image to Ecr repo

9. Now we need to  install metrics By using below command

                kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml

10.  Run below command to check metrics is installed or not 
              Kubectl top nodes

11.   Now we need to create Deployment and service files in order to deploy our application in Kubernetes
              Kubectl apply -f Deployment.yml
              kubectl apply -f service.yml

12.  once our application is deployed we can access it through our endpoint i.e loadbalancer, nodeport
   
13. Now we need to create  HorizontalPodAutoscaler using manifest file and configure it with application . so that when load consumption increses more instances will be created.

By using below command we can get to clear weather HPA as created and configured correctly

        kubectl get all/ kubectl get hpa

14. So in order to test load on our application we need to run this command this will stress our application by giving number of requests once consumption increses our target new 

              kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while true; do wget -q 
               -O /dev/null http://hello-service & sleep 0.0001; done"

15. same thing applies for the second application Too. 
    once our application is deployed we can access it through our endpoint i.e loadbalancer, nodeport
    In order to isolate one application with other we use namespaces.

16. Now we need to create  HorizontalPodAutoscaler using manifest file and configure it with application . so that when load consumption increses more instances will be created.

17. By using below command we can get to clear weather HPA as created and configured correctly

     kubectl get all/ kubectl get hpa

18. So in order to test load on our application we need to run this command this will stress our application by giving number of requests once consumption increses our target new 

           kubectl run -i --tty load-generator --rm --image=busybox --restart=Never -- /bin/sh -c "while true; do wget -q 
            -O /dev/null http://hello-service-2 & sleep 0.0001; done"

19. To delete this cluster through CLOUDFORMATION we need run below command 

              ./delete-eks-stack.sh
20. After successful deleting cluster check if any resources pending in order to reduce the cost .
21.  I have done entire thing through CLOUDFORMATION but i also provided terraform file to create eks cluster optionally in order to do it muliple clouds .
22. I have shared scren shots of application hosting and Load testing for hello woprld application how replicas are created if cpu consumption reaches 50%. please have a look at those screenshots
