# Changelog - Wedding Website

## [2024-11-17] - Configuration Update

### ✨ Added
- **Centralized Configuration Object**: All wedding details now managed through a single `CONFIG` object at the top of the script
- **Dynamic Content Loading**: Event information and itinerary now load dynamically from configuration
- **Better Emoji for Ceremony**: Changed from 💒 to ✨ for a more elegant look
- **Google Maps Integration**: Embedded maps for both ceremony and celebration venues with responsive design
- **Map Styling**: Custom CSS for map containers with shadows and rounded corners

### 🔧 Changed
- **Ceremony Section**: Now uses ✨ emoji instead of 💒
- **Event Information**: Moved from hardcoded HTML to dynamic JavaScript configuration
- **Itinerary**: Now generated dynamically from CONFIG array, making it easy to add/remove events

### 📝 Configuration Structure

All customization now happens in one place:

```javascript
const CONFIG = {
    scriptUrl: 'YOUR_GOOGLE_SCRIPT_URL',
    
    ceremonia: {
        lugar: '[Church/Venue Name]',
        direccion: '[Full Address]',
        hora: '[Time]'
    },
    
    celebracion: {
        lugar: '[Restaurant/Venue Name]',
        direccion: '[Full Address]',
        hora: '[Time]'
    },
    
    itinerario: [
        { hora: '12:00', titulo: 'Ceremonia', descripcion: '...' },
        { hora: '13:30', titulo: 'Cóctel', descripcion: '...' },
        // Add more events as needed
    ]
};
```

### 🎯 Benefits
- ✅ Single source of truth for all wedding details
- ✅ Easy to update without touching HTML
- ✅ Perfect for GitHub Pages deployment
- ✅ No need to search through HTML for placeholders
- ✅ Can easily add/remove itinerary events

### 🐛 Fixed
- Form submission now correctly sends data to Google Sheets
- All columns (Asistencia, Acompañado, Adultos, Niños) now populate correctly
