# ⚡ INICIO RÁPIDO - PRÓXIMOS PASOS

## 🎯 TÚ ESTÁS AQUÍ

Has pedido mejoras en tu aplicación VERSUM CRM.

**Cambio PRINCIPAL**: Sistema de pedidos mejorado (múltiples productos + descuentos)

✅ **El código ya está actualizado**
⏳ **Falta: Actualizar la Base de Datos**

---

## 📋 CHECKLIST (Haz esto ahora)

### Paso 1: Leer la guía (5 min)
```
Abre este archivo: GUIA_SUPABASE.md
(Está en tu carpeta del proyecto)
```

### Paso 2: Ejecutar cambios en Supabase (10 min)
```
1. Abre https://supabase.com
2. Inicia sesión → Tu proyecto VERSUM CRM
3. Click "SQL Editor"
4. Copia los scripts de GUIA_SUPABASE.md
5. Pégalos y ejecuta
```

### Paso 3: Verificar cambios (2 min)
```
Supabase → Database → Tables
✅ ¿Existe tabla "pedidos" con items_json?
✅ ¿Existe tabla "comisiones_comerciales"?
✅ ¿Tabla "usuarios" tiene comision_pct?
```

### Paso 4: Probar en la app (5 min)
```
1. Abre tu aplicación
2. Login con ceo@versum.com / versum2024
3. Ve a "Productos"
4. Click "+ Nuevo Pedido"
5. Intenta agregar 2-3 productos
6. Aplica descuentos
7. Registra el pedido
```

### Paso 5: Reportar si algo falla
```
Si hay error:
1. Abre navegador: F12 (DevTools)
2. Ve a "Console"
3. Copia el error rojo
4. Revisa el archivo "CAMBIOS_SUPABASE.md"
```

---

## 📊 Qué ves ahora vs Qué verás después

### ANTES (Sistema antiguo)
```
Modal de Pedido:
┌─────────────────────┐
│ Nuevo Pedido        │
├─────────────────────┤
│ Cliente: [select]   │
│ Producto: [select]  │ ← Solo UNO
│ Cantidad: [número]  │
│ Total: €0.00        │
├─────────────────────┤
│ [Cancelar] [Guardar]│
└─────────────────────┘
```

### DESPUÉS (Sistema nuevo)
```
Modal de Pedido:
┌─────────────────────────────────┐
│ Nuevo Pedido (Múltiples Prod.)  │
├─────────────────────────────────┤
│ Cliente: [select]               │
│ ─── Agregar Productos ───       │
│ Producto: [select]              │
│ Cantidad: [número]              │
│ Descuento/unidad: [número]      │
│ [+ Agregar Producto]            │
│                                 │
│ ─── Productos Agregados ───     │
│ ✓ Botellas Cerveza: 10x €2.50   │ ← Múltiples
│ ✓ Latas Soda: 5x €1.50          │
│ ✓ Barriles Agua: 2x €10.00      │
│ [Eliminar] buttons              │
│                                 │
│ ─── Descuentos ───              │
│ Descuento Total €: [número]     │
│ Descuento %: [número]           │
│                                 │
│ Subtotal: €45.00                │
│ Descuentos: -€2.25              │
│ TOTAL: €42.75                   │
├─────────────────────────────────┤
│ [Cancelar] [Registrar Pedido]   │
└─────────────────────────────────┘
```

### En el Historial de Pedidos

**ANTES:**
```
Fecha    │ Cliente  │ Producto  │ Cantidad │ Total
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
28/01/26 │ Bar XXX  │ Cerveza   │ 10       │ €25.00
```

**DESPUÉS:**
```
Client: Bar XXX (Empresa Ejemplo)
Fecha: 28/01/2026

Productos (3 items):
  • 10x Botellas Cerveza @ €2.50/u (-€0.50/u = €20.00)
  • 5x Latas Soda @ €1.50/u (sin descuento = €7.50)
  • 2x Barriles Agua @ €10.00/u (-€1.00/u = €18.00)

Subtotal: €45.50
Descuentos: -€3.15
TOTAL: €42.35
```

---

## 🎉 Después de Supabase (Qué sigue)

Una vez hayas hecho los cambios en Supabase:

### Próximas Mejoras Disponibles

1. **Dashboard con Analíticas** (2-3 horas)
   - Ver ventas totales
   - Ranking de comerciales
   - Gráficos bonitos

2. **Sistema de Comisiones** (1-2 horas)
   - CEO asigna % a cada comercial
   - Se calcula automáticamente

3. **Gestión de Clientes Compartidos** (2-3 horas)
   - 2 comerciales en 1 cliente
   - Notificaciones de confirmación

4. **Reportes Exportables** (1-2 horas)
   - Exportar a PDF/Excel
   - Datos de ventas, clientes, etc.

---

## ❓ Preguntas Rápidas

**P: ¿Necesito programar para esto?**
R: NO. Solo copiar/pegar scripts SQL en Supabase.

**P: ¿Se pierden los datos antiguos?**
R: La "Opción B" en GUIA_SUPABASE.md migra los datos.

**P: ¿Cuánto tiempo toma?**
R: 20 minutos máximo (incluye verificación).

**P: ¿Si algo falla?**
R: No pasa nada. Puedes intentar de nuevo. Los datos están seguros.

**P: ¿Dónde ejecuto los scripts?**
R: Supabase → SQL Editor (está en el menú izquierdo).

---

## 📞 IMPORTANTE

### Si estás atascado

1. **Lee GUIA_SUPABASE.md** (tiene TODO explicado paso a paso)
2. **Revisa DevTools** (F12 en navegador → Console)
3. **Comprueba Supabase** (¿se guardó el pedido?)

### Si algo falla en Supabase

**Error: "Table already exists"**
→ Usa la Opción B (migración) en lugar de Opción A

**Error: "Foreign key"**
→ Limpia la tabla antigua primero (ver GUIA_SUPABASE.md)

**Error: "JSON format"**
→ No importa, la app lo maneja

---

## ✅ Finalizado

Cuando hayas completado todo:

1. ✅ Leído GUIA_SUPABASE.md
2. ✅ Ejecutado scripts en Supabase
3. ✅ Verificado cambios en BD
4. ✅ Probado en la app
5. ✅ Creado un pedido con múltiples productos

**Tu aplicación está LISTA para el siguiente feature.**

---

## 🚀 Siguiente Paso

👉 **Abre el archivo `GUIA_SUPABASE.md`**

Te va a guiar paso a paso por todo el proceso.

No hay nada que temer, es muy simple.

**¡Adelante! 🎊**
