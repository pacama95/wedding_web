# 💍 Wedding Website - Pablo & Paula

An elegant wedding website, fully responsive and easy to customize.

## 🌟 Features

- ✨ Modern and elegant design with smooth animations
- 📱 Fully responsive (adapts to mobile, tablet, and desktop)
- 🖼️ Section for the couple's photo
- 📍 Detailed ceremony and reception information
- 🗺️ Embedded Google Maps for both locations
- ⏰ Full event schedule (dynamically configurable)
- 🚌 Transportation information (bus, parking, taxi/Uber)
- ⚙️ Centralized configuration for easy customization
- 📝 RSVP form with:
  - Yes/No attendance confirmation
  - Option to attend with companion(s)
  - Number of adult companions (0-5)
  - Number of child companions (0-5)
  - Bus transport selection (One-way / Return / Round trip / No)
  - Field for food allergies and intolerances
  - Additional comments
- 🎨 Elegant color palette in gold and earthy tones
- 🔤 Premium fonts (Playfair Display and Lato)

## 🚀 How to Use

### Deploy to GitHub Pages

1. Download the `boda-pablo-paula.html` file
2. Rename it to `index.html`
3. Upload the file to your GitHub repository
4. Enable GitHub Pages in the repository settings
5. Your site will be available at: `https://your-username.github.io/repository-name/`

### View locally

Simply open the HTML file in your favorite web browser.

## 🎨 Customization

### 1. Centralized Configuration

All site configuration is in the `CONFIG` object at the beginning of the `<script>` (lines ~668-720). Here you can modify:

**Wedding Date:**
```javascript
weddingDate: 'April 25, 2026'
```

**Photo URL:**
```javascript
photoUrl: 'https://i.imgur.com/YOUR_IMAGE.jpg'
```

**Ceremony Information:**
```javascript
ceremonia: {
    lugar: 'San Miguel Church',
    direccion: '123 Main Street, Madrid',
    hora: '12:00 PM'
}
```

**Reception Information:**
```javascript
celebracion: {
    lugar: 'Los Olivos Estate',
    direccion: 'Toledo Road km 15, Madrid',
    hora: '2:00 PM'
}
```

**Event Schedule:**
```javascript
itinerario: [
    {
        hora: '12:00',
        titulo: 'Ceremony',
        descripcion: 'Welcome and wedding ceremony'
    },
    // Add more events here
]
```

### 2. Names

Find and replace the names in the HTML file:

- **Names**: `Pablo & Paula` (line ~525)

**Note:** The date is now configured in the CONFIG object (see point 1).

### 3. Couple Photo

**Note:** The photo is now configured in the CONFIG object (see point 1). You only need to update the URL.

