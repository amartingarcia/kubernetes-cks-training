# training_k8s_cks

# Table of Contents
- [training_k8s_cks](#training_k8s_cks)
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
    - [7.3.2. Set container User and Group](#732-set-container-user-and-group)
    - [7.3.3. Force container non-root](#733-force-container-non-root)
    - [7.3.4. Privileged Containers](#734-privileged-containers)
    - [7.3.5. Created Privileged Containers](#735-created-privileged-containers)
    - [7.3.6. PrivilegeScalation](#736-privilegescalation)
    - [7.3.7. Disable PrivilegeScalation](#737-disable-privilegescalation)
    - [7.3.8. PodSecurityPolicies](#738-podsecuritypolicies)
    - [7.3.9. Create and enable PodSecurityPolicies](#739-create-and-enable-podsecuritypolicies)
    - [7.3.10. Recap](#7310-recap)
  - [7.4. mTLS](#74-mtls)
    - [7.4.1. Intro](#741-intro)
    - [7.4.2. Create sidecar proxy](#742-create-sidecar-proxy)
    - [7.4.3. Recap](#743-recap)
- [8. Open Policy Agent (OPA)](#8-open-policy-agent-opa)
  - [8.1. Cluster Reset](#81-cluster-reset)
  - [8.2. Introduction](#82-introduction)
  - [8.3. Install OPA](#83-install-opa)
  - [8.4. Deny All Policy](#84-deny-all-policy)
  - [8.5. Enforce Namespace Labels](#85-enforce-namespace-labels)
  - [8.6. Enforce Deployment Replica](#86-enforce-deployment-replica)
  - [8.7. The Rego Playground and more examples](#87-the-rego-playground-and-more-examples)
  - [8.8. Recap](#88-recap)
- [9. Supply Chain Security](#9-supply-chain-security)
  - [9.1. Image footprint](#91-image-footprint)
    - [9.1.1. Introduction](#911-introduction)
    - [9.1.2. Reduce image Footprint with Multi-Stage](#912-reduce-image-footprint-with-multi-stage)
    - [9.1.3. Secure and Harden images](#913-secure-and-harden-images)
    - [9.1.4. Recap](#914-recap)
  - [9.2. Static Analysis](#92-static-analysis)
    - [9.2.1. Introduction](#921-introduction)
    - [9.2.2. Kubesec](#922-kubesec)
    - [9.2.3. Practice Kubesec](#923-practice-kubesec)
    - [9.2.4. OPA Conftest](#924-opa-conftest)
    - [9.2.5. OPA Conftest for K8s YAML](#925-opa-conftest-for-k8s-yaml)
    - [9.2.6. OPA Conftest for Dockerfile](#926-opa-conftest-for-dockerfile)
    - [9.2.7. Recap](#927-recap)
  - [9.3. Image Vulnerability Scanning](#93-image-vulnerability-scanning)
    - [9.3.1. Introduction](#931-introduction)
    - [9.3.2. Clair and Trivy](#932-clair-and-trivy)
    - [9.3.3. Use Trivy to scan images](#933-use-trivy-to-scan-images)
    - [9.3.4. Recap](#934-recap)
  - [9.4. Secure Supply Chain](#94-secure-supply-chain)
    - [9.4.1. Introduction](#941-introduction)
    - [9.4.2. Image Digest](#942-image-digest)
    - [9.4.3. Whitelist Registries with OPA](#943-whitelist-registries-with-opa)
    - [9.4.4. ImagePolicyWebhook](#944-imagepolicywebhook)
    - [9.4.5. Practice ImagePolicyWebhook](#945-practice-imagepolicywebhook)
    - [9.4.6. Recap](#946-recap)
- [10. Runtime Security](#10-runtime-security)
  - [10.1. Behavioral Analytics at host and ...](#101-behavioral-analytics-at-host-and-)
    - [10.1.1. Introduction](#1011-introduction)
    - [10.1.2. Strace](#1012-strace)
    - [10.1.3. Strace and /proc on ETCD](#1013-strace-and-proc-on-etcd)
    - [10.1.4. /proc and env variables](#1014-proc-and-env-variables)
    - [10.1.5. Falco and Installation](#1015-falco-and-installation)
    - [10.1.6. Use Falco to find malicious processes](#1016-use-falco-to-find-malicious-processes)
    - [10.1.7. Investigate Falco rules](#1017-investigate-falco-rules)
    - [10.1.8. Change Falco rule](#1018-change-falco-rule)
    - [10.1.9. Recap](#1019-recap)
  - [10.2. Inmutability of containers at runtime](#102-inmutability-of-containers-at-runtime)
    - [10.2.1. Introduction](#1021-introduction)
    - [10.2.2. Ways to enforce immutability](#1022-ways-to-enforce-immutability)
    - [10.2.3. StartupProbe changes container](#1023-startupprobe-changes-container)
    - [10.2.4. SecurityContext renders container immutable](#1024-securitycontext-renders-container-immutable)
    - [10.2.5. Recap](#1025-recap)
  - [10.3. Auditing](#103-auditing)
    - [10.3.1. Introduction](#1031-introduction)
    - [10.3.2. Enable Auditing Logging in Apiserver](#1032-enable-auditing-logging-in-apiserver)
    - [10.3.3. Create Secret and check Audit Logs](#1033-create-secret-and-check-audit-logs)
    - [10.3.4. Create advanced Audit Policy](#1034-create-advanced-audit-policy)
    - [10.3.5. Recap](#1035-recap)
- [11. System Hardening](#11-system-hardening)
  - [11.1. Kernel Hardening Tools](#111-kernel-hardening-tools)
    - [11.1.1. Introduction](#1111-introduction)
    - [11.1.2. AppArmor](#1112-apparmor)
    - [11.1.3. AppArmor for curl](#1113-apparmor-for-curl)
    - [11.1.4. AppArmor for Docker Nginx](#1114-apparmor-for-docker-nginx)
    - [11.1.5. AppArmor for Kubernetes Nginx](#1115-apparmor-for-kubernetes-nginx)
    - [11.1.6. Seccomp](#1116-seccomp)
    - [11.1.7. Seccomp for Docker Nginx](#1117-seccomp-for-docker-nginx)
    - [11.1.8. Seccomp for Kubernetes Nginx](#1118-seccomp-for-kubernetes-nginx)
    - [11.1.9. Recap](#1119-recap)
  - [11.2. Reduce Attack Surface](#112-reduce-attack-surface)
    - [11.2.1. Introduction](#1121-introduction)
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
controlplane $ kubectl get node -owide
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
### 7.3.2. Set container User and Group
### 7.3.3. Force container non-root
### 7.3.4. Privileged Containers
### 7.3.5. Created Privileged Containers
### 7.3.6. PrivilegeScalation
### 7.3.7. Disable PrivilegeScalation
### 7.3.8. PodSecurityPolicies
### 7.3.9. Create and enable PodSecurityPolicies
### 7.3.10. Recap
## 7.4. mTLS
### 7.4.1. Intro
### 7.4.2. Create sidecar proxy
### 7.4.3. Recap



# 8. Open Policy Agent (OPA)
## 8.1. Cluster Reset
## 8.2. Introduction
## 8.3. Install OPA
## 8.4. Deny All Policy
## 8.5. Enforce Namespace Labels
## 8.6. Enforce Deployment Replica
## 8.7. The Rego Playground and more examples
## 8.8. Recap



# 9. Supply Chain Security
## 9.1. Image footprint
### 9.1.1. Introduction
### 9.1.2. Reduce image Footprint with Multi-Stage
### 9.1.3. Secure and Harden images
### 9.1.4. Recap
## 9.2. Static Analysis
### 9.2.1. Introduction
### 9.2.2. Kubesec
### 9.2.3. Practice Kubesec
### 9.2.4. OPA Conftest
### 9.2.5. OPA Conftest for K8s YAML
### 9.2.6. OPA Conftest for Dockerfile
### 9.2.7. Recap
## 9.3. Image Vulnerability Scanning
### 9.3.1. Introduction
### 9.3.2. Clair and Trivy
### 9.3.3. Use Trivy to scan images
### 9.3.4. Recap
## 9.4. Secure Supply Chain
### 9.4.1. Introduction
### 9.4.2. Image Digest
### 9.4.3. Whitelist Registries with OPA
### 9.4.4. ImagePolicyWebhook
### 9.4.5. Practice ImagePolicyWebhook
### 9.4.6. Recap



# 10. Runtime Security
## 10.1. Behavioral Analytics at host and ...
### 10.1.1. Introduction

### 10.1.2. Strace
### 10.1.3. Strace and /proc on ETCD
### 10.1.4. /proc and env variables
### 10.1.5. Falco and Installation
### 10.1.6. Use Falco to find malicious processes
### 10.1.7. Investigate Falco rules
### 10.1.8. Change Falco rule
### 10.1.9. Recap
## 10.2. Inmutability of containers at runtime
### 10.2.1. Introduction
### 10.2.2. Ways to enforce immutability
### 10.2.3. StartupProbe changes container
### 10.2.4. SecurityContext renders container immutable
### 10.2.5. Recap
## 10.3. Auditing
### 10.3.1. Introduction
### 10.3.2. Enable Auditing Logging in Apiserver
### 10.3.3. Create Secret and check Audit Logs
### 10.3.4. Create advanced Audit Policy
### 10.3.5. Recap



# 11. System Hardening
## 11.1. Kernel Hardening Tools
### 11.1.1. Introduction
### 11.1.2. AppArmor
### 11.1.3. AppArmor for curl
### 11.1.4. AppArmor for Docker Nginx
### 11.1.5. AppArmor for Kubernetes Nginx
### 11.1.6. Seccomp
### 11.1.7. Seccomp for Docker Nginx
### 11.1.8. Seccomp for Kubernetes Nginx
### 11.1.9. Recap
## 11.2. Reduce Attack Surface
### 11.2.1. Introduction
### 11.2.2. Systemctl and Services
### 11.2.3. Install and investigate Services
### 11.2.4. Disabled application listening on port
### 11.2.5. Investigate Linux Users
### 11.2.6. Recap



# 12. Linux Foundation Simulator Sessions