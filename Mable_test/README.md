##  EKS and RoR deployment:
-- deploy the new EKS cluster through Terraform

    cd terraform_eks
    terraform init
    terraform apply

-- destroy the created EKS cluster 

    cd terraform_eks
    terraform destory

-- build the RoR image in docker

  docker build -t mable-rail 
     --build-arg USER_ID=       
     --build-arg USER_ID=$(id -u)  \
     --build-arg GROUP_ID=$(id -g) \
     -f Dockerfile.rails .

-- publish image to Docker Hub, this image will be using in Kubernertes deployment
   docker login
   docker push $DOCKER_USERNAME/mable-rail:latest

-- create Kube deployment

   kubectl apply -f ./deployment.yaml

-- create Kube service

   kubectl apply -f ./service.yaml

## end to end pipeline

   Jenkinsfile.deploy.pipeline
   

##  recommendation and improvement:
- store the credential and sensitive data (database username & password) in seperate key store
- database url could be managed through variable depends on hte environment
- Helm chart would be the best option to manage the deployment chart in K8s cluster



Assumption:

- the docker image above is not real published, just put it as a dummy example
- https & SSL is not implemented
