# VERSUM CRM - Instrucciones para Agentes de IA

## Arquitectura General

**VERSUM CRM** es una aplicación monolítica SPA (Single Page Application) basada en HTML5 + Vanilla JavaScript, **NO usa frameworks como React/Vue**. Toda la lógica reside en `index.html` como una clase `VersumCRM` con ~2800 líneas.

### Stack Técnico
- **Frontend**: HTML5 + CSS inline + Vanilla JS (Clase VersumCRM)
- **Backend**: Supabase (PostgreSQL) con autenticación email/password
- **Almacenamiento**: localStorage para sesión del usuario actual
- **Dependencias**: Solo Supabase JS SDK via CDN

## Estructura del Código (Módulos en index.html)

El código está organizado en **6 módulos comentados** dentro de la clase:

1. **Módulo 2**: Configuración + Login
   - Variables Supabase (SUPABASE_URL, SUPABASE_KEY) requeridas en líneas 65-66
   - Validación de credenciales: `ceo@versum.com` / `versum2024`
   - Método: `handleLogin()`

2. **Módulo 3**: Dashboard + Navegación
   - Sistema de tabs: clientes, productos, agenda, finanzas, usuarios (admin-only)
   - Método: `navigateTo(tab)` - cambia contenido según rol

3. **Módulo 4**: Gestión Completa de Clientes
   - CRUD de clientes con fases: contacto → muestras → cliente → perdido
   - Modal reutilizable para crear/editar
   - Control de permisos: comerciales solo ven sus clientes + sin asignar

4. **Módulo 5**: Auditoría + Notificaciones + Tareas Automáticas
   - Tabla historial: `historial_actividad` (no `auditorias`)
   - Métodos: `registrarActividad(accion, tabla, registroId, datos)`, `notificarCEO(tipo, mensaje, datos)`, `crearTareaAutomatica(cliente)`
   - Las tareas se crean automáticamente con delays según fase (contacto: 2d, muestras: 7d, cliente: 30d)

5. **Módulo 6**: Gestión de Productos, Pedidos (Mejorado) y Agenda
   - CRUD productos, crear/listar pedidos
   - **NUEVO**: Pedidos con múltiples productos por pedido
   - **NUEVO**: Sistema de descuentos (por unidad y totales)
   - Sistema de tareas con filtros (pendientes, completadas, vencidas)

6. **Módulo 7**: Finanzas + Gestión de Usuarios
   - Cálculo de ingresos, comisiones, deudas
   - Admin panel para crear/editar usuarios

## Patrones Clave del Proyecto

### Control de Acceso basado en Roles
```javascript
// Roles: 'ceo', 'admin', 'comercial'
if (this.currentUser.rol === 'comercial') {
  // Solo clientes asignados o sin asignar
  query = query.or(`asignado_a.eq.${this.currentUser.id},asignado_a.is.null`);
}
```
- **CEO/Admin**: Acceso total, ven sección "Usuarios"
- **Comercial**: Solo clientes propios

### Modales Reutilizables
Todas las formas usan un pattern similar: `openModal()` → `handleSubmit()` → `closeModal()`
- Modal ID fijo: `modalCliente`, `modalProducto`, etc.
- Modo edición: verifica `this.editingCliente` para diferenciar INSERT vs UPDATE
- Validación: campos requeridos con `required` HTML

### Integración Supabase
```javascript
// Patrón estándar en métodos loadXXX() y handleXXX()
const { data, error } = await supabaseClient
  .from('tabla_nombre')
  .select('campos, relacionados:tabla_fk(id, nombre)')
  .eq('condicion', valor)
  .single(); // o .order() o .or()
```
- **Relaciones**: Join automático con campos especiales como `asignado:usuarios!clientes_asignado_a_fkey()`
- **Filtros**: `.eq()`, `.or()`, `.order()`, `.limit()`

### UI Sistema (CSS inline)
- Colores: Gold `#D4A574`, Black `#000`, Blue `#3b82f6`
- Componentes predefinidos: `.btn`, `.card`, `.modal`, `.badge` (azul/verde/naranja)
- Grid responsive: `.grid-2` con `auto-fit minmax(250px, 1fr)`

## Flujos de Datos Críticos

