# training_k8s_cks

# Table of Contents
- [training_k8s_cks](#training_k8s_cks)
- [Table of Contents](#table-of-contents)
- [1. Introduction](#1-introduction)
  - [1.1. Welcome](#11-welcome)
  - [1.2. Best Video Quality](#12-best-video-quality)
  - [1.3. K8s Security Best Practices](#13-k8s-security-best-practices)
    - [Security Principles](#security-principles)
    - [K8s Security Categories](#k8s-security-categories)
    - [K8s Best Practices](#k8s-best-practices)
    - [Links](#links)
- [2. Create your course K8S cluster](#2-create-your-course-k8s-cluster)
  - [2.1. Cluster Specification](#21-cluster-specification)
  - [2.2. Create GCP Account](#22-create-gcp-account)
  - [2.3. Configure gcloud command](#23-configure-gcloud-command)
  - [2.4. Create Kubeadm Cluster in GCP](#24-create-kubeadm-cluster-in-gcp)
  - [2.5. Firewall rules for NodePorts](#25-firewall-rules-for-nodeports)
  - [2.6. Always stop your instances](#26-always-stop-your-instances)
  - [2.7. Containerd Course Upgrade](#27-containerd-course-upgrade)
  - [2.8. Recap](#28-recap)
- [3. Killercoda Access](#3-killercoda-access)
  - [3.1. How to get access](#31-how-to-get-access)
  - [3.2. Your Access Code](#32-your-access-code)
- [4. Foundation](#4-foundation)
  - [4.1. Kubernetes Secure Arquitecture](#41-kubernetes-secure-arquitecture)
    - [4.1.1. Intro](#411-intro)
    - [4.1.2. Find various k8s certificates](#412-find-various-k8s-certificates)
    - [4.1.3. Recap](#413-recap)
  - [4.2. Containers under the hood](#42-containers-under-the-hood)
    - [4.2.1. Intro](#421-intro)
      - [4.2.1.1. Container and Image](#4211-container-and-image)
    - [4.2.2. Test Tools Introduction](#422-test-tools-introduction)
      - [4.2.2.1. Container tools](#4221-container-tools)
        - [Dockerfile](#dockerfile)
        - [Build Dockerfile](#build-dockerfile)
        - [List image](#list-image)
        - [Docker run](#docker-run)
        - [Podman build](#podman-build)
        - [Podman run](#podman-run)
    - [4.2.3. The PID Namespace](#423-the-pid-namespace)
      - [Run c1 container](#run-c1-container)
      - [Show process on c1 container](#show-process-on-c1-container)
      - [Run c2 container](#run-c2-container)
      - [Show process on c2 container](#show-process-on-c2-container)
      - [Show process on host](#show-process-on-host)
      - [Delete c2 container](#delete-c2-container)
      - [Recreate container with same namespace.](#recreate-container-with-same-namespace)
      - [Show process on c2 container (you can see other container process).](#show-process-on-c2-container-you-can-see-other-container-process)
      - [Show process on c1 container (you can see other container process).](#show-process-on-c1-container-you-can-see-other-container-process)
    - [4.2.4. Recap](#424-recap)
- [5. Cluster setup](#5-cluster-setup)
  - [5.1. Network Policies](#51-network-policies)
    - [5.1.1. Cluster Reset](#511-cluster-reset)
    - [5.1.2. Introduction 1](#512-introduction-1)
    - [5.1.3. Introduction 2](#513-introduction-2)
      - [NetworkPolicy example](#networkpolicy-example)
    - [5.1.4. Default Deny](#514-default-deny)
      - [Create a frontend and backend applications and expose.](#create-a-frontend-and-backend-applications-and-expose)
      - [Test connection between applications](#test-connection-between-applications)
      - [Create the NetworkPolicy](#create-the-networkpolicy)
      - [Test connection between apps](#test-connection-between-apps)
    - [5.1.5. Frontend to Backend traffic](#515-frontend-to-backend-traffic)
      - [Test connection](#test-connection)
      - [Test connection](#test-connection-1)
      - [If you want connect to DNS, you indicate Port 53](#if-you-want-connect-to-dns-you-indicate-port-53)
    - [5.1.6. Backend to database traffic](#516-backend-to-database-traffic)
      - [Create a namespace](#create-a-namespace)
      - [Create a Pod](#create-a-pod)
      - [Get Pod Cassandra IP](#get-pod-cassandra-ip)
      - [Test connection](#test-connection-2)
      - [Apply egress to cassandra namespace](#apply-egress-to-cassandra-namespace)
      - [Test connection](#test-connection-3)
      - [Create configuration to Deny all to cassandra Pod](#create-configuration-to-deny-all-to-cassandra-pod)
      - [And create NetworkPolicy to cassandra ingress from default](#and-create-networkpolicy-to-cassandra-ingress-from-default)
      - [Labeled default namespace and launch curl](#labeled-default-namespace-and-launch-curl)
    - [5.1.7. Recap](#517-recap)
  - [5.2. GUI Elements](#52-gui-elements)
    - [5.2.1. Introduction](#521-introduction)
    - [5.2.2. Install Dashboard](#522-install-dashboard)
    - [5.2.3. Outside Insecure Access](#523-outside-insecure-access)
    - [5.2.4. RBAC for the dashboard](#524-rbac-for-the-dashboard)
    - [5.2.5. Recap](#525-recap)
  - [5.3. Secure Ingress](#53-secure-ingress)
    - [5.3.1. K8S docs in correct version](#531-k8s-docs-in-correct-version)
    - [5.3.2. Introduction](#532-introduction)
    - [5.3.3. Create an Ingress](#533-create-an-ingress)
    - [5.3.4. Secure an Ingress](#534-secure-an-ingress)
    - [5.3.5. Recap](#535-recap)
  - [5.4. Node metadata protection](#54-node-metadata-protection)
    - [5.4.1. Introduction](#541-introduction)
    - [5.4.2. Access Node Metadata](#542-access-node-metadata)
    - [5.4.3. Protect Node Metadata via Network Policy](#543-protect-node-metadata-via-network-policy)
  - [5.5. CIS Bechmarck](#55-cis-bechmarck)
    - [5.5.1. Introduction](#551-introduction)
    - [5.5.2. CIS in action](#552-cis-in-action)
    - [5.5.3. kube-bench](#553-kube-bench)
    - [5.5.4. Recap](#554-recap)
  - [5.6. Verify Platform Binaries](#56-verify-platform-binaries)
    - [5.6.1. Introduction](#561-introduction)
    - [5.6.2. Download and verify k8s release](#562-download-and-verify-k8s-release)
    - [5.6.3. Verify apiserver binary running in our cluster](#563-verify-apiserver-binary-running-in-our-cluster)
    - [5.6.4. Recap](#564-recap)
- [6. Cluster Hardening](#6-cluster-hardening)
  - [6.1. RBAC](#61-rbac)
    - [6.1.1. Intro](#611-intro)
    - [6.1.2. Role and Rolebinding](#612-role-and-rolebinding)
    - [6.1.3. ClusterRole and ClusterRoleBinding](#613-clusterrole-and-clusterrolebinding)
    - [6.1.4. Accounts and Users](#614-accounts-and-users)
    - [6.1.5. CertificateSingingRequets](#615-certificatesingingrequets)
    - [6.1.6. Recap](#616-recap)
  - [6.2. Exercise caution in using ...](#62-exercise-caution-in-using-)
    - [6.2.1. Intro](#621-intro)
    - [6.2.2. Disable SA Mounting](#622-disable-sa-mounting)
    - [6.2.3. Limits SA using RBAC](#623-limits-sa-using-rbac)
    - [6.2.4. Recap](#624-recap)
  - [6.3. Restrict API Access](#63-restrict-api-access)
    - [6.3.1. Intro](#631-intro)
    - [6.3.2. Anonymous Access](#632-anonymous-access)
    - [6.3.3. Insecure Access](#633-insecure-access)
    - [6.3.4. Manual API Requests](#634-manual-api-requests)
    - [6.3.5. External Apiserver Access](#635-external-apiserver-access)
    - [6.3.6. NodeRestriction AdmissionController](#636-noderestriction-admissioncontroller)
    - [6.3.7. Verify NodeRestriction](#637-verify-noderestriction)
    - [6.3.8. Recap](#638-recap)
  - [6.4. Upgrade Kubernetes](#64-upgrade-kubernetes)
    - [6.4.1. Intro](#641-intro)
    - [6.4.2. Ubuntu 20.04 Update](#642-ubuntu-2004-update)
    - [6.4.3. Create outdated cluster](#643-create-outdated-cluster)
    - [6.4.4. Upgrade controlplane node](#644-upgrade-controlplane-node)
    - [6.4.5. Upgrade node](#645-upgrade-node)
    - [6.4.6. Recap](#646-recap)
- [7. Microservice Vulnerabilities](#7-microservice-vulnerabilities)
  - [7.1. Manage Kubernetes](#71-manage-kubernetes)
    - [7.1.1. Intro](#711-intro)
    - [7.1.2. Create Simple Secret Scenario](#712-create-simple-secret-scenario)
    - [7.1.3. Hacks Secret in Container Runtime](#713-hacks-secret-in-container-runtime)
    - [7.1.4. Hacks Secret in ETCD](#714-hacks-secret-in-etcd)
    - [7.1.5. ETCD Encryption](#715-etcd-encryption)
    - [7.1.6. Encrypt ETCD](#716-encrypt-etcd)
    - [7.1.7. Recap](#717-recap)
  - [7.2. Container Runtime](#72-container-runtime)
    - [7.2.1. Intro](#721-intro)
    - [7.2.2. Containers Calls Linux Kernel](#722-containers-calls-linux-kernel)
    - [7.2.3. Open Container Iniciative OCI](#723-open-container-iniciative-oci)
    - [7.2.4. Sandbox Runtime Katacontainer](#724-sandbox-runtime-katacontainer)
    - [7.2.5. Sandbox Runtime gVisor](#725-sandbox-runtime-gvisor)
    - [7.2.6. Create and use RuntimeClasses](#726-create-and-use-runtimeclasses)
    - [7.2.7. Install and use gVisor](#727-install-and-use-gvisor)
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
## 1.2. Best Video Quality
## 1.3. K8s Security Best Practices
### Security Principles
### K8s Security Categories
**Host Operating System Security (Ex. Linux)**
* Kubernetes Nodes should only do one thing: Kubernetes
* Reduce Attack Surface
  * Remove unnecessary applications
  * Keep up to date
* Runtime Security Tools
* Find and identify malicious processes
* Restrict IAM/SSH access

**Kubernetes Cluster Security (Ex. Kubernetes)**
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

**Application Security (Ex. Container)**
* Use Secrets/no hardcoded credentials
* RBAC
* Container Sandboxing
* Container Hardening
  * Attachk Surface
  * Run as user
  * Readonly filesystem
* Vulnerability Scanning
* mTLS/ServiceMeshes

### K8s Best Practices
### Links
https://www.youtube.com/watch?v=wqsUfvRyYpw



# 2. Create your course K8S cluster
## 2.1. Cluster Specification
```sh
# cks-master
sudo -i
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_master.sh)


# cks-worker
sudo -i
bash <(curl -s https://raw.githubusercontent.com/killer-sh/cks-course-environment/master/cluster-setup/latest/install_worker.sh)


# run the printed kubeadm-join-command from the master on the worker

```
## 2.2. Create GCP Account
## 2.3. Configure gcloud command
```sh
# install gcloud sdk from
https://cloud.google.com/sdk/auth_success

# then run locally
gcloud auth login
gcloud projects list
gcloud config set project YOUR-PROJECT-ID
gcloud compute instances list # should be empty right now
```

## 2.4. Create Kubeadm Cluster in GCP
```sh
# CREATE cks-master VM using gcloud command
# not necessary if created using the browser interface
gcloud compute instances create cks-master --zone=europe-west3-c \
--machine-type=e2-medium \
--image=ubuntu-2004-focal-v20220419 \
--image-project=ubuntu-os-cloud \
--boot-disk-size=50GB

# CREATE cks-worker VM using gcloud command
# not necessary if created using the browser interface
gcloud compute instances create cks-worker --zone=europe-west3-c \
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

## 2.5. Firewall rules for NodePorts
```sh
gcloud compute firewall-rules create nodeports --allow tcp:30000-40000
```

## 2.6. Always stop your instances
## 2.7. Containerd Course Upgrade
docker --> containerd
## 2.8. Recap
minikube start --network-plugin=cni --cni=calico -p cks


# 3. Killercoda Access
## 3.1. How to get access
## 3.2. Your Access Code



# 4. Foundation
## 4.1. Kubernetes Secure Arquitecture
### 4.1.1. Intro
### 4.1.2. Find various k8s certificates

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


### 4.1.3. Recap
## 4.2. Containers under the hood
### 4.2.1. Intro
#### 4.2.1.1. Container and Image
* **Dockerfile**: Script/text defines how to build an image
* **Image** (docker build): Multi layer binary representation of state
* **Container** (docker run): "running" instnace of an image
  * Collection of one or multiple applications.
  * Includes all its dependencies.
  * Just a process which runs on the Linux Kernel (but which cannot see everything).

### 4.2.2. Test Tools Introduction
#### 4.2.2.1. Container tools
**Docker**: Container Runtime + Tool for managing containers and images
**Containerd**: Container Runtime
**Crictl**: CLI for CRI-compatible Container Runtimes
**Podman**: Tool for managing containers and images

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

### 4.2.3. The PID Namespace
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

### 4.2.4. Recap
https://www.youtube.com/watch?v=MHv6cWjvQjM



# 5. Cluster setup
## 5.1. Network Policies
### 5.1.1. Cluster Reset
### 5.1.2. Introduction 1
**NetworkPolicies**
* Firewall rules in Kubernetes
* Implemented by the Network Plugins CNI (Calico/Weave)
* Namespace level
* Restrict the Ingress and/or Egress for a group of Pods based on certain rules and conditions

**Without NetworkPolicies**
* By default every pod can access every pod
* Pods are **NOT** isolated.

### 5.1.3. Introduction 2
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

**Multiple NetworkPolicies**
* Possible to have multiple NPs selecting the same pods
* If a pod has more than one NP
  * Then the union of all NPs is applied
  * order doesnt affect policy result

Merge example2a + example2b
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


### 5.1.4. Default Deny
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


### 5.1.5. Frontend to Backend traffic
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

### 5.1.6. Backend to database traffic
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

### 5.1.7. Recap
> https://kubernetes.io/docs/concepts/services-networking/network-policies
## 5.2. GUI Elements
### 5.2.1. Introduction
### 5.2.2. Install Dashboard
### 5.2.3. Outside Insecure Access
### 5.2.4. RBAC for the dashboard
### 5.2.5. Recap
## 5.3. Secure Ingress
### 5.3.1. K8S docs in correct version
### 5.3.2. Introduction
### 5.3.3. Create an Ingress
### 5.3.4. Secure an Ingress
### 5.3.5. Recap
## 5.4. Node metadata protection
### 5.4.1. Introduction
### 5.4.2. Access Node Metadata
### 5.4.3. Protect Node Metadata via Network Policy
## 5.5. CIS Bechmarck
### 5.5.1. Introduction
### 5.5.2. CIS in action
### 5.5.3. kube-bench
### 5.5.4. Recap
## 5.6. Verify Platform Binaries
### 5.6.1. Introduction
### 5.6.2. Download and verify k8s release
### 5.6.3. Verify apiserver binary running in our cluster
### 5.6.4. Recap



# 6. Cluster Hardening
## 6.1. RBAC
### 6.1.1. Intro
### 6.1.2. Role and Rolebinding
### 6.1.3. ClusterRole and ClusterRoleBinding
### 6.1.4. Accounts and Users
### 6.1.5. CertificateSingingRequets
### 6.1.6. Recap
## 6.2. Exercise caution in using ...
### 6.2.1. Intro
### 6.2.2. Disable SA Mounting
### 6.2.3. Limits SA using RBAC
### 6.2.4. Recap
## 6.3. Restrict API Access
### 6.3.1. Intro
### 6.3.2. Anonymous Access
### 6.3.3. Insecure Access
### 6.3.4. Manual API Requests
### 6.3.5. External Apiserver Access
### 6.3.6. NodeRestriction AdmissionController
### 6.3.7. Verify NodeRestriction
### 6.3.8. Recap
## 6.4. Upgrade Kubernetes
### 6.4.1. Intro
### 6.4.2. Ubuntu 20.04 Update
### 6.4.3. Create outdated cluster
### 6.4.4. Upgrade controlplane node
### 6.4.5. Upgrade node
### 6.4.6. Recap



# 7. Microservice Vulnerabilities
## 7.1. Manage Kubernetes
### 7.1.1. Intro
### 7.1.2. Create Simple Secret Scenario
### 7.1.3. Hacks Secret in Container Runtime
### 7.1.4. Hacks Secret in ETCD
### 7.1.5. ETCD Encryption
### 7.1.6. Encrypt ETCD
### 7.1.7. Recap
## 7.2. Container Runtime
### 7.2.1. Intro
### 7.2.2. Containers Calls Linux Kernel
### 7.2.3. Open Container Iniciative OCI
### 7.2.4. Sandbox Runtime Katacontainer
### 7.2.5. Sandbox Runtime gVisor
### 7.2.6. Create and use RuntimeClasses
### 7.2.7. Install and use gVisor
### 7.2.8. Recap
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