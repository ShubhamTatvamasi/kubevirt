# ubuntu cloud images

```yaml
kubectl create -f - << EOF
apiVersion: cdi.kubevirt.io/v1beta1
kind: DataVolume
metadata:
  name: ubuntu-volume
spec:
  source:
    http:
      url: "http://192.168.5.66/focal-server-cloudimg-amd64.img"
  pvc:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 20G
EOF
```

create a VM ID: `ubuntu` Pass: `ubuntu`:
```yaml
kubectl create -f - << EOF
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  labels:
    kubevirt.io/vm: vm-ubuntu
  name: vm-ubuntu
spec:
  running: false
  template:
    metadata:
      labels:
        kubevirt.io/vm: vm-ubuntu
    spec:
      domain:
        devices:
          disks:
          - disk:
              bus: virtio
            name: pvcdisk1
          - disk:
              bus: virtio
            name: cloudinitdisk
        resources:
          requests:
            cpu: 4
            memory: 4G
      terminationGracePeriodSeconds: 0
      volumes:
      - name: pvcdisk1
        persistentVolumeClaim:
          claimName: ubuntu-volume
      - name: cloudinitdisk
        cloudInitNoCloud:
          userDataBase64: I2Nsb3VkLWNvbmZpZwpwYXNzd29yZDogdWJ1bnR1CnNzaF9wd2F1dGg6IFRydWUKY2hwYXNzd2Q6IHsgZXhwaXJlOiBGYWxzZSB9Cg==
EOF
```

get list of VMs:
```bash
kubectl get vm
```

start VM:
```bash
virtctl start vm-ubuntu
```

access console of VM:
```bash
virtctl console vm-ubuntu
```

stop VM:
```bash
virtctl stop vm-ubuntu
```

delete VM:
```bash
kubectl delete vm vm-ubuntu
```

expose SSH service:
```bash
virtctl expose vmi vm-ubuntu --port=22 --name=vm-ubuntu-ssh --type=NodePort
```



