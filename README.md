# microservice-demo

This is a helm chart for a simple microservice for testing purposes, based on [weaveworks socks shop microservice] (https://github.com/microservices-demo/microservices-demo). However, several functionalities have been added and modified, to make it more flexible and secure, following best practices.

## Architecture
The high level architecture of the application is shown below:
![Architecture](https://github.com/microservices-demo/microservices-demo.github.io/blob/HEAD/assets/Architecture.png)

## Values

The values file is much more flexible than the original one, providing switches to turn on and off every single microservice conforming the full mesh, and its associated resources.
* validate Kubernetes manifests with [kubeval](https://github.com/instrumenta/kubeval)
* validate Flux Helm Releases with [hrval](https://github.com/stefanprodan/hrval-action)

## Storage
PV/PVCs support has been added for those microservices that were using in-memory storage for databases, such as those relying on mongo and mysql. Six microservices support now PVs, which are claimed by mean of the PVCs defined in [this file]():
* cart-db
* orders-db
* orders
* shipping
* user-db
* catalogue-db

## Ingress
The chart has been modified to use an ingress with basic http autentication and tls, which is dynamically generated depending on the environment following the format **microservice-{environment}.alteos.io.**.

## Sealed Secrets
Secrets are incorporated to pass sensitive environment variables securely to the containers. However, as the base64 encoding is not secure, [sealedsecrets](https://github.com/bitnami-labs/sealed-secrets) are used to cypher the original secrets. Thus, the sealed secrets can be stored in this public repository, without exposing the sensitive data.

## CI
In order to provide a CI cycle for this Helm Chart development, Github actions are used to check the templates. In this case, a [customized image](https://github.com/mifonpe/github-runner) with its own CI is used for the runners, which already contains helm and some other utilities, and runs in a personal cluster. 


