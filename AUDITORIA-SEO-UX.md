# Auditoría SEO, UX y Seguridad — Casa Wayak
**Sitio auditado:** http://casa-wayak.ilinium.com/  
**Páginas:** Homepage · /wayak/ · /kamala/  
**Fecha:** 7 mayo 2026  
**Preparado por:** ilinium — Transformación Digital  

---

## Resumen ejecutivo

Se auditaron las tres páginas principales del sitio junto con el código fuente del repositorio. Se identificaron **8 bloqueantes críticos** que deben resolverse antes del lanzamiento, más 15 mejoras de mediano plazo agrupadas en SEO, seguridad y UX.

El problema más grave es `noindex, nofollow` activo en todo el sitio — Google no indexa ninguna página. El segundo más urgente es la ausencia de menú de navegación en móvil.

---

## Tabla de estado general

| Área | Estado | Detalles |
|------|--------|----------|
| Indexación Google | 🔴 Bloqueada | `noindex, nofollow` en todas las páginas |
| H1 Tags | 🔴 Ausentes | Ninguna página tiene H1 |
| Titles | 🔴 Incompletos | "Casa Wayak -" truncado en homepage |
| Meta descriptions | 🟡 Parcial | Solo /kamala/ tiene descripción |
| Schema / Structured Data | 🟡 Parcial | LodgingBusiness solo en /kamala/ |
| Open Graph | 🟡 Parcial | Sin og:image en homepage y /wayak/ |
| Canonical Tags | 🟡 Parcial | Solo /kamala/ tiene canonical |
| Menú mobile | 🔴 Roto | Nav desaparece sin alternativa |
| Booking / Reservas | 🔴 Desconectado | Sin integración real con Hostfully |
| WP REST API | 🟡 Expuesta | Endpoint público sin restricción |
| WhatsApp | 🔴 Placeholder | Número `5219980000000` no es real |
| Imágenes (formato) | ✅ Bien | Todas en WebP |
| Imágenes (dimensiones) | ✅ Bien | width/height presentes |
| HTTPS | ✅ Bien | Activo |
| Sitemap | ✅ Generado | Yoast · Contiene pagina-ejemplo ⚠️ |
| robots.txt | ✅ Limpio | Sin bloqueos incorrectos |
| Hreflang EN/ES | ✅ Configurado | 17 URLs en 2 idiomas |

---

## 1. SEO — Hallazgos

### 🔴 Crítico

**1.1 `noindex, nofollow` en TODAS las páginas**  
Google no indexará ninguna página del sitio. Cambiar en:  
Yoast SEO → Configuración → Visibilidad del sitio → "Permitir que los motores de búsqueda indexen este sitio"

**1.2 Sin H1 en ninguna página**  
Homepage, /wayak/ y /kamala/ tienen cero tags H1.

| Página | H1 propuesto |
|--------|-------------|
| `/` | `Wayak Luxury Village — Isla Mujeres` |
| `/wayak/` | `Villa Wayak — Alberca Infinita con Vista al Caribe` |
| `/kamala/` | `Villa Kamala — Tu Jardín Privado en el Caribe` |

**1.3 Title tags incompletos**

| Página | Title actual | Title propuesto |
|--------|-------------|----------------|
| `/` | `Casa Wayak -` | `Wayak Luxury Village \| Villas Privadas en Isla Mujeres, México` |
| `/wayak/` | `Wayak - Casa Wayak` | `Villa Wayak \| Casa con Alberca Infinita — Isla Mujeres, Caribe` |
| `/kamala/` | `Kamala - Casa Wayak` | `Villa Kamala \| Villa Privada con Jardín Tropical — Isla Mujeres` |

> Nota: Los titles en el repositorio local (wayak.html y kamala.html) son mejores que los del live — solo hay que copiarlos a Yoast.

