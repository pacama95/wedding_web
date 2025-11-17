# Integración con Google Sheets

## Pasos para configurar

### 1. Crear la hoja de cálculo

1. Ve a [Google Sheets](https://sheets.google.com)
2. Crea una nueva hoja llamada "Confirmaciones Boda"
3. En la primera fila, añade estos encabezados:
   - `Fecha` | `Nombre` | `Asistencia` | `Acompañado` | `Adultos` | `Niños` | `Autobús` | `Alergias` | `Comentarios`

### 2. Crear el Google Apps Script

1. En tu hoja de cálculo, ve a **Extensiones** > **Apps Script**
2. Borra el código que aparece y pega este:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    
    // Obtener los parámetros del formulario
    var nombre = e.parameter.nombre || '';
    var asistencia = e.parameter.asistencia || '';
    var acompanado = e.parameter.acompanado || '';
    var adultos = e.parameter.adultos || '0';
    var ninos = e.parameter.ninos || '0';
    var autobus = e.parameter.autobus || 'no';
    var alergias = e.parameter.alergias || '';
    var comentarios = e.parameter.comentarios || '';
    
    // Añadir fila con los datos
    sheet.appendRow([
      new Date(),
      nombre,
      asistencia,
      acompanado,
      adultos,
      ninos,
      autobus,
      alergias,
      comentarios
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

El código HTML ya está configurado con la URL del script. Si necesitas cambiarla, busca esta línea en el archivo HTML:

```javascript
const SCRIPT_URL = 'https://script.google.com/macros/s/TU_URL_AQUI/exec';
```

### 4. Probar

1. Abre tu web
2. Completa el formulario
3. Verifica que los datos aparezcan en tu Google Sheet

## Campos del formulario

El formulario actual incluye:

- **Nombre completo** (requerido): Nombre del invitado
- **¿Asistirás a la boda?** (requerido): Sí / No
- **¿Vendrás acompañado/a?** (requerido): Sí / No
  - Si selecciona "Sí", aparecen:
    - **Número de adultos acompañantes**: 0-5
    - **Número de niños acompañantes**: 0-5
- **¿Necesitas autobús?** (requerido): Solo ida / Solo vuelta / Ida y vuelta / No
- **Alergias e Intolerancias** (opcional): Texto libre
- **Comentarios adicionales** (opcional): Texto libre

## Ventajas
- ✅ Completamente gratis
- ✅ Los datos se guardan automáticamente
- ✅ Puedes ver y exportar los datos fácilmente
- ✅ Puedes compartir la hoja con tu pareja o wedding planner
- ✅ No requiere servidor propio
- ✅ Diferenciación clara entre adultos y niños para planificación

## Notificaciones por Email (Opcional)

Si quieres recibir un email cada vez que alguien confirme, añade esto al Google Apps Script:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    
    // Obtener los parámetros del formulario
    var nombre = e.parameter.nombre || '';
    var asistencia = e.parameter.asistencia || '';
    var acompanado = e.parameter.acompanado || '';
    var adultos = e.parameter.adultos || '0';
    var ninos = e.parameter.ninos || '0';
    var autobus = e.parameter.autobus || 'no';
    var alergias = e.parameter.alergias || '';
    var comentarios = e.parameter.comentarios || '';
    
    // Añadir fila con los datos
    sheet.appendRow([
      new Date(),
      nombre,
      asistencia,
      acompanado,
      adultos,
      ninos,
      autobus,
      alergias,
      comentarios
    ]);
    
    // Construir información de acompañantes
    var infoAcompanantes = "No";
    if (acompanado === "si") {
      infoAcompanantes = "Sí - Adultos: " + adultos + ", Niños: " + ninos;
    }
    
    // Enviar email de notificación
    var emailBody = 
      "Nueva confirmación de asistencia:\n\n" +
      "Nombre: " + nombre + "\n" +
      "Asistencia: " + asistencia + "\n" +
      "Acompañado: " + infoAcompanantes + "\n" +
      "Autobús: " + autobus + "\n" +
      "Alergias: " + (alergias || "Ninguna") + "\n" +
      "Comentarios: " + (comentarios || "Ninguno");
    
    MailApp.sendEmail({
      to: "tu-email@gmail.com", // Cambia por tu email
      subject: "Nueva confirmación de boda - " + nombre,
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

## Análisis de datos útiles

Con esta estructura, puedes crear fórmulas en Google Sheets para:

**Total de asistentes confirmados:**
```
=COUNTIF(C:C,"si")
```

**Total de adultos (incluyendo invitados principales que asisten):**
```
=COUNTIF(C:C,"si") + SUMIF(C:C,"si",E:E)
```

**Total de niños:**
```
=SUMIF(C:C,"si",F:F)
```

**Total general de personas:**
```
=COUNTIF(C:C,"si") + SUMIF(C:C,"si",E:E) + SUMIF(C:C,"si",F:F)
```

**Personas con alergias:**
```
=COUNTIF(G:G,"<>")
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
- Revisa el console.log para ver qué datos se están enviando

**Problema**: La sección de acompañantes no aparece
- Verifica que hayas seleccionado "Sí" en "¿Vendrás acompañado/a?"
- La sección aparece/desaparece automáticamente según la selección