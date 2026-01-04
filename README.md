# ECommerce Microservices Platform

This document describes development setup using **Minikube**, **Kubernetes**, **Skaffold**, **ElasticSearch**, and **Kafka**.

---

## Minikube Docker Configuration

**(WSL only — NOT Docker Desktop)**

Set Docker environment to use Minikube’s Docker daemon:

```bash
eval $(minikube docker-env)
```

Unset the Minikube Docker environment:

```bash
eval $(minikube docker-env --unset)
```

---

## API Gateway Configuration (One-Time Setup)

Install the Kubernetes Gateway API CRDs:

```bash
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.2.0/standard-install.yaml
```

---

## ElasticSearch Setup

### Startup (Machine Hosted)

Navigate to the ElasticSearch directory and start the service:

```bash
cd C:/projects/elasticsearch
bin/elasticsearch.bat
```

### Kubernetes Configuration (One-Time for Containers)

Install Elastic Cloud on Kubernetes (ECK):

```bash
kubectl create -f https://download.elastic.co/downloads/eck/2.15.0/crds.yaml
kubectl apply -f https://download.elastic.co/downloads/eck/2.15.0/operator.yaml
```

---

## Kafka Setup (Machine Hosted via Git Bash)

Navigate to the Kafka directory:

```bash
cd /c/projects/kafka
```

### Start ZooKeeper

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

### Start Kafka Broker

```bash
bin/kafka-server-start.sh config/server.properties
```

### Create Topic

```bash
bin/kafka-topics.sh --create --topic product-changes --bootstrap-server localhost:9092
```

### Start Producer

```bash
bin/kafka-console-producer.sh --topic product-changes --bootstrap-server localhost:9092
```

### Start Consumer

```bash
bin/kafka-console-consumer.sh --topic product-changes --from-beginning --bootstrap-server localhost:9092
```

---

## Running the Project

### 1. Start Minikube

```bash
minikube start
```

### 2. Start the Kubernetes Project

Run the project using Skaffold development mode:

```bash
skaffold dev
```

---

## Notes

* Kafka and ElasticSearch are **machine-hosted**, not containerized, However, the configuration of both can be found in **kubernetes** folder for hosting in a containerized manner.
* Kubernetes workloads run inside **Minikube**.
* This setup is optimized for **local development and testing**.
