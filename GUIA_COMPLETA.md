# 📖 GUÍA COMPLETA - TODO LO QUE NECESITAS SABER

## 🎯 Objetivo General

Tu aplicación VERSUM CRM está en desarrollo. Has pedido mejoras progresivas.

**FASE 1 (HOY)**: Sistema de pedidos mejorado ✅ COMPLETADO
**FASE 2**: Analíticas y dashboards
**FASE 3**: Comisiones automáticas
**FASE 4**: Gestión de clientes compartidos

---

## 📝 QUÉ SE HA HECHO HOY (FASE 1)

### Cambios en el Código (`index.html`)
```
✅ Constructor: agregada propiedad this.pedidoItems = []
✅ Modal de pedidos: completamente rediseñado
✅ 7 nuevos métodos de JavaScript para manejar múltiples productos
✅ Función loadPedidos() reescrita para mostrar detalles completos
```

### Cambios en la Base de Datos (FALTA HACER)
```
⏳ Tabla pedidos: modificar estructura
⏳ Tabla comisiones_comerciales: crear nueva
⏳ Tabla usuarios: agregar 2 columnas
```

---

## 🔧 PRÓXIMOS PASOS (Muy Importante)

### 1. Ejecutar cambios en Supabase (CRÍTICO)
Lee el archivo `GUIA_SUPABASE.md` y ejecuta todos los scripts SQL.

**Sin esto, la aplicación NO funcionará.**

### 2. Probar la funcionalidad
Abre tu app → Productos → Nuevo Pedido → Intenta crear un pedido con múltiples productos

### 3. Reportar problemas
Si algo falla, revisa:
- Navegador: Abre DevTools (F12) → Console → ¿Hay errores rojo?
- Supabase: ¿Se insertó el pedido en la tabla?
- Red: ¿La conexión a Supabase está OK?

---

## 🎯 PRÓXIMAS FASES (Roadmap)

### FASE 2: Dashboards con Analíticas
Lo que el usuario pidió:

**Para CEO/Admin:**
- [ ] Ventas totales de la empresa (suma de todos los pedidos)
- [ ] Ranking de comerciales por ventas
- [ ] Número de clientes por comercial
- [ ] Pipeline (visualizar clientes en cada fase: contacto → muestras → cliente → perdido)
- [ ] Filtros: por comercial, fecha, producto

**Para Comercial:**
- [ ] Sus propias métricas (solo sus pedidos)
- [ ] Opcional: su posición en ranking (sin ver números de otros)

**Implementación:**
- Nueva sección "Analíticas" en el nav
- Gráficos visuales (números grandes, barras, líneas)
- Filtros interactivos

### FASE 3: Sistema de Comisiones
Lo que el usuario pidió:

- [ ] CEO/Admin asignan porcentaje fijo por comercial
- [ ] Tabla `comisiones_comerciales` (ya creada)
- [ ] Cálculo automático: total_venta * porcentaje = comisión
- [ ] Dashboard para CEO ver: "Comercial X ganó €200 en comisiones este mes"

**Implementación:**
- Interfaz para CEO: ver/editar comisión de cada comercial
- Cálculo automático en `handlePedidoSubmit()`
- Mostrar en dashboard financiero

### FASE 4: Clientes Compartidos/Duplicados
Lo que el usuario pidió:

- [ ] Permitir que 2 comerciales trabajen el mismo cliente (en casos especiales)
- [ ] El comercial secundario ve el cliente en su calendario
- [ ] CEO debe confirmar la asignación temporal
- [ ] Alertas de duplicados

**Implementación:**
- Agregar columna `comercial_secundario` a tabla `clientes`
- Flujo de notificación para CEO
- UI: mostrar cliente con "2 comerciales asignados"

### FASE 5: Historial de Actividad
Lo que el usuario pidió:

- [ ] Cada usuario puede ver su historial del último mes
- [ ] Solo para corregir errores o recordar trabajo anterior
- [ ] CEO/Admin pueden ver historial de todos

**Implementación:**
- Query a tabla `historial_actividad` con filtros de fecha
- UI: panel con lista de cambios hechos

### FASE 6: Reportes Exportables
Lo que el usuario pidió:

