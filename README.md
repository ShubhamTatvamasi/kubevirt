# kubevirt

Install kubevirt:
```bash
kubectl apply -f https://github.com/kubevirt/kubevirt/releases/download/v0.37.2/kubevirt-operator.yaml
kubectl apply -f https://github.com/kubevirt/kubevirt/releases/download/v0.37.2/kubevirt-cr.yaml

kubectl -n kubevirt wait kv kubevirt --for condition=Available
```

