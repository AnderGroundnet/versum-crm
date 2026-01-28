# 🍺 VERSUM CRM - Sistema de Gestión

**Estado**: En desarrollo - Fase 1 completada ✅

## 🎯 Descripción

VERSUM CRM es una aplicación web para gestionar clientes, productos, pedidos y finanzas.

### ⭐ Nuevas Características (Fase 1)

- **Pedidos Mejorados**: Múltiples productos por pedido
- **Sistema de Descuentos**: Por unidad, totales y porcentaje
- **Mejor Visualización**: Desglose completo de pedidos

### Características Principales

- 👥 Gestión de clientes y comerciales
- 📦 Gestión de productos y stock
- 💰 Pedidos avanzados
- 📅 Agenda de tareas automáticas
- 💵 Control de finanzas
- ⚙️ Panel de usuarios (CEO/Admin)

## 🚀 Quick Start

### 1. Configurar Supabase (IMPORTANTE)
Lee: [`GUIA_SUPABASE.md`](GUIA_SUPABASE.md)

### 2. Abrir aplicación
- Abre `index.html` en tu navegador
- Login: `ceo@versum.com` / `versum2024`

### 3. Crear un pedido (prueba)
- Productos → Nuevo Pedido
- Agrega múltiples productos
- Aplica descuentos
- Registra el pedido

## 📚 Documentación Completa

| Documento | Propósito |
|-----------|-----------|
| **INICIO_RAPIDO.md** | Guía visual - LEER PRIMERO ⭐ |
| **GUIA_SUPABASE.md** | Configurar BD (con scripts SQL) |
| **CAMBIOS_SUPABASE.md** | Scripts SQL exactos |
| **RESUMEN_CAMBIOS.md** | Qué cambió en el código |
| **GUIA_COMPLETA.md** | Documentación técnica completa |
| **VALIDACION_FINAL.md** | Checklist de verificación |

## 🏗️ Arquitectura

```
index.html (2900 líneas, Vanilla JS)
├── CSS inline (responsive)
├── Login + Autenticación
├── Dashboard con 5 secciones
│   ├── Clientes (CRUD + fases)
│   ├── Productos (CRUD)
│   ├── Pedidos ⭐ (múltiples + descuentos)
│   ├── Agenda (tareas automáticas)
│   ├── Finanzas (ingresos)
│   └── Usuarios (CEO/Admin)
└── Integración Supabase
```

## 🔐 Acceso por Rol

| Rol | Permisos |
|-----|----------|
| **CEO** | Todo. Ver "Usuarios" |
| **Admin** | Todo menos crear usuarios |
| **Comercial** | Solo sus clientes |

## 📊 Tablas Supabase (Actualizado)

- `usuarios` - Perfiles de usuarios
- `clientes` - Clientes con fases
- `productos` - Catálogo
- **`pedidos` ⭐** - Nuevo formato (items_json JSONB)
- **`comisiones_comerciales` ⭐** - Nueva tabla
- `tareas` - Agenda automática
- `historial_actividad` - Auditoría
- `notificaciones` - Notificaciones

## 🛠️ Stack Técnico

- **Frontend**: HTML5 + CSS3 (inline) + Vanilla JavaScript
- **Backend**: Supabase (PostgreSQL + REST API)
- **Auth**: Email/Password
- **Responsive**: Mobile, Tablet, Desktop

## 🎯 Próximas Fases

- [ ] **Fase 2**: Dashboards con analíticas
- [ ] **Fase 3**: Sistema de comisiones automáticas
- [ ] **Fase 4**: Clientes compartidos entre comerciales
- [ ] **Fase 5**: Historial de actividad
- [ ] **Fase 6**: Reportes exportables (PDF/Excel)

## 📱 Dispositivos

- ✅ Desktop
- ✅ Tablet
- ✅ Mobile

Abre en navegador de móvil para probar.

## 🔐 Credenciales de Prueba

```
Email: ceo@versum.com
Contraseña: versum2024
```

## 📞 Soporte

1. Revisa los archivos `*.md`
2. Abre DevTools (F12) en navegador
3. Verifica Supabase Console
4. Revisa `.github/copilot-instructions.md`

---

## ✨ Versiones

- **v1.1.0** - Pedidos mejorados (múltiples productos + descuentos)
- **v1.0.0** - Versión inicial

---

👉 **Próximo paso**: Lee [`INICIO_RAPIDO.md`](INICIO_RAPIDO.md)
