# OsteoRisk — Guía de despliegue en Vercel

App web de apoyo clínico para estratificación de riesgo de fractura por fragilidad.
Basada en guías SEIOMM 2022, IOF/ESCEO y NOF. Incluye FRAX integrado e informe PDF.

---

## Requisitos previos

- Cuenta gratuita en [github.com](https://github.com)
- Cuenta gratuita en [vercel.com](https://vercel.com)
- Clave de API de Anthropic (claude.ai → Settings → API Keys)

---

## Paso 1 — Editar la clave de API

Antes de subir, abre `index.html` con cualquier editor de texto (Bloc de notas, TextEdit, VS Code...)
y busca la línea:

```
headers: { 'Content-Type': 'application/json' },
```

**IMPORTANTE:** La app usa la API de Anthropic directamente desde el navegador.
Para producción real con datos de pacientes, la clave debe ir en un proxy de servidor
(ver sección "Uso en entorno hospitalario" más abajo).

---

## Paso 2 — Subir a GitHub

1. Ve a [github.com/new](https://github.com/new)
2. Nombre del repositorio: `osteoporosis-app` (o el que prefieras)
3. Visibilidad: **Privado** (recomendado)
4. Pulsa "Create repository"
5. En la página del repositorio vacío, pulsa "uploading an existing file"
6. Arrastra los dos ficheros: `index.html` y `vercel.json`
7. Pulsa "Commit changes"

---

## Paso 3 — Desplegar en Vercel

1. Ve a [vercel.com](https://vercel.com) e inicia sesión con tu cuenta de GitHub
2. Pulsa "Add New Project"
3. Selecciona el repositorio `osteoporosis-app`
4. Framework Preset: **Other** (estático)
5. Pulsa "Deploy"
6. En ~30 segundos tendrás una URL tipo: `osteoporosis-app.vercel.app`

---

## Paso 4 — Configurar la clave de API como variable de entorno (recomendado)

En lugar de poner la clave directamente en el HTML:

1. En Vercel → tu proyecto → Settings → Environment Variables
2. Añade: `ANTHROPIC_API_KEY` = tu clave
3. Redeploy

Luego modifica el `fetch` en `index.html` para leer la variable desde un endpoint
serverless (ver carpeta `/api` de ejemplo si la necesitas).

---

## Actualizar la app

Cada vez que modifiques `index.html` y lo subas a GitHub,
Vercel redesplegará automáticamente en segundos.

---

## Uso en entorno hospitalario (datos reales de pacientes)

Para cumplir RGPD/LOPD con datos reales:

1. **Servidor interno (IT del hospital):** Copia `index.html` en cualquier servidor
   Apache/Nginx accesible solo desde la red interna o VPN.
2. **Proxy de API:** Crea un endpoint interno que reenvíe las peticiones a Anthropic,
   manteniendo la clave de API fuera del navegador.
3. **Sin almacenamiento:** La app actual no guarda ningún dato en servidor.
   Todo el procesamiento es stateless.

---

## Soporte

Esta herramienta es de apoyo clínico y no sustituye el criterio del especialista.
Revisar siempre contraindicaciones individuales y función renal antes de prescribir.
