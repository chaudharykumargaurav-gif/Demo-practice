# GitHub Actions CI/CD Setup

This project uses GitHub Actions to automatically build, test, and push Docker images to Docker Hub.

## Prerequisites

Before the workflow can run successfully, you need to configure Docker Hub credentials as GitHub repository secrets.

### Step 1: Create Docker Hub Access Token

1. Go to [Docker Hub](https://hub.docker.com/)
2. Log in to your account
3. Click on your username in the top right corner
4. Select **Account Settings**
5. Go to **Security** tab
6. Click **New Access Token**
7. Give it a description (e.g., "GitHub Actions")
8. Copy the generated token (you won't be able to see it again)

### Step 2: Add Secrets to GitHub Repository

1. Go to your GitHub repository
2. Click on **Settings** tab
3. In the left sidebar, click **Secrets and variables** → **Actions**
4. Click **New repository secret**
5. Add the following secrets:

   **Secret 1:**
   - Name: `DOCKERHUB_USERNAME`
   - Value: Your Docker Hub username

   **Secret 2:**
   - Name: `DOCKERHUB_TOKEN`
   - Value: The access token you created in Step 1

## Workflow Overview

The workflow (`.github/workflows/docker-publish.yml`) performs the following steps:

1. **Checkout code** - Retrieves the repository code
2. **Set up JDK 17** - Configures Java 17 with Maven caching
3. **Build with Maven** - Compiles the application and creates the JAR file
4. **Run tests** - Executes unit tests
5. **Set up Docker** - Configures QEMU and Buildx for multi-platform builds
6. **Log in to Docker Hub** - Authenticates with Docker Hub (only on push, not PRs)
7. **Build and push Docker image** - Builds and pushes the image with multiple tags
8. **Display digest** - Shows the image digest for verification

## Workflow Triggers

The workflow runs on:
- Push to `main` or `master` branches
- Pull requests to `main` or `master` branches
- Manual trigger via GitHub Actions UI (workflow_dispatch)

## Docker Image Tags

The workflow creates the following tags:
- `latest` - Latest build from the default branch
- `main` or `master` - Branch name
- `main-<sha>` - Branch with commit SHA
- Semantic version tags (if using version tags)

## Docker Image Location

After successful deployment, your image will be available at:
```
docker.io/<your-dockerhub-username>/minimal-springboot:latest
```

## Running the Published Image

To run the published Docker image:

```bash
docker pull <your-dockerhub-username>/minimal-springboot:latest
docker run -p 8080:8080 <your-dockerhub-username>/minimal-springboot:latest
```

Then access the application at: `http://localhost:8080`

## Testing the Workflow

1. Commit and push your changes to GitHub
2. Go to your repository on GitHub
3. Click the **Actions** tab
4. You should see the workflow running
5. Click on the workflow run to see detailed logs

## Troubleshooting

### Workflow fails with authentication error
- Verify your `DOCKERHUB_USERNAME` and `DOCKERHUB_TOKEN` secrets are correctly set
- Ensure the access token hasn't expired

### Build fails
- Check the Maven build logs in the workflow
- Ensure your `pom.xml` is valid and dependencies are accessible

### Docker push fails
- Verify your Docker Hub account has sufficient storage
- Check if the repository name matches your username

## Multi-platform Support

The workflow builds images for:
- `linux/amd64` (Intel/AMD processors)
- `linux/arm64` (ARM processors like Apple Silicon, AWS Graviton)

## Caching

The workflow uses Docker layer caching to speed up builds. The cache is stored in Docker Hub as `buildcache` tag.

## Next Steps

1. Set up the repository secrets as described above
2. Push your code to GitHub
3. Monitor the Actions tab for the first build
4. Once successful, pull and run your image from Docker Hub

