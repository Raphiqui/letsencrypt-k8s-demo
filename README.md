z# letsencrypt-k8s-demo

## Purpose and Scope

This project serves as a practical demonstration of automated TLS certificate management in Kubernetes using Let's Encrypt and cert-manager. The system showcases how to deploy a simple nginx web application with fully automated HTTPS certificate provisioning and renewal using DNS-01 challenges through Cloudflare.

## System Architecture Overview

The demo system consists of five core Kubernetes resources that work together to provide a TLS-enabled web application with automated certificate management:

| Component           | Resource Type | Name                        | Primary Function                             |
| :---                |     :----:    |           :----:            | :---                                         |
| Application         | Deployment    | nginx-deployment            | Runs nginx web server                        |
| Networking          | Service       | nginxservice                | Exposes nginx pods internally                |
| External Access     | Ingress       | nginx-ingress               | Routes external traffic with TLS termination |
| Certificate Request | Certificate   | nginx-tls                   | Defines certificate requirements             |
| Certificate Issuer  | Issuer        | le-dns01-cloudflare-staging | Handles Let's Encrypt integration            |

## High-Level System Architecture

![High-Level System Architecture](/doc/images/hl.png "High-Level System Architecture")

## Core Components Overview

The system implements a three-tier architecture with clear separation between application logic, networking, and certificate management:

### Application Tier

 - nginx-deployment: Manages a single replica of nginx pods using the nginx:latest image
 - nginxservice: Provides internal load balancing and service discovery for nginx pods on port 80
 - Label selector: app: nginxdeployment links the Service to Deployment pods

### Network Tier

 - nginx-ingress: Handles external traffic routing to app.norsse.org domain
 - TLS termination using certificates stored in the nginx-tls Secret
 - Routes all traffic to nginxservice on port 80

### Certificate Management Tier

 - le-dns01-cloudflare-staging: Issuer configured for Let's Encrypt staging environment
 - nginx-tls: Certificate resource requesting TLS certificate for app.norsse.org
 - Automated DNS-01 challenge resolution through Cloudflare API integration

## Notes

Use of DNS-01 instead of HTTP-01 because it works with a private domain not accessible through the internet 
which is not the case with HTTP-01 which will need Let's Encrypt to access the nginx server.

To perform this task I had to buy a domain under Cloudflare and create a subdomain (DNS section).

Installing `cert-manager` which will create a namespace under the minikube cluster.

The secret is needed under the server to validate the challenge.

Hosts file was updated.

Some useful commands:

```
minikube dashboard # starts the dashboard to be accessed from the browser
kubectl -n ingress-nginx port-forward svc/ingress-nginx-controller 8080:80 8443:443 # (HTTP and HTTPS)
kubectl describe certificate
```

A challenge must appear if everything worked (removed when done).

By doing kubectl describe certificate it mush display the status of the actuall challenge

It might be needed to wait up to 10 minutes for the all process to be done.

[![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/Raphiqui/letsencrypt-k8s-demo)