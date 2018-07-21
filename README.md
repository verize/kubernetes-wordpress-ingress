## Kubernetes NGINX-Ingress + Wordpress9.5 + MariaDB + Persistent Volumes

### Description

This is a template project for deploy on Kubernetes (Minikube) a Wordpress 9.5 application with well-optimized nginx ingress controller, config Maps, secrets and more.

- Author: Gonzalo Plaza <gonzalo@verize.com>
- Version: 1.0.0

### Requirements:

- Minikube installed locally
- Kubectl v1.11.1

## Reference

- https://hub.docker.com/r/bitnami/wordpress/
- https://hub.docker.com/r/bitnami/mariadb/

### Installation:

Generate certs manually:

```
$ mkdir certs
$ openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ./certs/tls-key.key -out -/certs/tls-cert.crt  
```

Create k8s secret for certs:

```
$ kubectl create secret tls tls-certificate --key ./certs/tls-key.key --cert ./certs/tls-cert.crt -l app=wpdemo
$ kubectl label secret tls-certificate app=wpdemo
```

Create Resources (Services, Ingress, Deployments ...):
```
$ kubectl apply -f secrets.yaml
$ kubectl apply -f db-tier-deployment.yaml
$ kubectl apply -f app-tier-deployment.yaml
$ kubectl apply -f nginx-ingress.yaml
```

Test it!
```
$ minikube ip
$ minukube dashboard //Check node resources
```


Delete all resources
```
$ kubectl delete svc,pv,pvc,deployments,pods,secrets,ingress -l app=wpdemo
```