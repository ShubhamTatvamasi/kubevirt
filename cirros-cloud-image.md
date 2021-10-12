# cirros cloud image

create volume with disk image:
```yaml
kubectl create -f - << EOF
apiVersion: cdi.kubevirt.io/v1beta1
kind: DataVolume
metadata:
  name: my-data-volume
spec:
  source:
      http:
         url: "https://download.cirros-cloud.net/0.4.0/cirros-0.4.0-x86_64-disk.img"
  pvc:
    accessModes:
      - ReadWriteOnce
    resources:
      requests:
        storage: 500Mi
EOF
```



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
