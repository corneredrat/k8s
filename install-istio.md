# Prerequisites

- create necessary namespaces
- label namespaces as follows

```
istio-injection: enabled
```

Download istio charts
```
curl -L https://istio.io/downloadIstio | sh -
```

change directory
```
cd istio-1.4.3/install/kubernetes/helm
```

Install custom resource definitions
```
for i in istio-init/files/crd-*.yaml; do kubectl apply -f $i ; done
```

Install istio-init
```
helm install istio-init istio-init --namespace istio-system
```

### Create secret for kiali
username and password : admin
```
apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: YWRtaW4=
  passphrase: YWRtaW4=

```

## Edit values.yaml for istio to include kiali and grafana

install istio
```
helm install istio istio --namespace istio-system
```

