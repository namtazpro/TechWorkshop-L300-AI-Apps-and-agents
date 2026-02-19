# GitHub Actions Workflows

## Deploy to Azure Container Registry (deploy-to-acr.yml)

This workflow builds and pushes a Docker image from the `src/` folder to Azure Container Registry whenever code is pushed to the `main` branch.

### Required GitHub Secrets

Before this workflow can run successfully, you need to configure the following secrets in your GitHub repository:

1. **ACR_REGISTRY**: Your Azure Container Registry URL (e.g., `myregistry.azurecr.io`)
2. **ACR_USERNAME**: Azure Container Registry username (can be the registry name itself)
3. **ACR_PASSWORD**: Azure Container Registry password or access token
4. **ENV**: The complete contents of your `.env` file (will be copied into the container during build)

### How to Add Secrets

1. Go to your repository on GitHub
2. Click on **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add each of the secrets listed above

### What the Workflow Does

1. Checks out the code from the repository
2. Authenticates with Azure Container Registry using the provided credentials
3. Builds a Docker image using the Dockerfile in the `src/` folder
4. Passes the `ENV` secret as a build argument to create the `.env` file inside the container
5. Pushes the image to ACR with two tags:
   - `latest`: Always points to the most recent build
   - `<commit-sha>`: Specific version tied to the git commit

### Security Notes

- The `.env` file is **never** committed to the repository (excluded via `.gitignore`)
- Environment variables are passed securely through GitHub Secrets during the Docker build process
- The `.env` file is created inside the Docker image at build time, not stored in the repository
