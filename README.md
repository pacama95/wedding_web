# 💍 Sitio Web de Boda - Pablo & Paula

Una elegante página web para bodas, completamente responsive y personalizable.

## 🌟 Características

- ✨ Diseño elegante y moderno con animaciones suaves
- 📱 Totalmente responsive (se adapta a móviles, tablets y escritorio)
- 🖼️ Sección para foto de los novios
- 📍 Información detallada de ceremonia y celebración
- ⏰ Itinerario completo del evento
- 📝 Formulario de confirmación de asistencia con:
  - Confirmación Sí/No
  - Opción de asistir acompañado
  - Número de adultos acompañantes (0-5)
  - Número de niños acompañantes (0-5)
  - Campo para alergias e intolerancias alimentarias
  - Comentarios adicionales
- 🎨 Paleta de colores elegante en tonos dorados y tierra
- 🔤 Tipografías premium (Playfair Display y Lato)

## 🚀 Cómo Usar

### Subir a GitHub Pages

1. Descarga el archivo `boda-pablo-paula.html`
2. Renómbralo a `index.html`
3. Sube el archivo a tu repositorio de GitHub
4. Activa GitHub Pages en la configuración del repositorio
5. Tu web estará disponible en: `https://tu-usuario.github.io/nombre-repositorio/`

### Ver localmente

Simplemente abre el archivo HTML en tu navegador web favorito.

## 🎨 Personalización

### 1. Información Básica

Busca y reemplaza los siguientes textos en el archivo HTML:

- **Nombres**: `Pablo & Paula` (línea ~186)
- **Fecha**: Añade la fecha debajo de "Nos casamos" (línea ~187)

### 2. Foto de los Novios

Reemplaza el `div` con clase `photo-placeholder` (línea ~195-202) con:

```html
<img src="ruta-de-tu-foto.jpg" 
     alt="Pablo y Paula" 
     style="width: 100%; max-width: 700px; border-radius: 15px; box-shadow: 0 15px 50px rgba(0,0,0,0.15);">
```

**Opciones para la foto:**
- **Opción 1**: Sube la foto al mismo directorio y usa: `src="foto-boda.jpg"`
- **Opción 2**: Usa un servicio de hosting de imágenes (Imgur, Cloudinary) y usa la URL
- **Opción 3**: Súbela a la carpeta del repositorio de GitHub

### 3. Información de Lugares

**Ceremonia** (líneas ~210-214):
```html
<p><strong>Lugar:</strong> Iglesia de San Miguel</p>
<p><strong>Dirección:</strong> Calle Principal 123, Madrid</p>
<p><strong>Hora:</strong> 12:00 PM</p>
```

**Celebración** (líneas ~217-221):
```html
<p><strong>Lugar:</strong> Finca Los Olivos</p>
<p><strong>Dirección:</strong> Carretera de Toledo km 15, Madrid</p>
<p><strong>Hora:</strong> 14:00 PM</p>
```

### 4. Itinerario

Modifica los horarios y eventos (líneas ~230-268):

```html
<div class="itinerary-item">
    <div class="time">12:00</div>
    <div class="event-details">
        <h4>Ceremonia</h4>
        <p>Descripción del evento</p>
    </div>
</div>
```

Puedes añadir más eventos copiando y pegando este bloque.

### 5. Colores del Sitio

Los colores principales se definen en las variables CSS. Busca estos valores y cámbialos:

- **Color principal**: `#8b6f47` (marrón dorado)
- **Color secundario**: `#d4af7a` (dorado claro)
- **Gradientes**: `#e8dcc4`, `#d4c4a8`, `#c9b896`

Ejemplo para cambiar el color principal a azul:
```css
/* Busca #8b6f47 y reemplaza con */
#4a7c8b
```

### 6. Tipografías

Las fuentes actuales son:
- **Títulos**: Playfair Display (serif elegante)
- **Texto**: Lato (sans-serif moderna)

Para cambiar, modifica la línea ~6:
```css
@import url('https://fonts.googleapis.com/css2?family=TU-FUENTE&display=swap');
```

Luego actualiza las referencias de `font-family` en el CSS.

