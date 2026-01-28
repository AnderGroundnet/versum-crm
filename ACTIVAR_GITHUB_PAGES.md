# 🚀 ACTIVAR GITHUB PAGES

Tu código ya está en GitHub. Ahora necesitas **activar GitHub Pages** para que la app esté disponible en línea.

## 📋 PASOS PARA ACTIVAR GITHUB PAGES

### 1. Abre tu repositorio en GitHub

Ve a: https://github.com/AnderGroundnet/versum-crm

### 2. Entra en Settings

- Click en pestaña **"Settings"** (arriba a la derecha)
- En el menú izquierdo, busca **"Pages"** (bajo "Code and automation")

### 3. Configura GitHub Pages

En la sección "GitHub Pages":

**Source (Origen):**
- Selecciona: **"Deploy from a branch"**

**Branch:**
- Rama: **"main"**
- Carpeta: **"/ (root)"**

**Luego click "Save"**

### 4. Espera a que se publique (1-2 minutos)

GitHub Pages compilará tu sitio. Verás un mensaje:
```
Your site is published at: https://andergroundnet.github.io/versum-crm/
```

---

## ✅ ¡LISTO!

Tu aplicación estará disponible en:

**https://andergroundnet.github.io/versum-crm/**

---

## 📝 Verificación

Después de activar, puedes:

1. Abre: https://andergroundnet.github.io/versum-crm/
2. Login con: `ceo@versum.com` / `versum2024`
3. ¡Prueba la aplicación!

---

## ⚠️ IMPORTANTE

- La app seguirá necesitando **Supabase configurado** para funcionar
- GitHub Pages solo sirve los archivos HTML/JS/CSS
- La conexión a Supabase debe estar ya configurada en `index.html`

---

## 🔄 Próximas actualizaciones

Cada vez que hagas cambios locales:

```bash
git add -A
git commit -m "tu mensaje"
git push origin main
```

**GitHub Pages se actualizará automáticamente en 1-2 minutos.**

---

## 💡 Alternativa: Usar un dominio personalizado

Si quieres usar un dominio propio (ej: `versum-crm.com`):

1. Ve a Settings → Pages
2. En "Custom domain" agrega tu dominio
3. Sigue las instrucciones de DNS

Pero por ahora, **usa el dominio gratuito de GitHub Pages.**

---

**¡Adelante! 🎊**
