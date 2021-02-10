# cdi

setup Ingress for cdi-uploadproxy:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: cdi-uploadproxy
  namespace: cdi
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt
    nginx.org/ssl-services: cdi-uploadproxy
    nginx.org/client-max-body-size: 0m
spec:
  tls:
    - hosts:
      - cdi-uploadproxy.k8s.shubhamtatvamasi.com
      secretName: letsencrypt-cdi-uploadproxy
  rules:
    - host: cdi-uploadproxy.k8s.shubhamtatvamasi.com
      http:
        paths:
        - backend:
            serviceName: cdi-uploadproxy
            servicePort: 443
EOF
```

upload image:
```bash
virtctl image-upload dv cirros-vm-dv \
  --size=1Gi \
  --image-path=/home/$USER/Downloads/cirros-0.5.1-x86_64-disk.img \
  --uploadproxy-url=https://cdi-uploadproxy.k8s.shubhamtatvamasi.com
```

Check data volume status:
```bash
kubectl get dv
```

Create Virtual Machine Instance:
```
kubectl apply -f - << EOF
apiVersion: kubevirt.io/v1
kind: VirtualMachineInstance
metadata:
  name: cirros-vm
spec:
  domain:
    resources:
      requests:
        memory: 64M
    devices:
      disks:
      - name: dvdisk
        disk:
          bus: virtio
  volumes:
  - name: dvdisk
    dataVolume:
      name: cirros-vm-dv
EOF
```

Check vmi:
```bash
kubectl get vmi
```

