# Frontend Configuration

## API Endpoint Configuration

The frontend is configured to send form submissions to the Flask backend API.

### Location in Code

In `index.html`, around line 937:

```javascript
const CONFIG = {
    // Backend API URL for form submission
    apiUrl: 'http://localhost:5001/api/guests',
    
    // ... other config
};
```

### Environment-Specific URLs

**Local Development:**
```javascript
apiUrl: 'http://localhost:5001/api/guests'
```

**Production (Railway):**
```javascript
apiUrl: 'https://your-app-name.railway.app/api/guests'
```

## How It Works

1. **Form Submission**: User fills out the RSVP form
2. **Data Collection**: JavaScript collects form data (nombre, apellidos, etc.)
3. **API Request**: Sends JSON POST request to `CONFIG.apiUrl`
4. **Backend Processing**: 
   - Validates data
   - Checks for duplicates (case/accent insensitive)
   - Stores in PostgreSQL
   - Syncs to Google Sheets (if configured)
5. **Response Handling**:
   - Success (201): Shows success message
   - Error (409): Shows duplicate error
   - Error (400): Shows validation error

## Request Format

```javascript
{
  "nombre": "Juan",
  "apellidos": "García López",
  "asistencia": "si",
  "acompanado": "si",
  "adultos": 2,
  "ninos": 1,
  "autobus": "ida_y_vuelta",
  "alergias": "Ninguna",
  "comentarios": "¡Muy emocionados!"
}
```

## Response Format

**Success (201):**
```json
{
  "success": true,
  "message": "¡Confirmación recibida con éxito!",
  "guest": {
    "id": 1,
    "nombre": "Juan",
    "apellidos": "García López",
    "created_at": "2024-01-15T10:30:00"
  },
  "synced_to_sheets": true
}
```

**Error (409 - Duplicate):**
```json
{
  "error": "Este nombre ya ha sido registrado. Si necesitas actualizar tu confirmación, por favor contacta con los novios."
}
```

**Error (400 - Validation):**
```json
{
  "error": "Missing required field: apellidos"
}
```

## Testing Locally

1. **Start the backend:**
   ```bash
   cd backend
   ./dev.sh
   ```

2. **Open the frontend:**
   - Open `index.html` in your browser
   - Or use a local server:
     ```bash
     python -m http.server 8000
     # Visit http://localhost:8000
     ```

3. **Submit the form** and check:
   - Browser console for logs
   - Backend terminal for API logs
   - Database for stored data

## Deployment Notes

When deploying to production:

1. **Update apiUrl** in `index.html`:
   ```javascript
   apiUrl: 'https://your-railway-app.railway.app/api/guests'
   ```

2. **Deploy backend to Railway** first

3. **Get the Railway URL** from your deployment

4. **Update frontend** with the production URL

5. **Deploy frontend** (GitHub Pages, Netlify, etc.)

## CORS

The backend has CORS enabled for all origins. In production, you may want to restrict this to your frontend domain only.

In `backend/app.py`:
```python
# Current (allows all origins)
CORS(app)

# Production (restrict to your domain)
CORS(app, origins=["https://yourdomain.com"])
```
