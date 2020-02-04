```
kubectl create ns app
```

```
kubectl label ns app istio-injection=enabled
```

```
kubectl apply -n app -f https://raw.githubusercontent.com/raghunandanbs/microservices-demo/master/custom-manifests/release-without-fe.yaml
```

```
kubectl apply -n app -f https://raw.githubusercontent.com/raghunandanbs/microservices-demo/master/custom-manifests/frontend-v1.yaml
```

```
kubectl apply -n app -f https://raw.githubusercontent.com/raghunandanbs/microservices-demo/master/custom-manifests/frontend-v2.yaml
```

```
kubectl apply -n app -f https://raw.githubusercontent.com/raghunandanbs/microservices-demo/master/custom-manifests/gateway.yaml
```

```
kubectl apply -n app -f https://raw.githubusercontent.com/raghunandanbs/microservices-demo/master/custom-manifests/traffic-management.yaml
```
