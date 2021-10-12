# cirros cloud image

create volume with disk image:
```yaml
kubectl create -f - << EOF
apiVersion: cdi.kubevirt.io/v1beta1
kind: DataVolume
metadata:
  name: cirros-volume
spec:
  source:
      http:
         url: "https://download.cirros-cloud.net/0.5.2/cirros-0.5.2-x86_64-disk.img"
  pvc:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 1G
EOF
```

create a VM:
```yaml
kubectl create -f - << EOF
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  labels:
    kubevirt.io/vm: vm-cirros-volume
  name: vm-cirros-volume
spec:
  running: false
  template:
    metadata:
      labels:
        kubevirt.io/vm: vm-cirros-volume
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
            cpu: 2
            memory: 512Mi
      terminationGracePeriodSeconds: 0
      volumes:
      - name: pvcdisk1
        persistentVolumeClaim:
          claimName: cirros-volume
      - name: cloudinitdisk
        cloudInitNoCloud:
          userData: |
            #!/bin/sh
            echo 'printed from cloud-init userdata'
EOF
```

start VM:
```bash
virtctl start vm-cirros-volume
```

stop VM:
```bash
virtctl stop vm-cirros-volume
kubectl delete vm vm-cirros-volume
```

---

# docker VMI

docker image:
```yaml
kubectl create -f - << EOF
apiVersion: kubevirt.io/v1
kind: VirtualMachineInstance
metadata:
  labels:
    kubevirt.io/flavor: small
  name: vmi-flavor-small
spec:
  domain:
    devices:
      disks:
      - disk:
          bus: virtio
        name: containerdisk
    resources:
      requests:
        memory: 128Mi
  terminationGracePeriodSeconds: 0
  volumes:
  - containerDisk:
      image: kubevirt/cirros-container-disk-demo:devel
    name: containerdisk
EOF
```
