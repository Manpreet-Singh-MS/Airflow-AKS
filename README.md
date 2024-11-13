# Apache Airflow on Azure Kubernetes Service (AKS)

This guide outlines the steps to deploy Apache Airflow on Azure Kubernetes Service (AKS) with an NGINX ingress controller and optional HTTPS (TLS) configuration.

## Table of Contents
- [Step 1: Create a Resource Group](#step-1-create-a-resource-group)
- [Step 2: Create an AKS Cluster](#step-2-create-an-aks-cluster)
- [Step 3: Connect to AKS Cluster](#step-3-connect-to-aks-cluster)
- [Step 4: Install Helm](#step-4-install-helm)
- [Step 5: Add the Apache Airflow Helm Chart Repository](#step-5-add-the-apache-airflow-helm-chart-repository)
- [Step 6: Create a Namespace for Airflow](#step-6-create-a-namespace-for-airflow)
- [Step 7: Install NGINX Ingress Controller](#step-7-install-nginx-ingress-controller)
- [Step 8: Install Airflow with Helm](#step-8-install-airflow-with-helm)
- [Step 9: Create an Ingress Resource for Airflow](#step-9-create-an-ingress-resource-for-airflow)
- [Step 10: Get the External IP of the Ingress Controller](#step-10-get-the-external-ip-of-the-ingress-controller)
- [Optional: Enable HTTPS (TLS)](#optional-enable-https-tls)
  - [Step 1: Create a TLS Certificate](#step-1-create-a-tls-certificate)
  - [Step 2: Create a Cluster-issuer.yaml](#step-2-create-a-cluster-issueryaml)
  - [Step 3: Create a Certificate.yaml](#step-3-create-a-certificateyaml)
  - [Step 4: Update airflow-ingress.yaml to Enable TLS](#step-4-update-airflow-ingressyaml-to-enable-tls)
  - [Step 5: Verify HTTPS Access](#step-5-verify-https-access)

## Step 1: Create a Resource Group

<code>
az group create --name airflow-rg --location centralindia
</code>

## Step 2: Create an AKS Cluster

<code>
az aks create \
  --resource-group airflow-rg \
  --name airflow-aks-cluster \
  --node-count 3 \
  --enable-addons monitoring \
  --generate-ssh-keys
</code>

## Step 3: Connect to AKS Cluster

<code>
az aks get-credentials --resource-group airflow-rg --name airflow-aks-cluster
</code>

## Step 4: Install Helm

<code>
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
</code>

## Step 5: Add the Apache Airflow Helm Chart Repository

<code>
helm repo add apache-airflow https://airflow.apache.org
helm repo update
</code>

## Step 6: Create a Namespace for Airflow

<code>
kubectl create namespace airflow
</code>

## Step 7: Install NGINX Ingress Controller

<code>
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
</code>

This installs the Ingress controller in the `ingress-nginx` namespace. Verify the installation:

<code>
kubectl get pods -n ingress-nginx
</code>

## Step 8: Install Airflow with Helm

<code>
helm install airflow apache-airflow/airflow --namespace airflow
</code>

## Step 9: Create an Ingress Resource for Airflow

<code>
kubectl apply -f airflow-ingress.yaml
</code>

## Step 10: Get the External IP of the Ingress Controller

<code>
kubectl get svc -n ingress-nginx
</code>

## Optional: Enable HTTPS (TLS)

### Step 1: Create a TLS Certificate

<code>
kubectl apply -f https://github.com/jetstack/cert-manager/releases/latest/download/cert-manager.yaml
</code>

### Step 2: Create a Cluster-issuer.yaml

<code>
kubectl apply -f cluster-issuer.yaml
</code>

### Step 3: Create a Certificate.yaml

<code>
kubectl apply -f certificate.yaml
</code>

### Step 4: Update airflow-ingress.yaml to Enable TLS

<code>
kubectl apply -f airflow-ingress.yaml
</code>

### Step 5: Verify HTTPS Access

<code>
kubectl get secret airflow-tls -n airflow
</code>

---

This concludes the setup of Apache Airflow on AKS with optional HTTPS. Enjoy managing your workflows on Kubernetes!