**Options to get the photo URL:**
- **Option 1 (Recommended)**: Upload to [Imgur](https://imgur.com/upload) and copy the image URL
- **Option 2**: Use GitHub Issues – drag the photo into an issue and copy the generated URL
- **Option 3**: Upload the photo to the repository folder and use: `./photo-name.jpg`

### 4. Location & Schedule Information

**Everything is now configured from the CONFIG object** (see point 1). Changes are automatically applied throughout the site.

To add more events to the schedule, just add objects to the array:
```javascript
{
    hora: '20:00',
    titulo: 'Open Bar',
    descripcion: 'Cocktails and fun'
}
```

**Transportation Information:**
```javascript
transporte: {
    autobus: {
        ida: {
            lugar: 'Plaza de Cibeles',
            hora: '10:45',
            descripcion: 'The bus will depart for the ceremony'
        },
        vuelta: {
            lugar: 'Finca La Gaivota',
            hora: '23:30',
            descripcion: 'The bus will return to the departure point'
        }
    },
    aparcamiento: 'Message about limited parking...',
    taxi: 'Recommendations about Uber/Cabify/Taxi...'
}
```

### 5. Google Apps Script URL

In the CONFIG object, update the URL of your script:
```javascript
scriptUrl: 'https://script.google.com/macros/s/YOUR_URL_HERE/exec'
```

### 6. Site Colors

The main colors are defined in CSS variables. Find these values and change them:

- **Primary color**: `#8b6f47` (golden brown)
- **Secondary color**: `#d4af7a` (light gold)
- **Gradients**: `#e8dcc4`, `#d4c4a8`, `#c9b896`

Example to change the primary color to blue:
```css
/* Find #8b6f47 and replace with */
#4a7c8b
```

### 7. Fonts

Current fonts:
- **Headings**: Playfair Display (elegant serif)
- **Body text**: Lato (modern sans-serif)

To change them, edit around line ~6:
```css
@import url('https://fonts.googleapis.com/css2?family=YOUR-FONT&display=swap');
```

Then update the `font-family` references in the CSS.

### 8. RSVP Form

The form currently sends data to Google Sheets using Google Apps Script.

**Current form structure:**
- Full name (required)
- Will you attend the wedding? (Yes/No – required)
- Will you come with companion(s)? (Yes/No – required)
  - If "Yes" is selected, these fields appear:
    - Number of adult companions (0-5)
    - Number of child companions (0-5)
- Do you need the bus? (One-way / Return / Round trip / No – required)
- Allergies and intolerances (optional)
- Additional comments (optional)

**To configure storage:**

See the `google-sheets-integration.md` file for detailed instructions.

**Fields being sent:**
```javascript
{
  nombre: "Guest name",
  asistencia: "si" or "no",
  acompanado: "si" or "no",
  adultos: "0" to "5",
  ninos: "0" to "5",
  autobus: "solo_ida", "solo_vuelta", "ida_y_vuelta" or "no",
  alergias: "free text",
  comentarios: "free text"
}
```

**Other available options:**
- Formspree (email notifications)
- Firebase (real-time database)
- EmailJS (direct email sending)
- Custom backend

See `storage-options.md` for more alternatives.

### 9. Location Maps

Google Maps iframes are already embedded in the ceremony and reception cards. To change them:

1. Go to [Google Maps](https://www.google.com/maps)
2. Search for your location
3. Click **"Share"** > **"Embed a map"**
4. Copy the iframe code
5. Replace the existing iframe in the HTML (lines ~534 for ceremony, ~543 for reception)

**Example:**
```html
<iframe src="https://www.google.com/maps/embed?pb=YOUR_CODE_HERE" 
        width="100%" 
        height="300" 
        style="border:0; border-radius: 8px; margin-top: 20px;" 
        allowfullscreen="" 
        loading="lazy" 
        referrerpolicy="no-referrer-when-downgrade">
</iframe>
```

The maps are fully responsive and adapt to all devices.

## 📋 File Structure

```
boda-pablo-paula.html
│
├── <head>
│   ├── Meta tags
│   ├── Title
│   └── CSS styles
│
├── <header>
│   └── Names and wedding date
│
├── <section> Photo
│   └── Couple photo
│
├── <section> Event Information
│   ├── Ceremony card
│   └── Reception card
│
├── <section> Schedule
│   └── Timeline of events
│
├── <section> RSVP Form
│   └── Attendance confirmation
│
└── <footer>
    └── Farewell message
```

## 🎯 Customization Tips

### For an elegant/formal wedding:
- Keep the current colors (golds and browns)
- Use professional black-and-white photos
- Keep the text concise and elegant

### For a casual/rustic wedding:
- Change the colors to earthy greens and browns: `#6b8e6b`, `#8b7355`
- Add wooden textures in the backgrounds
- Use a more informal tone in the text

### For a modern/minimalist wedding:
- Simplify to black and white: `#000000`, `#ffffff`, `#808080`
- Reduce animations
- Use sans-serif fonts like Montserrat

## 🛠️ Technologies Used

- HTML5
- CSS3 (with animations and gradients)
- Vanilla JavaScript (no dependencies)
- Google Fonts (Playfair Display and Lato)

## 📱 Compatibility

- ✅ Chrome (all recent versions)
- ✅ Firefox (all recent versions)
- ✅ Safari (iOS and macOS)
- ✅ Edge (all recent versions)
- ✅ Mobile devices (responsive design)

## 📞 Support

If you run into problems or have questions:
1. Review the customization section
2. Check that all changes are inside the correct tags
3. Make sure quotes and parentheses are balanced

## 📝 License

This project is free to use. Feel free to use it and modify it for your wedding.

---

**Congratulations on your wedding!** 🎉💕

If you need additional help or want to add more functionality, feel free to modify the code or look up basic HTML/CSS tutorials.