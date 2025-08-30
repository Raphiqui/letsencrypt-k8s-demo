# letsencrypt-k8s-demo

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

[![High-Level System Architecture](/doc/images/High-Level\ System\ Architecture.PNG "High-Level System Architecture")]

