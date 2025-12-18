# Railway Deployment Guide

## Quick Fixes for Timeout Issues

The main issue is that large ML packages (TensorFlow, PyTorch, dlib) can cause build timeouts on Railway. Here's what I've optimized:

### Changes Made:

1. **CPU-only PyTorch** - Installed separately using CPU index (saves ~500MB)
2. **opencv-python-headless** - No GUI dependencies (smaller)
3. **tensorflow-cpu** - CPU-only version (if available, otherwise regular tensorflow works)
4. **Dockerfile option** - Pre-installs system dependencies for faster builds

### Deployment Options:

#### Option 1: Use Dockerfile (Recommended)
Railway will auto-detect the Dockerfile and use it. This is often faster because:
- System dependencies are pre-installed
- Better caching
- More control over the build process

#### Option 2: Use Nixpacks (Default)
Railway will use `nixpacks.toml` if present, which installs CPU-only PyTorch first.

#### Option 3: Manual Build Command
If Railway still times out, you can set a custom build command in Railway dashboard:
```bash
pip install --upgrade pip && pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu && pip install -r requirements.txt
```

### Environment Variables to Set in Railway:

1. **PORT** - Railway sets this automatically
2. **FLASK_DEBUG** - Set to `false` for production
3. **RAILWAY_HEALTHCHECK_TIMEOUT_SEC** - Set to `600` (10 minutes) to prevent timeout
4. **GOOGLE_APPLICATION_CREDENTIALS** - Path to Firebase credentials (if using service account)
5. Or upload `firebase-credentials.json` as a secret file

**Important**: Set `RAILWAY_HEALTHCHECK_TIMEOUT_SEC=600` in Railway dashboard to give the build more time!

### If Build Still Times Out:

1. **Increase Railway build timeout** (if on paid plan)
2. **Use Dockerfile** - Often faster than Nixpacks for complex builds
3. **Split dependencies** - Install heavy packages in a separate layer
4. **Consider using Railway's Dockerfile detection** instead of Nixpacks

### Testing Locally:

Before deploying, test the optimized requirements:
```bash
pip install --upgrade pip
pip install torch torchvision --index-url https://download.pytorch.org/whl/cpu
pip install -r requirements.txt
```

### Notes:

- `dlib` compilation can take 5-10 minutes - this is normal
- First build will be slower due to package downloads
- Subsequent builds will be faster due to caching
- The app will work the same - all models download automatically on first run