### Alta de Cliente
1. `openModalCliente()` → Rellena select de comerciales desde `this.comerciales`
2. `handleClienteSubmit()` → INSERT en tabla `clientes`
3. `crearTareaAutomatica()` → Crea tarea en tabla `tareas`
4. `registrarActividad()` → Registra en tabla `auditorias`
5. `notificarCEO()` → Si fase=cliente

### Ciclo de Pedidos
1. Usuario abre modal → Selecciona cliente + productos
2. `handlePedidoSubmit()` → Inserta en `pedidos` + calcula totales
3. Actualiza estado cliente a "cliente" si es nuevo

### Notificaciones Automáticas
- CEO notificado cuando: nuevo cliente activo, pedido > $5000, comercial no activo
- Tabla `notificaciones`: `{usuario_id, tipo, mensaje, datos_json, leido}`

## Nomenclatura Importantes

### Tabla Supabase Requeridas
- `usuarios`: {id, nombre, email, rol, password_hash, activo, comision_pct}
- `clientes`: {id, nombre, empresa, email, telefono, direccion, fase, asignado_a, notas, fecha_creacion, ultima_interaccion}
- `productos`: {id, nombre, precio, categoria, stock, margen_pct}
- `pedidos`: {id, cliente_id, comercial_id, items_json, subtotal, descuento_total, descuento_porcentaje, total, estado, fecha}
  - **items_json es un JSONB array** con estructura: `[{producto_id, producto_nombre, cantidad, precio_unitario, descuento_unitario, subtotal}, ...]`
- `tareas`: {id, cliente_id, comercial_id, descripcion, fecha, completada, tipo}
- `historial_actividad`: {id, usuario_id, accion, tabla, registro_id, datos_despues}
- `notificaciones`: {id, usuario_id, tipo, mensaje, datos, leida}
- `comisiones_comerciales`: {id, comercial_id, porcentaje, activo, fecha_inicio, fecha_fin}

### Tipos de Datos
- **Fase cliente**: 'contacto', 'muestras', 'cliente', 'perdido'
- **Estado tarea**: 'pendiente', 'completada', 'vencida'
- **Rol usuario**: 'ceo', 'admin', 'comercial'

## Flujos de Desarrollo Comunes

### Agregar Nueva Sección (ej: Reportes)
1. Agregar button en nav: `<button onclick="app.navigateTo('reportes')" class="nav-btn">📊 Reportes</button>`
2. En `navigateTo()`: nuevo case para renderizar vista
3. Crear `renderReportesView()` que retorna HTML
4. Crear `loadReportes()` que hace queries a Supabase
5. Usar badge/card existentes para consistencia UI

### Modificar Permisos
- Cambiar condicional en `navigateTo()` o verificar `this.currentUser.rol` en queries
- Todas las queries comercial están en `loadClientes()` como ejemplo

### Debugging
- Variables globales: `localStorage.setItem('versum_user', ...)` para sesión
- Console: `console.error()` en try/catch
- Notificaciones: `showNotification(msg, 3000)` visible en esquina superior derecha

## Flujos de Notificación y Eventos

### Cadena de Eventos al Crear Cliente
1. `handleClienteSubmit()` inserta en `clientes` + registra actividad
2. `crearTareaAutomatica()` crea tarea automática con delays según fase
3. `notificarCEO()` notifica al CEO si fase='cliente'
4. Se llama a `loadClientes()` para refrescar la UI

### Patrones de Notificación
- Usuario ve toasts con `showNotification()` (esquina superior derecha)
- CEO recibe notificaciones en base de datos (tabla `notificaciones`)
- Las actividades comercial se registran automáticamente para auditoría

## Anti-Patrones a Evitar

❌ **NO crear archivos JS/CSS separados** → Todo debe ir en `index.html`
❌ **NO hacer queries sin error handling** → Siempre usar try/catch y `.single()` con cuidado
❌ **NO confiar en localStorage para datos críticos** → Solo almacena sesión usuario
❌ **NO olvidar Role checks** → Verificar permisos antes de mostrar UI/datos
❌ **NO duplicar código HTML** → Usar template literals con `.map()` para listas
❌ **NO editar tablas directamente sin registrar actividad** → Llamar siempre a `registrarActividad()`

## Credenciales de Desarrollo

Reemplazar en líneas 65-66:
- SUPABASE_URL y SUPABASE_KEY (ya configurados, usar como referencia)
- Usuario inicial: `ceo@versum.com` / `versum2024`
