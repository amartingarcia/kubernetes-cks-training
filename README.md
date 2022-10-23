# training_k8s_cks

# Table of Contents
- [training_k8s_cks](#training_k8s_cks)
- [Table of Contents](#table-of-contents)
- [1. Introduction](#1-introduction)
  - [1.1. Welcome](#11-welcome)
  - [1.2. Best Video Quality](#12-best-video-quality)
  - [1.3. K8s Security Best Practices](#13-k8s-security-best-practices)
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
  - [4.2.2. Test Tools Introduction](#422-test-tools-introduction)
  - [4.2.3. The PID Namespace](#423-the-pid-namespace)
  - [4.2.4. Recap](#424-recap)
- [5. Cluster setup](#5-cluster-setup)
  - [5.1. Network Policies](#51-network-policies)
    - [5.1.1. Cluster Reset](#511-cluster-reset)
    - [5.1.2. Introduction 1](#512-introduction-1)
    - [5.1.3. Introduction 2](#513-introduction-2)
    - [5.1.4. Default Deny](#514-default-deny)
    - [5.1.5. Frontend to Backend traffic](#515-frontend-to-backend-traffic)
    - [5.1.6. Backend to database traffic](#516-backend-to-database-traffic)
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

# 2. Create your course K8S cluster
## 2.1. Cluster Specification
## 2.2. Create GCP Account
## 2.3. Configure gcloud command
## 2.4. Create Kubeadm Cluster in GCP
## 2.5. Firewall rules for NodePorts
## 2.6. Always stop your instances
## 2.7. Containerd Course Upgrade
## 2.8. Recap

# 3. Killercoda Access
## 3.1. How to get access
## 3.2. Your Access Code


# 4. Foundation
## 4.1. Kubernetes Secure Arquitecture
## 4.1.1. Intro
## 4.1.2. Find various k8s certificates
## 4.1.3. Recap
## 4.2. Containers under the hood
## 4.2.1. Intro
## 4.2.2. Test Tools Introduction
## 4.2.3. The PID Namespace
## 4.2.4. Recap

# 5. Cluster setup
## 5.1. Network Policies
### 5.1.1. Cluster Reset
### 5.1.2. Introduction 1
### 5.1.3. Introduction 2
### 5.1.4. Default Deny
### 5.1.5. Frontend to Backend traffic
### 5.1.6. Backend to database traffic
### 5.1.7. Recap
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