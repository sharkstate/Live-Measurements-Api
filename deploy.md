# Deployment Guide

This AI Body Measurement System can be deployed to various cloud platforms. Here are the deployment instructions:

## 🚀 Quick Deploy Options

### Option 1: DigitalOcean App Platform (Recommended)

1. **Create a DigitalOcean account** at [digitalocean.com](https://digitalocean.com)

2. **Connect your GitHub repository:**
   - Go to DigitalOcean App Platform
   - Click "Create App"
   - Connect your GitHub account
   - Select this repository

3. **Configure the app:**
   - **Build Command:** `pip install -r requirements-deploy.txt`
   - **Run Command:** `gunicorn --bind 0.0.0.0:$PORT app:app`
   - **Environment:** Python 3.11
   - **Port:** 8000

4. **Deploy:**
   - Click "Create Resources"
   - Wait for deployment to complete
   - Your app will be available at the provided URL

### Option 2: Heroku

1. **Install Heroku CLI** from [heroku.com](https://heroku.com)

2. **Login and create app:**
   ```bash
   heroku login
   heroku create your-app-name
   ```

3. **Deploy:**
   ```bash
   git add .
   git commit -m "Deploy to Heroku"
   git push heroku main
   ```

4. **Scale the app:**
   ```bash
   heroku ps:scale web=1
   ```

### Option 3: Railway

1. **Go to [railway.app](https://railway.app)** and sign up

2. **Connect GitHub repository**

3. **Deploy:**
   - Railway will automatically detect the Python app
   - Use `requirements-deploy.txt` for dependencies
   - Set start command: `gunicorn --bind 0.0.0.0:$PORT app:app`

### Option 4: Render

1. **Go to [render.com](https://render.com)** and sign up

2. **Create a new Web Service**

3. **Connect your GitHub repository**

4. **Configure:**
   - **Build Command:** `pip install -r requirements-deploy.txt`
   - **Start Command:** `gunicorn --bind 0.0.0.0:$PORT app:app`
   - **Environment:** Python 3.11

## 🐳 Docker Deployment

### Local Docker Testing

```bash
# Build the image
docker build -t body-measurement-api .

# Run the container
docker run -p 8000:8000 body-measurement-api
```

### Deploy to Cloud with Docker

#### DigitalOcean Droplet
```bash
# Create a droplet and SSH into it
ssh root@your-droplet-ip

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh

# Clone your repository
git clone https://github.com/yourusername/Live-Measurements-Api.git
cd Live-Measurements-Api

# Build and run
docker build -t body-measurement-api .
docker run -d -p 80:8000 --name body-measurement body-measurement-api
```

#### AWS EC2
```bash
# Similar to DigitalOcean, but use AWS EC2 instance
# Make sure to configure security groups to allow HTTP traffic
```

## ⚙️ Environment Variables

For production deployment, you may want to set these environment variables:

```bash
FLASK_ENV=production
PORT=8000
```

## 🔧 Performance Optimization

### For High Traffic:

1. **Use multiple workers:**
   ```bash
   gunicorn --bind 0.0.0.0:$PORT --workers 4 app:app
   ```

2. **Add Redis for caching** (optional):
   ```bash
   pip install redis
   ```

3. **Use a CDN** for static files (Cloudflare, AWS CloudFront)

## 📊 Monitoring

### Health Check Endpoint

Add this to your app.py for monitoring:

```python
@app.route('/health')
def health_check():
    return jsonify({'status': 'healthy', 'timestamp': datetime.now().isoformat()})
```

### Logging

The app already includes basic logging. For production, consider:

```python
import logging
logging.basicConfig(level=logging.INFO)
```

## 🚨 Important Notes

1. **Memory Requirements:** This app requires at least 2GB RAM due to AI models
2. **Startup Time:** First request may take 30-60 seconds due to model loading
3. **File Size Limits:** Configure your platform to handle large image uploads (10MB+)
4. **HTTPS:** Always use HTTPS in production for security

## 🔒 Security Considerations

1. **Rate Limiting:** Add rate limiting to prevent abuse
2. **File Validation:** The app already validates image files
3. **CORS:** Configure CORS properly for your domain
4. **Environment Variables:** Never commit API keys or secrets

## 📱 Mobile Optimization

The web interface is already mobile-responsive and works well on:
- iOS Safari
- Android Chrome
- Mobile browsers

## 🆘 Troubleshooting

### Common Issues:

1. **Port binding errors:** Make sure to use `$PORT` environment variable
2. **Memory issues:** Increase memory allocation on your platform
3. **Model loading errors:** The app gracefully handles MiDaS model failures
4. **Image upload errors:** Check file size limits and formats

### Support:

- Check the logs in your platform's dashboard
- Test locally first with `python app.py`
- Verify all dependencies are in `requirements-deploy.txt`

## 🎯 Next Steps

After deployment:

1. **Test the API** with your deployed URL
2. **Set up monitoring** and alerts
3. **Configure custom domain** (optional)
4. **Add SSL certificate** (usually automatic on most platforms)
5. **Set up CI/CD** for automatic deployments

Your AI Body Measurement System is now ready for production! 🚀
