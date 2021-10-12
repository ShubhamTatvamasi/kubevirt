# ubuntu iso

create Data Volume:
```yaml
kubectl create -f - << EOF
apiVersion: cdi.kubevirt.io/v1beta1
kind: DataVolume
metadata:
  name: ubuntu-iso-volume
spec:
  source:
    http:
      url: "http://192.168.5.66/ubuntu-20.04.3-live-server-amd64.iso"
  pvc:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 20Gi
EOF
```

create VM:
```yaml
kubectl create -f - << EOF
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  name: vm-ubuntu-iso
spec:
  running: false
  template:
    spec:
      domain:
        devices:
          disks:
          - disk:
              bus: virtio
            name: pvcdisk1
        resources:
          requests:
            cpu: 4
            memory: 4G
      terminationGracePeriodSeconds: 0
      volumes:
      - name: pvcdisk1
        persistentVolumeClaim:
          claimName: ubuntu-iso-volume
EOF
```

start vm:
```bash
virtctl start vm-ubuntu-iso
```

access console of VM:
```bash
virtctl console vm-ubuntu-iso
```

stop VM:
```bash
virtctl stop vm-ubuntu-iso
```

delete VM:
```bash
kubectl delete vm-ubuntu-iso
```


