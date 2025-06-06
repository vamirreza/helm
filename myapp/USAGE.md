# Helm Chart Usage Guide

## Quick Start

### 1. Validate the Chart
```bash
helm lint .
```

### 2. Test Template Rendering
```bash
helm template test-release .
```

### 3. Install for Development
```bash
# Install with development values
helm install myapp-dev . -f values-development.yaml

# Install in specific namespace
helm install myapp-dev . -f values-development.yaml -n development --create-namespace
```

### 4. Install for Production
```bash
# Install with production values
helm install myapp-prod . -f values-production.yaml -n production --create-namespace
```

## Customization Examples

### Enable/Disable Components
```bash
# Disable init containers
helm install myapp . --set initContainers.enabled=false

# Disable Gateway API, use Ingress instead
helm install myapp . --set gateway.enabled=false --set httpRoute.enabled=false --set ingress.enabled=true

# Scale replicas
helm install myapp . --set replicaCount=5
```

### Override Image
```bash
# Use different image
helm install myapp . --set image.repository=myregistry/myapp --set image.tag=v2.0.0
```

### Custom ConfigMap Data
```bash
# Override specific config
helm install myapp . --set-string configMap.data.app\.properties="app.name=customapp\napp.env=staging"
```

## Component Overview

### 🚀 Main Components
- **Deployment**: Application pods with init and sidecar containers
- **Service**: ClusterIP service for internal communication
- **ConfigMap**: Application configuration
- **Secret**: Sensitive data (passwords, API keys, certificates)

### 🌐 Networking
- **Ingress**: Traditional ingress controller support
- **Gateway**: Gateway API for advanced traffic management
- **HTTPRoute**: Gateway API routing rules

### 🔧 Supporting Components
- **ServiceAccount**: RBAC configuration
- **HPA**: Horizontal Pod Autoscaler
- **PersistentVolumeClaim**: Storage (when configured)

### 📊 Observability
- **Init Containers**: Database readiness checks, storage initialization
- **Sidecar Containers**: Logging (Fluentd), monitoring (Prometheus)
- **Health Checks**: Liveness and readiness probes

## Environment-Specific Configurations

### Development (`values-development.yaml`)
- Single replica
- Minimal resources
- Debug logging
- Traditional ingress
- No init containers (faster startup)
- Simple sidecar logging

### Production (`values-production.yaml`)
- Multiple replicas (3+)
- Resource limits and requests
- Gateway API with TLS
- Database and cache init containers
- Full observability stack
- Node affinity and tolerations
- Horizontal Pod Autoscaler

## Upgrade and Rollback

### Upgrade
```bash
# Upgrade with new values
helm upgrade myapp . -f values-production.yaml

# Upgrade with specific image version
helm upgrade myapp . --set image.tag=v1.2.1
```

### Rollback
```bash
# View release history
helm history myapp

# Rollback to previous version
helm rollback myapp

# Rollback to specific revision
helm rollback myapp 2
```

## Troubleshooting

### Check Release Status
```bash
helm status myapp
helm list
```

### Debug Template Issues
```bash
helm template myapp . --debug
helm install myapp . --dry-run --debug
```

### View Generated Manifests
```bash
helm get manifest myapp
```

### Test Deployment
```bash
helm test myapp
```

## VS Code Tasks

Use the following VS Code tasks (Ctrl+Shift+P → "Tasks: Run Task"):

1. **Helm Lint Chart** - Validate chart syntax
2. **Helm Template Render** - Test template rendering
3. **Helm Install Development** - Dry-run development install
4. **Helm Install Production** - Dry-run production install
5. **Helm Package Chart** - Create chart package
6. **Test All Configurations** - Run all tests in sequence

## Security Considerations

- Secrets are base64 encoded (encode before adding to values)
- Use proper RBAC with ServiceAccount
- Configure security contexts for containers
- Use TLS with Gateway API for production
- Implement network policies if needed
- Scan container images for vulnerabilities

## Monitoring and Logging

The chart includes:
- Prometheus sidecar for metrics collection
- Fluentd sidecar for log aggregation
- Health check endpoints
- Resource usage monitoring via HPA

Configure these according to your observability stack.