**1.4 Meta descriptions ausentes en homepage y /wayak/**

| Página | Descripción propuesta |
|--------|----------------------|
| `/` | `Wayak Luxury Village, Isla Mujeres. Dos villas privadas con alberca, chef, concierge 24/7 y el Caribe a tu medida. Reserva tu escapada.` |
| `/wayak/` | `Villa Wayak: alberca infinita con vista al Caribe, 4 recámaras, chef privado y concierge 24/7 en Isla Mujeres. Lujo sin límites.` |

### 🔴 Schema / Structured Data

**1.5 LodgingBusiness solo en /kamala/ — falta en /wayak/ y homepage**

Schema JSON-LD a añadir en `/wayak/`:
```json
{
  "@context": "https://schema.org",
  "@type": "LodgingBusiness",
  "name": "Villa Wayak — Wayak Luxury Village",
  "description": "Villa privada de lujo en Isla Mujeres con alberca infinita, vista panorámica al Caribe y concierge 24/7.",
  "url": "https://casa-wayak.ilinium.com/wayak/",
  "image": ["https://casa-wayak.ilinium.com/wp-content/themes/wayak/assets/img/wayak/wayak-intro.webp"],
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Isla Mujeres",
    "addressRegion": "Quintana Roo",
    "addressCountry": "MX"
  },
  "priceRange": "$$$$",
  "amenityFeature": [
    {"@type": "LocationFeatureSpecification", "name": "Alberca Infinita", "value": true},
    {"@type": "LocationFeatureSpecification", "name": "Chef Privado", "value": true},
    {"@type": "LocationFeatureSpecification", "name": "Concierge 24/7", "value": true},
    {"@type": "LocationFeatureSpecification", "name": "Traslados VIP", "value": true}
  ]
}
```

**1.6 LodgingBusiness en /kamala/ incompleto**

Campos faltantes que mejoran rich results:

| Campo | Valor |
|-------|-------|
| `telephone` | Número real de contacto |
| `geo.latitude` / `geo.longitude` | Coordenadas de Isla Mujeres |
| `checkInTime` | Hora de check-in |
| `checkOutTime` | Hora de check-out |
| `numberOfRooms` | 3 |
| `@id` | `https://casa-wayak.ilinium.com/kamala/#lodging` |

**1.7 WebSite schema con `description` vacío**  
Yoast genera `"description": ""` en todas las páginas. Llenar en: Yoast → SEO → Título del sitio / descripción.

### 🟡 Importante

**1.8 Open Graph incompleto**

| Tag | Homepage | /wayak/ | /kamala/ |
|-----|----------|---------|---------|
| `og:title` | "Casa Wayak -" ⚠️ | "Wayak - Casa Wayak" ⚠️ | "Kamala - Casa Wayak" ⚠️ |
| `og:description` | ❌ | ❌ | ✅ |
| `og:image` | ❌ | ❌ | ✅ `wayak-og.webp` |

Sin `og:image`, los shares en WhatsApp y redes sociales no muestran preview de imagen.

**1.9 Canonical tags ausentes**  
Homepage y /wayak/ no tienen `<link rel="canonical">`. Solo /kamala/ lo tiene.

**1.10 Links HTTP mezclados con HTTPS en homepage**
```
http://casa-wayak.ilinium.com/wayak/     → cambiar a https://
http://casa-wayak.ilinium.com/ubicacion  → cambiar a https:// + trailing slash
```

**1.11 `pagina-ejemplo` en sitemap**  
`https://casa-wayak.ilinium.com/pagina-ejemplo/` aparece en el sitemap. Eliminar o poner `noindex`.

**1.12 Typo en H2 de /wayak/**  
`"When is your getaway??"` — doble signo de interrogación.

**1.13 Número de WhatsApp placeholder en todas las páginas**  
`https://wa.me/5219980000000` — reemplazar con número real.

### ✅ SEO bien implementado

- Imágenes en formato WebP
- Atributos `width` y `height` en todas las imágenes (previene CLS)
- HTTPS activo
- Hreflang EN/ES configurado correctamente
- Sitemap generado y referenciado en robots.txt
- Twitter Card `summary_large_image`
- Viewport meta configurado
- Lazy loading en imágenes below-fold

---

## 2. Seguridad — Hallazgos

### 🔴 S1. WP REST API pública sin restricción

La API expone estructura interna del sitio:
```
https://casa-wayak.ilinium.com/wp-json/
https://casa-wayak.ilinium.com/wp-json/wp/v2/pages/6
https://casa-wayak.ilinium.com/wp-json/wp/v2/pages/45
```

**Fix — añadir en `functions.php` del tema:**
```php
add_filter('rest_authentication_errors', function($result) {
    if (!is_user_logged_in()) {
        return new WP_Error('rest_forbidden', 'REST API disabled for public.', ['status' => 401]);
    }
    return $result;
});
```

### 🟡 S2. WordPress fingerprinting expuesto

El generador de Yoast en el sitemap y los meta tags revelan que es WordPress. Ocultar versión:
```php
// En functions.php
remove_action('wp_head', 'wp_generator');
add_filter('the_generator', '__return_empty_string');
```

### 🟡 S3. Fotos fuente con metadatos EXIF sin limpiar

Los 55 archivos JPEG en `assets/fotos/` posiblemente contienen metadatos de cámara, fecha y ubicación GPS. Limpiar antes de uso en producción:
```bash
exiftool -all= assets/fotos/wayak/*.jpg
exiftool -all= assets/fotos/kamala/*.jpg
```

### 🟡 S4. Sin Content Security Policy (CSP)

El sitio carga recursos externos (Google Fonts, Hostfully). Sin CSP, un XSS puede cargar scripts arbitrarios. Implementar en `.htaccess` o vía plugin de seguridad WordPress.

### 🟡 S5. Formulario de reservas sin protección CSRF

Si el booking form conecta a un endpoint, requiere nonce de WordPress + validación server-side.

---

## 3. UX — Hallazgos

### 🔴 U1. Sin menú hamburguesa en mobile

```css
@media (max-width: 768px) {
  .nav-links { display: none; } /* desaparece sin alternativa */
}
```

En mobile solo queda logo + "Reservar". Las secciones Amenidades, Ubicación, Sobre Wayak son inaccesibles. **Crítico para un sitio luxury en el dispositivo más usado.**

### 🔴 U2. Booking form desconectado de Hostfully

El botón "Verificar disponibilidad" no tiene acción. Opciones de integración:
- Widget embebido de Hostfully (iframe/JS)
- Redirect a URL de reservas con parámetros de fecha y villa pre-llenados

### 🟡 U3. Galería sin lightbox — fotos no ampliables

Los gallery items tienen `cursor: pointer` y hover effects pero no abren nada al hacer clic.  
**Recomendación:** [GLightbox](https://biati-digital.github.io/glightbox/) — 9KB, sin jQuery.

### 🟡 U4. Hero image vía CSS en prototipo — no accesible

```css
/* En index.html del repo */
.hero-bg { background: url('img/hero.jpg') center/cover; }
```

Una imagen CSS no tiene alt text, no es indexable por Google Images y no acepta `fetchpriority`. El live WordPress ya usa `<img>` real — ese patrón debe mantenerse.

### 🟡 U5. Textos demasiado pequeños (accesibilidad)

```css
font-size: 0.6rem;   /* 9.6px — labels, nav */
font-size: 0.58rem;  /* 9.3px — stat labels */
font-size: 0.65rem;  /* 10.4px — botones CTA */
```

WCAG 2.1 recomienda mínimo 16px para cuerpo y 14px para texto secundario.

### 🟡 U6. CTA "Cómo llegar" apunta a la sección de reservas

```html
<a href="#reservaciones">Cómo llegar</a>
```

Lleva al formulario de reservas, no a información de cómo llegar a Isla Mujeres. Confunde la intención del usuario.

### 🟡 U7. Sin mapa real en sección Ubicación

La sección de ubicación muestra una foto en lugar de un mapa interactivo. Para un destino en una isla que requiere ferry, un Google Maps embed añade valor decisivo a la UX.

### 🟡 U8. `fetchpriority="high"` faltante en hero images

La imagen hero de cada villa debería tener:
```html
<img src="..." fetchpriority="high" alt="..." width="..." height="...">
```
Mejora el LCP (Largest Contentful Paint).

---

## 4. Assets — Inventario y gaps

### Estado del banco de imágenes

| Villa | Fotos en repo | Formato | Usadas en live | Sin utilizar |
|-------|--------------|---------|----------------|-------------|
| Wayak | 40 (39 + portada) | JPEG | ~10 (WebP) | **30 disponibles** |
| Kamala | 15 (14 + portada) | JPEG | ~8 (WebP) | **7 disponibles** |
| Videos Wayak | 4 MP4 | MP4 | 0 | **4 sin integrar** |
| Videos Kamala | 2 MP4 | MP4 | 0 | **2 sin integrar** |

### A1. 6 videos MP4 sin integrar — oportunidad de diferenciación

Para un sitio luxury, el video es el diferenciador más potente. Implementación recomendada:

```html
<video autoplay muted loop playsinline poster="wayak-hero-poster.webp"
       aria-hidden="true" class="hero-video">
  <source src="wayak-hero.mp4" type="video/mp4">
