# Kubernetes Troubleshooting Lab

## Overview
This lab focuses on troubleshooting a failed Kubernetes Deployment update and recovering the application using Rollback.

## Lab Environment
- Kubernetes
- NGINX
- kubectl

## Scenario
An incorrect image tag was applied to the NGINX Deployment:

`nginx:999.99`

This caused newly created Pods to fail with `ImagePullBackOff`.

## Troubleshooting

### 1. Inspect Pods

```bash
kubectl get pods
```

Some newly created Pods were in `ImagePullBackOff` state.

### 2. Inspect Deployment

```bash
kubectl get deployment
```

The Deployment was not fully updated.

### 3. Inspect ReplicaSets

```bash
kubectl get replicaset
```

The old ReplicaSet continued running healthy Pods while the new ReplicaSet attempted to create Pods using the invalid image.

### 4. Inspect Kubernetes Events

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
```

The events revealed image pull failures related to the invalid image tag.

### 5. Identify the Root Cause

The image tag `nginx:999.99` was invalid, resulting in `ImagePullBackOff`.

## Recovery

I performed a rollback to the previous working Deployment revision:

```bash
kubectl rollout undo deployment/nginx-app
```

Then I verified the rollout:

```bash
kubectl rollout status deployment/nginx-app
```

## Verification

```bash
kubectl get deployment
kubectl get pods
```

The Deployment returned to a healthy state with 4/4 replicas running.

## Key Learnings

- Kubernetes RollingUpdate behavior
- Deployment troubleshooting
- ReplicaSet management
- Diagnosing ImagePullBackOff
- Rollback and recovery
- Inspecting Kubernetes Events

## Outcome

Successfully diagnosed a failed Deployment update and restored the application using Kubernetes Rollback.
