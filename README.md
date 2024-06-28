# KEDA HTTP addon scale down to zero example

This repository showcases the usage of KEDA and KEDA HTTP addon to scale down a deployment to zero based on the HTTP traffic.


- [x] Basic example with step-by-step-guide
- [ ] extend documentation
- [ ] add example for different scaling methods

## Requirements:

* minikube
* Docker

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
helm install http-add-on kedacore/keda-add-ons-http --namespace keda-http-example --create-namespace
```

### Deploy sample nginx app

#### 1. Install sample nginx helm chart
```zsh
helm install -n keda-http-example keda-http-example keda-http-example/ --values keda-http-example/values.yaml
```

#### 2. Port forward keda-add-ons-http-interceptor-proxy
```zsh
kubectl port-forward -n keda-http-example svc/keda-add-ons-http-interceptor-proxy 8080:8080
```

#### 3. Send HTTP request

```zsh
curl -H "Host: keda-example.local" 127.0.0.1:8080
```

