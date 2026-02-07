# Contributing to Project Wolverine

Thanks for your interest in contributing! Here's how to get started.

## Development Setup

1. Fork and clone the repo
2. Install prerequisites listed in the README
3. Set up your AWS credentials and environment variables
4. Follow the Quick Start guide to deploy locally

## Branch Strategy

- `main` — stable, production-ready code
- `dev` — active development branch
- Feature branches: `feature/<description>`
- Bug fixes: `fix/<description>`

## Pull Request Process

1. Create a feature branch from `dev`
2. Make your changes with clear, descriptive commits
3. Ensure Terraform validates (`terraform validate`)
4. Test Kubernetes manifests (`kubectl apply --dry-run=client`)
5. Update documentation if needed
6. Open a PR against `dev` with a clear description

## Commit Messages

Use clear, descriptive commit messages:

```
feat: add auto-scaling remediation for high CPU alerts
fix: correct Prometheus ServiceMonitor selector labels
docs: update architecture diagram with new monitoring flow
infra: add RDS module with multi-AZ support
```

## Code Style

- **Terraform**: Follow [HashiCorp style conventions](https://developer.hashicorp.com/terraform/language/syntax/style)
- **Python**: Follow PEP 8, use type hints
- **YAML/K8s manifests**: 2-space indentation, clear comments

## Questions?

Open an issue and we'll get back to you.
