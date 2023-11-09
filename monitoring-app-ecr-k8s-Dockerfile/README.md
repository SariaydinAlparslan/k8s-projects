# **Cloud Native Resource Monitoring Python App on K8s!**

# ✨Let’s Start the Project ✨

## **Part 1: Deploying the Flask application in EC2 and Dockerizing the Flask app**

### **Step 1: Install Docker and Git**

sudo apt update

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

apt-cache policy docker-ce

sudo apt install docker-ce

sudo systemctl enable docker && sudo systemctl start docker

sudo groupadd docker
sudo usermod -aG docker ${USER}
newgrp docker

sudo dnf install git-all

git clone app

### **Step 2: Create a Dockerfile**

Create a **`Dockerfile`** in the root directory of the project with the following contents:

```
# Use the official Python image as the base image
FROM python:3.9-slim-buster

# Set the working directory in the container
WORKDIR /app

# Copy the requirements file to the working directory
COPY requirements.txt .

RUN pip3 install --no-cache-dir -r requirements.txt

# Copy the application code to the working directory
COPY . .

# Set the environment variables for the Flask app
ENV FLASK_RUN_HOST=0.0.0.0

# Expose the port on which the Flask app will run
EXPOSE 80

# Start the Flask app when the container is run
CMD ["flask", "run"]
```

### **Step 3: Build the Docker image**

To build the Docker image, execute the following command:

```
docker build -t <image_name> .
```

### **Step 4: Run the Docker container. Check the app**

To run the Docker container, execute the following command:

```
docker run -p 80:80 <image_name>
```

## **Part 2: Pushing the Docker image to ECR**

### **Step 1: Create an ECR repository**

### **Step 2: Push the Docker image to ECR**

Push the Docker image to ECR using the push commands on the console:

You should take instance role to push container images

docker tag <image_name> <repo_name>

docker push <repo_name>

## **Part 3: Create an EKS cluster and Deployment**

### **Step 1: Create an EKS cluster**

Create an EKS cluster and add node group

You should take eks role to pull container images

### **Step 2: Create a node group**

Create a node group in the EKS cluster.

### **Step 3: Create deployment and service**

kubectl create deployment app --image=<repo_name> --port=80

kubectl expose deployment app --name=svc

```jsx
kubectl get deployment -n default (check deployments)
kubectl get service -n default (check service)s
kubectl get pods -n default (to check the pods)
```

Check the app
