# 📚 GUÍA PASO A PASO - CONFIGURACIÓN EN SUPABASE

## ⚠️ IMPORTANTE: Lee esto ANTES de hacer nada

Tu aplicación ya está lista, pero necesita cambios en Supabase para soportar la nueva funcionalidad de pedidos mejorada.

## 🎯 Objetivo

Modificar la tabla `pedidos` para que pueda tener **múltiples productos por pedido** + **descuentos**.

---

## 📋 PASO 1: Ir a Supabase

1. Abre [supabase.com](https://supabase.com) en tu navegador
2. Inicia sesión con tu cuenta
3. Entra en tu proyecto VERSUM CRM
4. En el menú izquierdo, busca **"SQL Editor"**

---

## 🔧 PASO 2: Ejecutar el Script SQL

En **SQL Editor**, copia y pega **UNO** de estos scripts:

### Opción A: Si NO tienes datos importantes en la tabla `pedidos` (RECOMENDADO)

```sql
-- Eliminar tabla antigua
DROP TABLE IF EXISTS pedidos CASCADE;

-- Crear tabla nueva con estructura correcta
CREATE TABLE pedidos (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  cliente_id UUID NOT NULL REFERENCES clientes(id) ON DELETE CASCADE,
  comercial_id UUID NOT NULL REFERENCES usuarios(id) ON DELETE CASCADE,
  items_json JSONB NOT NULL DEFAULT '[]',
  subtotal NUMERIC NOT NULL DEFAULT 0,
  descuento_total NUMERIC DEFAULT 0,
  descuento_porcentaje NUMERIC DEFAULT 0,
  total NUMERIC NOT NULL DEFAULT 0,
  estado TEXT DEFAULT 'pendiente',
  fecha TIMESTAMP DEFAULT NOW(),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Crear índices para mejor rendimiento
CREATE INDEX pedidos_cliente_id_idx ON pedidos(cliente_id);
CREATE INDEX pedidos_comercial_id_idx ON pedidos(comercial_id);
CREATE INDEX pedidos_fecha_idx ON pedidos(fecha DESC);
```

### Opción B: Si tienes datos que quieres migrar

```sql
-- Crear tabla nueva
CREATE TABLE pedidos_new (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  cliente_id UUID NOT NULL REFERENCES clientes(id) ON DELETE CASCADE,
  comercial_id UUID NOT NULL REFERENCES usuarios(id) ON DELETE CASCADE,
  items_json JSONB NOT NULL DEFAULT '[]',
  subtotal NUMERIC NOT NULL DEFAULT 0,
  descuento_total NUMERIC DEFAULT 0,
  descuento_porcentaje NUMERIC DEFAULT 0,
  total NUMERIC NOT NULL DEFAULT 0,
  estado TEXT DEFAULT 'completado',
  fecha TIMESTAMP DEFAULT NOW(),
  created_at TIMESTAMP DEFAULT NOW()
);

-- Copiar datos antiguos (convirtiendo formato antiguo al nuevo)
INSERT INTO pedidos_new (cliente_id, comercial_id, items_json, subtotal, total, estado, fecha)
SELECT 
  cliente_id, 
  comercial_id,
  CASE 
    WHEN producto_id IS NOT NULL THEN jsonb_build_array(jsonb_build_object(
      'producto_id', producto_id,
      'cantidad', cantidad,
      'precio_unitario', precio_unitario,
      'descuento_unitario', 0,
      'subtotal', cantidad * precio_unitario
    ))
    ELSE '[]'::jsonb
  END,
  total,
  total,
  'completado',
  fecha
FROM pedidos
WHERE client_id IS NOT NULL;

-- Renombrar tablas
ALTER TABLE pedidos RENAME TO pedidos_old;
ALTER TABLE pedidos_new RENAME TO pedidos;

-- Crear índices
CREATE INDEX pedidos_cliente_id_idx ON pedidos(cliente_id);
CREATE INDEX pedidos_comercial_id_idx ON pedidos(comercial_id);
CREATE INDEX pedidos_fecha_idx ON pedidos(fecha DESC);
```

Después de migrar, puedes eliminar la tabla antigua:
```sql
DROP TABLE IF EXISTS pedidos_old CASCADE;
```

---

## ✅ PASO 3: Crear tabla de Comisiones (IMPORTANTE)

En el mismo **SQL Editor**, ejecuta este script:

```sql
-- Crear tabla de comisiones por comercial
CREATE TABLE comisiones_comerciales (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  comercial_id UUID NOT NULL UNIQUE REFERENCES usuarios(id) ON DELETE CASCADE,
  porcentaje NUMERIC NOT NULL DEFAULT 5,
  activo BOOLEAN DEFAULT TRUE,
  fecha_inicio TIMESTAMP DEFAULT NOW(),
  fecha_fin TIMESTAMP,
  notas TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Crear índice
CREATE INDEX comisiones_comercial_id_idx ON comisiones_comerciales(comercial_id);
```

---

## 📋 PASO 4: Verificar cambios en tabla `usuarios`

En **SQL Editor**, ejecuta:

```sql
-- Agregar columnas si no existen
ALTER TABLE usuarios ADD COLUMN IF NOT EXISTS comision_pct NUMERIC DEFAULT 5;
ALTER TABLE usuarios ADD COLUMN IF NOT EXISTS password_visible BOOLEAN DEFAULT FALSE;
```

---

## 🔍 PASO 5: Verificar que todo está bien

En el menú lateral de Supabase, va a **"Database"** → **"Tables"** y verifica:

✅ **Tabla `pedidos` existe** con columnas:
  - `cliente_id`
  - `comercial_id`
  - `items_json` (tipo JSONB)
  - `subtotal`, `descuento_total`, `descuento_porcentaje`
  - `total`, `estado`, `fecha`

✅ **Tabla `comisiones_comerciales` existe** con columnas:
  - `comercial_id`
  - `porcentaje`
  - `activo`
  - `fecha_inicio`, `fecha_fin`

✅ **Tabla `usuarios` tiene** columnas:
  - `comision_pct`
  - `password_visible`

---

## 🎉 ¡LISTO!

Cuando hayas completado todos los pasos, la aplicación va a:

✅ Permitir crear pedidos con **múltiples productos**
✅ Permitir aplicar **descuentos** por producto o al pedido completo
✅ Mostrar pedidos con **desglose detallado** de productos
✅ Registrar **comisiones** para cada comercial

---

## ❓ Si algo falla...

**Error: "Table already exists"**
→ Significa que la tabla ya existe. Usa la **Opción B** (migración)

**Error: "Foreign key constraint"**
→ Hay datos inconsistentes. Intenta limpiar la tabla antigua primero:
```sql
DELETE FROM pedidos WHERE cliente_id IS NULL OR comercial_id IS NULL;
```

**Error: "JSON format"**
→ No importa, la aplicación lo maneja automáticamente

---

## 📞 Siguiente paso

Cuando termines:
1. Abre la aplicación en tu navegador
2. Ve a la sección **"Productos"**
3. Haz clic en **"+ Nuevo Pedido"**
4. ¡Prueba agregando múltiples productos!

¿Necesitas ayuda? Revisa la sección de debajo...