</video>
```

### A2. Imágenes en live con nombres genéricos — oportunidad SEO

```
wayak-gallery-1.webp  →  wayak-alberca-infinita-caribe.webp
wayak-gallery-2.webp  →  wayak-recamara-king-palapa.webp
kamala-gallery-1.webp →  kamala-jardin-bugambilia-piscina.webp
```

---

## 5. Crosscheck: Prototipo (repo) vs WordPress Live

| Elemento | Prototipo repo | Live WordPress |
|----------|---------------|----------------|
| Title homepage | ✅ "Wayak Luxury Village" | ❌ "Casa Wayak -" (roto) |
| Title /wayak/ | ✅ "Villa Wayak — Luxury Village Isla Mujeres" | ❌ "Wayak - Casa Wayak" |
| Meta desc /wayak/ | ✅ Con keywords | ❌ Ausente |
| H1 | ❌ Ninguno | ❌ Ninguno |
| Hero image | ❌ CSS background | ✅ `<img>` tag |
| WhatsApp número | ❌ Placeholder | ❌ Mismo placeholder |
| Footer links | ❌ Todos `href="#"` | Sin verificar |
| Videos | Presentes en repo | No integrados |

---

## 6. Plan de acción

### Sprint 1 — Antes de lanzar (bloqueantes)

| # | Tarea | Herramienta | Responsable |
|---|-------|-------------|-------------|
| 1 | Quitar `noindex` global | Yoast → Configuración | Cristian |
| 2 | Agregar H1 en las 3 páginas | Tema WP PHP | Cristian |
| 3 | Corregir titles (usar los del prototipo) | Yoast por página | Cristian |
| 4 | Meta description en homepage y /wayak/ | Yoast por página | Cristian |
| 5 | Número real de WhatsApp | Tema + prototipo | Adrian |
| 6 | Implementar menú hamburguesa mobile | CSS + JS tema | Cristian |
| 7 | Conectar booking bar con Hostfully | Embed / redirect | Cristian |
| 8 | Cerrar WP REST API al público | `functions.php` | Cristian |

### Sprint 2 — Primera semana post-lanzamiento

| # | Tarea | Detalle |
|---|-------|---------|
| 9 | Canonical tags en homepage y /wayak/ | Yoast |
| 10 | LodgingBusiness schema en /wayak/ | JSON-LD en tema |
| 11 | Completar LodgingBusiness en /kamala/ | geo, tel, checkIn |
| 12 | og:image y og:description en homepage y /wayak/ | Yoast + imagen OG |
| 13 | Corregir links HTTP → HTTPS | Tema / DB |
| 14 | Eliminar `pagina-ejemplo` del sitemap | Yoast / borrar página |
| 15 | Renombrar imágenes con descriptores SEO | Servidor + Yoast |
| 16 | Limpiar EXIF de fotos en repo | exiftool |

### Sprint 3 — Mejoras de calidad

| # | Tarea | Impacto |
|---|-------|---------|
| 17 | Lightbox para galerías | UX alto |
| 18 | Integrar videos MP4 en hero o galería | UX alto |
| 19 | Google Maps embed en sección Ubicación | UX medio |
| 20 | Aumentar tamaños de fuente (mín. 14px) | Accesibilidad |
| 21 | `fetchpriority="high"` en imagen hero | LCP / Core Web Vitals |
| 22 | Convertir fotos restantes a WebP | Performance |
| 23 | Implementar CSP headers | Seguridad |

---

*Auditoría realizada por ilinium — Transformación Digital | ilinium.com*  
*Monterrey, México · Mayo 2026*
