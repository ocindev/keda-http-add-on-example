# KEDA HTTP addon scale down to zero example

This repository showcases the usage of [KEDA](https://keda.sh) and [KEDA HTTP addon](https://kedacore.github.io/http-add-on/) to scale down a deployment to zero based on the HTTP traffic.


## Requirements:

* [minikube](https://minikube.sigs.k8s.io/docs/)
* [Helm](https://helm.sh)
* [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl)

## Steps

### Start minikube
```zsh
minikube start
```

### Install KEDA

#### 1. Add KEDA helm repository
```zsh
helm repo add kedacore https://kedacore.github.io/charts
```

#### 2. Update Helm repo
```zsh
helm repo update
```

#### 3. Install KEDA chart
```zsh
helm install keda kedacore/keda --namespace keda --create-namespace
```

### Install KEDA HTTP addon

#### 1. Install HTTP addon chart to the app namespace
```zsh
helm install http-add-on kedacore/keda-add-ons-http --namespace keda
```

### Create sample nginx deployment

#### 1. Install sample nginx helm chart
```zsh
helm install -n keda-http-example keda-http-example keda-http-example/ --values keda-http-example/values.yaml
```

### Test autoscaling

#### 1. Add keda-http-example to /etc/hosts
```
127.0.0.1 keda-example.local
```

#### 2. Start minikube tunnel
```zsh
minikube tunnel
```

#### 3. Send HTTP request to keda-example deployment

```zsh
curl -H "Host: keda-example.local" keda-example.local
Hello from Nginx!
```
