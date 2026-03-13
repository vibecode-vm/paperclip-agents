# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**IRON LAW: NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.**

**4-Phase Process for K8s Failures:**
1. **Root Cause Investigation:**
```bash
kubectl describe pod <name> -n <ns>  # Events section shows WHY
kubectl logs <pod> --tail=50 -n <ns>  # App logs
kubectl logs <pod> --previous -n <ns>  # Previous crash logs
kubectl get events --sort-by=.metadata.creationTimestamp -n <ns>
```
2. **Pattern Analysis**: Compare working pod (staging) vs failing (prod). Check: image tag, env vars, resource limits, node affinity.
3. **Hypothesis Testing**: ONE change — fix image tag? Increase memory? Fix env var? Verify BEFORE applying more changes.
4. **Implementation**: Fix at root cause. Don't just increase `restartPolicy` retries.

**Common K8s Root Causes:**
| Symptom | Likely Cause | Diagnostic |
|---------|-------------|------------|
| `CrashLoopBackOff` | App crashes on start | `kubectl logs --previous` |
| `ImagePullBackOff` | Wrong image/tag/registry auth | `kubectl describe pod` → Events |
| `Pending` | No resources / node selector | `kubectl describe pod` → Events |
| `OOMKilled` | Memory limit too low | Increase `resources.limits.memory` |
| `Readiness probe failed` | App not ready on `/api/health` | Check `initialDelaySeconds` |

### 2. Verification — verification-before-completion (obra/superpowers, 16K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**Before claiming "deployed":**
```bash
kubectl rollout status deployment/<name> -n <ns>  # Shows Ready
kubectl get pods -n <ns> -l app=<name>             # All Running
curl -s https://<host>/api/health | jq .status     # Returns "healthy"
```
Evidence before claims — show ALL three outputs.

---

## Patterns

### Deployment with Health Probes
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Release.Name }}
  namespace: {{ .Values.namespace }}
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: {{ .Release.Name }}
  template:
    metadata:
      labels:
        app: {{ .Release.Name }}
    spec:
      containers:
        - name: {{ .Release.Name }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          ports:
            - containerPort: 3000
          resources:
            requests:
              cpu: {{ .Values.resources.requests.cpu }}
              memory: {{ .Values.resources.requests.memory }}
            limits:
              cpu: {{ .Values.resources.limits.cpu }}
              memory: {{ .Values.resources.limits.memory }}
          readinessProbe:
            httpGet:
              path: /api/health
              port: 3000
            initialDelaySeconds: 10
            periodSeconds: 5
          livenessProbe:
            httpGet:
              path: /api/health
              port: 3000
            initialDelaySeconds: 30
            periodSeconds: 10
          startupProbe:
            httpGet:
              path: /api/health
              port: 3000
            failureThreshold: 30
            periodSeconds: 2
          envFrom:
            - configMapRef:
                name: {{ .Release.Name }}-config
            - secretRef:
                name: {{ .Release.Name }}-secrets
```

### Ingress with TLS
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: {{ .Release.Name }}-ingress
  annotations:
    cert-manager.io/cluster-issuer: letsencrypt-prod
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
spec:
  ingressClassName: nginx
  tls:
    - hosts:
        - {{ .Values.ingress.host }}
      secretName: {{ .Release.Name }}-tls
  rules:
    - host: {{ .Values.ingress.host }}
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: {{ .Release.Name }}
                port:
                  number: 3000
```

### NetworkPolicy (restrict ingress)
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: {{ .Release.Name }}-netpol
spec:
  podSelector:
    matchLabels:
      app: {{ .Release.Name }}
  policyTypes:
    - Ingress
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              name: ingress-nginx
      ports:
        - port: 3000
```

---

## Useful Commands
```bash
# Cluster info
kubectl cluster-info
kubectl get nodes -o wide
kubectl top nodes

# Deployments
kubectl get deployments -n <namespace>
kubectl rollout status deployment/<name>
kubectl rollout undo deployment/<name>

# Pods
kubectl get pods -n <namespace>
kubectl describe pod <name>
kubectl logs <pod> --tail=100 -f

# Helm
helm template <chart-dir> --values values-staging.yaml
helm install <name> <chart-dir> -n <namespace> -f values.yaml
helm upgrade <name> <chart-dir> -n <namespace> -f values.yaml
helm list -A
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack
- Shared rules