### 7. Formulario de Confirmación

El formulario actualmente envía datos a Google Sheets usando Google Apps Script.

**Estructura actual del formulario:**
- Nombre completo (requerido)
- ¿Asistirás a la boda? (Sí/No - requerido)
- ¿Vendrás acompañado/a? (Sí/No - requerido)
  - Si es "Sí", se muestran:
    - Número de adultos acompañantes (0-5)
    - Número de niños acompañantes (0-5)
- Alergias e intolerancias (opcional)
- Comentarios adicionales (opcional)

**Para configurar el almacenamiento:**

Ver el archivo `google-sheets-integration.md` para instrucciones detalladas.

**Campos que se envían:**
```javascript
{
  nombre: "Nombre del invitado",
  asistencia: "si" o "no",
  acompanado: "si" o "no",
  adultos: "0" a "5",
  ninos: "0" a "5",
  alergias: "texto libre",
  comentarios: "texto libre"
}
```

**Otras opciones disponibles:**
- Formspree (email notifications)
- Firebase (base de datos en tiempo real)
- EmailJS (envío directo por email)
- Backend propio

Ver `storage-options.md` para más alternativas.

### 8. Añadir Mapa de Ubicación

Añade un mapa de Google Maps después de la información de lugares:

```html
<div style="margin-top: 30px;">
    <iframe 
        src="https://www.google.com/maps/embed?pb=TU_CODIGO_DE_EMBED"
        width="100%" 
        height="400" 
        style="border:0; border-radius: 10px;" 
        allowfullscreen="" 
        loading="lazy">
    </iframe>
</div>
```

Para obtener el código:
1. Ve a Google Maps
2. Busca tu ubicación
3. Click en "Compartir" > "Incorporar un mapa"
4. Copia el código iframe

## 📋 Estructura del Archivo

```
boda-pablo-paula.html
│
├── <head>
│   ├── Meta tags
│   ├── Título
│   └── Estilos CSS
│
├── <header>
│   └── Nombres y fecha de boda
│
├── <section> Foto
│   └── Imagen de los novios
│
├── <section> Información del Evento
│   ├── Tarjeta de Ceremonia
│   └── Tarjeta de Celebración
│
├── <section> Itinerario
│   └── Timeline de eventos
│
├── <section> Formulario RSVP
│   └── Confirmación de asistencia
│
└── <footer>
    └── Mensaje de despedida
```

## 🎯 Consejos de Personalización

### Para una boda elegante/formal:
- Mantén los colores actuales (dorados y marrones)
- Usa fotos profesionales en blanco y negro
- Mantén el texto conciso y elegante

### Para una boda casual/rústica:
- Cambia colores a verdes y marrones tierra: `#6b8e6b`, `#8b7355`
- Añade texturas de madera en los fondos
- Usa un tono más informal en los textos

### Para una boda moderna/minimalista:
- Simplifica a blanco y negro: `#000000`, `#ffffff`, `#808080`
- Reduce las animaciones
- Usa tipografías sans-serif como Montserrat

## 🛠️ Tecnologías Utilizadas

- HTML5
- CSS3 (con animaciones y gradientes)
- JavaScript vanilla (sin dependencias)
- Google Fonts (Playfair Display y Lato)

## 📱 Compatibilidad

- ✅ Chrome (todas las versiones recientes)
- ✅ Firefox (todas las versiones recientes)
- ✅ Safari (iOS y macOS)
- ✅ Edge (todas las versiones recientes)
- ✅ Dispositivos móviles (responsive design)

## 📞 Soporte

Si encuentras algún problema o tienes preguntas:
1. Revisa la sección de personalización
2. Verifica que todos los cambios estén dentro de las etiquetas correctas
3. Asegúrate de que las comillas y paréntesis estén balanceados

## 📝 Licencia

Este proyecto es de uso libre. Siéntete libre de usarlo y modificarlo para tu boda.

---

**¡Felicidades por tu boda!** 🎉💕

Si necesitas ayuda adicional o quieres añadir más funcionalidades, no dudes en modificar el código o buscar tutoriales de HTML/CSS básicos.