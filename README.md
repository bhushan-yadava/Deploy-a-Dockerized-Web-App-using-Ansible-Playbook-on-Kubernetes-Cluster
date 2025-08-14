# Deploy a Dockerized Web App using Ansible Playbook on Kubernetes Cluster

## Project Overview

This project demonstrates how to automate the deployment of a Dockerized web application on a Kubernetes cluster using Ansible Playbooks. The main goal is to leverage infrastructure as code and automation tools to simplify application deployment and management.

The project covers:

- Containerizing a Flask web application with Docker.
- Automating image building and pushing to Docker Hub.
- Deploying the app on a local Kubernetes cluster (Minikube) using Ansible.
- Managing Kubernetes resources declaratively through Ansible playbooks.
- Ensuring repeatable and scalable deployments using automation.

This setup is ideal for learning how to combine Docker, Kubernetes, and Ansible for modern DevOps workflows.

---

## Prerequisites

- Docker installed and running  
- Minikube installed and running  
- Ansible installed (local machine or inside Docker container)  
- kubectl installed and configured  
- A Docker Hub account (for pushing images)  

---

## Project Structure

```plaintext
.
├── ansible
│   ├── inventory          # Ansible inventory file
│   ├── playbook.yml       # Ansible playbook for deployment
│   └── roles
│       └── app-deploy     # Role for deploying the Flask app
├── app
│   ├── Dockerfile         # Dockerfile for Flask app
│   └── app.py             # Flask application source code
└── README.md



Step-by-Step Commands
1. Start Minikube
minikube start

2. Build Docker Image Locally (using Minikube’s Docker daemon)
eval $(minikube -p minikube docker-env)
docker build -t yourdockerhubusername/flask-app:latest ./app


Or build and push from your local Docker directly (requires Docker Hub login):

docker build -t yourdockerhubusername/flask-app:latest ./app
docker push yourdockerhubusername/flask-app:latest

3. Configure kubeconfig file if necessary

Make sure your ~/.kube/config points to the correct cluster IP or Minikube server address.

4. Run Ansible Playbook to Deploy App

If you have Ansible installed locally:

ansible-playbook -i ansible/inventory ansible/playbook.yml


Or run via Docker container with Ansible installed:

docker run -it --rm \
  -v "${PWD}:/ansible" \
  -v "${HOME}/.kube:/root/.kube" \
  -v "${HOME}/.minikube:/root/.minikube" \
  -w /ansible \
  ansible/ansible-runner \
  ansible-playbook -i ansible/inventory ansible/playbook.yml

5. Verify Deployment

Check the pods and services in the Kubernetes cluster:

kubectl get pods
kubectl get svc


To access the Flask app, forward the service port or get the Minikube IP:

minikube service flask-service
