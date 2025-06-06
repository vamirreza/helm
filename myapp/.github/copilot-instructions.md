<!-- Use this file to provide workspace-specific custom instructions to Copilot. For more details, visit https://code.visualstudio.com/docs/copilot/copilot-customization#_use-a-githubcopilotinstructionsmd-file -->

# Helm Chart Project Instructions

This is a comprehensive Helm chart project for Kubernetes deployments with the following features:

## Chart Components
- **Deployment**: Supports main application containers, init containers, and sidecar containers
- **Service**: ClusterIP service for internal communication
- **Ingress**: Traditional ingress for HTTP/HTTPS traffic routing
- **ConfigMap**: Application configuration management
- **Secret**: Sensitive data storage (passwords, API keys, TLS certificates)
- **Gateway**: Gateway API for advanced traffic management
- **HTTPRoute**: Gateway API routing rules for HTTP traffic
- **ServiceAccount**: Kubernetes service account with RBAC
- **HPA**: Horizontal Pod Autoscaler for scaling

## Key Features
- **Init Containers**: Pre-deployment initialization tasks (database readiness, storage setup)
- **Sidecar Containers**: Auxiliary containers (logging, monitoring, proxies)
- **Volume Management**: Shared storage between containers
- **Gateway API Support**: Modern Kubernetes ingress alternative
- **Flexible Configuration**: Comprehensive values.yaml for customization

## Development Guidelines
- Follow Helm best practices for template organization
- Use semantic versioning for chart releases
- Implement proper resource limits and requests
- Include health checks (liveness/readiness probes)
- Support multiple environments through values files
- Maintain backward compatibility when possible

## Testing
- Use `helm template` to validate template rendering
- Test with `helm lint` for chart validation
- Use `helm test` for deployment verification
- Validate against different Kubernetes versions
