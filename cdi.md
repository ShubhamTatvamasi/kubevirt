# cdi

setup Ingress for cdi-uploadproxy:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: uploadproxy
  namespace: cdi
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
    - hosts:
      - uploadproxy.k8s.shubhamtatvamasi.com
      secretName: letsencrypt-uploadproxy
  rules:
    - host: uploadproxy.k8s.shubhamtatvamasi.com
      http:
        paths:
        - backend:
            serviceName: cdi-uploadproxy
            servicePort: 443
EOF
```

