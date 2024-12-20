# Operationalizing a Coworking Space Microservice

## Project Overview: Coworking Space Service

The Coworking Space Service is a set of APIs that enables users to request one-time tokens and administrators to authorize access to a coworking space. This service follows a microservice pattern and the APIs are split into distinct services that can be deployed and managed independently of one another. For this project, you are a DevOps engineer who will be collaborating with a team that is building an API for business analysts. The API provides business analysts with basic analytics data on user activity in the coworking space service. The application they provide you functions as expected, and you will help build a pipeline to deploy it to Kubernetes. You'll submit artefacts from the build and deployment of this service.

## Step 1: Set Up a Postgres Database with Helm Chart
Pre-requisites:
- Have Kubernetes cluster ready.
- Have `kubectl` installed and configured to interact with your cluster.
- Have Helm installed.
//
step1: create cluster
eksctl create cluster --name coworking --version 1.29 --region us-east-1 --nodegroup-name coworking-nodes --node-type t3.small --nodes 1 --nodes-min 1 --nodes-max 2

step2:Update the Kubeconfig
aws eks --region us-east-1 update-kubeconfig --name coworking
kubectl config current-context
kubectl config view

step3: Apply YAML configurations
kubectl apply -f deployment/configmap.yml
kubectl apply -f deployment/postgresql-deployment.yml
kubectl apply -f deployment/postgresql-service.yml
kubectl apply -f deployment/coworking.yml
kubectl apply -f deployment/pv.yml
kubectl apply -f deployment/pvc.yml
kubectl apply -f deployment/secrets.yml


step 4:
kubectl get pods


step 5:
kubectl exec -it postgresql-74f8c5678f-tjtbw -- bash

step 6: 
psql -U postgres -d postgres
psql -U postgres -d postgres -h 127.0.0.1 -a -f db/1_create_tables.sql
psql -U postgres -d postgres -h 127.0.0.1 -a -f db/2_seed_users.sql
psql -U postgres -d postgres -h 127.0.0.1 -a -f db/3_seed_tokens.sql

//
DanNLV
Instructions:
1. Install Helm:
   ```bash
   curl -LO https://get.helm.sh/helm-v3.12.2-linux-amd64.tar.gz
   ```
2. Add the Bitnami Helm Repository:
    ```bash
   helm repo add bitnami https://charts.bitnami.com/bitnami
   helm repo update
   ```
3. Install the PostgreSQL Chart:
   ```bash
   helm install my-postgres bitnami/postgresql
   ```
4. Verify the Installation:
   ```bash
   helm list
   kubectl get pods
   ```
5. Get the PostgreSQL Connection Details:
   ```bash
   export POSTGRES_PASSWORD=$(kubectl get secret --namespace default my-postgres-postgresql -o jsonpath="{.data.postgres-password}" | base64 --decode)
   ```

#   c o w o r k i n g  
 