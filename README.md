# thingsboard

create a deployment and service:
```bash
kubectl create deployment thingsboard --image=thingsboard/tb
kubectl expose deployment thingsboard --port=9090 --name=thingsboard
```

Ingress value for thingsboard:
```bash
kubectl apply -f - << EOF
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: thingsboard
  annotations:
    nginx.org/websocket-services: thingsboard
    cert-manager.io/cluster-issuer: letsencrypt
spec:
  tls:
  - hosts:
      - thingsboard.k8s.shubhamtatvamasi.com
    secretName: thingsboard-tls
  rules:
  - host: thingsboard.k8s.shubhamtatvamasi.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: thingsboard
            port:
              number: 9090
EOF
```

Use the following default credentials:

 
Role | ID | Password
---|---|---
Systen Administrator: | sysadmin@thingsboard.org | sysadmin
Tenant Administrator: | tenant@thingsboard.org | tenant
Customer User: | customer@thingsboard.org | customer


delete everything:
```bash
kubectl delete deploy/thingsboard svc/thingsboard ing/thingsboard
```
