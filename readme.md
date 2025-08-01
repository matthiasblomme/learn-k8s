## Nugget 005 - Basic Deployment and Service
- **Topic**: Deploying a simple application with a service.
- **Files**: `echo-deployment.yaml`, `echo-service.yaml`
- **Test**: `kubectl apply -f .`, then access via the service.

## Nugget 006 - Updating Deployments
- **Topic**: Understanding updates and `kubectl rollout`.
- **Files**: `echo-deployment.yaml`, `echo-service.yaml`
- **Test**: Edit image tag and use `kubectl rollout status`.

## Nugget 007 - Using ConfigMaps
- **Topic**: Injecting configuration into pods using ConfigMaps.
- **Files**: `echo-configmap.yaml`, `echo-deployment.yaml`
- **Test**: Apply and verify env variables via `kubectl exec`.

## Nugget 008 - Using Secrets as Volumes
- **Topic**: Mounting secrets as files inside the container.
- **Files**: `echo-secret-volume.yaml`, `echo-deployment-volume.yaml`
- **Test**: Check `/mnt/secret/secret.txt` inside pod.

## Nugget 009 - Liveness and Readiness Probes
- **Topic**: Probe setup and behavior on failures.
- **Files**: `echo-health.yaml`, `debug-prober-fail.yaml`, `echo-health-nginx.yaml`
- **Test**: Observe pod restart behavior with failing liveness/readiness.

## Nugget 010 - Ingress and TLS
- **Topic**: Secure Ingress with TLS certificates.
- **Files**: `ingress.yaml`, `ingress-tls.yaml`, `tls.crt`, `tls.key`, `nginx-tls-deploy.yaml`, `echo-tls-deploy.yaml`
- **Test**: Access application over HTTPS via Ingress controller.

## Nugget 011 - Init Containers
- **Topic**: Running initialization logic before main container starts.
- **Files**: `init-deployment.yaml`
- **Test**: Use `kubectl describe pod` to view init container status.

## Nugget 012 - Multi-Container Pods and Environment Injection
- **Topic**: Using multiple containers in a pod and injecting env vars.
- **Files**: `mutli-container.yaml`, `echo-env-demo-deployment.yaml`, `echo-env-demo-service.yaml`, `nginx-health-deployment.yaml`, `nginx-health-service.yaml`
- **Test**: Log output and inter-container communication.

## Nugget 013 - Resource Requests and Limits
- **Topic**: Managing CPU/memory allocations.
- **Files**: `echo-resources.yaml`, `echo-oom.yaml`
- **Test**: Use `kubectl describe pod` to view resource usage and `kubectl top` if metrics are available.

## Nugget 014 - Horizontal Pod Autoscaler
- **Topic**: Autoscaling based on CPU.
- **Files**: `echo-autoscale.yaml`, `echo-hpa.yaml`
- **Test**: Generate CPU load, monitor pod scaling with `kubectl get hpa`.

## Nugget 015 - Probes in Detail
- **Topic**: Liveness vs readiness with simple app.
- **Files**: `pod-probes.yaml`
- **Test**: View effect of failing and passing probes on pod state.

## Nugget 016 - PreStop Hooks
- **Topic**: Graceful shutdown via preStop lifecycle.
- **Files**: `pod-prestop.yaml`, `pod-prestop-sidecar.yaml`
- **Test**: Delete pod and observe preStop behavior in logs.

## Nugget 017 - Rolling Updates & Rollbacks
- **Topic**: Image updates and rolling strategy.
- **Files**: `echo-app-deployment.yaml`
- **Test**: Update image, use `kubectl rollout status`, `kubectl rollout undo`.

## Nugget 018 - Taints and Tolerations
- **Topic**: Node scheduling control via taints.
- **Files**: `toleration-pod.yaml`, `no-toleration-pod.yaml`
- **Test**: Apply taint to node, observe pod scheduling behavior.

## Nugget 019 - Node Affinity
- **Topic**: Node selection based on labels.
- **Files**: `echo-node-affinity.yaml`, `echo-node-affinity-soft.yaml`
- **Test**: Label node and observe hard/soft affinity scheduling.

## Nugget 020 - NodeSelector
- **Topic**: Simple node selection using nodeSelector.
- **Files**: `pod-nodeselector.yaml`
- **Test**: Label node and confirm correct pod placement.

## Nugget 021 - Secret and ConfigMap Volume Mounts
- **Topic**: Mounting secrets and configmaps as files.
- **Files**: `secret21.yaml`, `cm21.yaml`, `pod21.yaml`
- **Test**: Check `/mnt/config/` and `/mnt/secret/` inside pod.

## Nugget 022 - ConfigMap as Environment Variables
- **Topic**: Mapping ConfigMap keys to env vars.
- **Files**: `configmap.yaml`, `config-map-pod.yaml`
- **Test**: Log env variables from pod logs.

## Nugget 023 - Secrets as Environment Variables
- **Topic**: Injecting secrets into pod environment.
- **Files**: `configmap.yaml`, `pod.yaml`
- **Test**: Log and confirm secret environment variable.

## Nugget 024 - Secure App Deployment (Security Context + Downward API)

- **Topic**: Hardening pod/container security using security contexts, and reading pod metadata using the Downward API.
- **Files**: `secure-echo.yaml`
- **Summary**:
    - Set container-level security context:
        - `runAsUser`: Runs the container as a specific user ID (e.g., 1000).
        - `readOnlyRootFilesystem`: Makes the container’s root filesystem read-only.
        - `capabilities`: Drop unnecessary Linux capabilities.
    - Use the **Downward API** to expose pod metadata into the container using environment variables (e.g., pod name, namespace).

