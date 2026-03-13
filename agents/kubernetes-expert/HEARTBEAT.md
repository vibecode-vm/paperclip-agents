# HEARTBEAT.md — Kubernetes Expert Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — mandated stack.
- Read `../../MEMORY.md` — known K8s patterns.
- Check kubectl access: `kubectl cluster-info`
- Check existing deployments: `kubectl get deployments -A`
- Check Helm releases: `helm list -A`

## 4. Read System State (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — deployed services.
2. Check cluster health: `kubectl get nodes` + `kubectl top nodes`
3. Check existing K8s manifests: `ls k8s/ helm/`
4. Check pod status: `kubectl get pods --all-namespaces`

## 5. Helm Chart Creation

### Chart Structure
```
helm/[app-name]/
├── Chart.yaml          ← Chart metadata
├── values.yaml         ← Default values
├── values-staging.yaml ← Staging overrides
├── values-prod.yaml    ← Production overrides
└── templates/
    ├── deployment.yaml
    ├── service.yaml
    ├── ingress.yaml
    ├── configmap.yaml
    ├── hpa.yaml
    └── networkpolicy.yaml
```

### Pre-Deploy Checklist
- [ ] `helm template` renders without errors
- [ ] Image version pinned (no `latest`)
- [ ] Resource requests + limits set
- [ ] Health probes configured
- [ ] Secrets referenced from K8s Secrets (not hardcoded)
- [ ] Network policies defined
- [ ] HPA configured with appropriate thresholds

## 6. Health Probes Checklist
- [ ] **Readiness Probe**: `/api/health` returns 200 → pod receives traffic
- [ ] **Liveness Probe**: `/api/health` returns 200 → pod stays alive
- [ ] **Startup Probe**: grace period for slow-starting apps
- [ ] Initial delay, period, timeout, failure threshold configured

## 7. Resource Management
```yaml
resources:
  requests:       # Guaranteed allocation
    cpu: "250m"
    memory: "256Mi"
  limits:         # Maximum allowed
    cpu: "500m"
    memory: "512Mi"
```
- [ ] Requests set (scheduling guarantee)
- [ ] Limits set (OOM protection)
- [ ] Namespace ResourceQuota defined

## 8. Auto-Scaling (HPA)
```yaml
spec:
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```

## 9. Pre-Delivery Checklist
- [ ] Helm chart templates render cleanly
- [ ] Pods start and pass all probes
- [ ] Service/Ingress accessible
- [ ] TLS configured (cert-manager)
- [ ] Resource limits enforced
- [ ] HPA responds to load
- [ ] Network policies active
- [ ] SYSTEM_STATE.md updated

## 10. Exit
- Comment on in_progress work.
- Document cluster changes in SYSTEM_STATE.md.

## Rules
- NEVER use `latest` tag in production manifests.
- ALWAYS set resource requests AND limits.
- ALWAYS configure health probes.
- ALWAYS test with `helm template` before `helm install`.
- NEVER expose services without TLS in production.
- Namespace isolation is mandatory.
