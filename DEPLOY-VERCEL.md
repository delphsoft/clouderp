# RetailOS Argentina — Guía de Deploy en Vercel

## Estructura del proyecto

```
retailos-argentina/
├── index.html          ← Landing page (selector POS / Admin)
├── vercel.json         ← Configuración de rutas Vercel
├── pos/
│   └── index.html      ← Terminal POS (cajeros)
└── admin/
    └── index.html      ← Backoffice Admin (gerentes)
```

---

## PASO 1 — Instalar Vercel CLI

```bash
npm install -g vercel
```

Verificar instalación:
```bash
vercel --version
```

---

## PASO 2 — Crear cuenta Vercel (si no tenés)

1. Ir a https://vercel.com/signup
2. Registrarse con GitHub, GitLab o email
3. Confirmar el email

---

## PASO 3 — Preparar los archivos

Asegurate de tener esta estructura de carpetas con los 4 archivos descargados:

```
retailos-argentina/
├── index.html
├── vercel.json
├── pos/
│   └── index.html
└── admin/
    └── index.html
```

---

## PASO 4 — Deploy desde terminal

```bash
# Entrar a la carpeta del proyecto
cd retailos-argentina

# Iniciar sesión en Vercel
vercel login

# Deploy (primera vez)
vercel

# Te va a preguntar:
# → Set up and deploy? → Y
# → Which scope? → seleccioná tu cuenta
# → Link to existing project? → N
# → What's your project's name? → retailos-argentina
# → In which directory is your code? → ./
# → Want to override settings? → N

# Para ver la URL del deploy:
vercel --prod
```

---

## PASO 5 — URLs que obtenés

Después del deploy vas a tener:

| Ruta | Descripción | Para quién |
|---|---|---|
| `https://tu-proyecto.vercel.app/` | Landing page | Todos |
| `https://tu-proyecto.vercel.app/pos` | Terminal POS | Cajeros |
| `https://tu-proyecto.vercel.app/admin` | Backoffice Admin | Gerentes |

---

## PASO 6 — Dominio personalizado (opcional)

```bash
# Agregar dominio propio
vercel domains add tupOS.com.ar

# Asignar al proyecto
vercel alias set tu-proyecto.vercel.app tupOS.com.ar
```

O desde el panel de Vercel:
1. Ir a tu proyecto en https://vercel.com/dashboard
2. Settings → Domains → Add Domain
3. Escribir `pos.tunegocio.com.ar` (o el dominio que tengas)
4. Configurar DNS en tu registrador:
   - Tipo: `CNAME`
   - Nombre: `pos`
   - Valor: `cname.vercel-dns.com`

---

## PASO 7 — Re-deploy cuando actualices archivos

```bash
# Actualizar producción después de cambios
vercel --prod
```

---

## Deploy alternativo — Sin terminal (Drag & Drop)

1. Ir a https://vercel.com/new
2. Clic en "Import" → "Browse" (si tenés Git)  
   O usar **Vercel CLI** como en Paso 4

### Con GitHub (recomendado para producción):

```bash
# 1. Crear repositorio en GitHub
git init
git add .
git commit -m "RetailOS Argentina v1.0"
git remote add origin https://github.com/tuusuario/retailos-argentina.git
git push -u origin main

# 2. En Vercel: New Project → Import Git Repository
# → Cada vez que hagas push a main, Vercel hace deploy automático
```

---

## Variables de entorno (para producción real)

En Vercel Dashboard → Tu proyecto → Settings → Environment Variables:

```
MODO_API_KEY=tu_clave_modo
MERCADOPAGO_ACCESS_TOKEN=tu_token_mp
GOOGLE_PAY_MERCHANT_ID=tu_id_gpay
SUPABASE_URL=https://xxx.supabase.co
SUPABASE_ANON_KEY=tu_clave_supabase
```

---

## Proteger el Admin con contraseña (Vercel Password Protection)

1. Vercel Dashboard → Tu proyecto → Settings → Security
2. "Password Protection" → Enable
3. Configurar contraseña para `/admin`

O con middleware de Vercel (gratis en plan Pro):

```javascript
// middleware.js en raíz del proyecto
export function middleware(request) {
  if (request.nextUrl.pathname.startsWith('/admin')) {
    // verificar sesión o redirigir a login
  }
}
```

---

## Costos de Vercel

| Plan | Costo | Límites |
|---|---|---|
| Hobby (gratis) | $0/mes | 100GB bandwidth, deploy ilimitados |
| Pro | $20/mes | Sin límites, password protection, analytics |
| Enterprise | Custom | SLA, soporte dedicado |

Para una tienda con tráfico normal, el plan **Hobby gratuito** es suficiente.

---

## Checklist final antes de lanzar

- [ ] Deploy funcionando en URL de Vercel
- [ ] `/pos` abre el POS correctamente
- [ ] `/admin` abre el backoffice
- [ ] Probado en Chrome, Firefox y móvil
- [ ] Dominio personalizado configurado (opcional)
- [ ] Backup del código en GitHub
- [ ] Credenciales de APIs de pago reales configuradas
- [ ] CUIT y datos del comercio actualizados en el código
