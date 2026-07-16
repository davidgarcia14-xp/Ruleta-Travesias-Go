# Poké Rifa · Travesías-GO — Deploy en Cloudflare Pages

## Archivos incluidos

```
deploy/
├── index.html           ← La app completa (un solo archivo)
├── favicon.ico          ← Favicon multi-resolución (16/32/48px)
├── favicon-32.png       ← Favicon PNG 32px
├── apple-touch-icon.png ← Ícono para iOS (180px)
├── icon-192.png         ← Ícono Android/PWA (192px)
├── icon-512.png         ← Ícono Android/PWA (512px)
├── og-image.png         ← Imagen para compartir en redes sociales
├── manifest.json        ← Manifiesto PWA (instalar como app)
├── _headers             ← Headers de caché y seguridad para Cloudflare
├── _redirects           ← Redireccionamiento SPA para Cloudflare
└── README.md            ← Este archivo
```

## Cómo subir a Cloudflare Pages

### Opción 1: Direct Upload (más fácil)

1. Ve a https://dash.cloudflare.com → **Workers & Pages** → **Create**
2. Pestaña **Pages** → **Upload assets**
3. Pon un nombre al proyecto (ej. `poke-rifa`)
4. Arrastra TODOS los archivos de esta carpeta (sin incluir README.md)
5. Click en **Deploy site**
6. ¡Listo! Tu URL será: `https://poke-rifa.pages.dev`

### Opción 2: Conectar con Git

1. Sube esta carpeta a un repo de GitHub/GitLab
2. En Cloudflare Pages → Create → **Connect to Git**
3. Selecciona el repo
4. Build command: (vacío, no se necesita)
5. Build output directory: `/` (o la carpeta donde estén los archivos)
6. Deploy

## Después de subir

### 1. Actualizar la URL de og:image
Abre `index.html` y busca esta línea:
```html
<meta property="og:image" content="/og-image.png">
```
Cámbiala por tu URL completa:
```html
<meta property="og:image" content="https://TU-DOMINIO.pages.dev/og-image.png">
```

### 2. Cambiar el código del organizador
Busca `CODIGO_ORGANIZADOR` en index.html y cámbialo:
```javascript
const CODIGO_ORGANIZADOR = "TU-CODIGO-SECRETO";
```

### 3. Dominio personalizado (opcional)
En Cloudflare Pages → tu proyecto → **Custom domains** → agrega tu dominio.

## Almacenamiento

⚠️ **Importante**: La app dentro del artefacto de Claude usa `window.storage`
(almacenamiento compartido de Claude). En tu propio dominio esto NO estará
disponible, así que la app funcionará en **modo demo** (datos locales en memoria,
sin sincronización entre dispositivos).

Para sincronización real necesitarás reemplazar el objeto `Store` en el código
por un backend como:
- **Firebase Realtime Database** (gratis hasta cierto uso)
- **Supabase** (PostgreSQL gratis)
- **Cloudflare KV** (ya que estás en Cloudflare)

El código ya está preparado para eso: solo cambia las funciones
`get`, `set`, `del` del objeto `Store`.
