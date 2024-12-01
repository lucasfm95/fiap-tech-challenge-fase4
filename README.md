# FIAP Tech Challenge Phase 4

## Overview

This project is part of the FIAP Tech Challenge Phase 4. It includes multiple microservices deployed using Kubernetes.

## Project Structure

The project is organized into several directories, each containing Kubernetes configurations for different services:

- `contacts-api/`: Manages the Contacts API service.
- `contacts-delete/`: Manages the Contacts Delete service.
- `contacts-insert/`: Manages the Contacts Insert service.
- `contacts-update/`: Manages the Contacts Update service.
- `grafana/`: Manages the Grafana monitoring service.
- `postgres/`: Manages the PostgreSQL database service.
- `rabbitmq/`: Manages the RabbitMQ messaging service.

## Services

### Contacts API

- Namespace: `contacts-api-cluster`
- Deployment: `contacts-api-deployment`
- Service: `contacts-api-service`
- ConfigMap: `contacts-configmap`
- Secrets: `contacts-api-secret`
- Horizontal Pod Autoscaler: `contacts-api-hpa`

### Contacts Delete

- Namespace: `contacts-delete-cluster`
- Deployment: `contact-delete-consumer-worker`
- Service: `contact-delete-consumer-worker`
- ConfigMap: `contacts-delete-configmap`
- Secrets: `contacts-delete-secret`

### Contacts Insert

- Namespace: `contacts-insert-cluster`
- Deployment: `contact-insert-consumer-worker`
- Service: `contact-insert-consumer-worker`
- ConfigMap: `contacts-insert-configmap`
- Secrets: `contacts-insert-secret`

### Contacts Update

- Namespace: `contacts-update-cluster`
- Deployment: `contact-update-consumer-worker`
- Service: `contact-update-consumer-worker`
- ConfigMap: `contacts-update-configmap`
- Secrets: `contacts-update-secret`

### Grafana

- Namespace: `grafana-cluster`
- Deployment: `grafana-deployment`
- Service: `grafana-service`
- ConfigMap: `grafana-config`
- Secrets: `grafana-secret`
- PersistentVolumeClaim: `grafana-pvc`
- Horizontal Pod Autoscaler: `grafana-hpa`

### PostgreSQL

- Namespace: `postgres-cluster`
- Pod: `postgres-pod`
- Service: `postgres-service`
- ConfigMap: `configmap-init-db`
- Secrets: `postgre-secret`
- PersistentVolume: `postgres-pv`
- PersistentVolumeClaim: `postgres-pvc`

### RabbitMQ

- Namespace: `rabbitmq-cluster`
- StatefulSet: `rabbitmq`
- Service: `rabbitmq`
- ConfigMap: `rabbitmq-config`
- Secrets: `rabbitmq-secret`
- Horizontal Pod Autoscaler: `rabbitmq-hpa`

## Getting Started

### Prerequisites

- Kubernetes cluster
- kubectl configured to interact with your cluster
- helm
### Before apply the yamls, configure KEDA for auto scale work
#### Via helm
1. install helm
2. instal keda via helm on cluster
    * `helm repo add kedacore https://kedacore.github.io/charts`
    * `helm repo update`
    * `helm install keda kedacore/keda --namespace keda --create-namespace`
    * obs: it take 7-10 minuts until keda pods are up and ready
#### Via yaml
2. run the command `kubectl apply --server-side -f https://github.com/kedacore/keda/releases/download/v2.16.0/keda-2.16.0.yaml`

### Deployment using all files once

1. Apply all files:
    ```sh
    kubectl apply -f . --recursive
    ```

2. Delete all resources
    ```sh
    kubectl delete -f . --recursive
    ```

### Deployment by config files

