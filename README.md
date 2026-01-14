# GitOps Monorepo for Infrastructure as Code

This repository contains Crossplane claims for provisioning AWS infrastructure via GitOps with ArgoCD.

## Structure

```
gitops/
├── environments/
│   ├── dev/          # Development environment
│   ├── staging/      # Staging environment
│   └── prod/         # Production environment
└── docs/
    └── CONTRIBUTING.md
```

Each environment contains:
- `storage/` - S3 buckets and storage resources
- `network/` - VPCs, subnets, and networking
- `database/` - RDS databases and data stores

## Workflow

1. **Create Resource**: Use Backstage UI to create infrastructure
2. **PR Created**: Backstage creates a PR with the Crossplane claim
3. **Review & Merge**: Team reviews and merges the PR
4. **Auto-Deploy**: ArgoCD detects changes and applies claims
5. **Provision**: Crossplane provisions AWS resources

## Adding Resources

Resources are added via Backstage templates. Each resource creates a PR to this repo.

### Manual Addition

If needed, you can manually add resources:

1. Create a YAML file in the appropriate environment/category folder
2. Commit and push to a new branch
3. Create a PR for review
4. Merge to trigger ArgoCD sync

## ArgoCD Applications

Each environment is monitored by a separate ArgoCD Application:
- `infrastructure-dev` → `environments/dev/`
- `infrastructure-staging` → `environments/staging/`
- `infrastructure-prod` → `environments/prod/`

## Documentation

- [Contributing Guide](docs/CONTRIBUTING.md)
- [Backstage Templates](https://github.com/manifest-crossplane-poc/jenkins-self-lab/tree/main/crossplane-backstage/3-backstage/templates)
