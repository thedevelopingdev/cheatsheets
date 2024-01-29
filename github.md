# Github

## Personal access tokens
https://stackoverflow.com/questions/72371417/how-to-git-clone-via-https-with-personal-access-token-in-private-project-gitlab

## Github Actions

### Guides
- https://docs.github.com/en/actions/learn-github-actions/contexts#github-context
- https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#workflow_dispatch

### Syntax
```yaml

# Using secrets
#   source: https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions
#   source: https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idstepsenv
env:
  SERVER: ${{ secrets.SERVER_IP }}

# Create short git commit SHA environment variable
steps: 
  - run: |
      echo "${GITHUB_SHA::7}" >> $GITHUB_ENV

# Set up containerd image store (for multi-arch images) in GHA
# source: https://github.com/docker/setup-buildx-action/issues/257
```

### List of essential published Actions
- Useful GHA for debugging workflows: https://github.com/marketplace/actions/debugging-with-tmate
- Git
    - Checkout repo: https://github.com/actions/checkout
- Docker
    - Set up Docker `buildx`: https://github.com/docker/setup-buildx-action

