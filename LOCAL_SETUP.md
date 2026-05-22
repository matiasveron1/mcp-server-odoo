# Local Odoo MCP Lab

Objetivo: probar un agente IA conectado a Odoo con riesgo bajo, empezando en modo lectura.

## Estado del repo

- Repo clonado: `ivnvxd/mcp-server-odoo`
- Runtime requerido: Python 3.10+
- Esta maquina tiene Python 3.10, pero no tiene `uv` ni `pip` instalado todavia.

## Opcion recomendada para la primera prueba

Usar `ODOO_YOLO=read`. Este modo no requiere instalar el modulo MCP en Odoo y solo permite lectura por XML-RPC.

Crear `.env` desde `.env.example` y ajustar:

```env
ODOO_URL=https://tu-odoo.com
ODOO_DB=tu_base
ODOO_USER=ai.agent@tuempresa.com
ODOO_PASSWORD=xxxxx
ODOO_YOLO=read
ODOO_LOCALE=es_ES
ODOO_MCP_TRANSPORT=streamable-http
ODOO_MCP_HOST=127.0.0.1
ODOO_MCP_PORT=8000
```

Si usas API key en vez de password:

```env
ODOO_API_KEY=xxxxx
ODOO_USER=ai.agent@tuempresa.com
```

En modo YOLO el proyecto exige `ODOO_USER` incluso cuando se usa API key.

## Pruebas v1

Validar primero estas operaciones:

- listar modelos disponibles;
- buscar clientes en `res.partner`;
- buscar productos en `product.product` o `product.template`;
- consultar stock desde productos/cuantos;
- leer facturas vencidas en `account.move`.

## Despues de validar lectura

Para crear cotizaciones en borrador hay dos caminos:

1. Instalar el modulo MCP de Odoo y habilitar permisos finos por modelo.
2. Usar `ODOO_YOLO=true` solo en una base de prueba aislada.

No activar al inicio:

```env
ODOO_MCP_ENABLE_METHOD_CALLS=true
```

Ese flag permite invocar metodos publicos de Odoo y puede confirmar ventas, publicar facturas u otras acciones sensibles si el usuario tecnico tiene permisos.

## Herramientas MCP relevantes

- `search_records`
- `get_record`
- `list_models`
- `create_record`
- `update_record`
- `delete_record`
- `post_message`
- `aggregate_records`
- `call_model_method` solo si se habilita explicitamente con full YOLO

## Siguiente paso tecnico

Instalar `uv`, instalar dependencias y levantar el servidor HTTP:

```bash
uv sync
uv run mcp-server-odoo --transport streamable-http --host 127.0.0.1 --port 8000
```

Endpoint MCP:

```text
http://127.0.0.1:8000/mcp/
```
