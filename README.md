# Evaluation Proxy Helm Chart

A Helm Chart for deploying the Amplitude Experiment [Evaluation Proxy](https://docs.developers.amplitude.com) on Kubernetes.

### Resources

| Link                                                                                      | Description                                                                                                                                                                        |
|-------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Developer Docs](https://docs.developers.amplitude.com/experiment/infra/evaluation-proxy) | Full documentation about the evaluation proxy. Contains more details about [configuration](https://docs.developers.amplitude.com/experiment/infra/evaluation-proxy#configuration). |
| [Helm Example](https://github.com/amplitude/tree/main/example)                            | Run the evaluation proxy using this helm chart locally using `minikube`                                                                                                            |


## Install

### Add repo

```
helm repo add amplitude/evaluation-proxy https://amplitude.github.io/evaluation-proxy-helm
```

### Configure `values.yaml`

Configure the chart values. The recommended approach to configuring and installing the helm chart is using a values.yaml configuration file.

The chart's `evaluationHeader` value contents exactly match the evaluation proxy's configuration file fields. Amplitude's developer docs contains [additional information and configuration options](https://docs.developers.amplitude.com/experiment/infra/evaluation-proxy#configuration).

```yaml
evaluationProxy:
  # At least one project is required.
  projects:
    - id: "TODO"
      apiKey: "TODO"
      secretKey: "TODO"
      deploymentKeys:
        - "TODO"
  configuration: {}
#    redis:
#      uri: "redis://redis-master.default.svc.cluster.local:6379"
```

| Value                     | Description                                                                                                                                                                                                                      |
|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `projects.id`             | The Amplitude project ID. Can be found in the project's settings in Amplitude.                                                                                                                                                   |
| `projects.apiKey`         | The project's API key. Can be found in the project's settings in Amplitude.                                                                                                                                                      |
| `projects.secretKey`      | The project's secret key. Can be found in the project's settings in Amplitude.                                                                                                                                                   |
| `projects.deploymentKeys` | The deployment keys to manage. Listed deployment keys have associated flags and cohorts downloaded and managed by the proxy. The deployment keys listed here must be associated with the project identified in the other fields. |
| `configuration.redis.uri` | The uri for connecting to redis within the cluster. If missing, the proxy will run in memory.                                                                                                                                    |

### Install chart

```
helm install -f values.yaml evaluation-proxy amplitude/evaluation-proxy
```
