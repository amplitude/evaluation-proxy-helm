# Helm+Kubernetes Evaluation Proxy Example

This example gets the evaluation proxy running on your local machine using `minikube`.

## Install Prerequisites

1. [Install `helm`](https://helm.sh/docs/intro/install/)
2. [Install `minikube` and `minikube start` your local cluster](https://minikube.sigs.k8s.io/docs/start/)

## Install Redis Helm Chart

Use the local `redis-values.yaml` file to install the [redis helm chart from bitnami](https://github.com/bitnami/charts/tree/main/bitnami/redis/).

```
helm install -f redis-values.yaml redis bitnami/redis
```

## Configure `values.yaml`

Update the fields in the `values.yaml` file:
1. `projects.id`: The Amplitude project ID. Can be found in the project's settings in Amplitude.
2. `projects.apiKey`: The project's API key. Can be found in the project's settings in Amplitude.
3. `projects.secretKey`: The project's secret key. Can be found in the project's settings in Amplitude.
4. `projects.deploymentKeys`: The deployment keys to manage. Listed deployment keys have associated flags and cohorts downloaded and managed by the proxy. The deployment keys listed here must be associated with the project identified in the other fields.

## Install Evaluation Proxy Helm Chart

```
helm repo add evaluation-proxy-helm https://amplitude.github.io/evaluation-proxy-helm
helm install -f values.yaml evaluation-proxy evaluation-proxy-helm/evaluation-proxy
```
