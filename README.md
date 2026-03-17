# NetWatch — Monitor de Estado en Tiempo Real

![Estado](https://img.shields.io/badge/estado-en%20vivo-22d3a0?style=flat-square)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-desplegado-f38020?style=flat-square)
![License](https://img.shields.io/badge/licencia-MIT-blue?style=flat-square)

**NetWatch** es un monitor de estado en tiempo real que consolida el estado de servicios de internet en una sola pagina. Desarrollado como un proyecto de una sola pagina HTML sin dependencias externas de frameworks.

🌐 **Sitio en vivo:** [xasve.github.io/netwatch](https://xasve.github.io/netwatch)

---

## Caracteristicas

- **3 categorias de monitoreo** en tabs separados:
  - ☁️ **Cloudflare** — 460+ componentes via API oficial de cloudflarestatus.com
  - 🎮 **Juegos** — Epic Games/Fortnite, Riot/LoL/Valorant, PlayStation, Xbox, Steam, Blizzard
  - 🌐 **Servicios Web** — Google, YouTube, Gmail, Instagram, WhatsApp, Claude, ChatGPT, Netflix, Discord, GitHub y mas
- **Datos en tiempo real** consumidos directamente desde APIs publicas de Statuspage
- **Actualizacion automatica** cada 2 minutos
- **Incidentes y mantenimientos** programados visibles
- **Filtros y busqueda** en la seccion de Cloudflare
- **Responsive** — funciona en movil y escritorio
- **Sin login requerido** — completamente publico

---

## Arquitectura

```
Usuario → GitHub Pages (index.html)
              ↓
         Cloudflare Worker (proxy CORS)
              ↓
         cloudflarestatus.com API
```

El sitio es un archivo HTML estatico. Para la seccion de Cloudflare se usa un **Cloudflare Worker** como proxy CORS, ya que la API de cloudflarestatus.com no permite llamadas directas desde el navegador. Los servicios de juegos y web se consultan directamente usando peticiones `no-cors` o sus propias APIs publicas de Statuspage.

---

## APIs Utilizadas

| Servicio | API |
|----------|-----|
| Cloudflare | `cloudflarestatus.com/api/v2/summary.json` (via Worker proxy) |
| Epic Games | `status.epicgames.com/api/v2/status.json` |
| GitHub | `www.githubstatus.com/api/v2/status.json` |
| Discord | `discordstatus.com/api/v2/status.json` |
| Microsoft | `status.microsoft.com/api/v2/status.json` |
| Resto | Ping `no-cors` + link a estado oficial |

---

## Estructura del Proyecto

```
netwatch/
├── index.html      # Aplicacion completa (HTML + CSS + JS)
├── worker.js       # Cloudflare Worker para proxy CORS
└── README.md       # Este archivo
```

---

## Cloudflare Worker (CORS Proxy)

El archivo `worker.js` contiene el codigo del proxy que se despliega en [workers.cloudflare.com](https://workers.cloudflare.com) (plan gratuito, 100,000 requests/dia).

**Endpoints que expone:**
- `/summary.json`
- `/status.json`
- `/incidents/unresolved.json`
- `/scheduled-maintenances/upcoming.json`
- `/scheduled-maintenances/active.json`

---

## Despliegue

El sitio se despliega automaticamente en **GitHub Pages** en cada push a `main`.

URL: `https://xasve.github.io/netwatch`

---

## Tecnologias

- HTML5 / CSS3 / JavaScript (Vanilla — sin frameworks)
- GitHub Pages (hosting estatico gratuito)
- Cloudflare Workers (proxy gratuito)
- APIs publicas de Statuspage.io

---

## Licencia

MIT — libre para usar y modificar.