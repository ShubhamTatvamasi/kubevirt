# kubevirt

Install kubevirt:
```bash
export VERSION=$(curl -s https://api.github.com/repos/kubevirt/kubevirt/releases | \
  grep tag_name | grep -v -- '-rc' | head -1 | awk -F': ' '{print $2}' | sed 's/,//' | xargs)

echo ${VERSION}

kubectl create -f "https://github.com/kubevirt/kubevirt/releases/download/${VERSION}/kubevirt-operator.yaml"
kubectl create -f "https://github.com/kubevirt/kubevirt/releases/download/${VERSION}/kubevirt-cr.yaml"
```

Install Containerized Data Importer (CDI)
```bash
VERSION=$(curl -s https://github.com/kubevirt/containerized-data-importer/releases/latest | \
  grep -o "v[0-9]\.[0-9]*\.[0-9]*")

echo ${VERSION}

kubectl create -f "https://github.com/kubevirt/containerized-data-importer/releases/download/${VERSION}/cdi-operator.yaml"
kubectl create -f "https://github.com/kubevirt/containerized-data-importer/releases/download/${VERSION}/cdi-cr.yaml"
```

Install virtctl:
```bash
VERSION=$(kubectl get kubevirt.kubevirt.io/kubevirt -n kubevirt -o=jsonpath="{.status.observedKubeVirtVersion}")
ARCH=$(uname -s | tr A-Z a-z)-$(uname -m | sed 's/x86_64/amd64/') || windows-amd64.exe

echo ${VERSION}
echo ${ARCH}

curl -L -o virtctl "https://github.com/kubevirt/kubevirt/releases/download/${VERSION}/virtctl-${VERSION}-${ARCH}"
chmod +x virtctl
sudo install virtctl /usr/local/bin
```

Delete kubevirt:
```bash
kubectl delete kv kubevirt -n kubevirt
kubectl delete ns kubevirt
```

Creating a virtual machine:
```bash
kubectl apply -f https://github.com/kubevirt/kubevirt/raw/main/examples/vm-cirros.yaml
```

Get the image list:
```bash
kubectl get vm
```

To start a VM you can use, this will create a VM instance (VMI)
```bash
virtctl start vm-cirros
```

To shut the VM down again:
```bash
virtctl stop vm-cirros
```

To delete VM:
```bash
kubectl delete vm vm-cirros
```

Connect to the serial console:
```bash
virtctl console vm-cirros
```

Connect to the graphical display:
```bash
virtctl vnc vm-cirros
```


