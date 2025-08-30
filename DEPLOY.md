# Deploy Guide for CapRover

This guide explains how to deploy Docling Serve to CapRover using GitHub Actions.

## Prerequisites

1. **CapRover server** running and accessible
2. **GitHub repository** with this code
3. **GitHub repository secrets** configured

## GitHub Secrets Configuration

In your GitHub repository, go to Settings → Secrets and variables → Actions, and add these secrets:

### Required Secrets

- `CAPROVER_SERVER`: Your CapRover server URL (e.g., `https://captain.yourdomain.com`)
- `CAPROVER_APP_NAME`: Your app name in CapRover (e.g., `docling-serve`)
- `CAPROVER_APP_TOKEN`: CapRover app token (generated from CapRover dashboard)

### Getting the CapRover App Token

1. Log into your CapRover dashboard
2. Go to Apps → Your App → Deployment
3. Click on "Method 3: Deploy from Github/Bitbucket/Gitlab"
4. Copy the token from the webhook URL

## Deployment Files

The following files are configured for CapRover deployment:

- `captain-definition`: CapRover configuration pointing to Dockerfile
- `Dockerfile`: Optimized Docker build for CapRover (listens on port 80)
- `.env.production`: Production environment variables
- `.github/workflows/caprover-deploy.yml`: GitHub Actions workflow

## Manual Deployment

If you prefer to deploy manually:

1. **Create CapRover App**:
   ```bash
   # In CapRover dashboard, create a new app named "docling-serve"
   ```

2. **Configure Environment Variables**:
   ```
   DOCLING_SERVE_ENABLE_UI=1
   UVICORN_HOST=0.0.0.0
   UVICORN_PORT=80
   UVICORN_WORKERS=1
   ```

3. **Deploy using Git**:
   - Go to your app's Deployment tab
   - Connect your GitHub repository
   - Set branch to `main`
   - Click Deploy

## Automatic Deployment

The GitHub Actions workflow will automatically deploy when you:

- Push to `main` branch
- Push to `deploy` branch
- Manually trigger the workflow

## Application URLs

After deployment, your app will be available at:

- **Main App**: `https://yourapp.yourdomain.com`
- **API Docs**: `https://yourapp.yourdomain.com/docs`
- **UI Interface**: `https://yourapp.yourdomain.com/ui`

## Resource Requirements

Docling Serve requires significant resources due to AI models:

- **RAM**: Minimum 4GB, recommended 8GB+
- **CPU**: 2+ cores recommended
- **Storage**: 10GB+ for models and temporary files

## Troubleshooting

### Common Issues

1. **Out of Memory**: Increase server RAM or reduce `UVICORN_WORKERS`
2. **Slow Startup**: First startup downloads AI models (can take 5-10 minutes)
3. **Build Timeout**: Increase build timeout in CapRover settings

### Logs

Check application logs in CapRover dashboard:
- Apps → Your App → App Logs

### Health Check

Test if the app is running:
```bash
curl https://yourapp.yourdomain.com/docs
```

## Production Optimizations

For production deployments, consider:

1. **Pre-built Images**: Use the official images from GHCR/Quay
2. **Persistent Storage**: Mount volumes for model cache
3. **Load Balancing**: Multiple instances with shared storage
4. **Monitoring**: Set up health checks and monitoring

## Model Cache

The app downloads AI models on first startup. To speed up deployments:

1. **Use Persistent Volume**: Mount `/opt/app-root/src/.cache/docling/models`
2. **Pre-download Models**: Use init containers to download models
3. **Shared Storage**: Use network storage for multiple instances