1. Apply the namespaces:
    ```sh
    kubectl apply -f kubernetes/contacts-api/1.contacts-api-namespace.yml
    kubectl apply -f kubernetes/contacts-delete/1.contacts-delete-namespace.yml
    kubectl apply -f kubernetes/contacts-insert/1.contacts-insert-namespace.yml
    kubectl apply -f kubernetes/contacts-update/1.contacts-update-namespace.yml
    kubectl apply -f kubernetes/grafana/1.grafana-namespace.yml
    kubectl apply -f kubernetes/postgres/1.postgres-namespace.yml
    kubectl apply -f kubernetes/rabbitmq/1.rmq-namespace.yml
    ```

2. Apply the ConfigMaps and Secrets:
    ```sh
    kubectl apply -f kubernetes/contacts-api/2.contacts-api-configmap.yml
    kubectl apply -f kubernetes/contacts-api/3.contacts-api-secrets.yml
    kubectl apply -f kubernetes/contacts-delete/2.contacts-delete-configmap.yml
    kubectl apply -f kubernetes/contacts-delete/3.contacts-delete-secrets.yml
    kubectl apply -f kubernetes/contacts-insert/2.contacts-insert-configmap.yml
    kubectl apply -f kubernetes/contacts-insert/3.contacts-insert-secrets.yml
    kubectl apply -f kubernetes/contacts-update/2.contacts-update-configmap.yml
    kubectl apply -f kubernetes/contacts-update/3.contacts-update-secrets.yml
    kubectl apply -f kubernetes/grafana/2.grafana-configmap.yml
    kubectl apply -f kubernetes/grafana/3.grafana-secrets.yml
    kubectl apply -f kubernetes/postgres/2.postgres-configmap.yml
    kubectl apply -f kubernetes/postgres/3.postgres-secrets.yml
    kubectl apply -f kubernetes/rabbitmq/2.rmq-configmap.yml
    kubectl apply -f kubernetes/rabbitmq/3.rmq-secrets.yml
    ```

3. Apply the PersistentVolumes and PersistentVolumeClaims:
    ```sh
    kubectl apply -f kubernetes/contacts-api/4.contacts-api-pv.yml
    kubectl apply -f kubernetes/grafana/4.grafana-pvc.yml
    kubectl apply -f kubernetes/postgres/4.postgres-pv.yml
    kubectl apply -f kubernetes/postgres/5.postgres-pvc.yml
    ```

4. Apply the Deployments and Services:
    ```sh
    kubectl apply -f kubernetes/contacts-api/6.contracts-api-deployment.yml
    kubectl apply -f kubernetes/contacts-api/5.contracts-api-service.yml
    kubectl apply -f kubernetes/contacts-delete/5.contacts-delete-deployment.yaml
    kubectl apply -f kubernetes/contacts-delete/4.contacts-delete-service.yaml
    kubectl apply -f kubernetes/contacts-insert/5.contacts-insert-deployment.yaml
    kubectl apply -f kubernetes/contacts-insert/4.contacts-insert-service.yaml
    kubectl apply -f kubernetes/contacts-update/5.contacts-update-deployment.yaml
    kubectl apply -f kubernetes/contacts-update/4.contacts-update-service.yaml
    kubectl apply -f kubernetes/grafana/6.grafana-deployment.yml
    kubectl apply -f kubernetes/grafana/5.grafana.service.yml
    kubectl apply -f kubernetes/postgres/6.postgres-pod.yml
    kubectl apply -f kubernetes/postgres/7.postgres-svc.yml
    kubectl apply -f kubernetes/rabbitmq/4.rmq-statefull.yml
    kubectl apply -f kubernetes/rabbitmq/5.rmq-service.yml
    ```

5. Apply the Horizontal Pod Autoscalers:
    ```sh
    kubectl apply -f kubernetes/contacts-api/7.contacts-api-hpa.yml
    kubectl apply -f kubernetes/grafana/7.grafana-hpa.yml
    kubectl apply -f kubernetes/rabbitmq/6.rmq-hpa.yml
    ```

## To test auto scale
1. Install k6
2. Configure http address in load-test.js file
3. run `k6 run load-test.js`
## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.