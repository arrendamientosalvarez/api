# API Arrendamientos Álvarez

Proxy en Node.js + Express + TypeScript que expone inmuebles al frontend y consulta la API Softinm (Empatia).

## Requisitos

- Node.js 16+
- npm o yarn

## Configuración

Crear `api/.env` (no versionar):

```env
PORT=8080
API_USER=nombreEmpresa
API_PASSWORD=token_jwt_bearer
API_BASE_URL=https://zonaclientes.softinm.com
```

| Variable | Descripción |
|----------|-------------|
| `API_USER` | Nombre de empresa en la URL de Softinm (`nombreEmpresa`) |
| `API_PASSWORD` | Token Bearer JWT (Empatia.Co Ltda) |
| `API_BASE_URL` | Host de la API Softinm |
| `PORT` | Puerto del servidor (opcional, default 8080) |

Documentación del proveedor: [`documentacionApiempatia.md`](documentacionApiempatia.md).

Guía de migración desde la API antigua: [`../MIGRACION_API_SOFTINM.md`](../MIGRACION_API_SOFTINM.md).

## Scripts

```bash
npm run dev    # desarrollo con recarga (ts-node-dev)
npm run build  # compila a dist/build
npm run start  # ejecuta build/index.js
npm run lint   # eslint
```

## Endpoint

### `GET /api/estates`

Proxy hacia `POST /api/inmuebles/consultar_inmuebles/{API_USER}` en Softinm.

Query params soportados:

| Parámetro | Descripción |
|-----------|-------------|
| `cantidadporpagina` | Tamaño de página (default 4) |
| `pagina` | Número de página (default 1) |
| `codigo` | Consecutivo del inmueble |
| `destinacion` | `venta`, `arriendo`, o valores de ruta `Venta`/`Arriendo` (se normalizan) |

Ejemplo:

```
GET http://localhost:8080/api/estates?cantidadporpagina=12&pagina=1&destinacion=venta
```

Respuesta: array JSON de inmuebles mapeados (`MapperResponse`).

## Estructura

```
src/
  index.ts       # Express + CORS
  controller/    # Rutas HTTP
  service/       # Integración Softinm (POST + Bearer)
  mapper/        # Adaptación al contrato del frontend
  interfaces/    # Tipos
  config/        # Variables de entorno
```

## Frontend

El cliente React debe usar `VITE_API_BASE_URL` apuntando a este servidor (ej. `http://localhost:8080`), no a Softinm directamente.