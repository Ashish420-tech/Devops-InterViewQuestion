Day-1
==========================================

Step 1. Check Cluster
kubectl cluster-info

Check API Server

kubectl version

Check Context

kubectl config current-context

Check all contexts

kubectl config get-contexts
Step 2. Check Nodes
kubectl get nodes

Expected

NAME      STATUS    ROLES
worker1   Ready
worker2   Ready

If

NotReady

Investigate

kubectl describe node worker1

Look for

DiskPressure
MemoryPressure
PIDPressure
NetworkUnavailable

Also

kubectl top nodes
Step 3. Check Namespace

Many people forget this.

kubectl get ns

Current namespace

kubectl config view --minify

Or

kubectl get pods -n production
Step 4. Check Pods

Most used command

kubectl get pods -A

Or

kubectl get pods

Look for

Running

Pending

CrashLoopBackOff

ImagePullBackOff

Error

Completed

Evicted

ContainerCreating
Step 5. Describe Pod
kubectl describe pod nginx

This is one of the most important commands.

Check

Events

Example

FailedScheduling

Insufficient CPU

FailedMount

ImagePullBackOff

Usually the answer is here.

Step 6. Check Logs
kubectl logs pod-name

Multiple containers

kubectl logs pod-name -c container-name

Previous crash

kubectl logs pod-name --previous
Step 7. Execute Inside Pod
kubectl exec -it pod-name -- sh

or

kubectl exec -it pod-name -- bash

Check

env

ping

curl

hostname

cat config

ls

ps
Step 8. Check Deployment
kubectl get deployment

Describe

kubectl describe deployment nginx

Check rollout

kubectl rollout status deployment nginx

History

kubectl rollout history deployment nginx

Rollback

kubectl rollout undo deployment nginx
Step 9. ReplicaSet
kubectl get rs

Describe

kubectl describe rs
Step 10. Service

Check

kubectl get svc

Describe

kubectl describe svc nginx

Endpoints

kubectl get endpoints

If endpoints are empty

Usually selector mismatch.

Step 11. EndpointSlice
kubectl get endpointslice

Very common in modern clusters.

Step 12. DNS

Inside pod

nslookup service-name

or

dig service-name

Test

curl http://service-name
Step 13. Network

Network Policies

kubectl get networkpolicy

If using Calico

calicoctl get policy
Step 14. Storage

PVC

kubectl get pvc

PV

kubectl get pv

Describe

kubectl describe pvc

Common issue

Pending

Means

StorageClass missing
CSI Driver missing
Storage unavailable

Remember your EKS Jenkins issue:

PVC Pending
↓

EBS CSI Driver missing
↓

Jenkins Pod Pending
Step 15. Events

One of the best commands

kubectl get events --sort-by=.metadata.creationTimestamp

Latest events

kubectl get events -A
Step 16. Resource Usage

Nodes

kubectl top nodes

Pods

kubectl top pods

Need Metrics Server.

Step 17. Check Image

Image exists?

kubectl describe pod

Look for

ImagePullBackOff

Causes

Wrong image
Wrong tag
Private registry
Missing imagePullSecret
Step 18. Secrets
kubectl get secrets

Describe

kubectl describe secret
Step 19. ConfigMaps
kubectl get configmap

Describe

kubectl describe configmap
Step 20. Ingress
kubectl get ingress

Describe

kubectl describe ingress

Check

Host
Path
Backend
TLS
Step 21. Helm (if applicable)

Releases

helm list -A

Status

helm status monitoring

Values

helm get values monitoring
Step 22. Controller Logs

Examples

Ingress Controller

kubectl logs deployment/ingress-nginx-controller -n ingress-nginx

CoreDNS

kubectl logs deployment/coredns -n kube-system
Step 23. API Resources
kubectl api-resources
Step 24. Overall Cluster Objects
kubectl get all

Namespace-specific

kubectl get all -n production
The 80/20 Troubleshooting Commands

These solve most Kubernetes issues:

kubectl get nodes
kubectl get pods -A
kubectl describe pod <pod-name>
kubectl logs <pod-name>
kubectl get svc
kubectl get endpoints
kubectl get pvc
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl top nodes
kubectl top pods
kubectl exec -it <pod-name> -- sh
kubectl rollout status deployment/<deployment-name>
kubectl get ingress
Interview Troubleshooting Order (Memorize)
1. Cluster
2. Nodes
3. Namespace
4. Deployment
5. ReplicaSet
6. Pods
7. Describe Pod
8. Logs
9. Exec into Pod
10. Service
11. Endpoints
12. DNS
13. NetworkPolicy
14. Storage (PV/PVC)
15. Ingress
16. Events
17. Resource Usage
18. ConfigMap
19. Secret
20. Controller Logs
