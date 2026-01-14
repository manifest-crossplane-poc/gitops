# Contributing to GitOps Infrastructure

## How to Add Infrastructure Resources

### Via Backstage (Recommended)

1. Navigate to Backstage UI: http://localhost:7007
2. Click **Create** → Choose template (S3 Bucket, VPC, RDS, etc.)
3. Fill in the configuration form
4. Click **Create** - this will create a PR in this repo
5. Review the PR and merge
6. ArgoCD will automatically sync and provision the resource

### Manual Addition

If you need to add resources manually:

1. **Create a new branch**
   ```bash
   git checkout -b add-<resource-type>-<resource-name>
   ```

2. **Add your Crossplane claim YAML**
   
   Place in the appropriate folder:
   - S3 Buckets: `environments/<env>/storage/`
   - VPCs: `environments/<env>/network/`
   - RDS: `environments/<env>/database/`

3. **Commit and push**
   ```bash
   git add .
   git commit -m "Add <resource-type>: <resource-name>"
   git push origin add-<resource-type>-<resource-name>
   ```

4. **Create PR and wait for review**

5. **Merge** - ArgoCD will auto-sync

## File Naming Convention

Use descriptive names:
- S3: `s3-<bucket-name>.yaml`
- VPC: `vpc-<network-name>.yaml`
- RDS: `rds-<database-name>.yaml`

## Resource Labels

All resources should include:
```yaml
metadata:
  labels:
    app.kubernetes.io/managed-by: backstage
    environment: dev|staging|prod
    resource-type: s3|vpc|rds
```

## Testing

After merging:
1. Check ArgoCD: `argocd app get infrastructure-<env>`
2. Check Crossplane: `kubectl get <resource-type>`
3. Verify in AWS Console

## Troubleshooting

### PR not creating ArgoCD sync
- Check ArgoCD Application is healthy
- Verify file is in correct path
- Check ArgoCD logs: `kubectl logs -n argocd deployment/argocd-application-controller`

### Crossplane not provisioning
- Check claim status: `kubectl describe <resource-type> <name>`
- Check provider credentials
- Check Crossplane logs: `kubectl logs -n crossplane-system deployment/crossplane`
