# Integración con Google Sheets

## Pasos para configurar

### 1. Crear la hoja de cálculo

1. Ve a [Google Sheets](https://sheets.google.com)
2. Crea una nueva hoja llamada "Confirmaciones Boda"
3. En la primera fila, añade estos encabezados:
   - `Fecha` | `Nombre` | `Email` | `Teléfono` | `Asistencia` | `Acompañantes` | `Alergias` | `Comentarios`

### 2. Crear el Google Apps Script

1. En tu hoja de cálculo, ve a **Extensiones** > **Apps Script**
2. Borra el código que aparece y pega este:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    // Añadir fila con los datos
    sheet.appendRow([
      new Date(),
      data.nombre,
      data.email,
      data.telefono,
      data.asistencia,
      data.acompanantes,
      data.alergias,
      data.comentarios
    ]);
    
    return ContentService
      .createTextOutput(JSON.stringify({success: true}))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch(error) {
    return ContentService
      .createTextOutput(JSON.stringify({success: false, error: error.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

3. Click en **Guardar** (icono de disquete)
4. Click en **Implementar** > **Nueva implementación**
5. Selecciona **Aplicación web**
6. Configura:
   - **Descripción**: "API Confirmaciones Boda"
   - **Ejecutar como**: Tu cuenta
   - **Quién tiene acceso**: Cualquier persona
7. Click en **Implementar**
8. **COPIA LA URL** que te da (algo como: `https://script.google.com/macros/s/ABC123.../exec`)
9. Click en **Autorizar acceso** y acepta los permisos

### 3. Actualizar el código HTML

Reemplaza el script del formulario (líneas ~404-424) con este código:

```javascript
// Reemplaza 'TU_URL_AQUI' con la URL que copiaste del paso anterior
const SCRIPT_URL = 'https://script.google.com/macros/s/ABC123.../exec';

document.getElementById('rsvpForm').addEventListener('submit', async function(e) {
    e.preventDefault();
    
    // Deshabilitar el botón mientras se envía
    const submitButton = this.querySelector('button[type="submit"]');
    const originalText = submitButton.textContent;
    submitButton.disabled = true;
    submitButton.textContent = 'Enviando...';
    
    const formData = new FormData(this);
    const data = {};
    formData.forEach((value, key) => {
        data[key] = value;
    });
    
    try {
        const response = await fetch(SCRIPT_URL, {
            method: 'POST',
            mode: 'no-cors', // Importante para Google Apps Script
            headers: {
                'Content-Type': 'application/json',
            },
            body: JSON.stringify(data)
        });
        
        // Con mode: 'no-cors', siempre asumimos éxito si no hay error
        document.getElementById('successMessage').style.display = 'block';
        this.style.display = 'none';
        document.getElementById('successMessage').scrollIntoView({ behavior: 'smooth' });
        
    } catch (error) {
        console.error('Error:', error);
        alert('Hubo un error al enviar el formulario. Por favor, inténtalo de nuevo.');
        submitButton.disabled = false;
        submitButton.textContent = originalText;
    }
});
```

### 4. Probar

1. Abre tu web
2. Completa el formulario
3. Verifica que los datos aparezcan en tu Google Sheet

## Ventajas
- ✅ Completamente gratis
- ✅ Los datos se guardan automáticamente
- ✅ Puedes ver y exportar los datos fácilmente
- ✅ Puedes compartir la hoja con tu pareja o wedding planner
- ✅ No requiere servidor propio

## Notificaciones por Email (Opcional)

Si quieres recibir un email cada vez que alguien confirme, añade esto al Google Apps Script:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    // Añadir fila con los datos
    sheet.appendRow([
      new Date(),
      data.nombre,
      data.email,
      data.telefono,
      data.asistencia,
      data.acompanantes,
      data.alergias,
      data.comentarios
    ]);
    
    // Enviar email de notificación
    var emailBody = 
      "Nueva confirmación de asistencia:\n\n" +
      "Nombre: " + data.nombre + "\n" +
      "Email: " + data.email + "\n" +
      "Teléfono: " + data.telefono + "\n" +
      "Asistencia: " + data.asistencia + "\n" +
      "Acompañantes: " + data.acompanantes + "\n" +
      "Alergias: " + data.alergias + "\n" +
      "Comentarios: " + data.comentarios;
    
    MailApp.sendEmail({
      to: "tu-email@gmail.com", // Cambia por tu email
      subject: "Nueva confirmación de boda - " + data.nombre,
      body: emailBody
    });
    
    return ContentService
      .createTextOutput(JSON.stringify({success: true}))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch(error) {
    return ContentService
      .createTextOutput(JSON.stringify({success: false, error: error.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

## Solución de Problemas

**Problema**: Los datos no llegan a la hoja
- Verifica que la URL del script sea correcta
- Asegúrate de haber dado permisos al script
- Revisa que el script esté implementado como "Cualquier persona"

**Problema**: Error de CORS
- El código usa `mode: 'no-cors'` que es correcto para Google Apps Script
- No puedes leer la respuesta, pero los datos sí se envían

**Problema**: El formulario no se envía
- Abre la consola del navegador (F12) y busca errores
- Verifica que SCRIPT_URL esté correctamente configurado
