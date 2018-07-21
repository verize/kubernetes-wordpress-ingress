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
$ kubectl create secret tls tls-certificate --key ./certs/tls-key.key --cert ./certs/tls-cert.crt
$ kubectl label secret tls-certificate app=wpdemo
```

Create Resources (Services, Ingress, Deployments ...):
```
$ kubectl apply -f secrets.yaml
$ kubectl apply -f db-tier-deployment.yaml
$ kubectl apply -f app-tier-deployment.yaml
$ kubectl apply -f nginx-ingress.yaml
```

Create local host for minikube.io domain (or edit it)
- Adds minikube.io to /etc/hosts file with Minikube cluster ip
- Example: 192.168.99.100  minikube.io

To get cluster IP:
```
$ minikube ip //Get Kubernetes Cluster IP
```

Test it!
```
$ minikube dashboard //Check cluster resources
```

Access https://minikube.io

Demo Wordpress Credentials:
- username: admin
- password: Demo12345!


Delete all resources
```
$ kubectl delete svc,pv,pvc,deployments,pods,secrets,ingress -l app=wpdemo
```

### TODO:
- Optimize NGINX Ingress configuration
- Generate installation sh script
- Configuration documentation