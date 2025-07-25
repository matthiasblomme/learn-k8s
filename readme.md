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

## Nugget 024 - Secure Application Deployment
- **Topic**: Mounting secrets securely and limiting access.
- **Files**: `secure-echo.yaml`
- **Test**: Mount secrets as files and verify access restrictions.