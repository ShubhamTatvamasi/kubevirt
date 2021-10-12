# ubuntu cloud images

```yaml
kubectl create -f - << EOF
apiVersion: cdi.kubevirt.io/v1beta1
kind: DataVolume
metadata:
  name: my-ubuntu-volume
spec:
  source:
    http:
      url: "https://cloud-images.ubuntu.com/focal/current/focal-server-cloudimg-amd64.img"
  pvc:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 20G
EOF
```

create a VM:
```yaml
kubectl create -f - << EOF
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  labels:
    kubevirt.io/vm: vm-ubuntu-volume
  name: vm-ubuntu-volume
spec:
  running: false
  template:
    metadata:
      labels:
        kubevirt.io/vm: vm-ubuntu-volume
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
          claimName: my-ubuntu-volume
      - name: cloudinitdisk
        cloudInitNoCloud:
          userData: |
            #!/bin/sh
            echo 'printed from cloud-init userdata'
EOF
```

start VM:
```bash
virtctl start vm-ubuntu-volume
```

access console of VM:
```bash
virtctl console vm-ubuntu-volume
```

stop VM:
```bash
virtctl stop vm-ubuntu-volume
kubectl delete vm-ubuntu-volume
```