- **Test**:
    1. Apply the YAML:
        ````bash
        kubectl apply -f secure-echo.yaml
        ````
    2. Exec into the pod and check user ID:
        ````bash
        kubectl exec -it <pod-name> -- whoami
        id
        ````
    3. Verify that the container has no write access to `/`:
        ````bash
        kubectl exec -it <pod-name> -- touch /test.txt
        # Should fail due to readOnlyRootFilesystem
        ````
    4. Check if pod metadata is available via environment variables:
        ````bash
        kubectl exec -it <pod-name> -- printenv | grep POD_

# Nugget 025 - Persistent Volumes (PV) & PVCs

This lesson demonstrates Kubernetes persistent storage using a `PersistentVolume` and a `PersistentVolumeClaim`.

You’ll:
- Create a `PersistentVolume` with `hostPath` as storage backend.
- Bind a `PersistentVolumeClaim` to it.
- Launch a pod that mounts the claim and writes a file to the mounted volume.

### Files:
- `pv.yaml`: Defines the persistent volume (1Gi).
- `pvc.yaml`: Requests 500Mi of storage.
- `pod.yaml`: Pod with Alpine container that mounts `/data`.

### Test:
1. Apply all resources.
2. Use `kubectl exec` to write a file inside the pod.
3. Restart the pod and verify the file still exists (on the same node).

> ⚠️ Note: `hostPath` is node-local. For real persistence across nodes, use NFS or a cloud volume.

## Nugget 26 - StatefulSets

**Topic**: Deploying stateful applications with predictable identity and persistent storage.

### Overview

This nugget demonstrates how to use StatefulSets to manage pods that require:

- Stable network identities.
- Persistent storage per pod.
- Ordered, graceful deployment and scaling.

We deploy a sample echo server with a headless service and PersistentVolumeClaims (PVCs) managed via `volumeClaimTemplates`.

### Files

- `headless-service.yaml`: Defines a headless service with `clusterIP: None`.
- `statefulset-echo.yaml`: Creates a StatefulSet with 3 replicas and a 100Mi persistent volume per pod.

### How to Run

```bash
kubectl apply -f headless-service.yaml
kubectl apply -f statefulset-echo.yaml
kubectl get pods
kubectl get pvc
kubectl exec -it echo-app-0 -- sh
```

## ⏱️ Nugget 27 – Jobs & CronJobs

This nugget explores how to run one-time and recurring tasks in Kubernetes using `Job` and `CronJob` resources.

### 🧠 What You Learn

- How to define a `Job` to ensure a task runs to completion.
- How `backoffLimit`, `completions`, and `restartPolicy` affect behavior.
- How to schedule recurring jobs using `CronJob` with cron syntax.
- Common pitfalls: missed schedules, pod history retention.

### 🔧 Files in this Nugget

- `job.yaml`: A simple job that echoes a message and exits.
- `cronjob.yaml`: A cronjob that runs every minute.

### ▶️ How to Test

Apply the job:

```bash
kubectl apply -f job.yaml
kubectl get jobs
kubectl logs job/hello-job
```

Apply the cronjob:

```bash
kubectl apply -f cronjob.yaml
kubectl get cronjobs
```

Wait ~2 minutes and then check the pods created by the cronjob:

```bash
kubectl get pods --selector=job-name
kubectl logs <pod-name>
```

### 📌 Notes

- Jobs will automatically clean up if TTL is configured.
- CronJobs retain successful and failed pods depending on `successfulJobsHistoryLimit` and `failedJobsHistoryLimit`.

```
# Example: Print pod logs from a specific job
kubectl logs $(kubectl get pods --selector=job-name=hello-job -o jsonpath="{.items[0].metadata.name}")
```

## Nugget 028 – Kubernetes Configuration Best Practices

This nugget introduces best practices for organizing and managing Kubernetes manifests. You'll learn about:

- Structuring your YAML files using `base` and `overlays`.
- Applying meaningful `labels` and `annotations`.
- Using `kustomize` for configuration reusability and environment-specific variations.

### 🗂 File Structure
The lesson is split into:
- `base/`: Contains core YAML like deployments and services.
- `overlays/dev` and `overlays/prod`: Customize things like replica counts or image versions.

### ▶️ Run Instructions
```bash
kubectl apply -k overlays/dev/
```

To inspect the result:
```bash
kubectl get deployment,svc -l app=echo
```

### 📌 Notes
- Labels help organize and query resources.
- Annotations carry metadata that tools can use.
- `kustomize` is built into `kubectl`, no plugin needed.


# Nugget 029 - Helm Introduction

This nugget introduces Helm, the package manager for Kubernetes.

## What You Learned

- How to install Helm and add chart repositories.
- How to install, inspect, and upgrade a Helm chart.
- The structure of a Helm release and how values files work.

## Commands Summary

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm install my-nginx bitnami/nginx
helm show values bitnami/nginx > nginx-values.yaml
helm upgrade my-nginx bitnami/nginx -f nginx-values.yaml
helm uninstall my-nginx
```

## Test & Verify

```bash
kubectl get all
kubectl port-forward svc/my-nginx 8080:80
curl localhost:8080
```

The default NGINX chart will show a Bitnami welcome page.
