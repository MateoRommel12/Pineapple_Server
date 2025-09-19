# Deploying PineappleServer to Render

## Prerequisites
- GitHub repository with your code
- Render account (free tier available)

## Model Files
- Total size: ~47MB (within Render limits)
- Files are included in the repository and will be deployed

## Deployment Steps

### 1. Push to GitHub
```bash
git add .
git commit -m "Prepare for Render deployment"
git push origin main
```

### 2. Create Render Service
1. Go to [Render Dashboard](https://dashboard.render.com/)
2. Click "New +" → "Web Service"
3. Connect your GitHub repository
4. Configure the service:
   - **Name**: `pineapple-server`
   - **Environment**: Python 3
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `uvicorn app:app --host 0.0.0.0 --port $PORT`

### 3. Environment Variables (Optional)
The app has sensible defaults, but you can override:
- `MODEL_PATH`: Path to sweetness classifier model
- `DETECTOR_PATH`: Path to pineapple detector model  
- `CLASS_ORDER`: Order of sweetness classes (High,Low,Medium)

### 4. Deploy
- Click "Create Web Service"
- Render will automatically build and deploy your app
- Deployment typically takes 5-10 minutes

## Service URLs
After deployment, your service will be available at:
- `https://your-service-name.onrender.com/health` - Health check
- `https://your-service-name.onrender.com/predict` - Main prediction endpoint
- `https://your-service-name.onrender.com/history` - Prediction history

## Important Notes

### Free Tier Limitations
- Service sleeps after 15 minutes of inactivity
- First request after sleep may take 30+ seconds (cold start)
- 750 hours/month free (then service stops)

### Cold Start Performance
- Large AI models take time to load
- Consider upgrading to paid tier for production use
- Models are cached after first load

### Database
- Uses SQLite (file-based)
- Data persists between deployments
- For production, consider PostgreSQL add-on

### File Storage
- Uploaded images stored in `/uploads` directory
- Files persist between deployments
- For production, consider external object storage

## Troubleshooting

### Build Failures
- Check `requirements.txt` for incompatible versions
- TensorFlow CPU version is optimized for deployment
- Models must be committed to git repository

### Memory Issues
- Free tier has 512MB RAM limit
- Large models may cause memory pressure
- Consider model optimization for production

### Model Loading Errors
- Ensure model files are in correct paths
- Check TensorFlow/Keras version compatibility
- Verify model files aren't corrupted

## Testing Deployment
1. Wait for deployment to complete
2. Test health endpoint: `curl https://your-app.onrender.com/health`
3. Upload test image to `/predict` endpoint
4. Monitor logs in Render dashboard

## Performance Optimization
- Use smaller model variants if needed
- Implement model caching strategies
- Consider upgrading to paid tier for better performance
- Add monitoring and alerting

## Deployment Notes
- Updated to Python 3.10.12 for TensorFlow 2.13 compatibility
- Last updated: 2024-12-19
