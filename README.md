# JAS 2026 · PWA de Registro

## Archivos
- `index.html` — App completa (participante + admin)
- `manifest.json` — Configuración PWA (para instalación en celular)
- `sw.js` — Service Worker (modo offline)
- `icon-192.png` / `icon-512.png` — Iconos de la app (agregar antes de subir)

## Despliegue en Vercel

1. Sube los 3 archivos a un repositorio GitHub (público o privado)
2. Entra a vercel.com → "New Project" → importa el repositorio
3. Sin configuración adicional, Vercel detecta archivos estáticos
4. Deploy → obtienes tu URL pública (ej. `jas2026.vercel.app`)

## Registro del Service Worker
Agrega esto antes del `</body>` en index.html (ya incluido en el código):

```html
<script>
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/sw.js');
  }
</script>
```

## Conectar a Supabase (siguiente paso)

Reemplaza el objeto `PARTICIPANTES` en index.html con llamadas reales:

```js
// Instalar: <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js"></script>
const supabase = window.supabase.createClient('TU_URL', 'TU_ANON_KEY');

async function buscarDNI() {
  const dni = document.getElementById('dni-input').value.trim();
  const { data, error } = await supabase
    .from('participantes')
    .select('*')
    .eq('dni', dni)
    .single();
  if (error || !data) { /* mostrar error */ return; }
  currentParticipante = data;
  // continuar con el flujo...
}
```

## Tabla Supabase recomendada: `participantes`

| Columna      | Tipo    | Descripción                        |
|--------------|---------|------------------------------------|
| id           | uuid    | PK auto                            |
| dni          | text    | DNI del participante               |
| nombre       | text    | Nombre completo                    |
| cc           | text    | Centro de Coordinación             |
| estaca       | text    | Estaca                             |
| bus          | text    | Número de bus                      |
| mesa         | text    | Mesa de recojo de materiales       |
| salon        | text    | Salón de equipaje                  |
| compania     | text    | Compañía asignada                  |
| consejeros   | text    | Nombres de consejeros              |
| habitacion   | text    | Número y pabellón de habitación    |
| talla_polo   | text    | XS / S / M / L / XL / XXL         |
| registrado   | boolean | false = pendiente, true = completo |
| created_at   | timestamp | Auto                             |

## Credenciales demo (cambiar antes del evento)
- Usuario admin: `admin`
- Contraseña: `jas2026`

En producción usar Supabase Auth con roles.

## Para "instalar" en celular
1. El participante abre la URL en Chrome/Safari
2. Chrome: menú ⋮ → "Agregar a pantalla de inicio"
3. Safari: botón compartir → "Agregar a inicio"
4. Queda como app con ícono propio
