# Railway Deployment - Quick Fix

## Problem
Railway is deploying the entire repository (including `index.html`) instead of just the backend API.

## Solution

### Option 1: Railway Dashboard (Easiest)

1. Go to your Railway project
2. Click on your service
3. Go to **Settings** tab
4. Find **Root Directory** setting
5. Set it to: `backend`
6. Click **Save**
7. Redeploy

### Option 2: Configuration Files (Automatic)

The repository now includes configuration files that tell Railway to use the backend directory:

**`railway.toml`** (at repository root):
```toml
[build]
builder = "NIXPACKS"

[deploy]
startCommand = "cd backend && gunicorn app:app"
restartPolicyType = "ON_FAILURE"
restartPolicyMaxRetries = 10
```

**`nixpacks.toml`** (at repository root):
```toml
[phases.setup]
nixPkgs = ["python311"]

[phases.install]
cmds = ["cd backend && pip install -r requirements.txt"]

[phases.build]
cmds = ["cd backend"]

[start]
cmd = "cd backend && gunicorn app:app --bind 0.0.0.0:$PORT"
```

**To use these:**
1. Commit and push these files to your repository
2. Railway will automatically detect them on next deploy
3. Trigger a redeploy in Railway

## Verify It's Working

After deployment, test these endpoints:

### Health Check
```bash
curl https://your-app.railway.app/health
```

Should return:
```json
{"status": "healthy"}
```

### Create Guest (Test)
```bash
curl -X POST https://your-app.railway.app/api/guests \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "Test",
    "apellidos": "Railway",
    "asistencia": "si",
    "acompanado": "no",
    "autobus": "no"
  }'
```

Should return:
```json
{
  "success": true,
  "message": "¡Confirmación recibida con éxito!",
  "guest": {...},
  "synced_to_sheets": true/false
}
```

## Common Issues

### Still serving HTML?
- Make sure Root Directory is set to `backend` in Railway settings
- Check Railway build logs - should show Python/pip installation
- Verify `Procfile` exists in backend folder

### Port errors?
- Railway automatically sets `$PORT` environment variable
- Make sure your app uses: `port = int(os.environ.get('PORT', 5000))`

### Database connection errors?
- Verify PostgreSQL database is added to Railway project
- Check `DATABASE_URL` is set in environment variables
- Railway automatically connects services in the same project

## Environment Variables Needed

In Railway project settings:

| Variable | Value | Notes |
|----------|-------|-------|
| `DATABASE_URL` | Auto-set by Railway | Added when you create PostgreSQL database |
| `GOOGLE_APPS_SCRIPT_URL` | Your script URL | Optional - for Google Sheets sync |
| `PORT` | Auto-set by Railway | Railway manages this automatically |

## Frontend Configuration

Once backend is deployed, update your frontend:

**`index.html`** (line ~935):
```javascript
const CONFIG = {
    // Change this to your Railway backend URL
    apiUrl: 'https://your-app.railway.app/api/guests',
    weddingDate: '25 de Abril de 2026',
    // ...
};
```

## Project Structure

```
wedding_web/
├── railway.toml          # Railway configuration (root)
├── nixpacks.toml         # Nixpacks configuration (root)
├── index.html            # Frontend (not deployed to Railway)
├── backend/              # This is what Railway should deploy
│   ├── app.py
│   ├── requirements.txt
│   ├── Procfile
│   ├── runtime.txt
│   └── ...
```

Railway should **only** deploy the `backend/` folder contents.
