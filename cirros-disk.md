# cirros disk

```yaml
kubectl create -f - << EOF
apiVersion: kubevirt.io/v1
kind: VirtualMachine
metadata:
  labels:
    kubevirt.io/vm: vm-cirros-img
  name: vm-cirros-img
spec:
  running: true
  template:
    metadata:
      labels:
        kubevirt.io/vm: vm-cirros-img
    spec:
      domain:
        devices:
          disks:
          - name: cirros-pvc
            disk:
              bus: virtio
          - name: containerdisk
            disk:
              bus: virtio
          - name: cloudinitdisk
            disk:
              bus: virtio
        resources:
          requests:
            cpu: 4
            memory: 4G
      terminationGracePeriodSeconds: 0
      volumes:
      - name: cirros-pvc
        persistentVolumeClaim:
          claimName: cirros-pvc
      - name: containerdisk
        containerDisk:
          image: shubhamtatvamasi/cirros-disk:0.1.0
      - name: cloudinitdisk
        cloudInitNoCloud:
          userData: |
            #!/bin/sh
            echo 'printed from cloud-init userdata'
EOF
```
