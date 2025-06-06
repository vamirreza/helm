# MyApp Helm Chart

A comprehensive Helm chart for deploying applications to Kubernetes with advanced features including init containers, sidecar containers, Gateway API support, and complete observability.

## Features

- **🚀 Main Application Deployment** - Configurable containerized application
- **🔧 Init Containers** - Pre-deployment initialization (database readiness, storage setup)
- **📊 Sidecar Containers** - Auxiliary services (logging, monitoring, proxies)
- **🗺️ Gateway API** - Modern ingress with advanced traffic management
- **🔒 Security** - Secrets management and service account configuration
- **📝 Configuration** - ConfigMap for application settings
- **📈 Auto-scaling** - Horizontal Pod Autoscaler support
- **🌐 Ingress** - Traditional ingress controller support
- **💾 Persistent Storage** - Volume and volume mount management

## Prerequisites

- Kubernetes 1.19+
- Helm 3.2.0+
- Gateway API CRDs installed (for Gateway/HTTPRoute features)

## Installation

### Add Helm Repository (if publishing to a repo)
```bash
helm repo add myapp https://example.com/helm-charts
helm repo update
```

### Install the Chart
```bash
# Install with default values
helm install myapp ./myapp

# Install with custom values
helm install myapp ./myapp -f values-production.yaml

# Install in specific namespace
helm install myapp ./myapp -n production --create-namespace
```

## Configuration

### Key Configuration Sections

#### Main Application
```yaml
image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: "latest"

replicaCount: 1
```

#### Init Containers
```yaml
initContainers:
  enabled: true
  containers:
    - name: init-db
      image: busybox:1.35
      command: ['sh', '-c', 'until nslookup mydb; do echo waiting for mydb; sleep 2; done;']
```

#### Sidecar Containers
```yaml
sidecarContainers:
  enabled: true
  containers:
    - name: logging-sidecar
      image: fluentd:v1.16-1
      resources:
        requests:
          memory: "64Mi"
          cpu: "50m"
```

#### Gateway API
```yaml
gateway:
  enabled: true
  gatewayClassName: istio
  listeners:
    - name: web
      hostname: "*.example.com"
      port: 80
      protocol: HTTP

httpRoute:
  enabled: true
  hostnames:
    - "myapp.example.com"
  rules:
    - matches:
        - path:
            type: PathPrefix
            value: "/"
```

### Complete Values Reference

| Parameter | Description | Default |
|-----------|-------------|---------|
| `replicaCount` | Number of replicas | `1` |
| `image.repository` | Container image repository | `nginx` |
| `image.tag` | Container image tag | `""` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `initContainers.enabled` | Enable init containers | `true` |
| `sidecarContainers.enabled` | Enable sidecar containers | `true` |
| `configMap.enabled` | Enable ConfigMap | `true` |
| `secret.enabled` | Enable Secret | `true` |
| `gateway.enabled` | Enable Gateway API | `true` |
| `httpRoute.enabled` | Enable HTTPRoute | `true` |
| `ingress.enabled` | Enable traditional Ingress | `false` |
| `service.type` | Service type | `ClusterIP` |
| `service.port` | Service port | `80` |

## Usage Examples

### Basic Deployment
```bash
helm install myapp ./myapp
```

### Production Deployment with Custom Values
```bash
helm install myapp ./myapp \
  --set image.repository=myapp/production \
  --set image.tag=v2.1.0 \
  --set replicaCount=3 \
  --set gateway.enabled=true \
  --set httpRoute.hostnames={myapp.prod.example.com}
```

### Development with Init Container Disabled
```bash
helm install myapp-dev ./myapp \
  --set initContainers.enabled=false \
  --set gateway.enabled=false \
  --set ingress.enabled=true
```

## Monitoring and Observability

The chart includes a monitoring sidecar container that can be configured for:
- Prometheus metrics collection
- Custom monitoring solutions
- Health check endpoints

## Security Considerations

- Secrets are base64 encoded and stored in Kubernetes secrets
- Service accounts with minimal required permissions
- Security contexts configured for containers
- TLS support through Gateway API

## Troubleshooting

### Check Helm Release
```bash
helm list
helm status myapp
```

### Debug Template Rendering
```bash
helm template myapp ./myapp --debug
```

### Validate Chart
```bash
helm lint ./myapp
```

### Test Deployment
```bash
helm test myapp
```

## Development

### Local Development
```bash
# Validate templates
helm template myapp ./myapp

# Lint chart
helm lint ./myapp

# Package chart
helm package ./myapp
```

### Contributing
1. Fork the repository
2. Create a feature branch
3. Make changes
4. Test thoroughly
5. Submit a pull request

## License

This Helm chart is licensed under the MIT License.

## Support

For issues and questions:
- Create an issue in the repository
- Check existing documentation
- Review Helm and Kubernetes documentation

## Changelog

### v0.1.0
- Initial release
- Basic deployment with init and sidecar containers
- Gateway API support
- ConfigMap and Secret management
- Complete observability stack
