# lab

https://kubevirt.io/labs/kubernetes/lab1

create a virtual machine:
```bash
kubectl apply -f https://kubevirt.io/labs/manifests/vm.yaml
```

get list of vms:
```bash
kubectl get vms
```

start vm:
```bash
virtctl start testvm
```
