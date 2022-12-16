# training_k8s_cks

# Table of Contents
- [training\_k8s\_cks](#training_k8s_cks)
- [Table of Contents](#table-of-contents)
- [1. Introduction](#1-introduction)
  - [1.1. Welcome](#11-welcome)
  - [1.2. K8s Security Best Practices](#12-k8s-security-best-practices)
    - [K8s Security Categories](#k8s-security-categories)
      - [Host Operating System Security (Ex. Linux)](#host-operating-system-security-ex-linux)
      - [Kubernetes Cluster Security (Ex. Kubernetes)](#kubernetes-cluster-security-ex-kubernetes)
      - [Application Security (Ex. Container)](#application-security-ex-container)
- [2. Create your course K8S cluster](#2-create-your-course-k8s-cluster)
  - [2.1. Cluster Specification](#21-cluster-specification)
  - [2.2. Configure gcloud command](#22-configure-gcloud-command)
  - [2.3. Create Kubeadm Cluster in GCP](#23-create-kubeadm-cluster-in-gcp)
  - [2.4. Firewall rules for NodePorts](#24-firewall-rules-for-nodeports)
  - [2.5. Containerd Course Upgrade](#25-containerd-course-upgrade)
  - [2.6. Recap](#26-recap)
- [3. Foundation](#3-foundation)
  - [3.1. Kubernetes Secure Arquitecture](#31-kubernetes-secure-arquitecture)
    - [3.1.1. Intro](#311-intro)
    - [3.1.2. Find various k8s certificates](#312-find-various-k8s-certificates)
  - [3.2. Containers under the hood](#32-containers-under-the-hood)
    - [3.2.1. Intro](#321-intro)
      - [3.2.1.1. Container and Image](#3211-container-and-image)
    - [3.2.2. Test Tools Introduction](#322-test-tools-introduction)
      - [Container tools](#container-tools)
        - [Dockerfile](#dockerfile)
        - [Build Dockerfile](#build-dockerfile)
        - [List image](#list-image)
        - [Docker run](#docker-run)
        - [Podman build](#podman-build)
        - [Podman run](#podman-run)
    - [3.2.3. The PID Namespace](#323-the-pid-namespace)
      - [Run c1 container](#run-c1-container)
      - [Show process on c1 container](#show-process-on-c1-container)
      - [Run c2 container](#run-c2-container)
      - [Show process on c2 container](#show-process-on-c2-container)
      - [Show process on host](#show-process-on-host)
      - [Delete c2 container](#delete-c2-container)
      - [Recreate container with same namespace.](#recreate-container-with-same-namespace)
      - [Show process on c2 container (you can see other container process).](#show-process-on-c2-container-you-can-see-other-container-process)
      - [Show process on c1 container (you can see other container process).](#show-process-on-c1-container-you-can-see-other-container-process)
    - [3.2.4. Recap](#324-recap)
- [5. Cluster setup](#5-cluster-setup)
  - [5.1. Network Policies](#51-network-policies)
    - [5.1.1. Introduction 1](#511-introduction-1)
      - [NetworkPolicies](#networkpolicies)
      - [Without NetworkPolicies](#without-networkpolicies)
    - [5.1.2. Introduction 2](#512-introduction-2)
      - [NetworkPolicy example](#networkpolicy-example)
      - [Multiple NetworkPolicies](#multiple-networkpolicies)
        - [Merge example2a + example2b](#merge-example2a--example2b)
    - [5.1.3. Default Deny](#513-default-deny)
      - [Create a frontend and backend applications and expose.](#create-a-frontend-and-backend-applications-and-expose)
      - [Test connection between applications](#test-connection-between-applications)
      - [Create the NetworkPolicy](#create-the-networkpolicy)
      - [Test connection between apps](#test-connection-between-apps)
    - [5.1.4. Frontend to Backend traffic](#514-frontend-to-backend-traffic)
      - [Test connection](#test-connection)
      - [Test connection](#test-connection-1)
      - [If you want connect to DNS, you indicate Port 53](#if-you-want-connect-to-dns-you-indicate-port-53)
    - [5.1.5. Backend to database traffic](#515-backend-to-database-traffic)
      - [Create a namespace](#create-a-namespace)
      - [Create a Pod](#create-a-pod)
      - [Get Pod Cassandra IP](#get-pod-cassandra-ip)
      - [Test connection](#test-connection-2)
      - [Apply egress to cassandra namespace](#apply-egress-to-cassandra-namespace)
      - [Test connection](#test-connection-3)
      - [Create configuration to Deny all to cassandra Pod](#create-configuration-to-deny-all-to-cassandra-pod)
      - [And create NetworkPolicy to cassandra ingress from default](#and-create-networkpolicy-to-cassandra-ingress-from-default)
      - [Labeled default namespace and launch curl](#labeled-default-namespace-and-launch-curl)
    - [5.1.6. Recap](#516-recap)
  - [5.2. GUI Elements](#52-gui-elements)
    - [5.2.1. Introduction](#521-introduction)
      - [Gui Elements and the Dashboard](#gui-elements-and-the-dashboard)
      - [Kubectl proxy](#kubectl-proxy)
      - [Kubectl port-forward](#kubectl-port-forward)
      - [Ingress](#ingress)
    - [5.2.2. Install Dashboard](#522-install-dashboard)
      - [Deploy Dashboard](#deploy-dashboard)
      - [Get objects](#get-objects)
    - [5.2.3. Outside Insecure Access](#523-outside-insecure-access)
      - [Expose insecure Dashboard](#expose-insecure-dashboard)
    - [5.2.4. RBAC for the dashboard](#524-rbac-for-the-dashboard)
    - [5.2.5. Recap](#525-recap)
      - [Interesting dashboard security arguments](#interesting-dashboard-security-arguments)
  - [5.3. Secure Ingress](#53-secure-ingress)
    - [5.3.3. Create an Ingress](#533-create-an-ingress)
      - [Setup an example Ingress](#setup-an-example-ingress)
    - [5.3.4. Secure an Ingress](#534-secure-an-ingress)
    - [5.3.5. Recap](#535-recap)
  - [5.4. Node metadata protection](#54-node-metadata-protection)
    - [5.4.1. Introduction](#541-introduction)
      - [Cloud Platform Node Metadata](#cloud-platform-node-metadata)
      - [Limit permissions for instance credentials](#limit-permissions-for-instance-credentials)
    - [5.4.2. Access Node Metadata](#542-access-node-metadata)
    - [5.4.3. Protect Node Metadata via Network Policy](#543-protect-node-metadata-via-network-policy)
      - [All pods in namespace cannot access metadata endpoint](#all-pods-in-namespace-cannot-access-metadata-endpoint)
      - [Only pods with label are allowed to access metadata endpoint](#only-pods-with-label-are-allowed-to-access-metadata-endpoint)
      - [Labeled Pod](#labeled-pod)
  - [5.5. CIS Bechmarck](#55-cis-bechmarck)
    - [5.5.1. Introduction](#551-introduction)
      - [CIS - Center for Internet Security](#cis---center-for-internet-security)
    - [5.5.2. CIS in action](#552-cis-in-action)
    - [5.5.3. kube-bench](#553-kube-bench)
      - [How to run](#how-to-run)
    - [5.5.4. Recap](#554-recap)
  - [5.6. Verify Platform Binaries](#56-verify-platform-binaries)
    - [5.6.1. Verify apiserver binary running in our cluster](#561-verify-apiserver-binary-running-in-our-cluster)
    - [5.6.4. Recap](#564-recap)
- [6. Cluster Hardening](#6-cluster-hardening)
  - [6.1. RBAC](#61-rbac)
    - [6.1.1. Intro](#611-intro)
      - [RBAC](#rbac)
      - [POLP (Principle Of Least Privilege)](#polp-principle-of-least-privilege)
      - [RBAC- Namespaced Resources vs Cluster Resources](#rbac--namespaced-resources-vs-cluster-resources)
      - [RoleBinding](#rolebinding)
      - [ClusterRoleBinding](#clusterrolebinding)
    - [6.1.2. Role and Rolebinding](#612-role-and-rolebinding)
    - [6.1.3. ClusterRole and ClusterRoleBinding](#613-clusterrole-and-clusterrolebinding)
    - [6.1.4. Accounts and Users](#614-accounts-and-users)
    - [6.1.5. CertificateSingingRequets](#615-certificatesingingrequets)
      - [Users and Certificates](#users-and-certificates)
    - [6.1.6. Recap](#616-recap)
  - [6.2. Exercise caution in using ServiceAccounts](#62-exercise-caution-in-using-serviceaccounts)
    - [6.2.1. Intro](#621-intro)
      - [Accounts](#accounts)
      - [ServiceAccounts and Pods](#serviceaccounts-and-pods)
    - [6.2.2. Pods uses custom ServiceAccount](#622-pods-uses-custom-serviceaccount)
    - [6.2.3. Disable ServiceAccount Mounting](#623-disable-serviceaccount-mounting)
    - [6.2.4. Limits ServiceAccounts using RBAC](#624-limits-serviceaccounts-using-rbac)
    - [6.2.5. Recap](#625-recap)
  - [6.3. Restrict API Access](#63-restrict-api-access)
    - [6.3.1. Intro](#631-intro)
      - [Request workflow](#request-workflow)
      - [Restrictions](#restrictions)
    - [6.3.2. Anonymous Access](#632-anonymous-access)
      - [Anonymous Access](#anonymous-access)
        - [Set anonymous-auth=false](#set-anonymous-authfalse)
    - [6.3.3. Insecure Access](#633-insecure-access)
      - [HTTP/HTTPS Access](#httphttps-access)
      - [Insecure Access](#insecure-access)
    - [6.3.4. Manual API Requests](#634-manual-api-requests)
    - [6.3.5. NodeRestriction AdmissionController](#635-noderestriction-admissioncontroller)
      - [NodeRestriction](#noderestriction)
    - [6.3.6. Verify NodeRestriction](#636-verify-noderestriction)
    - [6.3.7. Recap](#637-recap)
  - [6.4. Upgrade Kubernetes](#64-upgrade-kubernetes)
    - [6.4.1. Intro](#641-intro)
      - [Why upgrade frequently?](#why-upgrade-frequently)
      - [Kubernetes Release Cycles](#kubernetes-release-cycles)
      - [Support](#support)
      - [How to upgrade a cluster](#how-to-upgrade-a-cluster)
      - [How to upgrade a node](#how-to-upgrade-a-node)
      - [How to make your application survive an upgrade](#how-to-make-your-application-survive-an-upgrade)
    - [6.4.2. Ubuntu 20.04 Update](#642-ubuntu-2004-update)
    - [6.4.3. Create outdated cluster](#643-create-outdated-cluster)
    - [6.4.4. Upgrade controlplane node](#644-upgrade-controlplane-node)
    - [6.4.5. Upgrade node](#645-upgrade-node)
    - [6.4.6. Recap](#646-recap)
- [7. Microservice Vulnerabilities](#7-microservice-vulnerabilities)
  - [7.1. Manage Kubernetes](#71-manage-kubernetes)
    - [7.1.1. Intro](#711-intro)
    - [7.1.2. Create Simple Secret Scenario](#712-create-simple-secret-scenario)
      - [Create a generic secret](#create-a-generic-secret)
      - [Mount secret in a Pod](#mount-secret-in-a-pod)
    - [7.1.3. Hacks Secret in Container Runtime](#713-hacks-secret-in-container-runtime)
      - [Search "mypod"](#search-mypod)
      - [Inspect container and show "envs" and "mounts"](#inspect-container-and-show-envs-and-mounts)
    - [7.1.4. Hacks Secret in ETCD](#714-hacks-secret-in-etcd)
      - [Access secret int etcd](#access-secret-int-etcd)
      - [Show secret](#show-secret)
    - [7.1.5. ETCD Encryption](#715-etcd-encryption)
      - [Encrypt](#encrypt)
      - [Encrypt (all Secrets) in ETCD](#encrypt-all-secrets-in-etcd)
      - [Decrypt all Secrets in ETCD](#decrypt-all-secrets-in-etcd)
    - [7.1.6. Encrypt ETCD (example)](#716-encrypt-etcd-example)
      - [/etc/kubernetes/etcd/ec.yaml](#etckubernetesetcdecyaml)
      - [Edit API Server](#edit-api-server)
      - [Encrypt existing Secrets](#encrypt-existing-secrets)
    - [7.1.7. Recap](#717-recap)
  - [7.2. Container Runtime](#72-container-runtime)
    - [7.2.1. Intro](#721-intro)
      - [Technical Overview](#technical-overview)
      - [Technical Overview: Containers/Docker](#technical-overview-containersdocker)
      - [Technical Overview: Sandbox](#technical-overview-sandbox)
      - [Technical Overview: Containers and system calls](#technical-overview-containers-and-system-calls)
      - [Technical Overview: Sandbox comes not for free](#technical-overview-sandbox-comes-not-for-free)
    - [7.2.2. Containers Calls Linux Kernel](#722-containers-calls-linux-kernel)
      - [Why even sandbox?](#why-even-sandbox)
    - [7.2.3. Open Container Iniciative OCI](#723-open-container-iniciative-oci)
      - [OCI - Open Container Initiative](#oci---open-container-initiative)
      - [Kubernetes runtimes and CRI (Container Runtime Interface)](#kubernetes-runtimes-and-cri-container-runtime-interface)
    - [7.2.4. Sandbox Runtime Katacontainers](#724-sandbox-runtime-katacontainers)
      - [kata containers](#kata-containers)
    - [7.2.5. Sandbox Runtime gVisor (Google)](#725-sandbox-runtime-gvisor-google)
    - [7.2.6. Create and use RuntimeClasses](#726-create-and-use-runtimeclasses)
      - [RuntimeClassess](#runtimeclassess)
      - [Create Pod](#create-pod)
      - [Describe pod](#describe-pod)
    - [7.2.7. Install and use gVisor](#727-install-and-use-gvisor)
      - [Install](#install)
    - [7.2.8. Recap](#728-recap)
  - [7.3. OS Level Security](#73-os-level-security)
    - [7.3.1. Intro and Security Context](#731-intro-and-security-context)
      - [Security Context](#security-context)
    - [7.3.2. Set container User and Group](#732-set-container-user-and-group)
      - [security Contexts \& UID GID](#security-contexts--uid-gid)
    - [7.3.3. Force container non-root](#733-force-container-non-root)
    - [7.3.4. Privileged Containers](#734-privileged-containers)
      - [Privileged Containers in Kubernetes](#privileged-containers-in-kubernetes)
    - [7.3.5. Created Privileged Containers](#735-created-privileged-containers)
    - [7.3.6. PrivilegeScalation](#736-privilegescalation)
    - [7.3.7. Disable PrivilegeScalation](#737-disable-privilegescalation)
    - [7.3.8. PodSecurityPolicies](#738-podsecuritypolicies)
      - [Pod Security Policies](#pod-security-policies)
    - [7.3.9. Create and enable PodSecurityPolicies](#739-create-and-enable-podsecuritypolicies)
    - [7.3.10. Recap](#7310-recap)
  - [7.4. mTLS](#74-mtls)
    - [7.4.1. Intro](#741-intro)
      - [mTLS - Mutual TLS](#mtls---mutual-tls)
    - [7.4.2. Create sidecar proxy](#742-create-sidecar-proxy)
      - [Without Capabilities](#without-capabilities)
      - [With Capabilities](#with-capabilities)
    - [7.4.3. Recap](#743-recap)
- [8. Open Policy Agent (OPA)](#8-open-policy-agent-opa)
  - [8.1. Introduction](#81-introduction)
    - [OPA - Open Policy Agent](#opa---open-policy-agent)
    - [OPA - Gatekeeper](#opa---gatekeeper)
  - [8.2. Install OPA](#82-install-opa)
  - [8.3. Deny All Policy](#83-deny-all-policy)
  - [8.4. Enforce Namespace Labels](#84-enforce-namespace-labels)
  - [8.5. Enforce Deployment Replica](#85-enforce-deployment-replica)
  - [8.6. The Rego Playground and more examples](#86-the-rego-playground-and-more-examples)
  - [8.7. Recap](#87-recap)
- [9. Supply Chain Security](#9-supply-chain-security)
  - [9.1. Image footprint](#91-image-footprint)
    - [9.1.1. Introduction](#911-introduction)
      - [Containers and Docker - Layers](#containers-and-docker---layers)
    - [9.1.2. Reduce image Footprint with Multi-Stage](#912-reduce-image-footprint-with-multi-stage)
      - [Build image with app code](#build-image-with-app-code)
      - [Show size](#show-size)
      - [Rebuild](#rebuild)
      - [Show size](#show-size-1)
    - [9.1.3. Secure and Harden images](#913-secure-and-harden-images)
      - [Use specifig package version](#use-specifig-package-version)
      - [Dont run as root](#dont-run-as-root)
      - [Make filesystem read only](#make-filesystem-read-only)
      - [Remove shell access](#remove-shell-access)
    - [9.1.4. Recap](#914-recap)
  - [9.2. Static Analysis](#92-static-analysis)
    - [9.2.1. Introduction](#921-introduction)
      - [Static Analysis](#static-analysis)
      - [Static Analysis Rules](#static-analysis-rules)
      - [Static Analysis in CI/CD](#static-analysis-in-cicd)
      - [Manual Check](#manual-check)
        - [Insecure](#insecure)
        - [Secure](#secure)
    - [9.2.2. Kubesec](#922-kubesec)
    - [9.2.3. Practice Kubesec](#923-practice-kubesec)
    - [9.2.4. OPA Conftest](#924-opa-conftest)
    - [9.2.5. OPA Conftest for K8s YAML](#925-opa-conftest-for-k8s-yaml)
      - [Fixed](#fixed)
    - [9.2.6. OPA Conftest for Dockerfile](#926-opa-conftest-for-dockerfile)
    - [9.2.7. Recap](#927-recap)
  - [9.3. Image Vulnerability Scanning](#93-image-vulnerability-scanning)
    - [9.3.1. Introduction](#931-introduction)
      - [Known Image Vulnerabilities](#known-image-vulnerabilities)
    - [9.3.2. Clair and Trivy](#932-clair-and-trivy)
      - [Clair](#clair)
      - [Trivy](#trivy)
    - [9.3.3. Use Trivy to scan images](#933-use-trivy-to-scan-images)
    - [9.3.4. Recap](#934-recap)
  - [9.4. Secure Supply Chain](#94-secure-supply-chain)
    - [9.4.1. Introduction](#941-introduction)
      - [K8s and Container Registries](#k8s-and-container-registries)
    - [9.4.2. Image Digest](#942-image-digest)
    - [9.4.3. Whitelist Registries with OPA](#943-whitelist-registries-with-opa)
    - [9.4.4. ImagePolicyWebhook](#944-imagepolicywebhook)
    - [9.4.5. Practice ImagePolicyWebhook](#945-practice-imagepolicywebhook)
    - [9.4.6. Recap](#946-recap)
- [10. Runtime Security](#10-runtime-security)
  - [10.1. Behavioral Analytics at host and ...](#101-behavioral-analytics-at-host-and-)
    - [10.1.1. Introduction](#1011-introduction)
      - [Kernel vs User Space](#kernel-vs-user-space)
    - [10.1.2. Strace](#1012-strace)
      - [strace: show syscalls](#strace-show-syscalls)
    - [10.1.3. Strace and /proc on ETCD](#1013-strace-and-proc-on-etcd)
      - [/prod directory](#prod-directory)
      - [strace and /proc: etcd](#strace-and-proc-etcd)
    - [10.1.4. /proc and env variables](#1014-proc-and-env-variables)
    - [10.1.5. Falco and Installation](#1015-falco-and-installation)
      - [Falco](#falco)
    - [10.1.7. Investigate Falco rules](#1017-investigate-falco-rules)
    - [10.1.8. Change Falco rule](#1018-change-falco-rule)
    - [10.1.9. Recap](#1019-recap)
  - [10.2. Inmutability of containers at runtime](#102-inmutability-of-containers-at-runtime)
    - [10.2.1. Introduction](#1021-introduction)
      - [Inmutability](#inmutability)
    - [10.2.2. Ways to enforce immutability](#1022-ways-to-enforce-immutability)
      - [Enforce on Container Image Level](#enforce-on-container-image-level)
      - [Make manual changes to container - Command ?](#make-manual-changes-to-container---command-)
      - [Make manual changes to container - StartupProbe ?](#make-manual-changes-to-container---startupprobe-)
      - [Enforce Read-Only Root Filesystem](#enforce-read-only-root-filesystem)
      - [Move logic to InitContainer ?](#move-logic-to-initcontainer-)
    - [10.2.3. StartupProbe changes container](#1023-startupprobe-changes-container)
      - [StartupProbe for Immutability](#startupprobe-for-immutability)
    - [10.2.4. SecurityContext renders container immutable](#1024-securitycontext-renders-container-immutable)
      - [Enforce RO-filesystem](#enforce-ro-filesystem)
    - [10.2.5. Recap](#1025-recap)
  - [10.3. Auditing](#103-auditing)
    - [10.3.1. Introduction](#1031-introduction)
      - [Audit Logs - Introduction](#audit-logs---introduction)
      - [API Request Stages](#api-request-stages)
      - [Audit Policy - Waht data to store?](#audit-policy---waht-data-to-store)
      - [Audit Backends - Where to store all that data?](#audit-backends---where-to-store-all-that-data)
      - [Audit Logs - Overview](#audit-logs---overview)
    - [10.3.2. Enable Auditing Logging in Apiserver](#1032-enable-auditing-logging-in-apiserver)
      - [Setup Audit Logs](#setup-audit-logs)
    - [10.3.3. Create Secret and check Audit Logs](#1033-create-secret-and-check-audit-logs)
    - [10.3.4. Create advanced Audit Policy](#1034-create-advanced-audit-policy)
    - [10.3.5. Recap](#1035-recap)
- [11. System Hardening](#11-system-hardening)
  - [11.1. Kernel Hardening Tools](#111-kernel-hardening-tools)
    - [11.1.1. Introduction](#1111-introduction)
      - [Linux Kernel Isolation](#linux-kernel-isolation)
      - [Kernel vs User Space](#kernel-vs-user-space-1)
      - [Overview](#overview)
    - [11.1.2. AppArmor](#1112-apparmor)
      - [AppArmor](#apparmor)
      - [Main Commands](#main-commands)
    - [11.1.3. AppArmor for curl](#1113-apparmor-for-curl)
    - [11.1.4. AppArmor for Docker Nginx](#1114-apparmor-for-docker-nginx)
    - [11.1.5. AppArmor for Kubernetes Nginx](#1115-apparmor-for-kubernetes-nginx)
    - [11.1.6. Seccomp](#1116-seccomp)
    - [11.1.7. Seccomp for Docker Nginx](#1117-seccomp-for-docker-nginx)
    - [11.1.8. Seccomp for Kubernetes Nginx](#1118-seccomp-for-kubernetes-nginx)
    - [11.1.9. Recap](#1119-recap)
  - [11.2. Reduce Attack Surface](#112-reduce-attack-surface)
    - [11.2.1. Introduction](#1121-introduction)
      - [Overview](#overview-1)
      - [Nodes that run Kubernetes](#nodes-that-run-kubernetes)
      - [Linux Distributions](#linux-distributions)
      - [Open Ports](#open-ports)
      - [Port used by which application?](#port-used-by-which-application)
      - [Running Services](#running-services)
      - [Processes and Users](#processes-and-users)
    - [11.2.2. Systemctl and Services](#1122-systemctl-and-services)
    - [11.2.3. Install and investigate Services](#1123-install-and-investigate-services)
    - [11.2.4. Disabled application listening on port](#1124-disabled-application-listening-on-port)
    - [11.2.5. Investigate Linux Users](#1125-investigate-linux-users)
    - [11.2.6. Recap](#1126-recap)
- [12. Linux Foundation Simulator Sessions](#12-linux-foundation-simulator-sessions)

# 1. Introduction
## 1.1. Welcome
This are my notes to prepare CKS exam.

## 1.2. K8s Security Best Practices
### K8s Security Categories
#### Host Operating System Security (Ex. Linux)
* Kubernetes Nodes should only do one thing: Kubernetes
* Reduce Attack Surface
  * Remove unnecessary applications
  * Keep up to date
* Runtime Security Tools
* Find and identify malicious processes
* Restrict IAM/SSH access

#### Kubernetes Cluster Security (Ex. Kubernetes)
* Kubernetes componentes are running secure and up-to-date:
  * Apiserver
  * Kubelet
  * ETCD
* Restrict (external) access
* AdmissionControllers
  * NodeRestriction
  * Custom Policies (OPA)
* Enable Audit Logging
* Security Benchmarking

#### Application Security (Ex. Container)
* Use Secrets/no hardcoded credentials
* RBAC
* Container Sandboxing
* Container Hardening
  * Attachk Surface
  * Run as user
  * Readonly filesystem
* Vulnerability Scanning
* mTLS/ServiceMeshes


> https://www.youtube.com/watch?v=wqsUfvRyYpw



# 2. Create your course K8S cluster
## 2.1. Cluster Specification
```sh
# Install Kubernetes master
sudo -i
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_master.sh)


# Install Kubernetes Worker
sudo -i
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_worker.sh)
```
## 2.2. Configure gcloud command
```sh
# install gcloud sdk from
https://cloud.google.com/sdk/auth_success

# then run locally
$ gcloud auth login
$ gcloud projects list
$ gcloud config set project YOUR-PROJECT-ID
$ gcloud compute instances list # should be empty right now
```

## 2.3. Create Kubeadm Cluster in GCP
```sh
# CREATE cks-master VM using gcloud command
# not necessary if created using the browser interface
$ gcloud compute instances create cks-master --zone=europe-west3-c \
--machine-type=e2-medium \
--image=ubuntu-2004-focal-v20220419 \
--image-project=ubuntu-os-cloud \
--boot-disk-size=50GB

# CREATE cks-worker VM using gcloud command
# not necessary if created using the browser interface
$ gcloud compute instances create cks-worker --zone=europe-west3-c \
--machine-type=e2-medium \
--image=ubuntu-2004-focal-v20220419 \
--image-project=ubuntu-os-cloud \
--boot-disk-size=50GB

# you can use a region near you
https://cloud.google.com/compute/docs/regions-zones

# INSTALL cks-master
gcloud compute ssh cks-master
sudo -i
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_master.sh)

# INSTALL cks-worker
gcloud compute ssh cks-worker
sudo -i
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_worker.sh)
```

## 2.4. Firewall rules for NodePorts
```sh
$ gcloud compute firewall-rules create nodeports --allow tcp:30000-40000
```

## 2.5. Containerd Course Upgrade
docker --> containerd

## 2.6. Recap
minikube start --network-plugin=cni --cni=calico -p cks



# 3. Foundation
## 3.1. Kubernetes Secure Arquitecture
### 3.1.1. Intro
### 3.1.2. Find various k8s certificates

| Default CN                   | recommended key path         | recommended cert path       | command        | key argument                 | cert argument                             |
|------------------------------|------------------------------|-----------------------------|----------------|------------------------------|-------------------------------------------|
| etcd-ca                      |     etcd/ca.key                         | etcd/ca.crt                 | kube-apiserver |                              | --etcd-cafile                             |
| kube-apiserver-etcd-client   | apiserver-etcd-client.key    | apiserver-etcd-client.crt   | kube-apiserver | --etcd-keyfile               | --etcd-certfile                           |
| kubernetes-ca                |    ca.key                          | ca.crt                      | kube-apiserver |                              | --client-ca-file                          |
| kubernetes-ca                |    ca.key                          | ca.crt                      | kube-controller-manager | --cluster-signing-key-file      | --client-ca-file, --root-ca-file, --cluster-signing-cert-file  |
| kube-apiserver               | apiserver.key                | apiserver.crt               | kube-apiserver | --tls-private-key-file       | --tls-cert-file                           |
| kube-apiserver-kubelet-client|     apiserver-kubelet-client.key                         | apiserver-kubelet-client.crt| kube-apiserver | --kubelet-client-key | --kubelet-client-certificate              |
| front-proxy-ca               |     front-proxy-ca.key                         | front-proxy-ca.crt          | kube-apiserver |                              | --requestheader-client-ca-file            |
| front-proxy-ca               |     front-proxy-ca.key                         | front-proxy-ca.crt          | kube-controller-manager |                              | --requestheader-client-ca-file |
| front-proxy-client           | front-proxy-client.key       | front-proxy-client.crt      | kube-apiserver | --proxy-client-key-file      | --proxy-client-cert-file                  |
| etcd-ca                      |         etcd/ca.key                     | etcd/ca.crt                 | etcd           |                              | --trusted-ca-file, --peer-trusted-ca-file |
| kube-etcd                    | etcd/server.key              | etcd/server.crt             | etcd           | --key-file                   | --cert-file                               |
| kube-etcd-peer               | etcd/peer.key                | etcd/peer.crt               | etcd           | --peer-key-file              | --peer-cert-file                          |
| etcd-ca                      |                              | etcd/ca.crt                 | etcdctl    |                              | --cacert                                  |
| kube-etcd-healthcheck-client | etcd/healthcheck-client.key  | etcd/healthcheck-client.crt | etcdctl     | --key                        | --cert                                    |

> https://kubernetes.io/docs/setup/best-practices/certificates/#certificate-paths

> https://www.youtube.com/watch?v=gXz4cq3PKdg

> https://kubernetes.io/docs/setup/best-practices/certificates

## 3.2. Containers under the hood
### 3.2.1. Intro
#### 3.2.1.1. Container and Image
* **Dockerfile**: Script/text defines how to build an image
* **Image** (docker build): Multi layer binary representation of state
* **Container** (docker run): "running" instnace of an image
  * Collection of one or multiple applications.
  * Includes all its dependencies.
  * Just a process which runs on the Linux Kernel (but which cannot see everything).

### 3.2.2. Test Tools Introduction
#### Container tools

**Docker**: Container Runtime + Tool for managing containers and images.
**Containerd**: Container Runtime.
**Crictl**: CLI for CRI-compatible Container Runtimes.
**Podman**: Tool for managing containers and images.

##### Dockerfile
```sh
FROM bash
CMD ["ping", "killer.sh"]
```

##### Build Dockerfile
```sh
$ docker build -t simple .

Sending build context to Docker daemon  9.242GB
Step 1/2 : FROM bash
latest: Pulling from library/bash
9621f1afde84: Pull complete 
a3c37d376888: Pull complete 
ad3bdcf0e4f6: Pull complete 
Digest: sha256:3814c0222f2036d56f45b683943b668b685e76aa3c4ffe80449be865cefc54f9
Status: Downloaded newer image for bash:latest
 ---> 9306da3708d9
Step 2/2 : CMD ["ping", "killer.sh"]
 ---> Running in 002dac3d4de7
Removing intermediate container 002dac3d4de7
 ---> 92ccea391f11
Successfully built 92ccea391f11
Successfully tagged simple:latest
```

##### List image
```sh
$ docker image ls | grep simple

simple  latest      92ccea391f11   21 seconds ago   13.3MB
```

##### Docker run
```sh
$ docker run simple

PING killer.sh (35.227.196.29): 56 data bytes
64 bytes from 35.227.196.29: seq=0 ttl=116 time=11.946 ms
64 bytes from 35.227.196.29: seq=1 ttl=116 time=11.609 ms
64 bytes from 35.227.196.29: seq=2 ttl=116 time=11.517 ms
64 bytes from 35.227.196.29: seq=3 ttl=116 time=11.538 ms
64 bytes from 35.227.196.29: seq=4 ttl=116 time=12.187 ms
64 bytes from 35.227.196.29: seq=5 ttl=116 time=11.891 ms
64 bytes from 35.227.196.29: seq=6 ttl=116 time=11.787 ms
^C
--- killer.sh ping statistics ---
7 packets transmitted, 7 packets received, 0% packet loss
round-trip min/avg/max = 11.517/11.782/12.187 ms
```

##### Podman build
```sh
$ podman build -t simple .

STEP 1/2: FROM bash
✔ docker.io/library/bash:latest
Trying to pull docker.io/library/bash:latest...
Getting image source signatures
Copying blob ad3bdcf0e4f6 done  
Copying blob 9621f1afde84 done  
Copying blob a3c37d376888 done  
Copying config 9306da3708 done  
Writing manifest to image destination
Storing signatures
STEP 2/2: CMD ["ping", "killer.sh"]
COMMIT simple
--> 3cbf70561b7
Successfully tagged localhost/simple:latest
3cbf70561b780951ece7abfb1f59f18018f7bb47fc8838e1496be2f7f82753bb
```

##### Podman run
```sh
$ podman run simple

PING killer.sh (35.227.196.29): 56 data bytes
64 bytes from 35.227.196.29: seq=0 ttl=42 time=13.926 ms
64 bytes from 35.227.196.29: seq=1 ttl=42 time=13.576 ms
64 bytes from 35.227.196.29: seq=2 ttl=42 time=13.696 ms
64 bytes from 35.227.196.29: seq=3 ttl=42 time=13.569 ms
^C
--- killer.sh ping statistics ---
4 packets transmitted, 4 packets received, 0% packet loss
round-trip min/avg/max = 13.569/13.691/13.926 ms
```

### 3.2.3. The PID Namespace
Create two containers and check they cannot see each other.

#### Run c1 container
```sh
$ docker run --name c1 -d ubuntu sh -c "sleep 1d"

Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
301a8b74f71f: Pull complete 
Digest: sha256:7cfe75438fc77c9d7235ae502bf229b15ca86647ac01c844b272b56326d56184
Status: Downloaded newer image for ubuntu:latest
8e3e209a6bccd98763d0a53843fcd0d3f6ba4034518d90f6739a62b101fecf13
```

#### Show process on c1 container
```sh
$ docker exec c1 ps aux

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.1  0.0   2888   988 ?        Ss   16:24   0:00 sh -c sleep 1d
root           7  0.0  0.0   2788  1052 ?        S    16:24   0:00 sleep 1d
root           8  0.0  0.0   7060  1584 ?        Rs   16:25   0:00 ps aux
```

#### Run c2 container
```sh
$ docker run --name c2 -d ubuntu sh -c "sleep 999d"

7868efe1dca5c0c97632ee9631974e85836a035120acf358a25ffa6e5b034a0b
```

#### Show process on c2 container
```sh
$ docker exec c2 ps aux

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.2  0.0   2888   964 ?        Ss   16:25   0:00 sh -c sleep 999d
root           7  0.0  0.0   2788  1020 ?        S    16:25   0:00 sleep 999d
root           8  0.0  0.0   7060  1664 ?        Rs   16:26   0:00 ps aux
```

#### Show process on host
```sh
$ ps aux | grep sleep

root       15871  0.0  0.0   2888   988 ?        Ss   18:24   0:00 sh -c sleep 1d
root       15942  0.0  0.0   2788  1052 ?        S    18:24   0:00 sleep 1d
root       16269  0.0  0.0   2888   964 ?        Ss   18:25   0:00 sh -c sleep 999d
root       16340  0.0  0.0   2788  1020 ?        S    18:25   0:00 sleep 999d
adrianm+   16599  0.0  0.0  11664  2624 pts/0    S+   18:26   0:00 grep --color=auto sleep
```

#### Delete c2 container
```sh
$ docker rm c2 --force
c2
```

#### Recreate container with same namespace.
```sh
$ docker run --name c2 --pid=container:c1 -d ubuntu sh -c "sleep 999d"

71fa5ea24dc86f99af4c2c04f7599409b4b1b92082bb07b57261a4d4418fd5a7
```

#### Show process on c2 container (you can see other container process).
```sh
$ docker exec c2 ps aux

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   2888   988 ?        Ss   16:24   0:00 sh -c sleep 1d
root           7  0.0  0.0   2788  1052 ?        S    16:24   0:00 sleep 1d
root          14  0.0  0.0   2888   960 ?        Ss   16:30   0:00 sh -c sleep 999d
root          20  0.0  0.0   2788  1028 ?        S    16:30   0:00 sleep 999d
root          28  0.0  0.0   7060  1588 ?        Rs   16:32   0:00 ps aux
```

#### Show process on c1 container (you can see other container process).
```sh
$ docker exec c1 ps aux

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   2888   988 ?        Ss   16:24   0:00 sh -c sleep 1d
root           7  0.0  0.0   2788  1052 ?        S    16:24   0:00 sleep 1d
root          14  0.0  0.0   2888   960 ?        Ss   16:30   0:00 sh -c sleep 999d
root          20  0.0  0.0   2788  1028 ?        S    16:30   0:00 sleep 999d
root          35  1.0  0.0   7060  1584 ?        Rs   16:32   0:00 ps aux
```

### 3.2.4. Recap
> https://www.youtube.com/watch?v=MHv6cWjvQjM



# 5. Cluster setup
## 5.1. Network Policies
### 5.1.1. Introduction 1
#### NetworkPolicies
* Firewall rules in Kubernetes
* Implemented by the Network Plugins CNI (Calico/Weave)
* Namespace level
* Restrict the Ingress and/or Egress for a group of Pods based on certain rules and conditions

#### Without NetworkPolicies
* By default every pod can access every pod
* Pods are **NOT** isolated.

### 5.1.2. Introduction 2
#### NetworkPolicy example
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: example
  namespace: default
spec:
  # Will be applied to these pods
  podSelector:
    matchLabels:
      id: frontend
  # Will be about outgoing traffic
  policyTypes:
    - Egress
  egress:
  # to namespace with lable id=ns1 and port 80
    - to:
      - namespaceSelector:
          matchLabels:
            id: ns1
      ports:
        - protocol: TCP
          port: 80
  # to pods with label id=backend in same namespace
    - to:
      - podSelector:
          matchLabels:
            id: backend
```

#### Multiple NetworkPolicies
* Possible to have multiple NPs selecting the same pods
* If a pod has more than one NP
  * Then the union of all NPs is applied
  * order doesnt affect policy result

##### Merge example2a + example2b
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: example2a
  namespace: default
spec:
  podSelector:
    matchLabels:
      id: frontend
  policyTypes:
    - Egress
  egress:
    - to:
      - namespaceSelector:
          matchLabels:
            id: ns1
      ports:
        - protocol: TCP
          port: 80
```

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: example2b
  namespace: default
spec:
  podSelector:
    matchLabels:
      id: frontend
  policyTypes:
    - Egress
  egress:
    - to:
      - podSelector:
          matchLabels:
            id: backend
```

### 5.1.3. Default Deny
#### Create a frontend and backend applications and expose.
```sh
$ kubectl run frontend --image=nginx
pod/frontend created

$ kubectl run backend --image=nginx
pod/backend created

$ kubectl expose pod frontend --port 80
service/frontend exposed

$ kubectl expose pod backend --port 80
service/backend exposed

$ kubectl get po,svc
NAME           READY   STATUS    RESTARTS   AGE
pod/backend    1/1     Running   0          112s
pod/frontend   1/1     Running   0          2m12s

NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/backend      ClusterIP   10.105.193.37   <none>        80/TCP    25s
service/frontend     ClusterIP   10.109.74.7     <none>        80/TCP    32s
service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP   7h
```

#### Test connection between applications
```sh
# From frontend
$ kubectl exec frontend -- curl backend

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
100   615  100   615    0     0   600k      0 --:--:-- --:--:-- --:--:--  600k

# From backend
$ kubectl exec backend -- curl frontend
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0   600k      0 --:--:-- --:--:-- --:--:--  600k
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

#### Create the NetworkPolicy
```yaml
# deny all incoming and outgoing traffic from all pods in namespace default
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Egress
  - Ingress
```

#### Test connection between apps
```sh
$ kubectl exec frontend -- curl backend

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:19 --:--:--     0curl: (6) Could not resolve host: backend
command terminated with exit code 6

$ kubectl exec backend -- curl frontend

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:19 --:--:--     0curl: (6) Could not resolve host: frontend
command terminated with exit code 6
```


### 5.1.4. Frontend to Backend traffic
```yaml
# allows frontend pods to communicate with backend pods
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: frontend
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: frontend
  policyTypes:
  - Egress
  egress:
  - to:
    - podSelector:
        matchLabels:
          run: backend
```

#### Test connection
```sh
kubectl exec frontend -- curl 10.98.148.165
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:41 --:--:--     0^C
```

```yaml
# allows backend pods to have incoming traffic from frontend pods
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: backend
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          run: frontend
```

#### Test connection
```sh
$ kubectl exec frontend -- curl 10.98.148.165             

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0   600k      0 --:--:-- --:--:-- --:--:--  600k
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

#### If you want connect to DNS, you indicate Port 53
```yaml
# deny all incoming and outgoing traffic from all pods in namespace default
# but allow DNS traffic. This way you can do for example: kubectl exec frontend -- curl backend
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Egress
  - Ingress
  egress:
  - ports:
    - port: 53
      protocol: TCP
    - port: 53
      protocol: UDP
```

### 5.1.5. Backend to database traffic
#### Create a namespace
```sh
$ kubectl create ns cassandra
namespace/cassandra created

$ kubectl label namespace cassandra "ns=cassandra"
namespace/cassandra labeled
```
#### Create a Pod
```sh
$ kubectl -n cassandra run cassandra --image nginx
pod/cassandra created
```

#### Get Pod Cassandra IP
```sh
kubectl -n cassandra get po -owide
NAME        READY   STATUS    RESTARTS   AGE   IP               NODE    NOMINATED NODE   READINESS GATES
cassandra   1/1     Running   0          35s   10.244.158.131   cksv1   <none>           <none>
```

#### Test connection
```sh
$ kubectl exec backend -- curl 10.244.158.131
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:31 --:--:--     0^C
```

#### Apply egress to cassandra namespace
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: backend-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      run: backend
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          run: frontend
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          ns: cassandra
```

#### Test connection
```sh
kubectl exec backend -- curl 10.244.158.131           
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0   600k      0 --:--:-- --:--:-- --:--:--  600k
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

#### Create configuration to Deny all to cassandra Pod
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cassandra-deny
  namespace: cassandra
spec:
  podSelector:
    matchLabels:
      run: cassandra
  policyTypes:
  - Ingress
  - Egress
```

#### And create NetworkPolicy to cassandra ingress from default
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cassandra-network-policy
  namespace: cassandra
spec:
  podSelector:
    matchLabels:
      run: cassandra
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          ns: default
```
#### Labeled default namespace and launch curl
```sh
$ kubectl label namespaces default "ns=default"
namespace/default labeled

$ kubectl exec backend -- curl 10.244.158.131
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   615  100   615    0     0   150k      0 --:--:-- --:--:-- --:--:--  200k
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

### 5.1.6. Recap
> https://kubernetes.io/docs/concepts/services-networking/network-policies

## 5.2. GUI Elements
### 5.2.1. Introduction
#### Gui Elements and the Dashboard
* only expose services externally if needed
* cluster internal services/dashboards can also be accessed using `kubectl port-forward`

#### Kubectl proxy
* Creates a proxy server between localhost and the Kubernetes API Server
* Uses connection as configured in the kubeconfig
* Allows to access API locally just over http and without authentication

![cks](images/07_intro_kubectl_proxy.png)

#### Kubectl port-forward
* Forwards connections from a localhost-por to a pod-port
* More generic than kubectl proxy
* Can be used for all TCP traffic not just HTTP

![cks](images/07_intro_kubectl_port-forward.png)

#### Ingress
![cks](images/07_intro_ingress.png)

### 5.2.2. Install Dashboard
#### Deploy Dashboard
```sh
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.1.0/aio/deploy/recommended.yaml
```

#### Get objects
```sh
# Get Namespaces
$ kubectl get ns

NAME                   STATUS   AGE
cassandra              Active   24h
default                Active   24h
kube-node-lease        Active   24h
kube-public            Active   24h
kube-system            Active   24h
kubernetes-dashboard   Active   66s

# Get Pod and SVCs
$ kubectl -n kubernetes-dashboard get po,svc

NAME                                             READY   STATUS    RESTARTS   AGE
pod/dashboard-metrics-scraper-7cc7856cfb-gz48q   1/1     Running   0          2m3s
pod/kubernetes-dashboard-b8df5b7bc-bxfk4         1/1     Running   0          2m3s

NAME                                TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/dashboard-metrics-scraper   ClusterIP   10.104.44.96     <none>        8000/TCP   2m3s
service/kubernetes-dashboard        ClusterIP   10.101.163.180   <none>        443/TCP
```

### 5.2.3. Outside Insecure Access
#### Expose insecure Dashboard
```sh
# Edit deployment
$ kubectl -n kubernetes-dashboard edit deploy kubernetes-dashboard

...

- args:
  - --auto-generate-certificates        # delete line
  - --namespace=kubernetes-dashboard
  - --insecure-port=9090                # include line
...

# Edit SVC
$ kubectl -n kubernetes-dashboard edit svc kubernetes-dashboard
...
  ports:
  - port: 443         # delete line
  - port: 9090        # include line
    protocol: TCP
    targetPort: 8443  # delete line
    targetPort: 9090  # include line
  selector:
    k8s-app: kubernetes-dashboard
  sessionAffinity: None
  type: ClusterIP     # delete line
  type: NodePort      # include line
...
```

> https://github.com/kubernetes/dashboard/blob/master/docs/common/dashboard-arguments.md


### 5.2.4. RBAC for the dashboard

```sh
# Get Service Accounts
$ kubectl -n kubernetes-dashboard get sa
NAME                   SECRETS   AGE
default                0         11m
kubernetes-dashboard   0         11m

# Get roles
$ kubectl get clusterroles | grep view

system:aggregate-to-view                                               2022-10-28T17:16:41Z
system:public-info-viewer                                              2022-10-28T17:16:41Z
view                                                                   2022-10-28T17:16:41Z

# Create Rolebinding
$ kubectl -n kubernetes-dashboard create rolebinding insecure --serviceaccount kubernetes-dashboard:kubernetes-dashboard --clusterrole view

# Create ClusterRoleBinding
$ kubectl -n kubernetes-dashboard create clusterrolebinding insecure --serviceaccount kubernetes-dashboard:kubernetes-dashboard --clusterrole view
```

### 5.2.5. Recap
#### Interesting dashboard security arguments
```sh
--authentication-mode=basic
--enable-skip=true
```
>https://github.com/kubernetes/dashboard/blob/master/docs/common/dashboard-arguments.md

>https://github.com/kubernetes/dashboard/blob/master/docs/user/access-control/README.md

## 5.3. Secure Ingress
### 5.3.3. Create an Ingress
#### Setup an example Ingress
![cks](images/08_create_an_ingress.png)

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: secure-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - http:
      paths:
      - path: /service1
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80

      - path: /service2
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 80
```

```sh
# Install NGINX Ingress
kubectl apply -f https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/course-content/cluster-setup/secure-ingress/nginx-ingress-controller.yaml

# Complete Example
https://github.com/killer-sh/cks-course-environment/tree/master/course-content/cluster-setup/secure-ingress
```

> https://kubernetes.io/docs/concepts/services-networking/ingress

### 5.3.4. Secure an Ingress
![cks](images/08_secure_an_ingress.png)

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: secure-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx # newer Nginx-Ingress versions NEED THIS
  tls:
  - hosts:
      - secure-ingress.com
    secretName: secure-ingress
  rules:
  - host: secure-ingress.com
    http:
      paths:
      - path: /service1
        pathType: Prefix
        backend:
          service:
            name: service1
            port:
              number: 80
      - path: /service2
        pathType: Prefix
        backend:
          service:
            name: service2
            port:
              number: 80
---
# openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
# set Common Name: secure-ingress.com
# kubectl create secret tls secure-ingress --cert=cert.pem --key=key.pem -oyaml --dry-run
apiVersion: v1
data:
  tls.crt: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUZwVENDQTQyZ0F3SUJBZ0lVWEJtMVNWZ3hRQURQclBObXhaSXo3WG1sRWRFd0RRWUpLb1pJaHZjTkFRRUwKQlFBd1lqRUxNQWtHQTFVRUJoTUNRVlV4RXpBUkJnTlZCQWdNQ2xOdmJXVXRVM1JoZEdVeElUQWZCZ05WQkFvTQpHRWx1ZEdWeWJtVjBJRmRwWkdkcGRITWdVSFI1SUV4MFpERWJNQmtHQTFVRUF3d1NjMlZqZFhKbExXbHVaM0psCmMzTXVZMjl0TUI0WERUSXhNVEF3TlRFek5UVXdPVm9YRFRJeU1UQXdOVEV6TlRVd09Wb3dZakVMTUFrR0ExVUUKQmhNQ1FWVXhFekFSQmdOVkJBZ01DbE52YldVdFUzUmhkR1V4SVRBZkJnTlZCQW9NR0VsdWRHVnlibVYwSUZkcApaR2RwZEhNZ1VIUjVJRXgwWkRFYk1Ca0dBMVVFQXd3U2MyVmpkWEpsTFdsdVozSmxjM011WTI5dE1JSUNJakFOCkJna3Foa2lHOXcwQkFRRUZBQU9DQWc4QU1JSUNDZ0tDQWdFQXptYkp3L05oMVR0a3pBSTl6MXl2dVI0SWhkc0sKTndFUnh5bkphS0tac3d5U2pxeU43T29iam9Wbnc2clJLYjVLdFBZQk9vNWVCQ3RIQzUzMFpTRFdKRG11ZWdHOAozSEtTcTNrNG9NV2RVcGJUK3lGMVJUU0c5c2llVGRYSFJpakpLenhnZ3Jtb05GNmh3SXVzc290TVFKYkFtVzI0ClA0K1QySERYUThYdDQxbk54U2RKTDNLNXpmQ21nVWJaWUQvaEpRWGxzMGtacWYxZ3R4dDR0a05aazNpOStYZ1kKWTkzR011WGQzZlVBZUpWS0hPV0x1RlZFOFVyOHJuMmE5M1I3VEk2bzdBTWp3VTJwTFUvYVBFQ0Y2VGJ2SFZiWgpyRWp4SzV2OGMxZTFOU091QUxOanZMckxiYUdManAwMzJ0dGRtS1FLczNBMHVyMzZDakNVTXZoWXpoTGkwNjVYCjJDRDlNQWtMN1RTM3FwcDBWRExtSnVISWh2V3kzcTVZNFNqL0Fad3VYdUxHeVlXL2did0pCSVVneXgwUzM4MDMKMEcxVGtGM00vd1BmWmxjbEI4RjN6ZVE0b2tWc0NHUDZLM0hodDE4MGgyRVQ1QTlNKys0RDR0T1dlTExxSFlnVApHeGdkTUFER09ON3BVVzM5aDFuaXZ5cWljVzNBSDI2TUZrR1dHUHVqT3hKMXFpQ2RuTWhlTWg3ZVRmenk2R3NOCm1MQnEwcTJqQVNQUUMzUjVpRjlsYkorWk9DcnhNRlFoNXJFejZYckJNbngrN2JMVkdvOWxKaUFPNEVsWkNhb0kKa1JnbjNNa2hwNjY3Mzh6Wi9VZCs2MGxGcGJyN2xNdGdka3huTkZvMEM5TlA5ZlV2cU1YSWl5K3V5V1ZBaWpiSAplaWRHZE43Vm5PMGNQTUVDQXdFQUFhTlRNRkV3SFFZRFZSME9CQllFRkhUci8vRWJqcGw2SWZEVkdhSkNEVlZECnpHaGlNQjhHQTFVZEl3UVlNQmFBRkhUci8vRWJqcGw2SWZEVkdhSkNEVlZEekdoaU1BOEdBMVVkRXdFQi93UUYKTUFNQkFmOHdEUVlKS29aSWh2Y05BUUVMQlFBRGdnSUJBSjZySFowVENTWnRYa0hFVTZuNzhVMWswTUR0eVQ4ZAp6cGh0R3A0Y2xoY05idE1JRWJUbksvKzFDSG4wTE9rM210bURwYU45MHZwa3krSDJJR3VPTVYzdXpuZGJqMjBNClB2N0dzZnB0MGx2Sm5UdTdGNHpQblFBVkxJTXFzcHErNmNGaUc3MkxPS1FyemJPNTYwQlZJQVdJQ0hLcnd4YUQKQkxuZW4zVFRlR0UveFI2WjZKOUJnTVQ4ejM5eFdzRjZja2hFTmtGMFlaWDJ5TmdDd1MvZUpFWEZpcTBPUWhDYwp0UnBpM0RmU0NMWkgyTVYrcnpXNExNQUZCcDRwVzlTUHBFaTcvTVBIZUNhNnRPeHdJN2Y2c0huK1E0bDZVRTMrCjNsSktaV09jOWVUMWRhb1NaUkpwMHc3eXA5MVNzSFNPRk5sZkNyZFVNbWdpVDRjcHRoR3FwWnJoMjUxajl3UUIKQ04rT1JyVzBDM2RNMUxqTmFYenBBVVhCbk9vZFRiNEpXNk5wWFVSdUFad3pkS1FiSTc2Y3VnbkhtQ3owYjBwdgpYRHNlcTVOYlJ5OWZnaTJPUEdYbEhDMVpwR2gvbUZIYXR3MCtOOWpGTTFMYWNKK0tPZlpadERJa1FWWXVEZWFxCktLRTJEb1dHcDJ1aU5GTTN2bmRRR2FQUmVnUDgwZHlWOXhtL3B3Rnk5YWZyc3ZOazJOZFV5UlJONE9ISnNmRkwKU2RVMUtHazNDKzRFWkZQeDY2U1g1T2xJSVpEeCsyY0xuVTJUcDNsbG0yYjk0NS9xMG9pSllZK2k3UjRSeVJTRApNQ1ZVdkpFZG0xb1JOMzB2YWZLLzk4Wm9oUXF2aVpYbXNLNTg1TnNmYU9TVjNObUJ4UmJ5S1BJdmhWWkU0R0FaCjk2Q0pHVURIOHllOQotLS0tLUVORCBDRVJUSUZJQ0FURS0tLS0tCg==
  tls.key: LS0tLS1CRUdJTiBQUklWQVRFIEtFWS0tLS0tCk1JSUpRZ0lCQURBTkJna3Foa2lHOXcwQkFRRUZBQVNDQ1N3d2dna29BZ0VBQW9JQ0FRRE9ac25EODJIVk8yVE0KQWozUFhLKzVIZ2lGMndvM0FSSEhLY2xvb3BtekRKS09ySTNzNmh1T2hXZkRxdEVwdmtxMDlnRTZqbDRFSzBjTApuZlJsSU5Za09hNTZBYnpjY3BLcmVUaWd4WjFTbHRQN0lYVkZOSWIyeUo1TjFjZEdLTWtyUEdDQ3VhZzBYcUhBCmk2eXlpMHhBbHNDWmJiZy9qNVBZY05kRHhlM2pXYzNGSjBrdmNybk44S2FCUnRsZ1ArRWxCZVd6U1JtcC9XQzMKRzNpMlExbVRlTDM1ZUJoajNjWXk1ZDNkOVFCNGxVb2M1WXU0VlVUeFN2eXVmWnIzZEh0TWpxanNBeVBCVGFrdApUOW84UUlYcE51OGRWdG1zU1BFcm0veHpWN1UxSTY0QXMyTzh1c3R0b1l1T25UZmEyMTJZcEFxemNEUzZ2Zm9LCk1KUXkrRmpPRXVMVHJsZllJUDB3Q1F2dE5MZXFtblJVTXVZbTRjaUc5YkxlcmxqaEtQOEJuQzVlNHNiSmhiK0IKdkFrRWhTRExIUkxmelRmUWJWT1FYY3ovQTk5bVZ5VUh3WGZONURpaVJXd0lZL29yY2VHM1h6U0hZUlBrRDB6Nwo3Z1BpMDVaNHN1b2RpQk1iR0Iwd0FNWTQzdWxSYmYySFdlSy9LcUp4YmNBZmJvd1dRWllZKzZNN0VuV3FJSjJjCnlGNHlIdDVOL1BMb2F3MllzR3JTcmFNQkk5QUxkSG1JWDJWc241azRLdkV3VkNIbXNUUHBlc0V5Zkg3dHN0VWEKajJVbUlBN2dTVmtKcWdpUkdDZmN5U0ducnJ2ZnpObjlSMzdyU1VXbHV2dVV5MkIyVEdjMFdqUUwwMC8xOVMrbwp4Y2lMTDY3SlpVQ0tOc2Q2SjBaMDN0V2M3Unc4d1FJREFRQUJBb0lDQURLMVZCcWRIOHNJVllJOWhydjhOSHZSClloeW9yTURJdFhwdHpMcTFQL2VhUGlOcFIxRU9Ud2pidzV0eHl3TnJhZVU5angyNHZtWmR6NDJPRis0RWZEZlkKS0FKM2pOUElIanFjaElvVElzeVltNm5XRlg2VUloaGRQMjgxOTBoSVd1d1JZRkNkbGpLUGtVUEJ6UUxzY0NacQpJeFZPdkhaNUtzU0JMSkhNL2QzZVFVeVBrMDVoN0Q0cFFtNytYZ2RraWtiVFJSU2YvL3NnY2ZOcWYyU2Y5VkRpCjJDR0RITkxrT0g1bXRQU0Q3Y0t5UXN3SXBTUDdadjIxQTVGRzhKeWM5SEhobHFTdFBNcVA3dUZmL2VqUm5WU24KbDJWbzRmK095Qm1Ec0NrU1FrVzA1MW5xdUxVMFd5Z3JDU05YZ3RKMUZIQ2MxTGd5OS9GdEhSSUZ6MU1hYW81OQpaZlZPeVdQVG5wREIzeUhqcFVBWndpUFhDbFhHZmowQk9mU3lQQ1VjT0RYYlNTc1p0VTVJeWtVbi9BQnl1d3dXCktKdEpKbFhhaTRxNlNGb1JKWVhxVTJ0UGdzcldnZnlYT0ptdHdMQmFMNHpuZ3hTdURmSFFuYjdiUi9CQUJlN1EKS0VIZnNzelluK3BqVTZVejliZjhnNExJT1hLTlA5TWxPWFVab0doNFJFdVBmbnlhQjh6OVd5SlErbndlTnkwNQp4Z3NaejVHbEhQcjd6TTJueFZYSkNRYnVPNXJkS1Q1SHh0dlZha3pQUks4T0FiOVg0WkFiTnpqanRMOVJ2TGhOCmlaYm1FMWgwYTMxSXc5YklCNG1CNVgyZE5zVm45YXBxTHgzVm5QY0wyK2pqV3I2b1k3WWxkbUNBNmlYckZwQVoKZFBkU0p0cmtodkphb3pQaDZVaFZBb0lCQVFEby9ueWpXOWQyZVJrSmdvYStSNlNyUEFpaXVPSE1yQXpmQ3dXbQpwZFEzc2pidzB0TU9sM1l5NUhUNGRRKzhJZ0tRbGp4Q1Y2ZURCeWVEUytXR1JNdFA0Z1hpK0NFRmd6TlNqSDROCmkvL3FwcHJDU0l1dThaM0NybXdETnJrcllGWWFzdzhSL0ZZaW5jS296aVdCWVdFU3NEdmNsTmRteG56aTRMQXYKd2FVSmQ5Y1dnT1IwNVJEODR0YWtCNUcyZHhsc3JWMlU5M1JvU0pMaUxNWk1VQ0ZGdXRpczk4R1loK016UUZPKwpjQlRUR0lWTGh2clZHOHorMFdaTEJKQVNhTzVac1NvZzVXak1XRmoxZWp1cHJ0UW9QejZMM3ZHeksvYStud082CnhOT3loV2w5aENDbTQ5b3Q4TzBKV0dpbmhENll6ck1UQ0pyUjYxbDF4eXhLbU5NM0FvSUJBUURpeUJ0Z0tkMEEKeC9tc0hQUGdwUm1tWFN3ODhJRWZTV3JSUlRFWVdVNjZ2a1FpZWkyOG5YcHZheUlZNUpPTUYzNk1jYTFLOXhJMgpZMGpwK1M4WEh4U1JUQXRJWjVEZHJaUFJFVXE3eE00ekN6eU42YXk0WDF6eWhlMFdzQXJEd3pzb2VLaHJDenZGCktxcDlZVjdZNGJGME9ZTFREQ0hSeUFMRm1MRWh6RFNoZ1RRZENWZTBUNUdCV0R1M3kzU2YvOTNaQmlzYlhjOWQKN0Jmd0dSVWlkM0tFSE95S2Jxc1FLUnhaUDdHN0dyVERJaVMvOXdVZFpWSjA1MUpYcDBhSUV5U2kwSVdJZjFuWAoxdTNMTlpaNVRkUDBvSnFaQld1dnRxZVlSK1dYZExmZ3F2bDV2K1hJQ3NNdXlvSHZzQ1pEdWZ4V2E3RFY1ZS9sCkR6QXVrWlo1WU52SEFvSUJBR3ZpZXNBQnBORWMzYlVEbFhUQ0k4T09Oc2x5Smt2cFZzUm9qQ05RSWVYd1JYUloKaXBUMUdTd1RrUDRDNWxoTXZ6ZEgrWHNXcjJBQ2pnOURzM0hxcE9IR1hNZHQ4WXhsNWZ2UlJnVHIwSUpNeHRnegpVMHFjWWxway9XcTNpaUpGcDFrUmxHYlZtdVRJZS94Q0NDZlNlV3AzNUNBTlkzZ1piSFo1Wjl1VkpPQXZkNDdaCisrOE1xa01PbmlpeHdJem10UVVYZVgraTNXbjBRTjh0c0Z4aHRpWmRrRHIzTmROMUNJVVF3allxRzlwclBqMXMKc1BUQXZMazVLTTZQdkU1cC9BUFgveFBnWmhoSXlGaFNVeElNKy92dTNQMVRMRU8wbGJwS1V5WEdRZWdsMG1UbQpLMkJiblFrc3gvVk4xSmZNcWxlRFRuUC94Z0J4bzZqZm91aWZ2eDBDZ2dFQUNrWVlmMlhHSGxmdzVxdzFIRE0rCmt6dmJXak5uRmh6RVd4Q3daYkwrRHhXNWpucE1naVA4UFBuMGhINHVkUVZIZFdOYy8yMXNCTXpBcStEZkVrUVYKTVhQcGV2RStMZHpFT285Mi9FU3hOcnpHbElOR2tOKzVIVCtWK3hZa2xyUE1oVXZhRFdkbjRNbkxDWDBVeCt5Sgpsb05ZZXVrc3l3MHRtdmdNNWtRVENsSUpJMXVkL293d1FsVFY5OENlMnZURGZ5WjVZM2IvZ3ZqRUtOdHFDckt3Cm5HMlhCYnAzdzNhcFV4M0Fsb0ppT0FqZTgxZGtndTFwSytTaTVWZXRxVko4c1dlUzlSa1gzK0JieTMzMUFDL1gKYjFpclNFMW5rSUZNM0dnOWJYd2JMSEZ5ZGVLTXJXQlhjVkk1U3J1SE1FQkh2ejZIdDFrQVlqY3E3cUVuMlAzYgpWd0tDQVFFQXpxYlc3cVEzd2xBK0J1RFdMTHpmWDU4amdKVmZmaGJTSDB1SitkcytEOXZhQVJqK3ZyOWVILy9VCndsQVZuTDRaOEJWUlhjS0VJdm9XMnFTbk51OCs1OXN4dlZUWDJ2Tys3Rm5vT2JIWHhFUDFDZW1JOVV0dkRERE4KQzNxTGp2V1gwaXE3YzhQbk9iZWptMVc0VlNORVJmSHBvaER0S3JVOUVyR05jRUFHMCtIRlJkbDIzK3BRYk1rawpxRFlYd2lSUVVLY0phWWRuNWhFdHVzWWxNUGhpTHRtMzFjWlpMdUxNdmRMWmJUVk1vblFHeGh1cGxmSEMvRk81CittakpoYjdCa2dRdE1HUkpyQ0Z4MVlKVVJEUmZtUjRSZnBZcDZSeW12MldYOFp3d1RUMkdBaVdERVRjVlYyVEgKcTROd0xHcGk0cTNuNXc5OFpaQTdLcnF5WW1taUlRPT0KLS0tLS1FTkQgUFJJVkFURSBLRVktLS0tLQo=
kind: Secret
metadata:
  name: secure-ingress
type: kubernetes.io/tls
```

```sh
# generate cert & key
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
	# Common Name: secure-ingress.com

# Complete Example
https://github.com/killer-sh/cks-course-environment/tree/master/course-content/cluster-setup/secure-ingress


# curl command to access, replace your IP and secure NodePort->443
curl https://secure-ingress.com:31047/service2 -kv --resolve secure-ingress.com:31047:34.105.246.174

# k8s docs
https://kubernetes.io/docs/concepts/services-networking/ingress/#tls
```

### 5.3.5. Recap
## 5.4. Node metadata protection
### 5.4.1. Introduction
#### Cloud Platform Node Metadata
* Metadata service API by default reachable from VMs
* Can contain cloud credentials for VMs/Nodes
* Can contain provisioning dat like kubelet credentials

#### Limit permissions for instance credentials
* Ensure that the cloud-instance-account has only the necessary permissions
* Each cloud provider has a set of recommendations to follow
* Not in the hands of Kubernetes


### 5.4.2. Access Node Metadata

```sh
# Example
curl "http://metadata.google.internal/computeMetadata/v1/instance/disks/" -H "Metadata-Flavor: Google"
```

> https://cloud.google.com/compute/docs/metadata/overview


### 5.4.3. Protect Node Metadata via Network Policy

#### All pods in namespace cannot access metadata endpoint
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cloud-metadata-deny
  namespace: default
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 0.0.0.0/0
        except:
        - 169.254.169.254/32
```

#### Only pods with label are allowed to access metadata endpoint
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: cloud-metadata-allow
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: metadata-accessor
  policyTypes:
  - Egress
  egress:
  - to:
    - ipBlock:
        cidr: 169.254.169.254/32
```

#### Labeled Pod
```sh
$ kubectl label pod nginx role=metadata-accessor 
```

## 5.5. CIS Bechmarck
### 5.5.1. Introduction
#### CIS - Center for Internet Security
* Best practices for the secure configuration of a target system
* Covering more than 14 technology groups
* Developed through a unique consensus-based process comprised of cybersecurity professionals and subject matter experts around the world

![cks](images/10_intro_cis.png)

### 5.5.2. CIS in action

> https://www.cisecurity.org/benchmark/kubernetes

> https://ettayeb.fr/content/files/2022/03/CIS_Kubernetes_Benchmark_v1.6.0.pdf

### 5.5.3. kube-bench
#### How to run
> https://github.com/aquasecurity/kube-bench/blob/main/docs/running.md

```sh
# run on master
docker run --pid=host -v /etc:/etc:ro -v /var:/var:ro -t aquasec/kube-bench:latest run --targets=master --version 1.22

# run on worker
docker run --pid=host -v /etc:/etc:ro -v /var:/var:ro -t aquasec/kube-bench:latest run --targets=node --version 1.22
```
### 5.5.4. Recap
> https://cloud.google.com/kubernetes-engine/docs/concepts/cis-benchmarks?hl=es-419#status

> https://www.youtube.com/watch?v=53-v3stlnCo

> https://github.com/docker/docker-bench-security


## 5.6. Verify Platform Binaries
### 5.6.1. Verify apiserver binary running in our cluster
```sh
# Get version
$ kubectl get nodes
NAME    STATUS   ROLES           AGE   VERSION
cksv1   Ready    control-plane   19s   v1.25.0

# Download release version
$ wget https://dl.k8s.io/v1.25.0/kubernetes-server-linux-amd64.tar.gz

# Check version
$ kubernetes/server/bin/kube-apiserver --version
Kubernetes v1.25.0

# Get hash from Github
$ sha512sum kubernetes/server/bin/kube-apiserver
c0826f1dbb94c224b888e7caba035a187e0dbd1bf23a57042eca99633fdf7aa9f0f1663745307b096aada158c2421fadafdd480028291c21c0dca74876d2beaf  kubernetes/server/bin/kube-apiserver

# Get hash from local
$ sha512sum /proc/26579/root/usr/local/bin/kube-apiserver
c0826f1dbb94c224b888e7caba035a187e0dbd1bf23a57042eca99633fdf7aa9f0f1663745307b096aada158c2421fadafdd480028291c21c0dca74876d2beaf  /proc/26579/root/usr/local/bin/kube-apiserver
```
### 5.6.4. Recap



# 6. Cluster Hardening
## 6.1. RBAC
### 6.1.1. Intro
#### RBAC
* "Role-based access control (RBAC) is a method of regulating access to computer or network resources based on the roles of individual users within your organization."

```sh
# kube-apiserver
`--authorization-mode` stringSlice Default: [AlwaysAllow]

Ordered list of plugins to do authorization on secure port. Comma-delimited list of: AlwaysAllow,AlwaysDeny,ABAC,Webhook,RBAC,Node.
```

* Restrict access to Kubernetes resources when accessed by Users and ServiceAccounts.
* Works with Roles and Bindings
* Specify what is ALLOWED, everything else is DENIED
  * whitelisting

#### POLP (Principle Of Least Privilege)
* Only access to data or information that is necessary for the legitimate purpose.

#### RBAC- Namespaced Resources vs Cluster Resources
![cks](images/12_intro_rbac.png)

```sh
# Print the supported namespaced resources
$ kubectl api-resources --namespaced=true

# Print the supported non-namespaced resources
$ kubectl api-resources --namespaced=false
```

![cks](images/12_intro_rbac_01.png)

* Same Role name behaves different in different namespaces
* User X can be secret-manager in multiple namespaces, but the permissions are different.
![cks](images/12_intro_rbac_role.png)

* ClusterRole is the same across all namespaaces (cluster wide).
* User X can be secret-m,anager in multiple namespaces, permissions are the same in each.
![cks](images/12_intro_rbac_clusterrole.png)

#### RoleBinding
![cks](images/12_intro_rbac_rolebinding.png)
![cks](images/12_intro_rbac_role_02.png)

#### ClusterRoleBinding
![cks](images/12_intro_rbac_clusterrolebinding.png)
![cks](images/12_intro_rbac_clusterrole_01.png)

### 6.1.2. Role and Rolebinding
```sh
# Create namespaces
$ kubectl create ns red
$ kubectl create ns blue

# Create Roles
$ kubectl -n red create role secret-manager --verb=get --resource=secrets
$ kubectl -n blue create role secret-manager --verb=get --verb=list --resource=secrets

# Create RoleBindings
$ kubectl -n red create rolebinding secret-manager --role=secret-manager --user=jane
$ kubectl -n blue create rolebinding secret-manager --role=secret-manager --user=jane

# Check Permissions
$ kubectl -n red auth can-i create pods --as jane # no
$ kubectl -n red auth can-i get secrets --as jane # yes
$ kubectl -n red auth can-i list secrets --as jane # no
$ kubectl -n blue auth can-i list secrets --as jane # yes
$ kubectl -n blue auth can-i get secrets --as jane # yes
$ kubectl -n default auth can-i get secrets --as jane #no
```

### 6.1.3. ClusterRole and ClusterRoleBinding
```sh
$ kubectl create clusterrole deploy-deleter --verb=delete --resource=deployment
$ kubectl create clusterrolebinding deploy-deleter --clusterrole=deploy-deleter --user=jane
$ kubectl -n red create rolebinding deploy-deleter --clusterrole=deploy-deleter --user=jim


# Test jane
$ kubectl auth can-i delete deploy --as jane # yes
$ kubectl auth can-i delete deploy --as jane -n red # yes
$ kubectl auth can-i delete deploy --as jane -n blue # yes
$ kubectl auth can-i delete deploy --as jane -A # yes
$ kubectl auth can-i create deploy --as jane --all-namespaces # no



# Test jim
$ kubectl auth can-i delete deploy --as jim # no
$ kubectl auth can-i delete deploy --as jim -A # no
$ kubectl auth can-i delete deploy --as jim -n red # yes
$ kubectl auth can-i delete deploy --as jim -n blue # no

```
### 6.1.4. Accounts and Users
![cks](images/12_accounts_users.png)

**ServiceAccount** is a resource managed by the k8s api
**Normal User** is no k8s User resource. It is assumed that a cluster-indepedent service manages normal users.


### 6.1.5. CertificateSingingRequets
#### Users and Certificates
Create a certificate+key and authenticate as user jane
* Create CSR
* Sign CSR using kubernetes API
* Usercert+key to connecto to k8s API

```sh
# Create key
$ openssl genrsa -out jane.key 2048

# Create CSR (only set Common Name = jane)
$ openssl req -new -key jane.key -out jane.csr 
```

```yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: myuser
spec:
  groups:
  - system:authenticated
  request: <cat jane.csr | base64 -w 0>
  signerName: kubernetes.io/kube-apiserver-client
  expirationSeconds: 86400  # one day
  usages:
  - client auth
```

> https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests

```sh
# Apply file
$ kubectl apply -f csr.yaml

# Get csr objects
$ kubectl get csr
NAME   AGE   SIGNERNAME                            REQUESTOR       REQUESTEDDURATION   CONDITION
jane   4s    kubernetes.io/kube-apiserver-client   minikube-user   <none>              Pending

# Approve cert
$ kubectl certificate approve jane
certificatesigningrequest.certificates.k8s.io/jane approved

# Export cert
$ kubectl get csr jane -ojsonpath='{.status.certificate}' | base64 -d > jane.crt

# Set credentials on kubeconfig
$ kubectl config set-credentials jane --client-key jane.key --client-certificate jane.crt --embed-certs

# Set context
$ kubectl config set-context jane --user jane --cluster cks 
Context "jane" created.

# Get context
$ kubectl config get-contexts 
CURRENT   NAME                  CLUSTER               AUTHINFO              NAMESPACE
*         cks                   cks                   cks                   default
          jane                  cks                   jane                  

# Use context
$ kubectl config use-context jane                          
Switched to context "jane".

# Get secrets
$ kubectl -n blue get secrets
NAME                  TYPE                                  DATA   AGE
default-token-cjklb   kubernetes.io/service-account-token   3      42m

# Delete secrets
$ kubectl -n blue delete secrets default-token-cjklb 
Error from server (Forbidden): secrets "default-token-cjklb" is forbidden: User "jane" cannot delete resource "secrets" in API group "" in the namespace "blue"

# check permissions
$ kubectl auth can-i delete pods # no
$ kubectl auth can-i delete pods -A # no
$ kubectl auth can-i get secrets -A # no
$ kubectl auth can-i get secrets -n red # yes
```

### 6.1.6. Recap

## 6.2. Exercise caution in using ServiceAccounts
### 6.2.1. Intro
#### Accounts
![cks](images/13_sa_intro.png)

#### ServiceAccounts and Pods
![cks](images/13_sa_intro_01.png)

### 6.2.2. Pods uses custom ServiceAccount
```sh
# Get SAs
$ kubectl get sa
NAME      SECRETS   AGE
default   0         3d1h

# Create SA
$ kubectl create sa accessor
serviceaccount/accessor created

# Get SAs
$ kubectl get sa
NAME       SECRETS   AGE
accessor   0         2s
default    0         3d1h

# Create Token
$ kubectl create token accessor 
eyJhbGciOiJSUzI1NiIsImtpZCI6IkFPV2lUbkhDV05CaHRROFpmOHNiZlNQME1wOWt6NWUybllvcVJuUmxkRTQifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjY3Mzk0NDA2LCJpYXQiOjE2NjczOTA4MDYsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJkZWZhdWx0Iiwic2VydmljZWFjY291bnQiOnsibmFtZSI6ImFjY2Vzc29yIiwidWlkIjoiM2JiYWJmZmYtOTRiNi00MjViLWJlMWQtZmViYTdmMTgxMDQ0In19LCJuYmYiOjE2NjczOTA4MDYsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDpkZWZhdWx0OmFjY2Vzc29yIn0.L2NFwQEsvntnaqMyvsn3L7gKXpPNoUMRUvFlAtdIayo4JGTaPVrVOEY9149KJONmqawlV0ZNJGuBbsoS1wTvtvXMbaza_MngB7RGW0ae91e7t6EF2sPGrJ3CVe1iIy1pIrc9aYWkvLK3NMVuz9Suz0z3bYeleXTjy1kMWSCtiKVdYUe_8O0tmq4NHZfMABgjIRHxyivFpXmVSHp1LR1JBINN0LWBHzHdHW0d1vW06DWNnIF2FM7_NhiLEwcZOKlBW6xo3TlM9gnPesdMzleRAgyaQoKCYdcr3rlTB-UfdspUHO0c6wGPlJbg83QEy0X-S4DOac37u6iqtXhLd0nHqQ
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: accessor
  name: accessor
spec:
  serviceAccountName: accessor
  containers:
  - image: nginx
    name: accessor
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl exec -it -- bash

# Inside container
$  mount | grep ser
tmpfs on /run/secrets/kubernetes.io/serviceaccount type tmpfs (ro,relatime,size=16113736k,inode64)

# Show token file
$ cat /run/secrets/kubernetes.io/serviceaccount/token 
eyJhbGciOiJSUzI1NiIsImtpZCI6IkFPV2lUbkhDV05CaHRROFpmOHNiZlNQME1wOWt6NWUybllvcVJuUmxkRTQifQ.eyJhdWQiOlsiaHR0cHM6Ly9rdWJlcm5ldGVzLmRlZmF1bHQuc3ZjLmNsdXN0ZXIubG9jYWwiXSwiZXhwIjoxNjk4OTI3MDIwLCJpYXQiOjE2NjczOTEwMjAsImlzcyI6Imh0dHBzOi8va3ViZXJuZXRlcy5kZWZhdWx0LnN2Yy5jbHVzdGVyLmxvY2FsIiwia3ViZXJuZXRlcy5pbyI6eyJuYW1lc3BhY2UiOiJkZWZhdWx0IiwicG9kIjp7Im5hbWUiOiJhY2Nlc3NvciIsInVpZCI6IjY0YTNjOWQ0LWJhMWUtNGJhZS04N2Q0LTA1YmEzNzMwYzAwYyJ9LCJzZXJ2aWNlYWNjb3VudCI6eyJuYW1lIjoiYWNjZXNzb3IiLCJ1aWQiOiIzYmJhYmZmZi05NGI2LTQyNWItYmUxZC1mZWJhN2YxODEwNDQifSwid2FybmFmdGVyIjoxNjY3Mzk0NjI3fSwibmJmIjoxNjY3MzkxMDIwLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6ZGVmYXVsdDphY2Nlc3NvciJ9.t0UUAiAoFp0fS51-ggL_BZbttF_WXuZiOQPeSPI9NPmpszNsAVWxIHbUqeYh61w2oUKKLyxQ30n-1qs2Y9RbO-s4R-DLWOe7c3Z33VdnZIb24-ztcfvYPi9XYQrEQ4nVwwEya2qCiHYtz5Ba4eXCLN5Q-mQthy_rbQeig02Md2lxMXw1UeoDkbXTRc5Ak9l10E7KzT1tLfRq4bYbiM4KF27gF9pBcOeX6w1_Tsw1q7o3bjp3nPr9e9YuiRtLaj_1rl1tRPM_UdWw75gKxdoSuKTHvCjr7b-9hKaAqpI3JXLM0FJrrSMAodpPKjS9eUkYRSVUkoxXTzIToxNZaT8dGg

# Get Envs
$ env
KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1

# Curl Kubernetes Port
$ curl https://10.96.0.1 -k
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403
}

# Curl with token
$ curl https://10.96.0.1 -k -H "Authorization: Bearer $(cat /run/secrets/kubernetes.io/serviceaccount/token)"
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:serviceaccount:default:accessor\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403
}
```

### 6.2.3. Disable ServiceAccount Mounting

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: build-robot
automountServiceAccountToken: false
...
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  serviceAccountName: build-robot
  automountServiceAccountToken: false
  ...
```

> https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account


### 6.2.4. Limits ServiceAccounts using RBAC
```sh
# Check with can-i
$ kubectl auth can-i delete secrets --as system:serviceaccount:default:accessor
no

# Set edit permissions
$ kubectl create clusterrolebinding accessor --clusterrole edit --serviceaccount default:accessor

# Check with can-i
$ kubectl auth can-i delete secrets --as system:serviceaccount:default:accessor
yes
```

### 6.2.5. Recap
> https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin

> https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account



## 6.3. Restrict API Access
### 6.3.1. Intro
#### Request workflow
![cks](images/14_restrict_api_intro.png)

* API requests are always tied to
  * A normal user
  * A Service Account
  * Are treated as anonymous requets
* Every request must authenticate
  * Or be treated as an anonymous user

#### Restrictions
1. **Dont allow anonymous access**
2. **Close insecure port**
3. **Dont expose ApiServer to the outside**
4. **Restrict access from Nodes to API (NodeRestriction)**
5. Prevent aunauthorized access (RBAC)
6. Prevent pods from accessing API
7. ApiServer port behind firewall/allowed ip ranges (cloud provider)

### 6.3.2. Anonymous Access
#### Anonymous Access
* kube-apiserver --anonymous-auth=true|false
* In 1.6+ anonymous access is enable by default
  * if authorization mode other than AlwaysAllow
  * but ABAC and RBAC require explicit authorization for anonymous

```sh
$ curl -k https://192.168.49.2:8443
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "forbidden: User \"system:anonymous\" cannot get path \"/\"",
  "reason": "Forbidden",
  "details": {},
  "code": 403
}
```

##### Set anonymous-auth=false
In kube-apiserver manifest set --anonymous-auth=false
```sh
$ curl -k https://192.168.49.2:8443
{
  "kind": "Status",
  "apiVersion": "v1",
  "metadata": {},
  "status": "Failure",
  "message": "Unauthorized",
  "reason": "Unauthorized",
  "code": 401
}
```

### 6.3.3. Insecure Access
> Since k8s 1.20 the insecure access is not longer posible. `kube-apiserver --insecure-port=8080`

#### HTTP/HTTPS Access
![cks](images/14_restrict_api_insecure_access.png)

#### Insecure Access
* kube-apiserver `--insecure-port=8080` (default: `--insecure-port=0`)
  * HTTP
  * Request bypasses authentication and authorization modules
  * Admision controller still enforces

### 6.3.4. Manual API Requests
```sh
$ curl -k https://192.168.49.2:8443 \
    --cert ~/.minikube/profiles/cksv1/client.crt \
    --key ~/.minikube/profiles/cksv1/client.key
{    
  "paths": [
    "/.well-known/openid-configuration",
    "/api",
    "/api/v1",
    "/apis",
    "/apis/",
    "/apis/admissionregistration.k8s.io",
    "/apis/admissionregistration.k8s.io/v1",
    "/apis/apiextensions.k8s.io",
    "/apis/apiextensions.k8s.io/v1",
    "/apis/apiregistration.k8s.io",
    "/apis/apiregistration.k8s.io/v1",
    "/apis/apps",
    "/apis/apps/v1",
    "/apis/authentication.k8s.io",
    "/apis/authentication.k8s.io/v1",
    "/apis/authorization.k8s.io",
    "/apis/authorization.k8s.io/v1",
    "/apis/autoscaling",
    "/apis/autoscaling/v1",
    "/apis/autoscaling/v2",
    "/apis/autoscaling/v2beta2",
    "/apis/batch",
    ...
  ]
}
```

### 6.3.5. NodeRestriction AdmissionController
![cks](images/14_restrict_api_adm_contr.png)

#### NodeRestriction
* **Admision Controller**
  * kube-apiserver --enable-admission-plugins=NodeRestriction
  * Limits the Node labels a kubelet can modify
* **Ensure secure workload isolation via labels**
  * No one can pretend to be a "secure" node and schedule secure pods

### 6.3.6. Verify NodeRestriction
On a worker node...

```sh
# Export as kubeconfig the kubelet config
$ export KUBECONFIG=/etc/kubernetes/kubelet.conf

$ kubectl get ns
Error from server (Forbidden): nampespaces is forbidden: User "system:node:cks-worker" cannot list resource "namespaces" in API group "" at the cluster scope

$ kubectl label node cks-master cks/test=yes
Error from server (Forbidden): nodes "cks-master" is forbidden: node "cks-worker" is not allowed to modify node "cks-master"

$ kubectl label node cks-worker cks/test=yes
node/cks-worker labeled

$ kubectl label node cks-worker node-restriction.kubernetes.io/test=yes
Error from server (Forbidden): nodes "cks-worker" is forbidden: is not allowed to modify labels: node-restricion.kubernetes.io/test
```

> https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#noderestriction


### 6.3.7. Recap
> https://kubernetes.io/docs/concepts/security/controlling-access

## 6.4. Upgrade Kubernetes
### 6.4.1. Intro
#### Why upgrade frequently?
* Support
* Security fixed
* Bug fixed
* Stay up to date for dependencies

#### Kubernetes Release Cycles
```sh
1.19.2
major.minor.path
```

* Minor version every 3 months
* No TLS (Long Term Support)

#### Support
Maintenance release branches for the most recent three minor releases (1.19, 1.18, 1.17)

Applicable fixes, including security fixes, may be backported to those three release branches, depending on severity and feasibility.

#### How to upgrade a cluster
* **First upgrade the master componentes**
  * apiserver, controller-manager, scheduler
* **Then the worker componentes**
  * kubelet, kube-proxy
* **Components same minor version as apiserver**
  * or one below

#### How to upgrade a node
1. `kubectl drain`
   1. Safely evict all pods from node
   2. Mark as node as SchedulingDisabled (`kubectl cordon`)
2. Do the upgrade
3. `kubectl uncordon`
   1. Unmark node as SchedulingDisabled
  
#### How to make your application survive an upgrade
* Pod graciePeriod/Terminating stateç
* Pod Lifecycle Events
* PodDisruptionBudget

### 6.4.2. Ubuntu 20.04 Update
### 6.4.3. Create outdated cluster
```sh
# master
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/previous/install_master.sh)

# worker
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/previous/install_worker.sh)
```

### 6.4.4. Upgrade controlplane node
```sh
# drain
$ kubectl drain cks-controlplane

# upgrade kubeadm
$ apt-get update
$ apt-cache show kubeadm | grep 1.22
$ apt-mark unhold kubeadm
$ apt-mark hold kubectl kubelet
$ apt-get install kubeadm=1.22.5-00
$ apt-mark hold kubeadm

# kubeadm upgrade
$ kubeadm version # correct version?
$ kubeadm upgrade plan
$ kubeadm upgrade apply 1.22.5

# kubelet and kubectl
$ apt-mark unhold kubelet kubectl
$ apt-get install kubelet=1.22.5-00 kubectl=1.22.5-00
$ apt-mark hold kubelet kubectl

# restart kubelet
$ service kubelet restart
$ service kubelet status

# show result
$ kubeadm upgrade plan
$ kubectl version

# uncordon
$ kubectl uncordon cks-controlplane
```
### 6.4.5. Upgrade node
```sh
# drain
$ kubectl drain cks-node

# upgrade kubeadm
$ apt-get update
$ apt-cache show kubeadm | grep 1.22
$ apt-mark unhold kubeadm
$ apt-mark hold kubectl kubelet
$ apt-get install kubeadm=1.22.5-00
$ apt-mark hold kubeadm

# kubeadm upgrade
$ kubeadm version # correct version?
$ kubeadm upgrade node

# kubelet and kubectl
$ apt-mark unhold kubelet kubectl
$ apt-get install kubelet=1.22.5-00 kubectl=1.22.5-00
$ apt-mark hold kubelet kubectl
 
# restart kubelet
$ service kubelet restart
$ service kubelet status

# uncordon
$ kubectl uncordon cks-node
```

### 6.4.6. Recap
> https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade

> https://kubernetes.io/docs/setup/release/version-skew-policy


# 7. Microservice Vulnerabilities
## 7.1. Manage Kubernetes
### 7.1.1. Intro
### 7.1.2. Create Simple Secret Scenario
#### Create a generic secret
```sh
$ kubectl create secret generic secret1 --from-literal pass=12345678
secret/secret1 created
```

#### Mount secret in a Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: redis
    env:
      - name: SECRET_PASSWORD
        valueFrom:
          # Mount secret as variables
          secretKeyRef:
            name: secret1
            key: pass
    # Mount secret as volume
    volumeMounts:
    - name: my-secret
      mountPath: "/etc/foo"
      readOnly: true
  volumes:
  - name: my-secret
    secret:
      secretName: secret1
``` 

```sh
$ kubectl exec -it mypod -- env | grep -i secret
SECRET_PASSWORD=12345678

$ kubectl exec -it mypod -- cat /etc/foo/pass
12345678
```

### 7.1.3. Hacks Secret in Container Runtime
#### Search "mypod"
```sh
$ crictl ps | grep mypod
a7f2d581cf409       redis@sha256:aeed51f49a6331df0cb2c1039ae3d1d70d882be3f48bde75cd240452a2348e88   8 minutes ago       Running             mypod                     0                   af04a70423e9d
```

#### Inspect container and show "envs" and "mounts"
```sh
$ crictl inspect a7f2d581cf409
{
  ...
    "env": [
      ...
      "SECRET_PASSWORD=12345678"
      ...
    ]
  ...
    "mounts": [
      {
        "containerPath": "/etc/foo",
        "hostPath": "/var/lib/kubelet/pods/89dffbe8-cf37-4ae3-b777-aa8091513c83/volumes/kubernetes.io~secret/my-secret",
        "propagation": "PROPAGATION_PRIVATE",
        "readonly": true,
        "selinuxRelabel": false
      }
  ...
}

```
### 7.1.4. Hacks Secret in ETCD
#### Access secret int etcd
```sh
$ ETCDCTL_API=3 etcdctl --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key --cacert /etc/kubernetes/pki/etcd/ca.crt endpoint health
127.0.0.1:2379 is healthy: successfully committed proposal: took = 1.34553ms
```
> --endpoints "https://127.0.0.1:2379" not necessary because we’re on same node

#### Show secret
```sh
$ ETCDCTL_API=3 etcdctl --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key --cacert /etc/kubernetes/pki/etcd/ca.crt get /registry/secrets/default/secret1

k8s


v1Secret

secret1"*$9b83951b-e49c-4490-903d-46676d885fa12餛za
kubectl-createUpdatev餛FieldsV1:-
+{"f:data":{".":{},"f:data":{}},"f:type":{}}B
datasecretOpaque"
```

### 7.1.5. ETCD Encryption
#### Encrypt
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    # https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data/#providers
    providers:
      - identity: {}
      - aesgcm:
          keys:
            - name: key1
              secret: c2VjcmV0IGlzIHNlY3VyZQ==
            - name: key2
              secret: dGhpcyBpcyBwYXNzd29yZA==
      - aescbc:
          keys:
            - name: key1
              secret: c2VjcmV0IGlzIHNlY3VyZQ==
            - name: key2
              secret: dGhpcyBpcyBwYXNzd29yZA==
      - secretbox:
          keys:
            - name: key1
              secret: YWJjZGVmZ2hpamtsbW5vcHFyc3R1dnd4eXoxMjM0NTY=
```

> `--encryption-provider-config=<file>` in API Server

#### Encrypt (all Secrets) in ETCD
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - aesgcm:
          keys:
            - name: key1
              secret: c2VjcmV0IGlzIHNlY3VyZQ==
            - name: key2
              secret: c2VjcmV0IGlzIHNlY3VyZQ==
      - identity: {}
```
```sh
$ kubectl get secrets -A -ojson | kubectl replace -f -
```

#### Decrypt all Secrets in ETCD
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
      - secrets
    providers:
      - identity: {}
      - aescbc:
          keys:
            - name: key1
              secret: <BASE 64 ENCODED SECRET>
      
```
```sh
$ kubectl get secrets -A -ojson | kubectl replace -f -
```

### 7.1.6. Encrypt ETCD (example)

#### /etc/kubernetes/etcd/ec.yaml
```yaml
apiVersion: apiserver.config.k8s.io/v1
kind: EncryptionConfiguration
resources:
  - resources:
    - secrets
    providers:
    - aesgcm:
        keys:
        - name: key1
          # echo -n this-is-very-sec | base64
          secret: dGhpcy1pcy12ZXJ5LXNlYw==
    - identity: {}
```

#### Edit API Server
```yaml
spec:
  containers:
  - command:
    - kube-apiserver
...
    - --encryption-provider-config=/etc/kubernetes/etcd/ec.yaml
...
    volumeMounts:
    - mountPath: /etc/kubernetes/etcd
      name: etcd
      readOnly: true
...
  hostNetwork: true
  priorityClassName: system-cluster-critical
  volumes:
  - hostPath:
      path: /etc/kubernetes/etcd
      type: DirectoryOrCreate
    name: etcd
...
```

#### Encrypt existing Secrets
```sh
$ ETCDCTL_API=3 etcdctl --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key --cacert /etc/kubernetes/pki/etcd/ca.crt get /registry/secrets/one/s1
/registry/secrets/one/s1
k8s


v1Secret

s1one"*$9b83951b-e49c-4490-903d-46676d885fa12餛za
kubectl-createUpdatev餛FieldsV1:-
+{"f:data":{".":{},"f:data":{}},"f:type":{}}B
datasecretOpaque"
```

```sh
kubectl get secrets -A -o json | kubectl replace -f -

$ ETCDCTL_API=3 etcdctl --cert /etc/kubernetes/pki/apiserver-etcd-client.crt --key /etc/kubernetes/pki/apiserver-etcd-client.key --cacert /etc/kubernetes/pki/etcd/ca.crt get /registry/secrets/one/s1
/registry/secrets/one/s1
k8s:enc:aesgcm:v1:key1:Li&?ųw!lSV2      ~(n4h͊ЗwyP"`;yQZ2=Jtet`%=qĕ@qӦss
                                                                       -^V*Yp\V|N3P+J@p.Dr$j]St/7e <ȏbUf"kh_?SyTV- v.Idr7(n&Pգo
```
> https://kubernetes.io/docs/tasks/administer-cluster/encrypt-data
### 7.1.7. Recap
> https://v1-22.docs.kubernetes.io/docs/concepts/configuration/secret/#risks

> https://www.youtube.com/watch?v=f4Ru6CPG1z4

> https://www.cncf.io/webinars/kubernetes-secrets-management-build-secure-apps-faster-without-secrets


## 7.2. Container Runtime
### 7.2.1. Intro
#### Technical Overview
**Containers are not contained**

Just because it runs in a container doesnt mean its more protected.

#### Technical Overview: Containers/Docker
![cks](images/17_container_runtime_intro.png)

#### Technical Overview: Sandbox

**Sandbox?**
* Playground when implement an API
* Simulated testing environment
* Development server
* When we talk about sandbox means **Security layer to reduce attack surface**

#### Technical Overview: Containers and system calls
![cks](images/17_container_runtime_intro_01.png)

#### Technical Overview: Sandbox comes not for free
* More resources needed
* Might be better for smaller containers
* Not good for syscall heavy workloads
* No direct access to hardware

### 7.2.2. Containers Calls Linux Kernel
#### Why even sandbox?
**Contact the Linux Kernel from inside a container**

```sh
# Create a Pod
$ kubectl run pod --image nginx
pod/pod created

# Exec Pod
$ kubectl exec -it pod -- bash

# Get Kernel Version inside pod
root@pod:/# uname -r
5.14.0-1054-oem

# Get Kernel Version on host
$ uname -r
5.14.0-1054-oem

# strace uname -r on host
$ strace uname -r
...
uname({sysname="Linux", nodename="adrianmartin", ...}) = 0
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0), ...}) = 0
write(1, "5.14.0-1054-oem\n", 165.14.0-1054-oem
)       = 16
close(1)                                = 0
close(2)                                = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```

[See Dirty Cow](https://dirtycow.ninja/)

### 7.2.3. Open Container Iniciative OCI
#### OCI - Open Container Initiative
* Open Container Initiative
* Linux Foundation project to design open standards for virtualization
* **Specification**
  * runtime, image, distribution
* **Runtime**
  * runc (container runtime that implements their specification)

![cks](images/17_container_runtime_oci.png)

#### Kubernetes runtimes and CRI (Container Runtime Interface)
![cks](images/17_container_runtime_oci_01.png)

![cks](images/17_container_runtime_oci_02.png)

### 7.2.4. Sandbox Runtime Katacontainers
#### kata containers
![cks](images/17_container_runtime_katacontainers.png)

* Strong separation layer
* Runs every container in its own private VM (Hypervisor based)
* QEMU as default
  * needs virtualization ,like nested virtualization in cloud

### 7.2.5. Sandbox Runtime gVisor (Google)
**user-space kernel for containers**
* Another layer of separation
* NOT hypervisor/VM based
* Simulates kernel syscalls with limited functionality
* Runs in userspace separated from linux kernel
* Runtime called **runsc**

![cks](images/17_container_runtime_gvisor.png)

### 7.2.6. Create and use RuntimeClasses
#### RuntimeClassess
**Create and use RuntimeClasses form runtime runsc (gvisor)**

```yaml
apiVersion: node.k8s.io/v1
kind: RuntimeClass
metadata:
  name: gvisor
handler: runsc
```

> https://kubernetes.io/docs/concepts/containers/runtime-class/

#### Create Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: gvisor
  name: gvisor
spec:
  runtimeClassName: gvisor
  containers:
    - image: nginx
      name: gvisor
      resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```

#### Describe pod
```sh
Events:
  Type     Reason                  Age   From               Message
  ----     ------                  ----  ----               -------
  Normal   Scheduled               10s   default-scheduler  Successfully assigned default/gvisor to cks
  Warning  FailedCreatePodSandBox  10s   kubelet            Failed to create pod sandbox: rpc error: code = Unknown desc = RuntimeHandler "runsc" not supported
```

### 7.2.7. Install and use gVisor
#### Install
```sh
#!/usr/bin/env bash
# IF THIS FAILS then you can try to change the URL= further down from specific to the latest release
# https://gvisor.dev/docs/user_guide/install

# gvisor
sudo apt-get update && \
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

# install from web
(
  set -e
  ARCH=$(uname -m)
  URL=https://storage.googleapis.com/gvisor/releases/release/20210806/${ARCH}
  # URL=https://storage.googleapis.com/gvisor/releases/release/latest/${ARCH} # TRY THIS URL INSTEAD IF THE SCRIPT DOESNT WORK FOR YOU
  wget ${URL}/runsc ${URL}/runsc.sha512 \
    ${URL}/containerd-shim-runsc-v1 ${URL}/containerd-shim-runsc-v1.sha512
  sha512sum -c runsc.sha512 \
    -c containerd-shim-runsc-v1.sha512
  rm -f *.sha512
  chmod a+rx runsc containerd-shim-runsc-v1
  sudo mv runsc containerd-shim-runsc-v1 /usr/local/bin
)

# containerd enable runsc
cat > /etc/containerd/config.toml <<EOF
disabled_plugins = []
imports = []
oom_score = 0
plugin_dir = ""
required_plugins = []
root = "/var/lib/containerd"
state = "/run/containerd"
version = 2

[plugins]
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runsc]
    runtime_type = "io.containerd.runsc.v1"

  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes]
    [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
      base_runtime_spec = ""
      container_annotations = []
      pod_annotations = []
      privileged_without_host_devices = false
      runtime_engine = ""
      runtime_root = ""
      runtime_type = "io.containerd.runc.v2"

      [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
        BinaryName = ""
        CriuImagePath = ""
        CriuPath = ""
        CriuWorkPath = ""
        IoGid = 0
        IoUid = 0
        NoNewKeyring = false
        NoPivotRoot = false
        Root = ""
        ShimCgroup = ""
        SystemdCgroup = true
EOF

systemctl restart containerd
```

```sh
$ kubectl get po
NAME   READY   STATUS    RESTARTS   AGE
gvisor    1/1     Running   0          10s

$ kubectl exec gvisor -- uname -r
4.4.0

$ kubectl get node -owide
NAME           STATUS   ROLES           AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
controlplane   Ready    control-plane   22d   v1.24.0   172.30.1.2    <none>        Ubuntu 20.04.3 LTS   5.4.0-88-generic   containerd://1.5.9
node01         Ready    <none>          22d   v1.24.0   172.30.2.2    <none>        Ubuntu 20.04.3 LTS   5.4.0-88-generic   containerd://1.5.9
```
### 7.2.8. Recap
> https://www.youtube.com/watch?v=RyXL1zOa8Bw

> https://www.youtube.com/watch?v=kxUZ4lVFuVo

> https://www.youtube.com/watch?v=4gmLXyMeYWI

## 7.3. OS Level Security
### 7.3.1. Intro and Security Context
#### Security Context
**Define privilege and access control for Pod/Container**
* userID and groupID
* Run privileged or unprivileged
* Linux Capabilities
* ...

```yaml
spec:
  # Pod Level (all containers)
  # $ id
  # uid=1000 gid=3000 groups=2000
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  containers:
  - name: sec-ctx-demo
    image: busybox:1.28
    command: [ "sh", "-c", "sleep 1h" ]
    # Container Level (pod-level override)
    securityContext:
      runAsUser: {}
```

> https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.25/#podsecuritycontext-v1-core

### 7.3.2. Set container User and Group
#### security Contexts & UID GID
**Change the user and group under which the container processes are running**

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod
  name: pod
spec:
  containers:
  - args:
    - sh
    - -c
    - sleep 1d
    image: busybox
    name: pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl exec -it pod -- sh

/ # id
uid=0(root) gid=0(root) groups=10(wheel)

/ # touch test

/ # ls -lh test
-rw-r--r--    1 root     root           0 Nov 10 12:59 test
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod
  name: pod
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
  containers:
  - args:
    - sh
    - -c
    - sleep 1d
    image: busybox
    name: pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl exec -it pod -- sh
/ $ id
uid=1000 gid=3000

/ $ touch test
touch: test: Permission denied

/ $ pwd
/

/ $ cd /tmp/

/tmp $ touch test

/tmp $ ls -lh test 
-rw-r--r--    1 1000     3000           0 Nov 10 13:00 test
```

### 7.3.3. Force container non-root
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod
  name: pod
spec:
  containers:
  #securityContext:
  #  runAsUser: 1000
  #  runAsGroup: 3000
  - args:
    - sh
    - -c
    - sleep 1d
    image: busybox
    name: pod
    resources: {}
    securityContext:
      runAsNonRoot: true
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
# Describe Pod
Events:
  Type     Reason     Age              From               Message
  ----     ------     ----             ----               -------
  Normal   Scheduled  9s               default-scheduler  Successfully assigned default/pod to cks
  Normal   Pulled     7s               kubelet            Successfully pulled image "busybox" in 1.512467061s
  Normal   Pulling    6s (x2 over 9s)  kubelet            Pulling image "busybox"
  Warning  Failed     5s (x2 over 7s)  kubelet            Error: container has runAsNonRoot and image will run as root (pod: "pod_default(acdd0e9e-e7eb-483d-abdc-586db8c5dafe)", container: pod)
```

### 7.3.4. Privileged Containers
* By default Docker containers run "unprivileged"
* Possible to run as privileged to
  * Access all devices
  * Run Docker daemon inside container. `docker run --privileged`

**Privileged means that container user 0 (root is directly mapped to host user 0 (root)**

#### Privileged Containers in Kubernetes
By default in Kubernetes containers ar not running privileged

```yaml
spec:
  containers:
  - args:
    - sh
    - -c
    - sleep 1d
    image: busybox
    name: pod
    resources: {}
    securityContext:
      privileged: true
```

### 7.3.5. Created Privileged Containers
**Enabled privileged and test using sysctl**
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod
  name: pod
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
  containers:
  - args:
    - sh
    - -c
    - sleep 1d
    image: busybox
    name: pod
    resources: {}
    securityContext:
      runAsNonRoot: true
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl exec -it pod -- sh
/ $ sysctl kernel.hostname=attacker
sysctl: error setting key 'kernel.hostname': Read-only file system
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod
  name: pod
spec:
  containers:
  - args:
    - sh
    - -c
    - sleep 1d
    image: busybox
    name: pod
    resources: {}
    securityContext:
      privileged: true
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl exec -it pod -- sh
/ # sysctl kernel.hostname=attacker
kernel.hostname = attacker
```

### 7.3.6. PrivilegeScalation
```yaml
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  containers:
  - args:
    - sh
    - -c
    - sleep 1d
    image: busybox
    name: pod
    securityContext:
      # By default Kubernetes allows PrivilegeEscalation
      allowPrivilegeEscalation: false
```

* **PrivilegeEscalation** controls whether a process can gain more privileges than its parent process.
* **Privileged** means that container user 0 (root) is directly mapped to host user 0 (root).

### 7.3.7. Disable PrivilegeScalation
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod
  name: pod
spec:
  containers:
  - args:
    - sh
    - -c
    - sleep 1d
    image: busybox
    name: pod
    resources: {}
    securityContext:
      allowPrivilegeEscalation: true
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```

```sh
$ kubectl exec -it pod -- sh
# cat /proc/1/status

Name:	sleep
...
NoNewPrivs:	0
...
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: pod
  name: pod
spec:
  containers:
  - args:
    - sh
    - -c
    - sleep 1d
    image: busybox
    name: pod
    resources: {}
    securityContext:
      allowPrivilegeEscalation: false
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```

```sh
$ kubectl exec -it pod -- sh
# cat /proc/1/status

Name:	sleep
...
NoNewPrivs:	1
...
```

### 7.3.8. PodSecurityPolicies
#### Pod Security Policies
* Cluster-level resource
* Constrols under which security conditions a Pod has to run

```yaml
containers:
  - command:
      - kube-apiserver
      - --enable-admission-plugins=PodSecurity

```

> PodSecurityPolicy was deprecated in Kubernetes v1.21, and removed from Kubernetes in v1.25.

> https://kubernetes.io/docs/concepts/security/pod-security-admission/

### 7.3.9. Create and enable PodSecurityPolicies
### 7.3.10. Recap

## 7.4. mTLS
### 7.4.1. Intro
#### mTLS - Mutual TLS
* Mutual authentication
* Two-way (bilateral) authentication
* Two parties authenticating each other at the same time

### 7.4.2. Create sidecar proxy
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: app
  name: app
spec:
  containers:
  - command:
    - sh
    - -c
    - ping google.com
    image: bash
    name: app
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl logs -f app
kubectl logs app -f
PING google.com (142.250.185.14): 56 data bytes
64 bytes from 142.250.185.14: seq=0 ttl=113 time=15.487 ms
64 bytes from 142.250.185.14: seq=1 ttl=113 time=17.122 ms
64 bytes from 142.250.185.14: seq=2 ttl=113 time=15.773 ms
64 bytes from 142.250.185.14: seq=3 ttl=113 time=18.471 ms
64 bytes from 142.250.185.14: seq=4 ttl=113 time=16.169 ms
64 bytes from 142.250.185.14: seq=5 ttl=113 time=16.307 ms
64 bytes from 142.250.185.14: seq=6 ttl=113 time=19.109 ms
64 bytes from 142.250.185.14: seq=7 ttl=113 time=15.561 ms
^C
```
#### Without Capabilities
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: app
  name: app
spec:
  containers:
  - command:
    - sh
    - -c
    - ping google.com
    image: bash
    name: app
    resources: {}
  - name: proxy
    image: ubuntu
    command:
    - sh
    - -c
    - 'apt-get update && apt-get install iptables -y && iptables -L && sleep 1d'
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl logs app proxy -f
....
update-alternatives: using /usr/sbin/ebtables-nft to provide /usr/sbin/ebtables (ebtables) in auto mode
Processing triggers for libc-bin (2.35-0ubuntu3.1) ...
iptables v1.8.7 (nf_tables): Could not fetch rule set generation id: Permission denied (you must be root)
```

#### With Capabilities
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: app
  name: app
spec:
  containers:
  - command:
    - sh
    - -c
    - ping google.com
    image: bash
    name: app
    resources: {}
  - name: proxy
    image: ubuntu
    command:
    - sh
    - -c
    - 'apt-get update && apt-get install iptables -y && iptables -L && sleep 1d'
    securityContext:
      capabilities:
        add: ["NET_ADMIN"]
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl logs app proxy -f
...
update-alternatives: using /usr/sbin/ebtables-nft to provide /usr/sbin/ebtables (ebtables) in auto mode
Processing triggers for libc-bin (2.35-0ubuntu3.1) ...
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination 
```

### 7.4.3. Recap



# 8. Open Policy Agent (OPA)
## 8.1. Introduction
### OPA - Open Policy Agent
"The Open Policy Agent (OPA) is an open source, general-purpose policy engine that enables unified, context-aware policy enforcement across the entire stack."
* Not Kubernetes specific
* Easy implementation of policies (Rego lenguage)
* Work with JSON/YAML
* In k8s it uses Admission Controllers
* Does not know concepts like pods or deployments

### OPA - Gatekeeper
Compared to using OPA with its sidecar kube-mgmt (aka Gatekeeper v1.0), Gatekeeper introduces the following functionality:

* An extensible, parameterized policy library
* Native Kubernetes CRDs for instantiating the policy library (aka "constraints")
* Native Kubernetes CRDs for extending the policy library (aka "constraint templates")
* Audit functionality

```yaml
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8srequiredlabels
spec:
...
---
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: ns-must-have-hr
spec:
...
```

## 8.2. Install OPA
## 8.3. Deny All Policy
```sh
$ kubectl get crd
NAME                                                 CREATED AT
configs.config.gatekeeper.sh                         2022-11-10T17:50:42Z
constraintpodstatuses.status.gatekeeper.sh           2022-11-10T17:50:42Z
constrainttemplatepodstatuses.status.gatekeeper.sh   2022-11-10T17:50:42Z
constrainttemplates.templates.gatekeeper.sh          2022-11-10T17:50:42Z
```

```yaml
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8salwaysdeny
spec:
  crd:
    spec:
      names:
        kind: K8sAlwaysDeny
      validation:
        # Schema for the `parameters` field
        openAPIV3Schema:
          properties:
            message:
              type: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8salwaysdeny
        violation[{"msg": msg}] {
          1 > 0
          msg := input.parameters.message
        }
---
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sAlwaysDeny
metadata:
  name: pod-always-deny
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
  parameters:
    message: "ACCESS DENIED!"
```

```sh
$ kubectl run pod --image nginx
Error from server ([pod-always-deny] ACCESS DENIED!): admission webhook "validation.gatekeeper.sh" denied the request: [pod-always-deny] ACCESS DENIED!

$ kubectl get k8salwaysdeny.constraints.gatekeeper.sh
...
  Total Violations:  11
...
```

## 8.4. Enforce Namespace Labels
```yaml
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8srequiredlabels
spec:
  crd:
    spec:
      names:
        kind: K8sRequiredLabels
      validation:
        # Schema for the `parameters` field
        openAPIV3Schema:
          properties:
            labels:
              type: array
              items: string
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8srequiredlabels
        violation[{"msg": msg, "details": {"missing_labels": missing}}] {
          provided := {label | input.review.object.metadata.labels[label]}
          required := {label | label := input.parameters.labels[_]}
          missing := required - provided
          count(missing) > 0
          msg := sprintf("you must provide labels: %v", [missing])
        }
---
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: ns-must-have-cks
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Namespace"]
  parameters:
    labels: ["cks"]
---
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sRequiredLabels
metadata:
  name: pod-must-have-cks
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
  parameters:
    labels: ["cks"]
```

```sh
$ kubectl describe k8srequiredlabels ns-must-have-cks
...
  Total Violations:  5
  Violations:
    Enforcement Action:  deny
    Kind:                Namespace
    Message:             you must provide labels: {"cks"}
    Name:                default
    Enforcement Action:  deny
    Kind:                Namespace
    Message:             you must provide labels: {"cks"}
    Name:                gatekeeper-system
    Enforcement Action:  deny
    Kind:                Namespace
    Message:             you must provide labels: {"cks"}
    Name:                kube-node-lease
    Enforcement Action:  deny
    Kind:                Namespace
    Message:             you must provide labels: {"cks"}
    Name:                kube-public
    Enforcement Action:  deny
    Kind:                Namespace
    Message:             you must provide labels: {"cks"}
    Name:                kube-system
```

```sh
$ kubectl edit ns default
...
  labels:
    cks: amazing
...
```

```sh
$ kubectl describe k8srequiredlabels ns-must-have-cks
...
  Total Violations:  4
  Violations:
    Enforcement Action:  deny
    Kind:                Namespace
    Message:             you must provide labels: {"cks"}
    Name:                gatekeeper-system
    Enforcement Action:  deny
    Kind:                Namespace
    Message:             you must provide labels: {"cks"}
    Name:                kube-node-lease
    Enforcement Action:  deny
    Kind:                Namespace
    Message:             you must provide labels: {"cks"}
    Name:                kube-public
    Enforcement Action:  deny
    Kind:                Namespace
    Message:             you must provide labels: {"cks"}
    Name:                kube-system
```

## 8.5. Enforce Deployment Replica
```yaml
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8sminreplicacount
spec:
  crd:
    spec:
      names:
        kind: K8sMinReplicaCount
      validation:
        # Schema for the `parameters` field
        openAPIV3Schema:
          properties:
            min:
              type: integer
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8sminreplicacount
        violation[{"msg": msg, "details": {"missing_replicas": missing}}] {
          provided := input.review.object.spec.replicas
          required := input.parameters.min
          missing := required - provided
          missing > 0
          msg := sprintf("you must provide %v more replicas", [missing])
        }
---
apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sMinReplicaCount
metadata:
  name: deployment-must-have-min-replicas
spec:
  match:
    kinds:
      - apiGroups: ["apps"]
        kinds: ["Deployment"]
  parameters:
    min: 2
```

```sh
$ kubectl create deploy test --image nginx 
error: failed to create deployment: admission webhook "validation.gatekeeper.sh" denied the request: [deployment-must-have-min-replicas] you must provide 1 more replicas
```
## 8.6. The Rego Playground and more examples
> https://play.openpolicyagent.org

> https://github.com/BouweCeunen/gatekeeper-policies
## 8.7. Recap
> https://www.youtube.com/watch?v=RDWndems-sk


# 9. Supply Chain Security
## 9.1. Image footprint
### 9.1.1. Introduction
#### Containers and Docker - Layers
```sh
FROM ubuntu # Import layers
RUN apt-get update && apt-get install -y golang-go # add new layer
CMD ["sh"]
```

> Only the instructions RUN, COPY, ADD create layers. Other instrucctions create temporary intermediante images, and do not increase the size of the build.

### 9.1.2. Reduce image Footprint with Multi-Stage
**We look at an example Golang Dockerfile and reduce the image footprint via Multi-Stage build**

#### Build image with app code
```sh
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go
CMD ["./app"]
```

```go
package main

import (
    "fmt"
    "time"
    "os/user"
)

func main () {
    user, err := user.Current()
    if err != nil {
        panic(err)
    }

    for {
        fmt.Println("user: " + user.Username + " id: " + user.Uid)
        time.Sleep(1 * time.Second)
    }
}
```

```sh
$ docker build -t app .
Sending build context to Docker daemon  3.072kB
Step 1/6 : FROM ubuntu
 ---> cdb68b455a14
Step 2/6 : ARG DEBIAN_FRONTEND=noninteractive
 ---> Running in 857b06df4878
Removing intermediate container 857b06df4878
 ---> ba624f4e3449
Step 3/6 : RUN apt-get update && apt-get install -y golang-go
 ---> Running in 0be82ffee63a
Get:1 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
Get:2 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
Get:3 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [114 kB]
Get:4 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [580 kB]
Get:5 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [99.8 kB]
Get:6 http://archive.ubuntu.com/ubuntu jammy/main amd64 Packages [1792 kB]
Get:7 http://security.ubuntu.com/ubuntu jammy-security/multiverse amd64 Packages [4644 B]
...
```

#### Show size
```sh
$ docker image ls | grep app
app                                                                latest              d5126f532faa   43 seconds ago   860MB
```

#### Rebuild 
```sh
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go

FROM alpine
COPY --from=0 /app .

CMD ["./app"]
```

#### Show size
```sh
docker image ls | grep app
app                                                                latest              399a853a78bc   9 seconds ago   7.41MB
```

### 9.1.3. Secure and Harden images
#### Use specifig package version
```sh
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go

FROM alpine:3.12.1:wq

COPY --from=0 /app .

CMD ["./app"]
```

#### Dont run as root
```
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go

FROM alpine:3.12.1
RUN addgroup -S appgroup && adduser -S appuser -G appgroup -h /home/appuser
COPY --from=0 /app .
USER appuser
CMD ["./app"]
```

#### Make filesystem read only
```
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go

FROM alpine:3.12.1
RUN chmod a-w /etc
RUN addgroup -S appgroup && adduser -S appuser -G appgroup -h /home/appuser
COPY --from=0 /app .
USER appuser
CMD ["./app"]
```

#### Remove shell access
```
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN CGO_ENABLED=0 go build app.go

FROM alpine:3.12.1
RUN chmod a-w /etc
RUN addgroup -S appgroup && adduser -S appuser -G appgroup -h /home/appuser
RUN rm -rf /bin/*
COPY --from=0 /app .
USER appuser
CMD ["./app"]
```

### 9.1.4. Recap
> https://docs.docker.com/develop/develop-images/dockerfile_best-practices

## 9.2. Static Analysis
### 9.2.1. Introduction
#### Static Analysis
* Looks at source code and text files
* check against rules
* Enforce rules

#### Static Analysis Rules
**Always define resource request and limits** (Depend on use case and company or project)
**Pods should never use the default ServiceAccount** (Generally: dont store sensitive data plain in K8s/Docker files)

#### Static Analysis in CI/CD
![cks](images/22_static_analysis_intro.png)

#### Manual Check
##### Insecure
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-secure-pod
spec:
  containers:
  - image: bash
    command: ['sh', '-c', 'curl http://my-service/auth?token=123456789']
    name: my-secure-pod
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
---
apiVersion: v1
kind: Pod
metadata:
  name: my-secure-pod
spec:
  containers:
  - image: bash
    command: ['sh', '-c', 'curl http://my-service/auth?token=$TOKEN']
    name: my-secure-pod
    env:
    - name: TOKEN
      value: 123456789
  dnsPolicy: ClusterFirst
  restartPolicy: Always
---
apiVersion: v1
kind: Pod
metadata:
  name: my-secure-pod
spec:
  containers:
  - image: bash
    command: ['sh', '-c', 'curl http://my-service/auth?token=$TOKEN']
    name: my-secure-pod
    env:
    - name: TOKEN
      valueFrom: 
        configMapKeyRef:
          name: my-secure-pod-token
          key: token
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```

##### Secure
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-secure-pod
spec:
  containers:
  - image: bash
    command: ['sh', '-c', 'curl http://my-service/auth?token=$TOKEN']
    name: my-secure-pod
    env:
    - name: TOKEN
      valueFrom: 
        secretKeyRef:
          name: my-secure-pod-token
          key: token
  dnsPolicy: ClusterFirst
  restartPolicy: Always
```

### 9.2.2. Kubesec
* Security risk analysis for Kubernetes resources
* Opensource
* Opinionated! Fixed set of rules (Security Best Practices)
* Run as:
  * Binary
  * Docker container
  * Kubectl plugin
  * Admission Controller (kubesec-webhook)

### 9.2.3. Practice Kubesec
**We use Kubesec to perform static analysis**
Using the kubesec public docker image

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
docker run -i kubesec/kubesec:512c5e0 scan /dev/stdin < pod.yaml
[
  {
    "object": "Pod/nginx.default",
    "valid": true,
    "message": "Passed with a score of 0 points",
    "score": 0,
    "scoring": {
      "advise": [
        {
          "selector": ".metadata .annotations .\"container.apparmor.security.beta.kubernetes.io/nginx\"",
          "reason": "Well defined AppArmor policies may provide greater protection from unknown threats. WARNING: NOT PRODUCTION READY"
        },
        {
          "selector": "containers[] .resources .limits .cpu",
          "reason": "Enforcing CPU limits prevents DOS via resource exhaustion"
        },
        {
          "selector": "containers[] .resources .limits .memory",
          "reason": "Enforcing memory limits prevents DOS via resource exhaustion"
        },
        {
          "selector": ".metadata .annotations .\"container.seccomp.security.alpha.kubernetes.io/pod\"",
          "reason": "Seccomp profiles set minimum privilege and secure against unknown threats"
        },
        {
          "selector": "containers[] .resources .requests .cpu",
          "reason": "Enforcing CPU requests aids a fair balancing of resources across the cluster"
        },
        {
          "selector": "containers[] .securityContext .runAsUser -gt 10000",
          "reason": "Run as a high-UID user to avoid conflicts with the host's user table"
        },
        {
          "selector": "containers[] .securityContext .runAsNonRoot == true",
          "reason": "Force the running image to run as a non-root user to ensure least privilege"
        },
        {
          "selector": "containers[] .securityContext .capabilities .drop",
          "reason": "Reducing kernel capabilities available to a container limits its attack surface"
        },
        {
          "selector": "containers[] .securityContext .readOnlyRootFilesystem == true",
          "reason": "An immutable root filesystem can prevent malicious binaries being added to PATH and increase attack cost"
        },
        {
          "selector": "containers[] .securityContext .capabilities .drop | index(\"ALL\")",
          "reason": "Drop all capabilities and add only those required to reduce syscall attack surface"
        },
        {
          "selector": ".spec .serviceAccountName",
          "reason": "Service accounts restrict Kubernetes API access and should be configured with least privilege"
        },
        {
          "selector": "containers[] .resources .requests .memory",
          "reason": "Enforcing memory requests aids a fair balancing of resources across the cluster"
        }
      ]
    }
  }
]
```

### 9.2.4. OPA Conftest
* OPA = Open Policy Agent
* Unit test framework for Kubernetes configurations
* Uses Rego lenguage

```
package main
deny[msg] {
  input.kind = "Deployment"
  not input.spec.template.spec.securityContext.runAsNonRoot = true
  msg = "Containers must not run as root"
}
```

### 9.2.5. OPA Conftest for K8s YAML
**Use conftest to check a k8s example**
```
# from https://www.conftest.dev
package main

deny[msg] {
  input.kind = "Deployment"
  not input.spec.template.spec.securityContext.runAsNonRoot = true
  msg = "Containers must not run as root"
}

deny[msg] {
  input.kind = "Deployment"
  not input.spec.selector.matchLabels.app
  msg = "Containers must provide app label for pod selectors"
}
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: test
  name: test
spec:
  replicas: 1
  selector:
    matchLabels:
      app: test
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: test
    spec:
      containers:
        - image: httpd
          name: httpd
          resources: {}
status: {}
```

```sh
$ docker run --rm -v $(pwd):/project openpolicyagent/conftest test deploy.yaml
...

FAIL - deploy.yaml - main - Containers must not run as root

2 tests, 1 passed, 0 warnings, 1 failure, 0 exceptions
```

#### Fixed
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: test
  name: test
spec:
  replicas: 1
  selector:
    matchLabels:
      app: test
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: test
    spec:
      securityContext:
        runAsNonRoot: true
      containers:
        - image: httpd
          name: httpd
          resources: {}
status: {}
```

```sh
$ docker run --rm -v $(pwd):/project openpolicyagent/conftest test deploy.yaml

2 tests, 2 passed, 0 warnings, 0 failures, 0 exceptions
```

### 9.2.6. OPA Conftest for Dockerfile
```sh
FROM ubuntu
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y golang-go
COPY app.go .
RUN go build app.go
CMD ["./app"]
```

```
# from https://www.conftest.dev
package main

denylist = [
  "ubuntu"
]

deny[msg] {
  input[i].Cmd == "from"
  val := input[i].Value
  contains(val[i], denylist[_])

  msg = sprintf("unallowed image found %s", [val])
}
```

```
# from https://www.conftest.dev

package commands

denylist = [
  "apk",
  "apt",
  "pip",
  "curl",
  "wget",
]

deny[msg] {
  input[i].Cmd == "run"
  val := input[i].Value
  contains(val[_], denylist[_])

  msg = sprintf("unallowed commands found %s", [val])
}
```

```sh
$ docker run --rm -v $(pwd):/project openpolicyagent/conftest test Dockerfile --all-namespaces
FAIL - Dockerfile - main - unallowed image found ["ubuntu"]
FAIL - Dockerfile - commands - unallowed commands found ["apt-get update && apt-get install -y golang-go"]

2 tests, 0 passed, 0 warnings, 2 failures, 0 exceptions
```

### 9.2.7. Recap

## 9.3. Image Vulnerability Scanning
### 9.3.1. Introduction
**Webservers or other apps can contain vulnerabilities** (Buffer overflows)

![cks](images/23_image_vulnerability_intro.png)

#### Known Image Vulnerabilities
**Databases**
* https://cve.mitre.org
* https://nvd.nist.gov

**Vulnerabilities can be discovered in our own image and dependencies**
* Check during build
* Check at runtime

### 9.3.2. Clair and Trivy
#### Clair
* Open source project
* Static analysis of vulnerabilities in application containers
* Ingests vulnerability metadata from a configured set of courses
* Provides API

#### Trivy
* Open source project
* "A simple and Comprehensive Vulnerability Scanner for Containers and other Artifacts, Suitable for CI"
* Simple, Easy and Fast

### 9.3.3. Use Trivy to scan images
```sh
$ docker run ghcr.io/aquasecurity/trivy:latest image nginx:latest
Unable to find image 'ghcr.io/aquasecurity/trivy:latest' locally
latest: Pulling from aquasecurity/trivy
213ec9aee27d: Already exists 
ad53b2e0219a: Pull complete 
2399349afd31: Pull complete 
dc0298aa2f10: Pull complete 
Digest: sha256:a5544f44ca957135921410f4d3fa340d42b6ab56bbb6bf7406d783df9e84f95f
Status: Downloaded newer image for ghcr.io/aquasecurity/trivy:latest
2022-11-11T11:55:22.284Z	INFO	Need to update DB
2022-11-11T11:55:22.285Z	INFO	DB Repository: ghcr.io/aquasecurity/trivy-db
2022-11-11T11:55:22.285Z	INFO	Downloading DB...
2022-11-11T11:55:45.709Z	INFO	Secret scanning is enabled
2022-11-11T11:55:45.709Z	INFO	If your scanning is slow, please try '--security-checks vuln' to disable secret scanning
2022-11-11T11:55:45.709Z	INFO	Please see also https://aquasecurity.github.io/trivy/v0.34/docs/secret/scanning/#recommendation for faster secret detection
...
├──────────────────┼──────────────────┤          ├─────────────────────────┼─────────────────────────┼──────────────────────────────────────────────────────────────┤
│ tar              │ CVE-2005-2541    │          │ 1.34+dfsg-1             │                         │ tar: does not properly warn the user when extracting setuid  │
│                  │                  │          │                         │                         │ or setgid...                                                 │
│                  │                  │          │                         │                         │ https://avd.aquasec.com/nvd/cve-2005-2541                    │
├──────────────────┼──────────────────┤          ├─────────────────────────┼─────────────────────────┼──────────────────────────────────────────────────────────────┤
│ util-linux       │ CVE-2022-0563    │          │ 2.36.1-8+deb11u1        │                         │ util-linux: partial disclosure of arbitrary files in chfn    │
│                  │                  │          │                         │                         │ and chsh when compiled...                                    │
│                  │                  │          │                         │                         │ https://avd.aquasec.com/nvd/cve-2022-0563                    │
└──────────────────┴──────────────────┴──────────┴─────────────────────────┴─────────────────────────┴──────────────────────────────────────────────────────────────┘
```
> https://github.com/aquasecurity/trivy#docker

### 9.3.4. Recap

## 9.4. Secure Supply Chain
### 9.4.1. Introduction
#### K8s and Container Registries
**Private registry with Docker**

```sh
$ docker pull wuestkamp/cks-hello-world
Using default tag: latest
Error response from daemon: pull access denied for wuestkamp/cks-hello-world, repository does not exist or may require 'docker login': denied: requested access to the resource is denied

$ docker login
Login Succeeded

$ docker pull wuestkamp/cks-hello-world
Using default tag: latest
latest: pulling from wuestkamp/cks-hello-world
...
...
Status: Downloaded newer image for wuestkamp/cks-hello-world
```

**Private registyry with Kubernetes**
```sh
$ kubectl create secret docker-registry my-private-registry \
  --docker-server=my-private-registry-server \
  --docker-username=username \
  --docker-pasword=password \
  --docker-email=email
secret/my-private-registry created
```

```sh
$ kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "my-private-registry"}]}'
serviceaccount/default patched
```

### 9.4.2. Image Digest
**List all image registries used in the whole cluster**
**Use Image digest for kube-apiserver**

```sh
$ kubectl get po -A -oyaml | grep "image:"
      image: openpolicyagent/gatekeeper:v3.5.2
      image: openpolicyagent/gatekeeper:v3.5.2
      image: openpolicyagent/gatekeeper:v3.5.2
      image: openpolicyagent/gatekeeper:v3.5.2
      image: openpolicyagent/gatekeeper:v3.5.2
      image: openpolicyagent/gatekeeper:v3.5.2
      image: openpolicyagent/gatekeeper:v3.5.2
      image: openpolicyagent/gatekeeper:v3.5.2
      image: registry.k8s.io/coredns/coredns:v1.9.3
      image: registry.k8s.io/coredns/coredns:v1.9.3
      image: registry.k8s.io/etcd:3.5.4-0
      image: registry.k8s.io/etcd:3.5.4-0
      image: registry.k8s.io/kube-apiserver:v1.25.0
      image: registry.k8s.io/kube-apiserver:v1.25.0
      image: registry.k8s.io/kube-controller-manager:v1.25.0
      image: registry.k8s.io/kube-controller-manager:v1.25.0
      image: registry.k8s.io/kube-proxy:v1.25.0
      image: registry.k8s.io/kube-proxy:v1.25.0
      image: registry.k8s.io/kube-scheduler:v1.25.0
      image: registry.k8s.io/kube-scheduler:v1.25.0
      image: gcr.io/k8s-minikube/storage-provisioner:v5
      image: gcr.io/k8s-minikube/storage-provisioner:v5
```

```sh
kubectl -n kube-system get pod kube-apiserver-cks -oyaml
...
  containerStatuses:
  - containerID: docker://7685bb38069b7d15312685fdb5dc2b60365b66c2c79cf14ed549116219702f44
    image: registry.k8s.io/kube-apiserver:v1.25.0
    imageID: docker-pullable://registry.k8s.io/kube-apiserver@sha256:f6902791fb9aa6e283ed7d1d743417b3c425eec73151517813bef1539a66aefa
    lastState:
...
```

```sh
$ vim /etc/kubernetes/manifest/kube-apiserver.yaml
# Set
image: registry.k8s.io/kube-apiserver@sha256:f6902791fb9aa6e283ed7d1d743417b3c425eec73151517813bef1539a66aefa
```
```sh
$ kubectl -n kube-system get pod | grep api
kube-apiserver-cks   1/1   Running  0  1m
```

### 9.4.3. Whitelist Registries with OPA
**Whitelist some registries using OPA**
Only images from docker.io and k8s.gcr.io can be used


```sh
# Install OPA
kubectl create -f https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/course-content/opa/gatekeeper.yaml
```

```yaml
apiVersion: templates.gatekeeper.sh/v1beta1
kind: ConstraintTemplate
metadata:
  name: k8strustedimages
spec:
  crd:
    spec:
      names:
        kind: K8sTrustedImages
  targets:
    - target: admission.k8s.gatekeeper.sh
      rego: |
        package k8strustedimages
        violation[{"msg": msg}] {
          image := input.review.object.spec.containers[_].image
          not startswith(image, "docker.io/")
          not startswith(image, "k8s.gcr.io/")
          msg := "not trusted image!"
        }
---

apiVersion: constraints.gatekeeper.sh/v1beta1
kind: K8sTrustedImages
metadata:
  name: pod-trusted-images
spec:
  match:
    kinds:
      - apiGroups: [""]
        kinds: ["Pod"]
```

```sh
$ kubectl get constrainttemplate
NAME               AGE
k8strustedimages   15s

$ kubectl get k8strustedimages.constraints.gatekeeper.sh 
NAME                 AGE
pod-trusted-images   94s

$ kubectl describe k8strustedimages.constraints.gatekeeper.sh
...
Total Violations:  11
  Violations:
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                gatekeeper-audit-649cbfdc8f-mfvlg
    Namespace:           gatekeeper-system
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                gatekeeper-controller-manager-57575f6fc7-26rjx
    Namespace:           gatekeeper-system
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                gatekeeper-controller-manager-57575f6fc7-fbc92
    Namespace:           gatekeeper-system
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                gatekeeper-controller-manager-57575f6fc7-ncvkj
    Namespace:           gatekeeper-system
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                coredns-565d847f94-wmk2b
    Namespace:           kube-system
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                etcd-cks
    Namespace:           kube-system
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                kube-apiserver-cks
    Namespace:           kube-system
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                kube-controller-manager-cks
    Namespace:           kube-system
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                kube-proxy-9skqg
    Namespace:           kube-system
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                kube-scheduler-cks
    Namespace:           kube-system
    Enforcement Action:  deny
    Kind:                Pod
    Message:             not trusted image!
    Name:                storage-provisioner
    Namespace:           kube-system
...

$ kubectl run nginx --image=nginx
Error from server ([pod-trusted-images] not trusted image!): admission webhook "validation.gatekeeper.sh" denied the request: [pod-trusted-images] not trusted image!
```

### 9.4.4. ImagePolicyWebhook
![cks](images/24_secure_supply_chain_imagepolicy.png)

```json
{
  "apiVersion":"imagepolicy.k8s.io/v1alpha1",
  "kind":"ImageReview",
  "spec":{
    "containers":[
      {
        "image":"myrepo/myimage:v1"
      },
      {
        "image":"myrepo/myimage@sha256:beb6bd6a68f114c1dc2ea4b28db81bdf91de202a9014972bec5e4d9171d90ed"
      }
    ],
    "annotations":{
      "mycluster.image-policy.k8s.io/ticket-1234": "break-glass"
    },
    "namespace":"mynamespace"
  }
}
```

### 9.4.5. Practice ImagePolicyWebhook
**Investigate ImagePolicyWebhook**
And use it up to the point where it calls an external service

```sh
$ vi /etc/kubernetes/manifest/kube-apiserver.yaml
...
- --enable-admission-plugins=NodeRestriction,ImagePolicyWebhook
- --admission-control-config-file=/etc/kubernetes/policywebhook/admission_config.json
...
```

```json
{
   "apiVersion": "apiserver.config.k8s.io/v1",
   "kind": "AdmissionConfiguration",
   "plugins": [
      {
         "name": "ImagePolicyWebhook",
         "configuration": {
            "imagePolicy": {
               "kubeConfigFile": "/etc/kubernetes/policywebhook/kubeconf",
               "allowTTL": 100,
               "denyTTL": 50,
               "retryBackoff": 500,
               "defaultAllow": false
            }
         }
      }
   ]
}
```

```yaml
apiVersion: v1
kind: Config

# clusters refers to the remote service.
clusters:
- cluster:
    certificate-authority: /etc/kubernetes/policywebhook/external-cert.pem  # CA for verifying the remote service.
    server: https://localhost:1234                   # URL of remote service to query. Must use 'https'.
  name: image-checker

contexts:
- context:
    cluster: image-checker
    user: api-server
  name: image-checker
current-context: image-checker
preferences: {}

# users refers to the API server's webhook configuration.
users:
- name: api-server
  user:
    client-certificate: /etc/kubernetes/policywebhook/apiserver-client-cert.pem     # cert for the webhook admission controller to use
    client-key:  /etc/kubernetes/policywebhook/apiserver-client-key.pem             # key matching the cert
```

```sh
# get example
git clone https://github.com/killer-sh/cks-course-environment.git
cp -r cks-course-environment/course-content/supply-chain-security/secure-the-supply-chain/whitelist-registries/ImagePolicyWebhook/ /etc/kubernetes/admission


# to debug the apiserver we check logs in:
/var/log/pods/kube-system_kube-apiserver*


# example of an external service which can be used
https://github.com/flavio/kube-image-bouncer
```
### 9.4.6. Recap



# 10. Runtime Security
## 10.1. Behavioral Analytics at host and ...
### 10.1.1. Introduction
#### Kernel vs User Space
![cks](images/25_behaviorial%20analytics_intro.png)

> https://man7.org/linux/man-pages/man2/syscalls.2.html


### 10.1.2. Strace
* Intercepts and logs system calls made by a process
* Log and display signals received by a process
* Diagnostic, Learning, Debugging

#### strace: show syscalls
**strace "ls"**
Investigate what it does and find all syscalls

```sh
$ strace
  -o filename
  -v verbose
  -f follow forks

  -cw (counts and summarise)
  -p pid
  -P path
```

```sh
$ strace ls /
execve("/usr/bin/ls", ["ls", "/"], 0x7ffd6243ad38 /* 66 vars */) = 0
brk(NULL)                               = 0x55bb5e13b000
arch_prctl(0x3001 /* ARCH_??? */, 0x7fff18746440) = -1 EINVAL (Invalid argument)
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=73965, ...}) = 0
mmap(NULL, 73965, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f785d146000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libselinux.so.1", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0@p\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=163200, ...}) = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f785d144000
mmap(NULL, 174600, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f785d119000
mprotect(0x7f785d11f000, 135168, PROT_NONE) = 0
mmap(0x7f785d11f000, 102400, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x6000) = 0x7f785d11f000
mmap(0x7f785d138000, 28672, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1f000) = 0x7f785d138000
mmap(0x7f785d140000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x26000) = 0x7f785d140000
mmap(0x7f785d142000, 6664, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f785d142000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\3\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\300A\2\0\0\0\0\0"..., 832) = 832
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0\20\0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0", 32, 848) = 32
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0\30x\346\264ur\f|Q\226\236i\253-'o"..., 68, 880) = 68
fstat(3, {st_mode=S_IFREG|0755, st_size=2029592, ...}) = 0
pread64(3, "\6\0\0\0\4\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0@\0\0\0\0\0\0\0"..., 784, 64) = 784
pread64(3, "\4\0\0\0\20\0\0\0\5\0\0\0GNU\0\2\0\0\300\4\0\0\0\3\0\0\0\0\0\0\0", 32, 848) = 32
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0\30x\346\264ur\f|Q\226\236i\253-'o"..., 68, 880) = 68
mmap(NULL, 2037344, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f785cf27000
mmap(0x7f785cf49000, 1540096, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x22000) = 0x7f785cf49000
mmap(0x7f785d0c1000, 319488, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x19a000) = 0x7f785d0c1000
mmap(0x7f785d10f000, 24576, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1e7000) = 0x7f785d10f000
mmap(0x7f785d115000, 13920, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f785d115000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpcre2-8.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\340\"\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=588488, ...}) = 0
mmap(NULL, 590632, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f785ce96000
mmap(0x7f785ce98000, 413696, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x2000) = 0x7f785ce98000
mmap(0x7f785cefd000, 163840, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x67000) = 0x7f785cefd000
mmap(0x7f785cf25000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x8e000) = 0x7f785cf25000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libdl.so.2", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0 \22\0\0\0\0\0\0"..., 832) = 832
fstat(3, {st_mode=S_IFREG|0644, st_size=18848, ...}) = 0
mmap(NULL, 20752, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f785ce90000
mmap(0x7f785ce91000, 8192, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1000) = 0x7f785ce91000
mmap(0x7f785ce93000, 4096, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3000) = 0x7f785ce93000
mmap(0x7f785ce94000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x3000) = 0x7f785ce94000
close(3)                                = 0
openat(AT_FDCWD, "/lib/x86_64-linux-gnu/libpthread.so.0", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\2\1\1\0\0\0\0\0\0\0\0\0\3\0>\0\1\0\0\0\220q\0\0\0\0\0\0"..., 832) = 832
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0{E6\364\34\332\245\210\204\10\350-\0106\343="..., 68, 824) = 68
fstat(3, {st_mode=S_IFREG|0755, st_size=157224, ...}) = 0
pread64(3, "\4\0\0\0\24\0\0\0\3\0\0\0GNU\0{E6\364\34\332\245\210\204\10\350-\0106\343="..., 68, 824) = 68
mmap(NULL, 140408, PROT_READ, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0x7f785ce6d000
mmap(0x7f785ce73000, 69632, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x6000) = 0x7f785ce73000
mmap(0x7f785ce84000, 24576, PROT_READ, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x17000) = 0x7f785ce84000
mmap(0x7f785ce8a000, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1c000) = 0x7f785ce8a000
mmap(0x7f785ce8c000, 13432, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0x7f785ce8c000
close(3)                                = 0
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f785ce6b000
arch_prctl(ARCH_SET_FS, 0x7f785ce6c400) = 0
mprotect(0x7f785d10f000, 16384, PROT_READ) = 0
mprotect(0x7f785ce8a000, 4096, PROT_READ) = 0
mprotect(0x7f785ce94000, 4096, PROT_READ) = 0
mprotect(0x7f785cf25000, 4096, PROT_READ) = 0
mprotect(0x7f785d140000, 4096, PROT_READ) = 0
mprotect(0x55bb5c315000, 4096, PROT_READ) = 0
mprotect(0x7f785d186000, 4096, PROT_READ) = 0
munmap(0x7f785d146000, 73965)           = 0
set_tid_address(0x7f785ce6c6d0)         = 128246
set_robust_list(0x7f785ce6c6e0, 24)     = 0
rt_sigaction(SIGRTMIN, {sa_handler=0x7f785ce73bf0, sa_mask=[], sa_flags=SA_RESTORER|SA_SIGINFO, sa_restorer=0x7f785ce81420}, NULL, 8) = 0
rt_sigaction(SIGRT_1, {sa_handler=0x7f785ce73c90, sa_mask=[], sa_flags=SA_RESTORER|SA_RESTART|SA_SIGINFO, sa_restorer=0x7f785ce81420}, NULL, 8) = 0
rt_sigprocmask(SIG_UNBLOCK, [RTMIN RT_1], NULL, 8) = 0
prlimit64(0, RLIMIT_STACK, NULL, {rlim_cur=8192*1024, rlim_max=RLIM64_INFINITY}) = 0
statfs("/sys/fs/selinux", 0x7fff18746390) = -1 ENOENT (No such file or directory)
statfs("/selinux", 0x7fff18746390)      = -1 ENOENT (No such file or directory)
brk(NULL)                               = 0x55bb5e13b000
brk(0x55bb5e15c000)                     = 0x55bb5e15c000
openat(AT_FDCWD, "/proc/filesystems", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0444, st_size=0, ...}) = 0
read(3, "nodev\tsysfs\nnodev\ttmpfs\nnodev\tbd"..., 1024) = 421
read(3, "", 1024)                       = 0
close(3)                                = 0
access("/etc/selinux/config", F_OK)     = -1 ENOENT (No such file or directory)
openat(AT_FDCWD, "/usr/lib/locale/locale-archive", O_RDONLY|O_CLOEXEC) = 3
fstat(3, {st_mode=S_IFREG|0644, st_size=8378608, ...}) = 0
mmap(NULL, 8378608, PROT_READ, MAP_PRIVATE, 3, 0) = 0x7f785c66d000
close(3)                                = 0
ioctl(1, TCGETS, {B38400 opost isig icanon echo ...}) = 0
ioctl(1, TIOCGWINSZ, {ws_row=103, ws_col=118, ws_xpixel=0, ws_ypixel=0}) = 0
stat("/", {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
openat(AT_FDCWD, "/", O_RDONLY|O_NONBLOCK|O_CLOEXEC|O_DIRECTORY) = 3
fstat(3, {st_mode=S_IFDIR|0755, st_size=4096, ...}) = 0
getdents64(3, /* 26 entries */, 32768)  = 672
getdents64(3, /* 0 entries */, 32768)   = 0
close(3)                                = 0
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(0x88, 0x1), ...}) = 0
write(1, "bin   cdrom  etc   lib\t  lib64  "..., 77bin   cdrom  etc   lib	  lib64   lost+found  mnt  proc  run   snap  sys  usr
) = 77
write(1, "boot  dev    home  lib32  libx32"..., 78boot  dev    home  lib32  libx32  media       opt  root  sbin  srv   tmp  var
) = 78
close(1)                                = 0
close(2)                                = 0
exit_group(0)                           = ?
+++ exited with 0 +++
```

### 10.1.3. Strace and /proc on ETCD
#### /prod directory
* Information and connections to processes and kernel
* Study it to learn how processes work
* Configuration and administrative tasks
* Contains files that dont exist, yet you can access these

```sh
root@cks:/home/docker# cd /proc/
Display all 104 possibilities? (y or n)
1/                 2266/              acpi/              ioports            pressure/
1528/              2269/              asound/            irq/               schedstat
1529/              2270/              bootconfig         kallsyms           scsi/
1530/              2271/              buddyinfo          kcore              self/
1581/              229/               bus/               key-users          slabinfo
1595/              232/               cgroups            keys               softirqs
1596/              2351/              cmdline            kmsg               stat
1648/              2372/              consoles           kpagecgroup        swaps
1673/              2384/              cpuinfo            kpagecount         sys/
1695/              2412/              crypto             kpageflags         sysrq-trigger
1708/              2431/              devices            loadavg            sysvipc/
1714/              2456/              diskstats          locks              thread-self/
1734/              247/               dma                mdstat             timer_list
1747/              2478/              driver/            meminfo            tty/
1782/              2479/              dynamic_debug/     misc               uptime
1837/              2531/              execdomains        modules            version
1857/              2589/              fb                 mounts             version_signature
2045/              2665/              filesystems        mtrr               vmallocinfo
217/               2686/              fs/                net/               vmstat
2263/              722/               interrupts         pagetypeinfo       zoneinfo
2265/              894/               iomem              partitions  
```

#### strace and /proc: etcd
**strace Kubernetes etcd**
1. List syscalls
2. Find open files
3. Read secret value

```sh
$ strace
  -o filename
  -v verbose
  -f follow forks

  -cw (count and summarise)
  -p pid
  -P path
```

```sh
$ crictl ps | grep etcd
e770e9ea66d8f       a8a176a5d5d69       2 minutes ago        Running             etcd                      0                   5a978da4fa432

$ ps aux | grep etcd
root        1734  6.9  2.2 1112316 361796 ?      Ssl  07:53   0:10 kube-apiserver --advertise-address=192.168.58.2 --allow-privileged=true --authorization-mode=Node,RBAC --client-ca-file=/var/lib/minikube/certs/ca.crt --enable-admission-plugins=NamespaceLifecycle,LimitRanger,ServiceAccount,DefaultStorageClass,DefaultTolerationSeconds,NodeRestriction,MutatingAdmissionWebhook,ValidatingAdmissionWebhook,ResourceQuota --enable-bootstrap-token-auth=true --etcd-cafile=/var/lib/minikube/certs/etcd/ca.crt --etcd-certfile=/var/lib/minikube/certs/apiserver-etcd-client.crt --etcd-keyfile=/var/lib/minikube/certs/apiserver-etcd-client.key --etcd-servers=https://127.0.0.1:2379 --kubelet-client-certificate=/var/lib/minikube/certs/apiserver-kubelet-client.crt --kubelet-client-key=/var/lib/minikube/certs/apiserver-kubelet-client.key --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname --proxy-client-cert-file=/var/lib/minikube/certs/front-proxy-client.crt --proxy-client-key-file=/var/lib/minikube/certs/front-proxy-client.key --requestheader-allowed-names=front-proxy-client --requestheader-client-ca-file=/var/lib/minikube/certs/front-proxy-ca.crt --requestheader-extra-headers-prefix=X-Remote-Extra- --requestheader-group-headers=X-Remote-Group --requestheader-username-headers=X-Remote-User --secure-port=8443 --service-account-issuer=https://kubernetes.default.svc.cluster.local --service-account-key-file=/var/lib/minikube/certs/sa.pub --service-account-signing-key-file=/var/lib/minikube/certs/sa.key --service-cluster-ip-range=10.96.0.0/12 --tls-cert-file=/var/lib/minikube/certs/apiserver.crt --tls-private-key-file=/var/lib/minikube/certs/apiserver.key
root        1782  2.9  0.2 11215552 46036 ?      Ssl  07:53   0:04 etcd --advertise-client-urls=https://192.168.58.2:2379 --cert-file=/var/lib/minikube/certs/etcd/server.crt --client-cert-auth=true --data-dir=/var/lib/minikube/etcd --experimental-initial-corrupt-check=true --experimental-watch-progress-notify-interval=5s --initial-advertise-peer-urls=https://192.168.58.2:2380 --initial-cluster=cks=https://192.168.58.2:2380 --key-file=/var/lib/minikube/certs/etcd/server.key --listen-client-urls=https://127.0.0.1:2379,https://192.168.58.2:2379 --listen-metrics-urls=http://127.0.0.1:2381 --listen-peer-urls=https://192.168.58.2:2380 --name=cks --peer-cert-file=/var/lib/minikube/certs/etcd/peer.crt --peer-client-cert-auth=true --peer-key-file=/var/lib/minikube/certs/etcd/peer.key --peer-trusted-ca-file=/var/lib/minikube/certs/etcd/ca.crt --proxy-refresh-interval=70000 --snapshot-count=10000 --trusted-ca-file=/var/lib/minikube/certs/etcd/ca.crt
root        4290  0.0  0.0   3312   660 pts/1    S+   07:55   0:00 grep --color=auto etcd
```

```sh
strace -p 1782 -f -cw
strace: Process 1782 attached with 14 threads

^Cstrace: Process 1782 detached
strace: Process 1812 detached
strace: Process 1814 detached
strace: Process 1816 detached
strace: Process 1817 detached
strace: Process 1818 detached
strace: Process 1819 detached
strace: Process 1820 detached
strace: Process 1828 detached
strace: Process 1829 detached
strace: Process 1862 detached
strace: Process 1863 detached
strace: Process 1946 detached
strace: Process 1984 detached
% time     seconds  usecs/call     calls    errors syscall
------ ----------- ----------- --------- --------- ----------------
 87.25  250.512304       27300      9176      1470 futex
  9.95   28.581509        3404      8396        19 epoll_pwait
  2.71    7.769630        1497      5188         2 nanosleep
  0.05    0.144804        1930        75           fdatasync
  0.02    0.070545          56      1245         1 write
  0.01    0.021451          19      1098       544 read
  0.01    0.019043       19043         1         1 restart_syscall
  0.00    0.003629          31       115           pwrite64
  0.00    0.002347          34        69           tgkill
  0.00    0.001642          23        69           getpid
  0.00    0.001360          19        69        21 rt_sigreturn
  0.00    0.000404          14        27           lseek
  0.00    0.000381          20        19           setsockopt
  0.00    0.000341          18        18        10 epoll_ctl
  0.00    0.000340          42         8         4 accept4
  0.00    0.000266          29         9           close
  0.00    0.000259          51         5           openat
  0.00    0.000218          16        13           sched_yield
  0.00    0.000161          26         6           getdents64
  0.00    0.000073          14         5           getrandom
  0.00    0.000058          14         4           getsockname
  0.00    0.000030          14         2           fstat
------ ----------- ----------- --------- --------- ----------------
100.00  287.130796                 25617      2072 total
```

```sh
$ cd /proc/1782/fd

$ ls -la
total 0
dr-x------ 2 root root  0 Nov 23 07:53 .
dr-xr-xr-x 9 root root  0 Nov 23 07:53 ..
lrwx------ 1 root root 64 Nov 23 07:53 0 -> /dev/null
l-wx------ 1 root root 64 Nov 23 07:53 1 -> 'pipe:[186895]'
lrwx------ 1 root root 64 Nov 23 07:53 10 -> /var/lib/minikube/etcd/member/snap/db
...
```

```sh
$ tail -f 10 
$/registry/leases/kube-node-lease/cks
                                     � *�k8s

coordination.k8s.io/v1Lease�
�
cks�kube-node-lease"*$5c2a50bb-1355-4e67-80b2-a476b43050a02����j5
Node�cks"$fee7040f-a209-44dc-949c-bf1c57efcbfd*v1��
kubeletUpdate�coordination.k8s.io/v����FieldsV1:�
�{"f:metadata":{"f:ownerReferences":{".":{},"k:{\"uid\":\"fee7040f-a209-44dc-949c-bf1c57efcbfd\"}":{}}},"f:spec":{"f:holderIdentity":{},"f:leaseDurationSeconds":{},"f:renewTime":{}}}B
cks("
    �������5�"*1!��"&')��������� �!�"�#�$�%�&�'�(�)�*�+�,�-�.�/�0�1�2�3�4�5�6�7�8�9�:�;�<�=�>�?�@�j_n_p_r_~_�_,_3_7_9_;_@_E_J_P_U_Z___e_j_o_t_y_~_�_�_�_�_�_�_�_�_�_�_�_�_�_�_�_�_�_�_�_�_�_�_�_�__+  !*_j_,
� �D�	 	 CY\���g �alarmauth
:                                 authRevisionauthRolesauthUsersclusterclusterVersion3.5.0key+lease0
,�z@���ҷ���,�,�z@����ҷ���,�,�z@%�ʀҷ���,membersQb2c6679ac05f2cf1{"id":12882097698489969905,"peerURLs":["https://192.168.58.2:2380"],"name":"cks"}members_removedmetaP	4��confState{"voters":[12882097698489969905],"auto_leave":false}consistent_index�finishedCompactRev0_scheduledCompactRev0_term
```

```sh
$ kubectl create secret generic credit-card --from-literal cc=1111222233334444
```

```sh
$ cat 10 | grep 1111222233334444
Binary file (standard input) matches

$ cat 10 | strings | grep 111122223333444
111122223333444
```
### 10.1.4. /proc and env variables
**Create Apache pod with a secret as environment variable**
Read that secret from host filesystem

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: apache
  name: apache
spec:
  containers:
  - image: httpd
    name: apache
    resources: {}
    env:
    - name: SECRET
      value: "55667788"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl create -f pod.yaml
pod/apache created

$ kubectl exec apache -- env | grep -i secret
SECRET=55667788

$ ps aux | grep httpd
root        4684  0.0  0.0   6004  4676 ?        Ss   09:58   0:00 httpd -DFOREGROUND
www-data    4698  0.0  0.0 1997112 3576 ?        Sl   09:58   0:00 httpd -DFOREGROUND
www-data    4699  0.0  0.0 1997112 3576 ?        Sl   09:58   0:00 httpd -DFOREGROUND
www-data    4700  0.0  0.0 1997112 3576 ?        Sl   09:58   0:00 httpd -DFOREGROUND
docker      4982  0.0  0.0   3312   724 pts/1    S+   09:59   0:00 grep --color=auto httpd

$ cd /proc/4684/

$ cat environ 
KUBERNETES_SERVICE_PORT=443KUBERNETES_PORT=tcp://10.96.0.1:443HTTPD_VERSION=2.4.54HOSTNAME=apacheHOME=/rootHTTPD_PATCHES=KUBERNETES_PORT_443_TCP_ADDR=10.96.0.1PATH=/usr/local/apache2/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/binKUBERNETES_PORT_443_TCP_PORT=443KUBERNETES_PORT_443_TCP_PROTO=tcpHTTPD_SHA256=eb397feeefccaf254f8d45de3768d9d68e8e73851c49afd5b7176d1ecf80c340SECRET=55667788HTTPD_PREFIX=/usr/local/apache2KUBERNETES_SERVICE_PORT_HTTPS=443KUBERNETES_PORT_443_TCP=tcp://10.96.0.1:443KUBERNETES_SERVICE_HOST=10.96.0.1PWD=/usr/local/apache2
```

**Secret as environment variables can be read from anyone who can access /proc on the host**

### 10.1.5. Falco and Installation
#### Falco
* Cloud-Native runtime security (CNCF)
* **ACCESS**
  * Deep kernel tracing built on the Linux Kernel
* **ASSERT**
  * Describe security rules against a system (+default ones)
  * Detect unwanted behaviour
* **ACTION**
  * Automated respond to a security violations

**Install Falco on worker node**
```sh

$ curl -s https://falco.org/repo/falcosecurity-3672BA8F.asc | apt-key add -
$ echo "deb https://download.falco.org/packages/deb stable main" | tee -a /etc/apt/sources.list.d/falcosecurity.list
$ apt-get update -y
$ apt-get -y install linux-headers-$(uname -r)
$ apt-get install -y falco=0.26.1

$ ls -l /etc/falco
total 184
drwxr-xr-x 4 root root   4096 Dec 16 08:28 ./
drwxr-xr-x 1 root root   4096 Dec 16 08:29 ../
-rw-r--r-- 1 root root   7834 Oct  1  2020 falco.yaml
-rw-r--r-- 1 root root   1136 Oct  1  2020 falco_rules.local.yaml
-rw-r--r-- 1 root root 126820 Oct  1  2020 falco_rules.yaml
-rw-r--r-- 1 root root  25941 Oct  1  2020 k8s_audit_rules.yaml
drwxr-xr-x 2 root root   4096 Dec 16 08:28 rules.available/
drwxr-xr-x 2 root root   4096 Oct  1  2020 rules.d/
```
> https://v1-16.docs.kubernetes.io/docs/tasks/debug-application-cluster/falco


### 10.1.7. Investigate Falco rules
**Look at some existing Falco rules**
```sh
$ cat /etc/falco/falco_rules.yaml
...
- rule: Terminal shell in container
  desc: A shell was used as the entrypoint/exec point into a container with an attached terminal.
  condition: >
    spawned_process and container
    and shell_procs and proc.tty != 0
    and container_entrypoint
    and not user_expected_terminal_shell_in_container_conditions
  output: >
    A shell was spawned in a container with an attached terminal (user=%user.name user_loginuid=%user.loginuid %container.info
    shell=%proc.name parent=%proc.pname cmdline=%proc.cmdline terminal=%proc.tty container_id=%container.id image=%container.image.repository)
  priority: NOTICE
  tags: [container, shell, mitre_execution]
...
```

```sh
$ cat /etc/falco/k8s_audit_rules.yaml
...
- rule: K8s Secret Created
  desc: Detect any attempt to create a secret. Service account tokens are excluded.
  condition: (kactivity and kcreate and secret and ka.target.namespace!=kube-system and non_system_user and response_successful)
  output: K8s Secret Created (user=%ka.user.name secret=%ka.target.name ns=%ka.target.namespace resp=%ka.response.code decision=%ka.auth.decision reason=%ka.auth.reason)
  priority: INFO
  source: k8s_audit
  tags: [k8s]
...
```

### 10.1.8. Change Falco rule
**Change Falco rule to get custom output format**
Rule:             "A shell was spawned in a container with an attached terminal"
Output Format:    TIME,USER-NAME,CONTAINER-NAME,CONTAINER-ID
Priority:         WARNING

Create a Pod
```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: apache
  name: apache
spec:
  containers:
  - image: httpd
    name: apache
    resources: {}
    env:
    - name: SECRET
      value: "55667788"
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ falco

09:29:25.296597224: Notice A shell was spawned in a container with an attached terminal (user=root user_loginuid=-1 container=7d4a76ce9019 shell=sh parent=runc cmdline=sh terminal=34816 container_id=7d4a76ce9019 image=httpd)
```

>https://falco.org/docs/rules/supported-fields

### 10.1.9. Recap

> https://www.youtube.com/watch?v=8g-NUUmCeGI
> https://www.youtube.com/watch?v=8N8IpToYOGM


## 10.2. Inmutability of containers at runtime
### 10.2.1. Introduction
#### Inmutability
![cks](images/26_immutability_intro.png)

### 10.2.2. Ways to enforce immutability
#### Enforce on Container Image Level
![cks](images/26_immutability_enforce.png)

#### Make manual changes to container - Command ?
![cks](images/26_immutability_enforce_02.png)

#### Make manual changes to container - StartupProbe ?
![cks](images/26_immutability_enforce_03.png)

#### Enforce Read-Only Root Filesystem
Enforce Read-Only root filesystem using **SecurityContexts** and **PodSecurityPolicies**

#### Move logic to InitContainer ?
![cks](images/26_immutability_enforce_04.png)

### 10.2.3. StartupProbe changes container
#### StartupProbe for Immutability
**Use StartupProbe to remove** `touch` and `bash` **from container**

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: immutable
  name: immutable
spec:
  containers:
  - image: httpd
    name: immutable
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl exec -it immutable -- bash

$ root@immutable:/usr/local/apache2# touch test
$ root@immutable:/usr/local/apache2# ls test
test
$ root@immutable:/usr/local/apache2# ls
bin  build  cgi-bin  conf  error  htdocs  icons  include  logs	modules  test
$ root@immutable:/usr/local/apache2# exit
exit
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: immutable
  name: immutable
spec:
  containers:
  - image: httpd
    name: immutable
    resources: {}
    startupProbe:
      exec:
        command:
          - rm
          - /bin/touch
      initialDelaySeconds: 1
      periodSeconds: 5
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl exec -it immutable -- bash

$ root@immutable:/usr/local/apache2# touch test
bash: touch: command not found

$ root@immutable:/usr/local/apache2# exit
exit
command terminated with exit code 127
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: immutable
  name: immutable
spec:
  containers:
  - image: httpd
    name: immutable
    resources: {}
    securityContext:
      readOnlyRootFilesystem: true
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl exec -it immutable -- bash
OCI runtime exec failed: exec failed: unable to start container process: exec: "bash": executable file not found in $PATH: unknown
command terminated with exit code 126
```

### 10.2.4. SecurityContext renders container immutable
#### Enforce RO-filesystem
**Create Pod SecurityContext to make filesystem Read-Only**
Ensure some directories are still writeable using emptyDir volume

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: immutable
  name: immutable
spec:
  containers:
  - image: httpd
    name: immutable
    resources: {}
    securityContext:
      readOnlyRootFilesystem: true
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl get po
NAME        READY   STATUS             RESTARTS        AGE
immutable   0/1     CrashLoopBackOff   1 (16s ago)     20s

$ kubectl logs immutable 
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.5. Set the 'ServerName' directive globally to suppress this message
AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 172.17.0.5. Set the 'ServerName' directive globally to suppress this message
[Fri Dec 16 09:50:48.540815 2022] [core:error] [pid 1:tid 139802489503040] (30)Read-only file system: AH00099: could not create /usr/local/apache2/logs/httpd.pid.frGzMi
[Fri Dec 16 09:50:48.540855 2022] [core:error] [pid 1:tid 139802489503040] AH00100: httpd: could not log pid to file /usr/local/apache2/logs/httpd.pid
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: immutable
  name: immutable
spec:
  containers:
  - image: httpd
    name: immutable
    resources: {}
    securityContext:
      readOnlyRootFilesystem: true
    volumeMounts:
    - name: cache-volume
      mountPath: /usr/local/apache2/logs
  volumes:
  - name: cache-volume
    emptyDir: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl get po
NAME        READY   STATUS    RESTARTS   AGE
immutable   1/1     Running   0          14s

$ kubectl exec -it immutable -- bash

$ root@immutable:/usr/local/apache2# touch test
touch: cannot touch 'test': Read-only file system

$ root@immutable:/usr/local/apache2# cd /usr/local/apache2/logs/
$ root@immutable:/usr/local/apache2/logs# ls
httpd.pid
$ root@immutable:/usr/local/apache2/logs# touch test
$ root@immutable:/usr/local/apache2/logs# ls
httpd.pid  test
```

```sh
docker run --read-only --tmpfs /run my-container
```

### 10.2.5. Recap
With RBAC it should be ensured that only certain people can even edit pod specs

## 10.3. Auditing
### 10.3.1. Introduction
#### Audit Logs - Introduction
![cks](images/27_auditing_intro.png)

* Did someone access an important secret while it was not protected?
* When was the last time that user X did access cluster Y?
* Does my CRD work properly?

![cks](images/27_auditing_intro_02.png)

#### API Request Stages
Each request can be recorded with an associated "stage". The known stages are:
* `RequestReceived` - The stage for events generated as soon as the audit handler receives the request, and before it is delegated down the handler chain.
* `ResponseStarted` - Once the response headers are sent, but before the response body is sent. This stage is only generated for long-running requests (e.g. watch).
* `ResponseComplete` - The response body has been completed and no more bytes will be sent.
* `Panic` - Events generated when a panic ocurred.

#### Audit Policy - Waht data to store?
![cks](images/27_auditing_intro_03.png)

**Wath events should be recorded and wath data should these contain?**

* `None` - dont log events that match this rule.
* `Metadata` - log request metadata (requesting user, timestamp, resource, verb, etc) but not request or response body
* `Request` - log event metadata and request body but not response body. This does not apply for non-resource requests.
* `RequestResponse` - log event metadata, request and response bodies. This does not apply for non-resources requests.

```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
omitStages:
  - "RequestReceived"
rules:
  # Log no "read" actions
  - level: None
    verbs: ["get", "watch", "list"]
  
  # log nothing regarding events
  - level: None
    resources:
    - group: "" # core
      resources: ["events"]
  
  # log nothing coming from some groups
  - level: None
    userGroups: ["system:nodes"]
  
  - level: RequestResponse
    resources:
    - group: ""
      resources: ["secrets"]
  
  # for everything else log
  - level: Metadata
```

#### Audit Backends - Where to store all that data?
![cks](images/27_auditing_intro_04.png)

#### Audit Logs - Overview
![cks](images/27_auditing_intro_05.png)

### 10.3.2. Enable Auditing Logging in Apiserver
#### Setup Audit Logs
**Configure apiserver to store Audit Logs in JSON format**
```sh
$ root@cks:/ cd /etc/kubernetes/audit
$ root@cks:/etc/kubernetes/audit# vim policy.yaml
```

```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
rules:
- level: Metadata
```

`Add lines into apiserver yaml definition`
```yaml
command:
...
- --audit-policy-file=/etc/kubernetes/audit/policy.yaml
- --audit-log-path=/etc/kubernetes/audit/logs/audit.log
- --audit-log-maxsize=500
- --audit-log-maxbackup=5
...

volumeMounts:
...
- mountPath: /etc/kubernetes/audit
  name: audit
...
volumes:
...
- hostPath:
    path: /etc/kubernetes/audit
    type: DirectoryOrCreate
  name: audit
...
```

```sh
$ tail -f audit.log

{"kind":"Event","apiVersion":"audit.k8s.io/v1","level":"Metadata","auditID":"158438ad-859c-43c7-8646-f33eb9dadbb1","stage":"ResponseComplete","requestURI":"/api/v1/namespaces/default/configmaps?fieldManager=kubectl-client-side-apply","verb":"create","user":{"username":"minikube-user","groups":["system:masters","system:authenticated"]},"sourceIPs":["192.168.59.1"],"userAgent":"kubectl/v1.23.5 (linux/amd64) kubernetes/c285e78","objectRef":{"resource":"configmaps","namespace":"default","name":"my-config","uid":"b4952dc3-d670-11e5-8cd0-68f728db1985","apiVersion":"v1"},"responseStatus":{"metadata":{},"code":201},"requestObject":{"kind":"ConfigMap","apiVersion":"v1","metadata":{"name":"my-config","namespace":"default","selfLink":"/api/v1/namespaces/default/configmaps/my-config","uid":"b4952dc3-d670-11e5-8cd0-68f728db1985","creationTimestamp":"2016-02-18T18:52:05Z","annotations":{"kubectl.kubernetes.io/last-applied-configuration":"{\"apiVersion\":\"v1\",\"data\":{\"access.properties\":\"aws_access_key_id = MY-ID\\naws_secret_access_key = MY-KEY\\n\",\"ui.properties\":\"color.good=purple\\ncolor.bad=yellow\\nallow.textmode=true\\n\"},\"kind\":\"ConfigMap\",\"metadata\":{\"annotations\":{},\"creationTimestamp\":\"2016-02-18T18:52:05Z\",\"name\":\"my-config\",\"namespace\":\"default\",\"resourceVersion\":\"516\",\"selfLink\":\"/api/v1/namespaces/default/configmaps/my-config\",\"uid\":\"b4952dc3-d670-11e5-8cd0-68f728db1985\"}}\n"}},"data":{"access.properties":"aws_access_key_id = MY-ID\naws_secret_access_key = MY-KEY\n","ui.properties":"color.good=purple\ncolor.bad=yellow\nallow.textmode=true\n"}},"responseObject":{"kind":"ConfigMap","apiVersion":"v1","metadata":{"name":"my-config","namespace":"default","uid":"8865fdb5-541d-4bc2-b6f7-dd1e6ff7db6b","resourceVersion":"4177","creationTimestamp":"2022-07-05T13:10:19Z","annotations":{"kubectl.kubernetes.io/last-applied-configuration":"{\"apiVersion\":\"v1\",\"data\":{\"access.properties\":\"aws_access_key_id = MY-ID\\naws_secret_access_key = MY-KEY\\n\",\"ui.properties\":\"color.good=purple\\ncolor.bad=yellow\\nallow.textmode=true\\n\"},\"kind\":\"ConfigMap\",\"metadata\":{\"annotations\":{},\"creationTimestamp\":\"2016-02-18T18:52:05Z\",\"name\":\"my-config\",\"namespace\":\"default\",\"resourceVersion\":\"516\",\"selfLink\":\"/api/v1/namespaces/default/configmaps/my-config\",\"uid\":\"b4952dc3-d670-11e5-8cd0-68f728db1985\"}}\n"},"managedFields":[{"manager":"kubectl-client-side-apply","operation":"Update","apiVersion":"v1","time":"2022-07-05T13:10:19Z","fieldsType":"FieldsV1","fieldsV1":{"f:data":{".":{},"f:access.properties":{},"f:ui.properties":{}},"f:metadata":{"f:annotations":{".":{},"f:kubectl.kubernetes.io/last-applied-configuration":{}}}}}]},"data":{"access.properties":"aws_access_key_id = MY-ID\naws_secret_access_key = MY-KEY\n","ui.properties":"color.good=purple\ncolor.bad=yellow\nallow.textmode=true\n"}},"requestReceivedTimestamp":"2022-07-05T13:10:19.960188Z","stageTimestamp":"2022-07-05T13:10:19.964977Z","annotations":{"authorization.k8s.io/decision":"allow","authorization.k8s.io/reason":""}}
```

### 10.3.3. Create Secret and check Audit Logs
**Create a secret and investigate the JSON audit log**
```sh
$ kubectl create secret generic very-secure --from-literal=user=admin
```

![cks](images/27_auditing_secret.png)

### 10.3.4. Create advanced Audit Policy
**We want to restrict logged data with an Audit Policy**
* Nothing from stage RequestReceived
* Nothing from "get", "watch", "list"
* From Secrets only metadata level
* Everything elese RequestResponse level

1. Change policy file
2. Disable audit logging in apiserver, wait till restart
3. Enable audit logging in apiserver, wait till restart
   1. If apiserver doesn't start, then check: `/var/log/pods/kube-system_kube-apiserver*`
4. Test your changes

```yaml
apiVersion: audit.k8s.io/v1
kind: Policy
omitStages:
  - "RequestReceived"

rules:
- level: None
  verbs: ["get", "watch", "list"]

- level: RequestResponse
  resources:
  - group: ""
    resources: ["secrets"]

- level: RequestResponse
```

![cks](images/27_auditing_advanced.png)

### 10.3.5. Recap
> https://www.youtube.com/watch?v=HXtLTxo30SY


# 11. System Hardening
## 11.1. Kernel Hardening Tools
### 11.1.1. Introduction
#### Linux Kernel Isolation
![cks](images/28_hardening_intro.png)

#### Kernel vs User Space
![cks](images/28_hardening_intro_02.png)
#### Overview
![cks](images/28_hardening_intro_03.png)

### 11.1.2. AppArmor
#### AppArmor
![cks](images/28_hardening_apparmor.png)
![cks](images/28_hardening_apparmor_02.png)

#### Main Commands
```sh
# Show all profiles
aa-status

## generate a new profile (smart wrapper around aa-logprof)
aa-genprof

## put profile in complain mode
aa-complain

## put profile in enforce mode
aa-enforce

## update the profile if app produced some more usage logs (syslog)
aa-logprof
```

### 11.1.3. AppArmor for curl
**Setup simple AppArmor profile for curl**
```sh
$ curl killer.sh -v
*   Trying 35.227.196.29:80...
* TCP_NODELAY set
* Connected to killer.sh (35.227.196.29) port 80 (#0)
> GET / HTTP/1.1
> Host: killer.sh
> User-Agent: curl/7.68.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 301 Moved Permanently
< Cache-Control: private
< Location: https://killer.sh:443/
< Content-Length: 0
< Date: Fri, 16 Dec 2022 10:47:39 GMT
< Content-Type: text/html; charset=UTF-8
< 
* Connection #0 to host killer.sh left intact

$ aa-status 
apparmor module is loaded.
60 profiles are loaded.
54 profiles are in enforce mode.
   /snap/core/14399/usr/lib/snapd/snap-confine
   /snap/core/14399/usr/lib/snapd/snap-confine//mount-namespace-capture-helper
   /snap/snapd/17883/usr/lib/snapd/snap-confine
   /snap/snapd/17883/usr/lib/snapd/snap-confine//mount-namespace-capture-helper
   /usr/bin/evince
   /usr/bin/evince-previewer
   /usr/bin/evince-previewer//sanitized_helper
   /usr/bin/evince-thumbnailer
   /usr/bin/evince//sanitized_helper
   /usr/bin/man
   /usr/lib/NetworkManager/nm-dhcp-client.action
   /usr/lib/NetworkManager/nm-dhcp-helper
   /usr/lib/connman/scripts/dhclient-script
   /usr/lib/cups/backend/cups-pdf
   /usr/lib/snapd/snap-confine
   /usr/lib/snapd/snap-confine//mount-namespace-capture-helper
   /usr/sbin/cups-browsed
   /usr/sbin/cupsd
   /usr/sbin/cupsd//third_party
   /usr/sbin/tcpdump
   /{,usr/}sbin/dhclient
   docker-default
   ippusbxd
   libreoffice-senddoc
   libreoffice-soffice//gpg
   libreoffice-xpdfimport
   lsb_release
   man_filter
   man_groff
   nvidia_modprobe
   nvidia_modprobe//kmod
   snap-update-ns.code
   snap-update-ns.core
   snap-update-ns.keepassxc
   snap-update-ns.kontena-lens
   snap-update-ns.snap-store
   snap-update-ns.spotify
   snap-update-ns.sublime-text
   snap-update-ns.telegram-desktop
   snap-update-ns.yq
   snap.core.hook.configure
   snap.keepassxc.cli
   snap.keepassxc.hook.configure
   snap.keepassxc.keepassxc
   snap.keepassxc.proxy
   snap.snap-store.hook.configure
   snap.snap-store.snap-store
   snap.snap-store.ubuntu-software
   snap.snap-store.ubuntu-software-local-file
   snap.spotify.hook.configure
   snap.spotify.spotify
   snap.telegram-desktop.hook.configure
   snap.telegram-desktop.telegram-desktop
   snap.yq.yq
6 profiles are in complain mode.
   libreoffice-oopslash
   libreoffice-soffice
   snap.code.code
   snap.code.url-handler
   snap.kontena-lens.kontena-lens
   snap.sublime-text.subl
25 processes have profiles defined.
4 processes are in enforce mode.
   /usr/sbin/cups-browsed (1978) 
   /usr/sbin/cupsd (2017) 
   /usr/lib/cups/notifier/dbus (2027) /usr/sbin/cupsd
   /snap/snap-store/638/usr/bin/snap-store (4086) snap.snap-store.ubuntu-software
21 processes are in complain mode.
   /snap/code/115/usr/share/code/code (7938) snap.code.code
   /snap/code/115/usr/share/code/code (7940) snap.code.code
   /snap/code/115/usr/share/code/code (7941) snap.code.code
   /snap/code/115/usr/share/code/code (7986) snap.code.code
   /snap/code/115/usr/share/code/code (8028) snap.code.code
   /snap/code/115/usr/share/code/code (8135) snap.code.code
   /snap/code/115/usr/share/code/code (8150) snap.code.code
   /snap/code/115/usr/share/code/code (8177) snap.code.code
   /usr/bin/bash (8257) snap.code.code
   /snap/code/115/usr/share/code/code (22976) snap.code.code
   /snap/code/115/usr/share/code/code (23021) snap.code.code
   /snap/code/115/usr/share/code/code (23086) snap.code.code
   /home/adrianmartin/.vscode/extensions/hashicorp.terraform-2.25.2-linux-x64/bin/terraform-ls (23121) snap.code.code
   /snap/code/115/usr/share/code/code (23141) snap.code.code
   /snap/code/115/usr/share/code/code (23155) snap.code.code
   /snap/code/115/usr/share/code/code (25573) snap.code.code
   /usr/bin/sudo (170532) snap.code.code
   /usr/bin/python3.8 (170558) snap.code.code
   /snap/sublime-text/116/opt/sublime_text/sublime_text (10584) snap.sublime-text.subl
   /snap/sublime-text/116/opt/sublime_text/plugin_host-3.3 (10605) snap.sublime-text.subl
   /snap/sublime-text/116/opt/sublime_text/plugin_host-3.8 (10608) snap.sublime-text.subl
0 processes are unconfined but have a profile defined.
```

```sh
$ apt-get install apparmor-utils
```
### 11.1.4. AppArmor for Docker Nginx
`/etc/apparmor.d/docker-nginx`
```sh
#include <tunables/global>

profile docker-nginx flags=(attach_disconnected,mediate_deleted) {
  #include <abstractions/base>

  network inet tcp,
  network inet udp,
  network inet icmp,

  deny network raw,

  deny network packet,

  file,
  umount,

  deny /bin/** wl,
  deny /boot/** wl,
  deny /dev/** wl,
  deny /etc/** wl,
  deny /home/** wl,
  deny /lib/** wl,
  deny /lib64/** wl,
  deny /media/** wl,
  deny /mnt/** wl,
  deny /opt/** wl,
  deny /proc/** wl,
  deny /root/** wl,
  deny /sbin/** wl,
  deny /srv/** wl,
  deny /tmp/** wl,
  deny /sys/** wl,
  deny /usr/** wl,

  audit /** w,

  /var/run/nginx.pid w,

  /usr/sbin/nginx ix,

  deny /bin/dash mrwklx,
  deny /bin/sh mrwklx,
  deny /usr/bin/top mrwklx,


  capability chown,
  capability dac_override,
  capability setuid,
  capability setgid,
  capability net_bind_service,

  deny @{PROC}/* w,   # deny write for all files directly in /proc (not in a subdir)
  # deny write to files not in /proc/<number>/** or /proc/sys/**
  deny @{PROC}/{[^1-9],[^1-9][^0-9],[^1-9s][^0-9y][^0-9s],[^1-9][^0-9][^0-9][^0-9]*}/** w,
  deny @{PROC}/sys/[^k]** w,  # deny /proc/sys except /proc/sys/k* (effectively /proc/sys/kernel)
  deny @{PROC}/sys/kernel/{?,??,[^s][^h][^m]**} w,  # deny everything except shm* in /proc/sys/kernel/
  deny @{PROC}/sysrq-trigger rwklx,
  deny @{PROC}/mem rwklx,
  deny @{PROC}/kmem rwklx,
  deny @{PROC}/kcore rwklx,

  deny mount,

  deny /sys/[^f]*/** wklx,
  deny /sys/f[^s]*/** wklx,
  deny /sys/fs/[^c]*/** wklx,
  deny /sys/fs/c[^g]*/** wklx,
  deny /sys/fs/cg[^r]*/** wklx,
  deny /sys/firmware/** rwklx,
  deny /sys/kernel/security/** rwklx,
}
```

```sh
$ docker run --security-apt apparmor=docker-nginx nginx

$ docker exec -it nginx sh
# touch /root/test
touch: cannot touch '/root/test': Permission denied
# touch /test
# sh
sh: 3: sh: Permission denied
# exit
```

> https://kubernetes.io/docs/tutorials/clusters/apparmor/#example

### 11.1.5. AppArmor for Kubernetes Nginx
* Container runtime needs to support AppArmor
* AppArmor needs to be installed on every node
* AppArmor profiles need to be available on every node
* AppArmor profiles are **specified per container**
  * done using annotations

![cks](images/28_hardening_apparmor_kubernetes.png)

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  annotations:
    container.apparmor.security.beta.kubernetes.io/secure: localhost/hello
  labels:
    run: secure
  name: secure
spec:
  containers:
  - image: nginx
    name: secure
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl get po
NAME     READY   STATUS    RESTARTS   AGE
secure   0/1     Blocked   0          8s

$ kubectl describe pod 
Name:             secure
Namespace:        default
Priority:         0
Service Account:  default
Node:             cks/192.168.58.2
Start Time:       Fri, 16 Dec 2022 11:59:25 +0100
Labels:           run=secure
Annotations:      container.apparmor.security.beta.kubernetes.io/secure: localhost/hello
Status:           Pending
Reason:           AppArmor
Message:          Cannot enforce AppArmor: AppArmor is not enabled on the host
...
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  annotations:
    container.apparmor.security.beta.kubernetes.io/secure: localhost/docker-nginx
  labels:
    run: secure
  name: secure
spec:
  containers:
  - image: nginx
    name: secure
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

```sh
$ kubectl get po
NAME     READY   STATUS    RESTARTS   AGE
secure   1/1     Running   0          8s

$ kubectl exec -it secure -- bash

$ root@secure:/# sh
bash: /bin/sh: Permission denied
```

### 11.1.6. Seccomp
* "secure computing mode"
* Security facility in the Linux Kernel
* Restricts execution of syscalls

![cks](images/28_hardening_seccomp.png)
![cks](images/28_hardening_seccomp_02.png)

### 11.1.7. Seccomp for Docker Nginx
### 11.1.8. Seccomp for Kubernetes Nginx
**Create a Nginx Pod in Kubernetes and assign a seccomp profile to it**

```yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: secure
  name: secure
spec:
  securityContext:
    seccompProfile:
      type: Localhost
      localhostProfile: profiles/audit.json
  containers:
  - image: nginx
    name: secure
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```

### 11.1.9. Recap
> https://www.youtube.com/watch?v=8g-NUUmCeGI

> https://www.youtube.com/watch?v=JFjXvIwAeVI


## 11.2. Reduce Attack Surface
### 11.2.1. Introduction
#### Overview
![cks](images/29_reduce_attack_surface_intro.png)

#### Nodes that run Kubernetes
* Only purpose: run Kubernetes components
  * Remove unnecesary services
* Node Recycling
  * Nodes should be ephemeral
  * Created from images
  * Can be recycled any time (and fas if necessary)

#### Linux Distributions
* Often include number of services
* Meant to help, but widen attack surface
* The more existing and running services, the more environment

#### Open Ports
`netstat` (Red Hat: ssh command)
![cks](images/29_reduce_attack_surface_intro_02.png)

#### Port used by which application?
`netstat` or `lsfot`
![cks](images/29_reduce_attack_surface_intro_03.png)

#### Running Services
`systemctl`
![cks](images/29_reduce_attack_surface_intro_04.png)

#### Processes and Users
`ps`
![cks](images/29_reduce_attack_surface_intro_05.png)

### 11.2.2. Systemctl and Services
**Disable Service Snapd via systemctl**
```
$ sudo systemctl status snapd
● snapd.service - Snap Daemon
     Loaded: loaded (/lib/systemd/system/snapd.service; enabled; vendor preset: enabled)
     Active: active (running) since Fri 2022-12-16 08:39:25 CET; 4h 6min ago
TriggeredBy: ● snapd.socket
   Main PID: 1820 (snapd)
      Tasks: 22 (limit: 18750)
     Memory: 210.3M
     CGroup: /system.slice/snapd.service
             └─1820 /usr/lib/snapd/snapdf

$ sudo systemctl stop snapd
Warning: Stopping snapd.service, but it can still be activated by: 
  snapd.socket

$ systemctl list-units | grep snapd
  run-snapd-ns-snap\x2dstore.mnt.mount                                                                             loaded active     mounted   /run/snapd/ns/snap-store.mnt                                                                    
  run-snapd-ns-telegram\x2ddesktop.mnt.mount                                                                       loaded active     mounted   /run/snapd/ns/telegram-desktop.mnt                                                              
  run-snapd-ns.mount                                                                                               loaded active     mounted   /run/snapd/ns                                                                                   
  snap-snapd-17883.mount                                                                                           loaded active     mounted   Mount unit for snapd, revision 17883                                                            
  snapd.apparmor.service                                                                                           loaded active     exited    Load AppArmor profiles managed internally by snapd                                              
  snapd.seeded.service                                                                                             loaded active     exited    Wait until snapd is fully seeded                                                                
  snapd.service                                                                                                    loaded active     running   Snap Daemon                                                                                     
  snapd.socket                                                                                                     loaded active     running   Socket activation for snappy daemon

$ systemctl list-units --type=service | grep snapd
  snapd.apparmor.service                                                                    loaded active exited  Load AppArmor profiles managed internally by snapd                           
  snapd.seeded.service                                                                      loaded active exited  Wait until snapd is fully seeded                                             
  snapd.service                                                                             loaded active running Snap Daemon 

$ systemctl list-units --type=service --state=running | grep snapd

$ systemctl start snapd

$ systemctl list-units --type=service --state=running | grep snapd
snapd.service             loaded active running Snap Daemon

$ systemctl disable snapd
Removed /etc/systemd/system/multi-user.target.wants/snapd.service.
```

### 11.2.3. Install and investigate Services
```sh
$ ps aux | grep postgres
postgres    2036  0.0  0.1 223612 30044 ?        Ss   08:39   0:00 /usr/lib/postgresql/15/bin/postgres -D /var/lib/postgresql/15/main -c config_file=/etc/postgresql/15/main/postgresql.conf
postgres    2049  0.0  0.0 223772  7912 ?        Ss   08:39   0:00 postgres: 15/main: checkpointer 
postgres    2050  0.0  0.0 223756  5968 ?        Ss   08:39   0:00 postgres: 15/main: background writer 
postgres    2053  0.0  0.0 223756 10432 ?        Ss   08:39   0:00 postgres: 15/main: walwriter 
postgres    2054  0.0  0.0 225188  8564 ?        Ss   08:39   0:00 postgres: 15/main: autovacuum launcher 
postgres    2055  0.0  0.0 225220  6684 ?        Ss   08:39   0:00 postgres: 15/main: logical replication launcher 
adrianm+  245841  0.0  0.0  11664  2780 pts/2    S+   13:03   0:00 grep --color=auto postgres


$ sudo netstat -plnt
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:5939          0.0.0.0:*               LISTEN      2742/teamviewerd    
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      1693/systemd-resolv 
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      2017/cupsd          
tcp        0      0 127.0.1.1:5432          0.0.0.0:*               LISTEN      2036/postgres       
tcp        0      0 127.0.0.1:5432          0.0.0.0:*               LISTEN      2036/postgres       
tcp        0      0 127.0.0.1:39003         0.0.0.0:*               LISTEN      1811/confighandler  
tcp        0      0 127.0.0.1:49153         0.0.0.0:*               LISTEN      26007/docker-proxy  
tcp        0      0 127.0.0.1:49154         0.0.0.0:*               LISTEN      26021/docker-proxy  
tcp        0      0 127.0.0.1:49155         0.0.0.0:*               LISTEN      26033/docker-proxy  
tcp        0      0 127.0.0.1:49156         0.0.0.0:*               LISTEN      26046/docker-proxy  
tcp        0      0 127.0.0.1:49157         0.0.0.0:*               LISTEN      26058/docker-proxy  
tcp        0      0 127.0.0.1:39245         0.0.0.0:*               LISTEN      22976/code
```

### 11.2.4. Disabled application listening on port
### 11.2.5. Investigate Linux Users
```sh
$ root@cks:~# whoami
root

$ root@cks:~# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
_rpc:x:101:65534::/run/rpcbind:/usr/sbin/nologin
statd:x:102:65534::/var/lib/nfs:/usr/sbin/nologin
systemd-timesync:x:103:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
systemd-network:x:104:106:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:105:107:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
messagebus:x:108:110::/nonexistent:/usr/sbin/nologin
docker:x:1000:999:,,,:/home/docker:/bin/bash
systemd-coredump:x:998:998:systemd Core Dumper:/:/usr/sbin/nologin

$ root@cks:~# su docker

$ docker@cks:/root$ whoami
docker

$ docker@cks:/root$ sudo -i

root@cks:~# ps aux | grep bash
docker    176876  0.0  0.0   4248  3508 pts/1    Ss   12:04   0:00 -bash
root      176906  0.0  0.0   4248  3496 pts/1    S    12:04   0:00 bash
docker    177188  0.0  0.0   4248  3556 pts/1    S    12:04   0:00 bash
root      177335  0.0  0.0   4248  3544 pts/1    S    12:04   0:00 -bash
root      177363  0.0  0.0   3312   712 pts/1    S+   12:05   0:00 grep --color=auto bash

$ root@cks:~# exit
logout

$ docker@cks:/root$ ps aux | grep bash
docker    176876  0.0  0.0   4248  3508 pts/1    Ss   12:04   0:00 -bash
root      176906  0.0  0.0   4248  3496 pts/1    S    12:04   0:00 bash
docker    177188  0.0  0.0   4248  3556 pts/1    S    12:04   0:00 bash
docker    177651  0.0  0.0   3312   648 pts/1    S+   12:05   0:00 grep --color=auto bash

$ root@cks:~# adduser test
Adding user `test' ...
Adding new group `test' (1000) ...
Adding new user `test' (1001) with group `test' ...
Creating home directory `/home/test' ...
Copying files from `/etc/skel' ...
New password: 
Retype new password: 
No password supplied
New password: 
Retype new password: 
No password supplied
New password: 
Retype new password: 
No password supplied
passwd: Authentication token manipulation error
passwd: password unchanged
Try again? [y/N] N
Changing the user information for test
Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []: 
	Work Phone []: 
	Home Phone []: 
	Other []: 
Is the information correct? [Y/n] Y

$ root@cks:~# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
_rpc:x:101:65534::/run/rpcbind:/usr/sbin/nologin
statd:x:102:65534::/var/lib/nfs:/usr/sbin/nologin
systemd-timesync:x:103:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
systemd-network:x:104:106:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:105:107:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
messagebus:x:108:110::/nonexistent:/usr/sbin/nologin
docker:x:1000:999:,,,:/home/docker:/bin/bash
systemd-coredump:x:998:998:systemd Core Dumper:/:/usr/sbin/nologin
test:x:1001:1000:,,,:/home/test:/bin/bash

$ root@cks:~# su test

$ test@cks:/root$ whoami
test
```
### 11.2.6. Recap



# 12. Linux Foundation Simulator Sessions