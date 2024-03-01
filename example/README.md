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
1. `projects.apiKey`: The project's API key. Can be found in the project's settings in Amplitude.
2. `projects.secretKey`: The project's secret key. Can be found in the project's settings in Amplitude.
3. `projects.managementKey`: The Experiment management api key. Must be created for the same project as the configured API and secret key. Used to automatically access and update deployments used for the project.

## Install Evaluation Proxy Helm Chart

```
helm repo add evaluation-proxy-helm https://amplitude.github.io/evaluation-proxy-helm
helm install -f values.yaml evaluation-proxy evaluation-proxy-helm/evaluation-proxy
```

## Evaluate a user using the proxy

You can use `minikube` to set up [NodePort access](https://minikube.sigs.k8s.io/docs/handbook/accessing/#nodeport-access) with your local cluster:

```
minikube service evaluation-proxy --url
```

This command will print a localhost link with a port. Use the uri to configure an SDK's `serverUrl` config or cURL a request using the [Evaluation API](https://docs.developers.amplitude.com/experiment/apis/evaluation-api/).

1. Replace `<PORT>` with the port returned by `minikube service` 
2. Replace `<DEPLOYMENT_KEY>` with a deployment key being managed by the proxy
3. Replace `<USER_ID>` with the user ID to evaluate. You can also include device ID and additional context in the query params.

```
curl --request GET \
     --url 'http://localhost:<PORT>/v1/vardata?user_id=<USER_ID>' \
     --header 'Authorization: Api-Key <DEPLOYMENT_KEY>'
```

