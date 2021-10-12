# Create Container Disks

https://github.com/kubevirt/kubevirt/blob/main/docs/container-register-disks.md \
https://kubevirt.io/user-guide/virtual_machines/disks_and_volumes/

```Dockerfile
FROM scratch

ADD ubuntu2004.qcow2 /disk/
```

build docker image:
```bash
docker build -t shubhamtatvamasi/ubuntu2004:0.1.0 .
docker push shubhamtatvamasi/ubuntu2004:0.1.0
```
