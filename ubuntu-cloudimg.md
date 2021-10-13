# ubuntu cloudimg

```yaml
kubectl create -f - << EOF
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  labels:
    kubevirt.io/vm: vm-ubuntu-img
  name: vm-ubuntu-img
spec:
  running: true
  template:
    metadata:
      labels:
        kubevirt.io/vm: vm-ubuntu-img
    spec:
      domain:
        devices:
          disks:
          - disk:
              bus: virtio
            name: containerdisk
          - disk:
              bus: virtio
            name: cloudinitdisk
        resources:
          requests:
            cpu: 4
            memory: 4G
      terminationGracePeriodSeconds: 0
      volumes:
      - name: containerdisk
        containerDisk:
          image: shubhamtatvamasi/focal-server-cloudimg-amd64:0.1.0
      - name: cloudinitdisk
        cloudInitNoCloud:
          userDataBase64: I2Nsb3VkLWNvbmZpZwpwYXNzd29yZDogdWJ1bnR1CnNzaF9wd2F1dGg6IFRydWUKY2hwYXNzd2Q6IHsgZXhwaXJlOiBGYWxzZSB9Cg==
EOF
```
