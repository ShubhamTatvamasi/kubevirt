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