- [ ] CEO y Admin pueden exportar PDF/Excel
- [ ] Reportes: ventas por mes, ranking, clientes, etc.

**Implementación:**
- Usar librería `jspdf` para PDF
- Usar librería `xlsx` para Excel
- Botón "Exportar" en cada dashboard

---

## 💡 DECISIONES IMPORTANTES

### Sobre contraseñas (usuario preguntó)
- ✅ **CEO/Admin pueden VER contraseñas**
- ✅ Columna `password_visible` agregada a `usuarios`
- ⚠️ **SEGURIDAD**: Las contraseñas deben estar hasheadas en la BD, no en texto plano
- 💡 **Recomendación**: Usa "reset de contraseña" en lugar de "ver"

### Sobre competencia entre comerciales (usuario preguntó)
- ✅ **NO mostrar ranking competitivo directo**
- ✅ **SÍ mostrar**: Métricas de cada uno (ventas totales, clientes, comisión ganada)
- 💡 **Idea**: Dashboard personal para cada comercial: "Tú: €5000, Meta: €6000, Falta: €1000"

### Sobre dispositivos móviles (usuario preguntó)
- ✅ **Web responsive es suficiente**
- ✅ CSS ya está optimizado para móvil
- 💡 **Mejora**: Media queries para pantallas < 768px

---

## 📚 Archivos de Referencia

```
/workspaces/versum-crm/
├── index.html ← El archivo principal (AQUÍ ESTÁ TODO)
├── .github/copilot-instructions.md ← Guía para agentes IA
├── README.md ← Info general
├── CAMBIOS_SUPABASE.md ← Scripts SQL (ejecutar)
├── GUIA_SUPABASE.md ← Paso a paso para Supabase
├── RESUMEN_CAMBIOS.md ← Qué se cambió hoy
└── GUIA_COMPLETA.md ← Este archivo
```

---

## 🚀 Recomendaciones

### Para Desarrollo Futuro
1. **Usa commits claros**: "feat: agregar analíticas" no "arreglar"
2. **Prueba en Supabase**: Antes de implementar, verifica estructura de datos
3. **Mantén localStorage limpio**: Solo para `versum_user`
4. **CSS inline: OK pero complejo**: Si crece mucho, considera separar a `<style>`

### Para Seguridad
1. **Nunca guardes credenciales en localStorage**: Solo el usuario actual
2. **Valida SIEMPRE en backend**: El frontend puede fallar
3. **Contraseñas: usa hashing**: Bcrypt o Argon2 (Supabase lo hace automático)
4. **Limpia localStorage al logout**: Ya lo hace ✅

### Para Performance
1. **Lazy load de datos**: Solo cuando se necesitan
2. **Caché en JavaScript**: `this.comerciales` está bien
3. **Índices en BD**: Ya creados para pedidos
4. **Limita cantidad de registros**: `limit(50)` en queries

---

## ❓ Preguntas Frecuentes

**P: ¿Cuándo tengo lista la Fase 2?**
R: Después de hacer los cambios en Supabase. La Fase 2 (dashboards) durará ~2-3 horas.

**P: ¿Necesito cambiar el código del login?**
R: No. El login actual funciona perfecto.

**P: ¿Puedo cambiar el color de oro?**
R: Sí. Busca `#D4A574` en index.html y cámbialo.

**P: ¿Qué pasa si se cae Supabase?**
R: La app muestra error. No hay offline mode configurado (todavía).

**P: ¿Puedo usar la app en móvil?**
R: Sí. Abre en navegador del móvil. Es responsive.

**P: ¿Se pueden eliminar datos?**
R: Sí, CEO/Admin pueden eliminar clientes, productos, usuarios. No hay papelera de reciclaje.

---

## 📞 Soporte

Si necesitas ayuda:

1. **Revisa los archivos GUIA_*.md**
2. **Abre DevTools (F12) en navegador**: ¿Hay errores?
3. **Revisa Supabase Console**: ¿Los datos están?
4. **Prueba en incógnito**: A veces es caché del navegador

---

## ✨ Próximo Paso

👉 **Abre `GUIA_SUPABASE.md` y ejecuta los scripts SQL**

Cuando termines, vuelve aquí para la Fase 2.
