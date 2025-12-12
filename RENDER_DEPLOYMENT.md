# Render Deployment Guide

## Quick Deploy to Render

### Step 1: Create PostgreSQL Database on Render

1. Go to https://dashboard.render.com/
2. Click "New +" → "PostgreSQL"
3. Fill in:
   - **Name**: `asset-management-db`
   - **Database**: `asset_management`
   - **User**: `asset_user`
   - **Region**: Choose closest to you
   - **Plan**: Free
4. Click "Create Database"
5. **IMPORTANT**: Copy the "Internal Database URL" (starts with `postgres://`)

### Step 2: Create Web Service

1. Click "New +" → "Web Service"
2. Connect your GitHub repository: `shanker-18/ktte-Tematic`
3. Fill in:
   - **Name**: `asset-management`
   - **Region**: Same as database
   - **Branch**: `main`
   - **Runtime**: `Node`
   - **Build Command**: `npm install`
   - **Start Command**: `node app.js`
   - **Plan**: Free

### Step 3: Add Environment Variables

In the "Environment" section, add:

1. **DATABASE_URL**
   - Value: Paste the Internal Database URL from Step 1
   
2. **NODE_ENV**
   - Value: `production`
   
3. **SESSION_SECRET**
   - Value: Click "Generate" or use any random string

### Step 4: Deploy

1. Click "Create Web Service"
2. Wait for deployment (2-3 minutes)
3. Once deployed, click the URL (e.g., `https://asset-management.onrender.com`)

### Step 5: Seed Database (Optional)

After first deployment, you can seed the database:

1. In Render dashboard, go to your web service
2. Click "Shell" tab
3. Run:
```bash
node seed.js
```

This will create sample data.

---

## Environment Variables Reference

Your Render service needs these environment variables:

| Variable | Description | Example |
|----------|-------------|---------|
| `DATABASE_URL` | PostgreSQL connection string | `postgres://user:pass@host:5432/db` |
| `NODE_ENV` | Environment mode | `production` |
| `SESSION_SECRET` | Session encryption key | Random string |

---

## Troubleshooting

### Error: "DATABASE CONNECTION FAILED"
- Ensure DATABASE_URL is set correctly in Environment Variables
- Check that PostgreSQL database is created and running
- Verify the Internal Database URL (not External)

### Error: "Cannot find module"
- Check that `npm install` ran successfully in build logs
- Verify package.json is committed to git

### App keeps restarting
- Check logs for specific errors
- Ensure database connection is working
- Verify all environment variables are set

---

## Important Notes

1. **Free Tier Limitations**:
   - Service spins down after 15 minutes of inactivity
   - First request after inactivity may take 30-60 seconds
   - Database limited to 1GB storage

2. **Database URL**:
   - Always use "Internal Database URL" for web services on Render
   - External URL is for connecting from outside Render

3. **Auto-Deploy**:
   - Any push to `main` branch will trigger auto-deployment

---

## Manual Configuration (Alternative)

If you prefer manual setup without render.yaml:

1. Create PostgreSQL database manually
2. Create Web Service manually  
3. Add environment variables in dashboard
4. Deploy

Both methods work the same way!

---

## Post-Deployment

Your app will be live at: `https://your-service-name.onrender.com`

Default features:
- Automatic HTTPS
- Auto-deploy on git push
- Health checks
- Logs and monitoring

---

## Cost

- Free tier: $0/month
- PostgreSQL: Free (1GB limit)
- Web Service: Free (750 hours/month)

Perfect for academic projects and demos!
