# Exercise 21 – Production ALB Ingress Setup

## Overview

This project demonstrates a production-style Kubernetes Ingress configuration using AWS Application Load Balancer (ALB) concepts. Three applications are exposed through a single Ingress using path-based routing with SSL termination, automatic HTTP to HTTPS redirection, and health check annotations.

> **Note:** This exercise was implemented and validated on a kubeadm-based Kubernetes cluster. The Ingress manifest is designed for Amazon EKS with the AWS Load Balancer Controller. In a local kubeadm environment, the Ingress resource is created successfully, but an actual AWS ALB is not provisioned.

---

## Objective

* Deploy three independent applications.
* Expose all applications through a single ALB Ingress.
* Configure path-based routing.
* Enable SSL support.
* Redirect HTTP traffic to HTTPS.
* Configure target group health checks.

---

## Architecture

```
                Internet
                    │
              AWS ALB (Production)
                    │
        ┌───────────┼───────────┐
        │           │           │
      /api       /admin     /dashboard
        │           │           │
 API Service  Admin Service  Dashboard Service
        │           │           │
   API Pods    Admin Pods   Dashboard Pods
```

---

## Project Structure

```
Exercise-21-Production-ALB-Ingress/
│
├── app1.yaml
├── app2.yaml
├── app3.yaml
├── service1.yaml
├── service2.yaml
├── service3.yaml
├── ingress.yaml
└── README.md
```

---

## Technologies Used

* Kubernetes
* kubeadm
* NGINX
* AWS ALB Ingress Annotations
* YAML
* Ubuntu
* Git
* GitHub

---

## Kubernetes Resources

### Deployments

* API Application
* Admin Application
* Dashboard Application

### Services

* api-service
* admin-service
* dashboard-service

### Ingress

* production-alb

---

## Implemented Features

* Path-based routing
* Single Ingress for multiple applications
* SSL certificate annotation
* HTTP to HTTPS redirection
* Target group health checks
* Internet-facing ALB configuration
* IP target type

---

## Routing Configuration

| Path         | Backend Service   |
| ------------ | ----------------- |
| `/api`       | api-service       |
| `/admin`     | admin-service     |
| `/dashboard` | dashboard-service |

---

## SSL Configuration

The Ingress includes an ACM certificate annotation for HTTPS.

```
alb.ingress.kubernetes.io/certificate-arn
```

---

## HTTP to HTTPS Redirect

HTTP requests are automatically redirected to HTTPS using:

```
alb.ingress.kubernetes.io/ssl-redirect: "443"
```

---

## Health Check

The ALB periodically checks the backend applications using:

```
/
```

Healthy targets continue receiving traffic, while unhealthy targets are removed from the load balancer until they recover.

---

## Validation

Resources verified during the exercise:

* Namespace created
* Three Deployments running
* Six Pods in Running state
* Three ClusterIP Services created
* One Ingress resource created successfully

---

## Commands Used

```bash
kubectl apply -f app1.yaml
kubectl apply -f app2.yaml
kubectl apply -f app3.yaml

kubectl apply -f service1.yaml
kubectl apply -f service2.yaml
kubectl apply -f service3.yaml

kubectl apply -f ingress.yaml

kubectl get pods -n production
kubectl get svc -n production
kubectl get ingress -n production
kubectl describe ingress production-alb -n production
```

---

## Learning Outcomes

Through this exercise, I learned how to:

* Configure Kubernetes Ingress resources.
* Implement path-based routing.
* Understand AWS ALB annotations.
* Configure SSL termination.
* Enable HTTP to HTTPS redirection.
* Configure ALB health checks.
* Design a production-style ingress architecture for Kubernetes applications.

---

## Conclusion

This exercise demonstrates how a production-ready AWS ALB Ingress can be configured for Kubernetes applications using path-based routing, SSL, health checks, and secure traffic redirection. Although implemented on a local kubeadm cluster, the manifest follows Amazon EKS best practices and is ready to be used with the AWS Load Balancer Controller in a production environment.
