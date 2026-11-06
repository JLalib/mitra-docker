# 📅 Mitra Docker - Calendario y tareas en una sola línea de tiempo

[![GitHub Repo](https://img.shields.io/badge/GitHub-a11delavar%2Fmitra-blue?logo=github)](https://github.com/a11delavar/mitra)
[![Docker Image](https://img.shields.io/badge/Docker-ghcr.io%2Fa11delavar%2Fmitra-blue?logo=docker)](https://github.com/a11delavar/mitra/pkgs/container/mitra)
[![License](https://img.shields.io/badge/License-AGPL--3.0-orange)](https://github.com/a11delavar/mitra/blob/main/LICENSE)

## 📋 Descripción general

**Mitra** es un calendario autohospedado con tareas integradas que pone eventos y tareas en la misma línea de tiempo, para que veas cuándo tienes hueco de verdad para hacer algo. Es un contenedor ligero con base de datos SQLite que controlas tú, y se puede sincronizar con los calendarios que ya usas (CalDAV, Google Calendar, Apple Calendar, Notion, Todoist, Tempo).

> ⚠️ **Proyecto en fase temprana**: el propio README oficial avisa de que el proyecto se mueve rápido, puede haber fallos y habrá cambios incompatibles antes de la versión 1.0. Ten copias de seguridad de la carpeta de datos y actualiza con cuidado.

## ✨ Características principales

- **Eventos y tareas juntos**: vistas de semana, mes, año, línea de tiempo y tabla; arrastra para crear, mover o redimensionar
- **Planificación**: tareas sin fecha esperan en la pestaña de planificación hasta que las arrastras a tu semana
- **Sincronización bidireccional**: CalDAV, Google Calendar, Apple Calendar, Notion, Todoist y Tempo; calendarios publicados como suscripciones de solo lectura
- **Subtareas y dependencias**: disponibilidad, archivos adjuntos, recordatorios push e invitados con respuestas
- **Conexión con IA**: Claude, Cursor y otras herramientas pueden leer y planificar tus calendarios mediante MCP
- **Idiomas y temas**: ocho idiomas (incluido RTL) y temas claro/oscuro
- **Single-user out of the box**: sin inicio de sesión requerido para uso personal

## 📋 Requisitos del sistema

- Docker y Docker Compose instalados
- Puerto **3000** libre en el host
- Una carpeta en tu servidor para los datos (ej. `~/mitra`)
- Opcional: un dominio con HTTPS para acceso externo

## 🐳 Instalación

### Paso 1: Crea el `compose.yaml`

```yaml
# Basado en el compose del README oficial
services:
  mitra:
    image: ghcr.io/a11delavar/mitra:latest
    restart: unless-stopped
    ports:
      - '3000:3000'
    environment:
      # Zona horaria que usa Mitra para mostrar las fechas
      TZ: 'Europe/Madrid'
      # Descomenta cuando tengas una dirección pública
      # MITRA_URL: 'https://mitra.tudominio.com'
    volumes:
      # Todo lo que guarda Mitra está en esta carpeta
      - ~/mitra:/app/data
```

> **Versiones**: `latest` es la versión estable más reciente. Para controlar actualizaciones, fija una etiqueta como `1` (versión mayor 1) o `1.4.2`. La etiqueta `dev` sigue la rama principal y solo es para pruebas.

### Paso 2: Arranca Mitra

```bash
docker compose up -d
# Comprueba que el contenedor pasa el healthcheck
docker compose ps
```

### Paso 3: Abre la aplicación

Entra en **http://localhost:3000**. Fuera de la caja, Mitra es para una sola persona y no pide inicio de sesión.

**Acceso:**
- Desde el propio servidor: `http://localhost:3000`
- Desde otro dispositivo de tu red: `http://IP_DEL_SERVIDOR:3000`

## ⚙️ Configuración

1. **Zona horaria**: ajusta `TZ` en el compose (ej. `America/Mexico_City`, `UTC`)
2. **Dirección pública**: cuando tengas dominio, descomenta y define `MITRA_URL: 'https://mitra.tudominio.com'`
3. **Puerto interno**: si cambias el puerto interno con `MITRA_PORT`, el healthcheck lo usa automáticamente; recuerda ajustar también el mapeo de puertos
4. **Datos persistentes**: la base de datos SQLite vive en `~/mitra` (mapeado a `/app/data`)

## 🚀 Primeros pasos

1. Crea tu primer calendario desde la interfaz web
2. Conecta tus cuentas si quieres sincronizar con otro servicio (CalDAV, Google, Notion, Todoist, Tempo, Apple)
3. Empieza por una tarea sin fecha en la pestaña de **Planificación**
4. Haz una copia de la carpeta `~/mitra`: con eso tienes todo tu contenido respaldado

## 💡 Casos de uso

- **Planificación personal**: ver de un vistazo eventos y tareas en la misma semana
- **Familia**: calendario compartido con varias personas (al activar inicio de sesión multi-usuario)
- **Asistentes de IA**: dejar que Claude, Cursor u otras herramientas lean y organicen tu agenda por MCP
- **Sincronización centralizada**: unificar Google Calendar, Notion, Todoist y CalDAV en una sola vista

## 🔒 Acceso remoto seguro

Para exponer Mitra fuera de tu red local de forma segura:

1. Usa un **reverse proxy** (Nginx Proxy Manager, Traefik, Caddy) con **HTTPS automático** (Let's Encrypt)
2. Define `MITRA_URL: 'https://mitra.tudominio.com'` en el compose
3. No expongas el puerto 3000 directamente a Internet sin TLS ni autenticación adicional
4. Considera añadir **Authelia**, **Authentik** o **Cloudflare Access** delante para 2FA y control de acceso

## 🛠️ Gestión y mantenimiento

### Ver logs
```bash
docker compose logs -f mitra
```

### Actualizar
```bash
docker compose pull
docker compose up -d
```

### Detener Mitra
```bash
docker compose down
```

### Copias de seguridad
La base de datos SQLite está dentro de `~/mitra`. Haz la copia **con el contenedor parado** (`docker compose down`), o con una herramienta que copie la base de datos de forma consistente (ej. `sqlite3 .backup`), para no guardar un fichero a medio escribir.

## 📝 Licencia

**AGPL-3.0-only** — Código fuente en [github.com/a11delavar/mitra](https://github.com/a11delavar/mitra)

---

> 📖 **Artículo original**: [Cómo instalar Mitra en Docker - Calendario y tareas en una sola línea de tiempo](https://genbyte.blogspot.com/2026/11/como-instalar-mitra-en-docker.html)  
> 🎥 **Vídeo tutorial**: [Canal GENBYTE en YouTube](https://www.youtube.com/@genbyte)  
> 💬 **Comunidad**: [Telegram](https://t.me/genbyte) · [Discord](https://discord.gg/genbyte) · [GitHub](https://github.com/genbyte)