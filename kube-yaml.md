# Kubernetes `yaml` files

- [Kubernetes 1.26 API docs](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.26/)

```yaml
# Ingress with TLS
# https://opensource.com/article/20/3/ssl-letsencrypt-k3s

apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: mysite-nginx-ingress
  annotations:
    kubernetes.io/ingress.class: "traefik"
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  rules:
  # matches URL patterns to service)
  - host: mattfeng.xyz # YOUR DOMAIN HERE
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: mattfeng-xyz-service # YOUR SERVICE HERE
            port:
              number: 80
  tls:
  - hosts: # subject alterative names (SANs) for certificate
    - mattfeng.xyz # YOUR DOMAIN HERE
    secretName: mattfeng-xyz-tls
```

