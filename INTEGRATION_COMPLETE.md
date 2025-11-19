# ✅ Frontend-Backend Integration Complete

## What Changed

### ❌ **Removed: Google Apps Script**
- Old: Form sent data to Google Sheets directly via Apps Script
- URL: `https://script.google.com/macros/s/.../exec`

### ✅ **New: Flask Backend API**
- New: Form sends data to Flask API
- URL: `http://localhost:5001/api/guests` (local)
- Backend handles: validation, PostgreSQL storage, Google Sheets sync

## Changes Made

### 1. Frontend (`index.html`)

**Before:**
```javascript
scriptUrl: 'https://script.google.com/macros/s/.../exec'

const params = new URLSearchParams();
params.append('nombre', data.nombre);
// ... more params

fetch(SCRIPT_URL, {
    method: 'POST',
    body: params
});
```

**After:**
```javascript
apiUrl: 'http://localhost:5001/api/guests'

fetch(CONFIG.apiUrl, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
    },
    body: JSON.stringify(data)
});
```

### 2. Data Flow

```
┌─────────────┐
│   Browser   │
│  (Form)     │
└──────┬──────┘
       │ JSON POST
       ▼
┌─────────────┐
│  Flask API  │
│  Port 5001  │
└──────┬──────┘
       │
       ├─────────────┐
       │             │
       ▼             ▼
┌──────────┐  ┌────────────┐
│PostgreSQL│  │Google Sheet│
│ (Docker) │  │ (optional) │
└──────────┘  └────────────┘
```

## Testing

### 1. Test with curl
```bash
curl -X POST http://localhost:5001/api/guests \
  -H "Content-Type: application/json" \
  -d '{
    "nombre": "Juan",
    "apellidos": "García",
    "asistencia": "si",
    "acompanado": "no",
    "autobus": "no"
  }'
```

### 2. Test with simple form
Open `test_form.html` in your browser:
```bash
open test_form.html
```

### 3. Test with full website
Open `index.html` in your browser:
```bash
open index.html
```

## Features Working

✅ **Form Submission**: Sends JSON to Flask API
✅ **Validation**: Backend validates required fields
✅ **Duplicate Detection**: Case/accent insensitive (José = Jose)
✅ **PostgreSQL Storage**: Data persisted in database
✅ **Error Handling**: Shows user-friendly error messages
✅ **Success Message**: Shows confirmation on success
✅ **Google Sheets**: Backend syncs automatically (if configured)

## Configuration

### Local Development
In `index.html` line ~937:
```javascript
apiUrl: 'http://localhost:5001/api/guests'
```

### Production (Railway)
Update to your Railway URL:
```javascript
apiUrl: 'https://your-app-name.railway.app/api/guests'
```

## Next Steps

1. ✅ Backend is running locally
2. ✅ Frontend is configured to use backend
3. 🔜 Test the full form in `index.html`
4. 🔜 Configure Google Sheets credentials (optional)
5. 🔜 Deploy backend to Railway
6. 🔜 Update frontend with production URL
7. 🔜 Deploy frontend

## Verification Checklist

- [ ] Backend running: `curl http://localhost:5001/health`
- [ ] Test form works: Open `test_form.html`
- [ ] Full form works: Open `index.html`
- [ ] Duplicate detection works: Try same name twice
- [ ] Data in database: `docker exec -it wedding-postgres psql -U wedding_user -d wedding_db -c "SELECT * FROM guests;"`
- [ ] Error messages display correctly
- [ ] Success message displays correctly

## Troubleshooting

### CORS Error
Make sure backend is running and CORS is enabled (it is by default).

### Connection Refused
Backend not running. Start it:
```bash
cd backend
./dev.sh
```

### 404 Not Found
Check the URL in `CONFIG.apiUrl` matches your backend URL.

### Duplicate Error Not Working
The normalization is working! Try:
- "José García" then "Jose Garcia" (should be blocked)
- "MARÍA LÓPEZ" then "maria lopez" (should be blocked)
