# Kafdrop: Kafka Web UI

## Introduction

This chart is a self-hosted helm chart. Chart definition is copied directly from Kafdrop repository](https://github.com/obsidiandynamics/kafdrop)

More details on this helm chart: https://github.com/obsidiandynamics/kafdrop/tree/master#running-in-kubernetes-using-a-helm-chart


## Installing Helm Chart

Add Helm repository

```shell
helm repo add ricsanfre https://ricsanfre.github.io/helm-charts
```

Update helm repo

```shell
helm repo update
```

Install the chart:
```sh
helm upgrade -i kafdrop ricsanfre/kafdrop --set image.tag=3.x.x \
    --set kafka.brokerConnect=<host:port,host:port> \
    --set server.servlet.contextPath="/" \
    --set cmdArgs="--message.format=AVRO --schemaregistry.connect=http://localhost:8080" \ #optional
    --set jvm.opts="-Xms32M -Xmx64M"
```

For all Helm configuration options, have a peek into [chart/values.yaml](chart/values.yaml).

Replace `3.x.x` with the image tag of [obsidiandynamics/kafdrop](https://hub.docker.com/r/obsidiandynamics/kafdrop). Services will be bound on port 9000 by default (node port 30900).

**Note:** The context path _must_ begin with a slash.

Proxy to the Kubernetes cluster:
```sh
kubectl proxy
```

Navigate to [http://localhost:8001/api/v1/namespaces/default/services/http:kafdrop:9000/proxy](http://localhost:8001/api/v1/namespaces/default/services/http:kafdrop:9000/proxy).


### Protobuf support via helm chart:
To install with protobuf support, a "facility" option is provided for the deployment, to mount the descriptor files folder, as well as passing the required CMD arguments, via option _mountProtoDesc_.
Example:

```sh
helm upgrade -i kafdrop ricsanfre/kafdrop --set image.tag=3.x.x \
    --set kafka.brokerConnect=<host:port,host:port> \
    --set server.servlet.contextPath="/" \
    --set mountProtoDesc.enabled=true \
    --set mountProtoDesc.hostPath="<path/to/desc/folder>" \
    --set jvm.opts="-Xms32M -Xmx64M"
```
