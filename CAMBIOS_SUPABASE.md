# 📋 Cambios Necesarios en Supabase

## ⚠️ IMPORTANTE: Ejecuta estos cambios en Supabase ANTES de actualizar el código

### Paso 1: Modificar tabla `pedidos`

La tabla `pedidos` necesita cambiar su estructura para soportar múltiples productos por pedido.

**Script SQL a ejecutar en Supabase (SQL Editor):**

```sql
-- Alternativa 1: Si la tabla existe, modificarla (CUIDADO con datos existentes)
ALTER TABLE pedidos ADD COLUMN IF NOT EXISTS items_json JSONB;
ALTER TABLE pedidos ADD COLUMN IF NOT EXISTS descuento_total NUMERIC DEFAULT 0;
ALTER TABLE pedidos ADD COLUMN IF NOT EXISTS descuento_porcentaje NUMERIC DEFAULT 0;
ALTER TABLE pedidos DROP COLUMN IF EXISTS producto_id CASCADE;
ALTER TABLE pedidos DROP COLUMN IF EXISTS cantidad CASCADE;
ALTER TABLE pedidos DROP COLUMN IF EXISTS precio_unitario CASCADE;

-- Alternativa 2: Si quieres crear tabla nueva (MÁS SEGURO si tienes datos)
CREATE TABLE pedidos_v2 (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  cliente_id UUID NOT NULL REFERENCES clientes(id),
  comercial_id UUID NOT NULL REFERENCES usuarios(id),
  items_json JSONB NOT NULL DEFAULT '[]',
  subtotal NUMERIC NOT NULL DEFAULT 0,
  descuento_total NUMERIC DEFAULT 0,
  descuento_porcentaje NUMERIC DEFAULT 0,
  total NUMERIC NOT NULL DEFAULT 0,
  estado TEXT DEFAULT 'pendiente',
  fecha TIMESTAMP DEFAULT NOW(),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Copiar datos antiguos si es necesario
INSERT INTO pedidos_v2 (cliente_id, comercial_id, items_json, subtotal, total, estado, fecha)
SELECT 
  cliente_id, 
  comercial_id,
  jsonb_build_array(jsonb_build_object(
    'producto_id', producto_id,
    'cantidad', cantidad,
    'precio_unitario', precio_unitario,
    'descuento', 0,
    'subtotal', cantidad * precio_unitario
  )),
  total,
  total,
  'completado',
  fecha
FROM pedidos;

-- Renombrar tablas
ALTER TABLE pedidos RENAME TO pedidos_old;
ALTER TABLE pedidos_v2 RENAME TO pedidos;
```

### Paso 2: Crear tabla `comisiones_comerciales` (nueva)

```sql
CREATE TABLE comisiones_comerciales (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  comercial_id UUID NOT NULL UNIQUE REFERENCES usuarios(id),
  porcentaje NUMERIC NOT NULL DEFAULT 5,
  activo BOOLEAN DEFAULT TRUE,
  fecha_inicio TIMESTAMP DEFAULT NOW(),
  fecha_fin TIMESTAMP,
  notas TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### Paso 3: Agregar columnas a tabla `usuarios` (si no están)

```sql
ALTER TABLE usuarios ADD COLUMN IF NOT EXISTS comision_pct NUMERIC DEFAULT 5;
ALTER TABLE usuarios ADD COLUMN IF NOT EXISTS password_visible BOOLEAN DEFAULT FALSE;
```

## ✅ Checklist de verificación en Supabase

Después de ejecutar los scripts, verifica en Supabase Console:

- [ ] Tabla `pedidos` tiene columnas: `items_json`, `descuento_total`, `descuento_porcentaje`
- [ ] Tabla `pedidos` NO tiene columnas: `producto_id`, `cantidad`, `precio_unitario`
- [ ] Tabla `comisiones_comerciales` existe con estructura correcta
- [ ] Tabla `usuarios` tiene columnas: `comision_pct`, `password_visible`

## 🔄 Migración de datos (si tienes pedidos antiguos)

Si tienes datos existentes y ejecutaste "Alternativa 2", puedes eliminar la tabla vieja:

```sql
DROP TABLE pedidos_old CASCADE;
```

## ℹ️ Estructura esperada de items_json

Cada item en el array `items_json` debe tener esta estructura:

```json
{
  "producto_id": "uuid...",
  "producto_nombre": "Nombre del producto",
  "cantidad": 10,
  "precio_unitario": 25.50,
  "descuento_unitario": 2.50,
  "subtotal": 230.00,
  "margen_pct": 20
}
```

El array puede contener múltiples productos en un solo pedido.
