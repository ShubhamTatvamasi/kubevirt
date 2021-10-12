# ubuntu iso


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


