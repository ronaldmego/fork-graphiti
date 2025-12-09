# Contribución: Fix Graphiti MCP Server

**PR:** https://github.com/getzep/graphiti/pull/1094
**Fecha:** 2025-12-04
**Autor:** Ronald Mego

---

## Resumen

Corregimos 2 bugs críticos que impedían usar el MCP server de Graphiti.

---

## Bug 1: Modelos OpenAI inexistentes

**Problema:** El código referenciaba modelos que no existen (`gpt-5-mini`, `gpt-5-nano`, `gpt-4.1`), causando error de API.

```
ERROR - The model `gpt-5-mini` does not exist or you do not have access to it.
```

**Fix:** Reemplazamos por `gpt-4o-mini` (modelo válido).

**Archivos modificados:**
- `graphiti_core/llm_client/openai_base_client.py`
- `mcp_server/src/config/schema.py`
- `mcp_server/config/default_config.yaml`
- `mcp_server/config/docker-compose-config.yaml`
- `mcp_server/config/local_config.yaml`
- `mcp_server/config/zep_cloud_config.yaml`

---

## Bug 2: Race condition en índices Neo4j

**Problema:** Al iniciar el MCP server, múltiples queries intentaban crear el mismo índice concurrentemente, fallando con `EquivalentSchemaRuleAlreadyExists`.

**Fix:** Agregamos manejo de excepción en `_execute_index_query` que ignora el error cuando el índice ya existe.

**Archivo modificado:**
- `graphiti_core/driver/neo4j_driver.py`

---

## Commits

```
9192441 fix: Replace non-existent model names with valid gpt-4o-mini
ea074c7 fix: Handle race condition in concurrent Neo4j index creation
```

---

## Validaciones

- ✅ `ruff format` - Sin errores
- ✅ `ruff check` - Sin errores
- ✅ `pyright` - 0 errores de tipos
- ✅ MCP server inicia correctamente
- ✅ 9 tools disponibles y funcionales
