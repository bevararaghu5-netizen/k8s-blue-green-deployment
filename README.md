\# Kubernetes Blue-Green Deployment



\## Project Overview



This project demonstrates the Blue-Green Deployment strategy using Docker and Kubernetes with Minikube.



Two application versions are deployed independently: Blue (Version 1.0) and Green (Version 2.0). A Kubernetes Service controls which version receives application traffic.



The project demonstrates how to switch between application versions and roll back to the previous version by updating the Service selector.



\## Objectives



\- Deploy two application versions using Kubernetes Deployments.

\- Containerize the applications using Docker.

\- Run the containers locally using Minikube.

\- Route application traffic through a Kubernetes Service.

\- Switch traffic between Blue and Green environments.

\- Demonstrate rollback to the previous application version.



\## Technologies Used



\- Docker

\- Kubernetes

\- Minikube

\- kubectl

\- NGINX

\- YAML

\- HTML and CSS

\- Windows PowerShell



\## Architecture



The application follows this deployment flow:



User Browser → Minikube NodePort Service → Selected Application Pods



The Kubernetes Service uses labels to select the active environment.



\- Blue Environment: Version 1.0

\- Green Environment: Version 2.0



Both deployments have two replicas.



\## Project Structure



```text

kubernetes-blue-green-deployment/

├── app/

│   ├── blue/

│   │   ├── index.html

│   │   └── Dockerfile

│   ├── green/

│   │   ├── index.html

│   │   └── Dockerfile

│   └── Dockerfile

├── k8s/

│   ├── blue-deployment.yaml

│   ├── green-deployment.yaml

│   └── service.yaml

├── screenshots/

└── README.md

```



\## Deployment Process



\### 1. Start Minikube



```powershell

minikube start --driver=docker

```



\### 2. Build the Docker Images



```powershell

docker build -t blue-green-app:1.0 ./app/blue

docker build -t blue-green-app:2.0 ./app/green

```



\### 3. Load Images into Minikube



```powershell

minikube image load blue-green-app:1.0

minikube image load blue-green-app:2.0

```



\### 4. Deploy Both Application Versions



```powershell

kubectl apply -f k8s/blue-deployment.yaml

kubectl apply -f k8s/green-deployment.yaml

```



\### 5. Configure the Service



```powershell

kubectl apply -f k8s/service.yaml

```



\### 6. Verify Deployments and Pods



```powershell

kubectl get deployments

kubectl get pods --show-labels

kubectl describe service blue-green-service

```



\### 7. Access the Application



```powershell

minikube service blue-green-service --url

```



Open the URL printed in the terminal in a browser.



\## Blue-Green Traffic Switching



The Kubernetes Service initially selects the Blue Pods using this selector:



```yaml

selector:

&#x20; app: blue-green-app

&#x20; version: blue

```



To switch traffic to Green, update the selector in `k8s/service.yaml`:



```yaml

selector:

&#x20; app: blue-green-app

&#x20; version: green

```



Apply the updated configuration:



```powershell

kubectl apply -f k8s/service.yaml

```



Verify the Service selector and endpoints:



```powershell

kubectl describe service blue-green-service

kubectl get endpoints blue-green-service

```



Refresh the application in the browser to verify that Version 2.0 is served.



\## Rollback



To roll back to Blue, change the Service selector from `version: green` to `version: blue` in `k8s/service.yaml`.



Apply the change:



```powershell

kubectl apply -f k8s/service.yaml

```



Verify the selector and refresh the browser to confirm that Version 1.0 is served.



\## Screenshots



Add the screenshots captured during deployment and testing in the `screenshots` folder.



Examples:



\- Blue application: `screenshots/blue-environment.png`

\- Green application: `screenshots/green-environment.png`

\- Rollback: `screenshots/blue-rollback.png`

\- Kubernetes deployments: `screenshots/deployments.png`

\- Kubernetes Pods: `screenshots/pods.png`



Update these paths to match your actual screenshot filenames.



\## Key Learnings



\- Deploying multiple application versions in Kubernetes.

\- Building and loading Docker images into Minikube.

\- Using Kubernetes labels and selectors to route traffic.

\- Switching application traffic between environments.

\- Performing a rollback by changing a Kubernetes Service selector.



\## Conclusion



This project demonstrates a local Blue-Green Deployment workflow using Docker, Kubernetes, and Minikube. It provides practical experience with application deployment, Service-based traffic switching, and rollback.

