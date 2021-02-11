Create ubuntu image:
```bash
virtctl image-upload dv ubuntu-vm-dv \
  --size=1.3Gi \
  --image-path=/root/ubuntu-20.04.2-live-server-amd64.iso \
  --uploadproxy-url=https://cdi-uploadproxy.k8s.shubhamtatvamasi.com
```

Create Virtual Machine Instance:
```bash
kubectl apply -f - << EOF
apiVersion: kubevirt.io/v1
kind: VirtualMachineInstance
metadata:
  name: ubuntu-vm
spec:
  domain:
    resources:
      requests:
        memory: 512M
    devices:
      disks:
      - name: dvdisk
        disk:
          bus: virtio
  volumes:
  - name: dvdisk
    dataVolume:
      name: ubuntu-vm-dv
EOF
```
