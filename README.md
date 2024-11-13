# Apache Airflow on Azure Kubernetes Service (AKS)

This guide provides instructions to deploy Apache Airflow on an Azure Kubernetes Service (AKS) cluster, with an NGINX Ingress controller and optional HTTPS (TLS) configuration.

## Table of Contents
- [Prerequisites](#prerequisites)
- [Deployment Steps](#deployment-steps)
  - [1. Create a Resource Group](#1-create-a-resource-group)
  - [2. Create an AKS Cluster](#2-create-an-aks-cluster)
  - [3. Connect to AKS Cluster](#3-connect-to-aks-cluster)
  - [4. Install Helm](#4-install-helm)
  - [5. Add the Apache Airflow Helm Chart Repository](#5-add-the-apache-airflow-helm-chart-repository)
  - [6. Create a Namespace for Airflow](#6-create-a-namespace-for-airflow)
  - [7. Install NGINX Ingress Controller](#7-install-nginx-ingress-controller)
  - [8. Install Apache Airflow with Helm](#8-install-apache-airflow-with-helm)
  - [9. Create an Ingress Resource for Airflow](#9-create-an-ingress-resource-for-airflow)
  - [10. Get the External IP of the Ingress Controller](#10-get-the-external-ip-of-the-ingress-controller)
- [Optional: Enable HTTPS (TLS)](#optional-enable-https-tls)
  - [Step 1: Install Cert-Manager](#step-1-install-cert-manager)
  - [Step 2: Create a Cluster Issuer](#step-2-create-a-cluster-issuer)
  - [Step 3: Create a TLS Certificate](#step-3-create-a-tls-certificate)
  - [Step 4: Update Airflow Ingress to Enable TLS](#step-4-update-airflow-ingress-to-enable-tls)
  - [Step 5: Verify HTTPS Access](#step-5-verify-https-access)
- [Accessing Airflow](#accessing-airflow)

---

## Prerequisites
- **Azure CLI** ([Install Guide](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli))
- **Kubernetes CLI (`kubectl`)** ([Install Guide](https://kubernetes.io/docs/tasks/tools/))
- **Helm 3** ([Install Guide](https://helm.sh/docs/intro/install/))

## Deployment Steps

### 1. Create a Resource Group

```bash
az group create --name airflow-rg --location centralIndia
