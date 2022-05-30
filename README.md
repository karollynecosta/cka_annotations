# CKA Annotations
Repository to make annotations about the Certified Kubernetes Administrator (CKA)


## Indice
1. [Resume](#Resume)
2. [Core Concepts](#Core)
3. [Scheduling](#Resume)
4. [Logging & Monitoring](#Resume)
5. [Application Lifecycle Management](#Resume)
6. [Cluster Maintenance](#Resume)
7. [Security](#Resume)
8. [Storage](#Resume)
9. [Networking](#Resume)
10. [Design and Install a Kubernetes Cluster](#Resume)
11. [Install "K8s the kubeadm way"](#Resume)
12. [End to end Tests on a k8s cluster](#Resume)
13. [Troubleshooting](#Resume)
14. [Other topics](#Resume)
15. [Labs](#Resume)


## Resume <a name="Resume"></a>

Exam Details & Resources
This exam is an online, proctored, performance-based test that requires solving multiple tasks from a command line running Kubernetes.
Candidates have 2 hours to complete the tasks.

Candidates who register for the Certified Kubernetes Administrator (CKA) exam will have 2 attempts (per exam registration) to an exam simulator, provided by Killer.sh.

The exam is based on Kubernetes v1.21
The CKA exam environment will be aligned with the most recent K8s minor version within approximately 4 to 8 weeks of the K8s release date

[Certified Kubernetes Administrator](https://www.cncf.io/certification/cka/)

[Exam Curriculum (Topics):](https://github.com/cncf/curriculum)

[Candidate Handbook:](https://www.cncf.io/certification/candidate-handbook)

[Exam Tips:](http://training.linuxfoundation.org/go//Important-Tips-CKA-CKAD)

## Core Concepts <a name="Core"></a>

Pod:
```
Is a single instance of an application
Is the smallest object that you can create in k8s
1 - 1 relationship with container
Multi-container:
    When a pod exists in one container and another brings to help in another container, act as as helper.

Comandos:
    1 - implements one pod:  k run <nomepod> --image=nginx
    2 - lista de pods running: k get pods
    3 - ver os pods com os nodes que estao rodando: kubectl get pods -o wide
    4 -  deleta o pod: kubectl delete pod <podname>
    5 - Edita o pod: k edit pod <podname>
```

-- kube-apiserver --
ETCD Cluster:
```
ETCD is a distributed reliable key-value store that is Simple, Secure & Fast
Keeps the information about:
    Master: nodes, Pods, configs, secrets, accounts, roles, bindings
Every solicitation made by KUBECTL GET/APPLY is in ETCD

Commands:
Additional information about ETCDCTL Utility

ETCDCTL is the CLI tool used to interact with ETCD.

ETCDCTL can interact with ETCD Server using 2 API versions - Version 2 and Version 3.  By default its set to use Version 2. Each version has different sets of commands.

For example ETCDCTL version 2 supports the following commands:

etcdctl backup
etcdctl cluster-health
etcdctl mk
etcdctl mkdir
etcdctl set


Whereas the commands are different in version 3

etcdctl snapshot save
etcdctl endpoint health
etcdctl get
etcdctl put

To set the right version of API set the environment variable ETCDCTL_API command

export ETCDCTL_API=3


When API version is not set, it is assumed to be set to version 2. And version 3 commands listed above don't work. When API version is set to version 3, version 2 commands listed above don't work.



Apart from that, you must also specify path to certificate files so that ETCDCTL can authenticate to the ETCD API Server. The certificate files are available in the etcd-master at the following path. We discuss more about certificates in the security section of this course. So don't worry if this looks complex:

--cacert /etc/kubernetes/pki/etcd/ca.crt
--cert /etc/kubernetes/pki/etcd/server.crt
--key /etc/kubernetes/pki/etcd/server.key


So for the commands I showed in the previous video to work you must specify the ETCDCTL API version and path to certificate files. Below is the final form:



kubectl exec etcd-master -n kube-system -- sh -c "ETCDCTL_API=3 etcdctl get / --prefix --keys-only --limit=10 --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/server.crt  --key /etc/kubernetes/pki/etcd/server.key"
8. Core Concepts Se
```

kube-api Server:
```
The only component that interacts DIRECTLY with ETCD
is a binary to use when you can't utilize kubectl
to see the options of api-server:

ps -aux | grep kube-apiserver
```

Node-controller:
```
Node Monitor period = 5s
Node Monitor Grace period = 40s
POD Eviction timeout = 5m

kubectl get nodes
```

Replication-Controller(RC) == Replica Set(RS):
```
If one pod dies, this replication creates another one
Provides HA, by adding replicas to pods that are in your and template and anothers that can be target by Labes
Load Balancing & Scaling
!= is that selector is required to RS

Commands:
    1. Create a new rc: k create -f rc-definition.yml
    2. See the rc configuration: k get replicationcontroller
    3. See all the replicaset: k get replicaset
    4. See all the pods: k get pods
    5. To scale/update the RS:
        k replace -f replicaset-definition.yml
        k scale --replicas=6 -f replicaset-definition.yml
        k scale --replicas=6 replicaset myapp-replicaset
    6. Delete: k delete replicaset myapp-rs
               kubectl delete -f replicaset-definition-1.yaml

```

Deployments:
```
A Deployment provides declarative updates for Pods and ReplicaSets.

Commands:
    1. Create: k create -f deployment-definition.yml
               kubectl create deployment --image=nginx nginx
               kubectl create deployment --image=nginx nginx --dry-run=client -o yaml
               kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml
    2. Get: k get deployments
    3. Get replicasets: k get replicaset
```

Kube-scheduler:
```
Schedule nods and pods, choose the best node to pod and balaciante

```

Kube Controller-Manager:
```
kubectl get pods -n kube-system
cat /etc/systemd/system/kube-controller-manager.service
ps -aux | grep kube-controller-manager
```

Kubelet:
```
Register the node in cluster
create and monitor the PODs

You have to install in the cluster, kubeadm does not deploy kubelet
```

Kube-proxy: is inside every node, giving IP Address.

Services:
```

```