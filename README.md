# Práctica OAuth 2.0: iniciar sesión con Twitch

Página en **Next.js** con un botón **"Continuar con Twitch"** usando OAuth 2.0 a través de [Auth.js](https://authjs.dev). Después de iniciar sesión muestra los datos del perfil que Twitch comparte con la app, sin conocer nunca la contraseña.

La guía de la práctica está en **[PRACTICA.md](PRACTICA.md)**.

## Archivos importantes

```
auth.ts                              Configuración de OAuth: aquí se agregan los proveedores
app/page.tsx                         Página: botones de inicio de sesión y datos del usuario
app/api/auth/[...nextauth]/route.ts  Rutas de OAuth (login, callback, logout)
.env.local                           Tus credenciales (NO se sube a GitHub)
```

## 1. Credenciales de Twitch

1. Activa la verificación en dos pasos en tu cuenta de Twitch si aún no lo has hecho.
2. Abre [dev.twitch.tv/console/apps](https://dev.twitch.tv/console/apps) y registra una aplicación de tipo **Website Integration** con cliente **Confidencial**.
3. Registra `http://localhost:3000/api/auth/callback/twitch` como URL de OAuth de redireccionamiento.
4. Copia el **Client ID** y genera un **Client Secret**.

## 2. Correr en tu computadora

Necesitas [Node.js 20 o superior](https://nodejs.org). En la carpeta del proyecto:

```bash
npm install
cp .env.example .env.local
npx auth secret
open -e .env.local
```

`npx auth secret` agrega `AUTH_SECRET` a `.env.local` automáticamente. En el archivo que se abre, pega las credenciales de Twitch y guarda:

```
AUTH_TWITCH_ID=tu-client-id
AUTH_TWITCH_SECRET=tu-client-secret
```

```bash
npm run dev
```

Abre [http://localhost:3000](http://localhost:3000).

> En Windows usa `copy` en lugar de `cp` y abre `.env.local` con el Bloc de notas.

## 3. Subir a GitHub

Crea un repositorio vacío en [github.com/new](https://github.com/new) y en la carpeta del proyecto:

```bash
git init
git add .
git commit -m "Práctica OAuth 2.0"
git branch -M main
git remote add origin https://github.com/TU-USUARIO/oauth-demo.git
git push -u origin main
```

## 4. Desplegar en Vercel

1. En [vercel.com](https://vercel.com) entra con GitHub → **Add New → Project** → importa el repositorio.
2. En **Environment Variables** agrega `AUTH_SECRET`, `AUTH_TWITCH_ID` y `AUTH_TWITCH_SECRET` (copia los valores de tu `.env.local`).
3. **Deploy.** Obtendrás una URL como `https://oauth-demo-xxxx.vercel.app`.
4. Agrega el dominio de producción en la configuración de la aplicación de Twitch y registra `https://oauth-demo-xxxx.vercel.app/api/auth/callback/twitch` como URL de redireccionamiento. Usa las credenciales correspondientes en Vercel.

> Usa el dominio de producción de Vercel; las URLs de *preview* cambian y el proveedor las rechazará.

## Problemas comunes

| Error | Solución |
| --- | --- |
| *Falta configurar las credenciales* | Revisa que `.env.local` tenga `AUTH_SECRET` y las credenciales, y reinicia `npm run dev`. |
| `redirect_uri_mismatch` | La URL de callback no está registrada exactamente igual en el proveedor. |
| Agregaste variables y no funcionan | Detén el servidor (Ctrl + C) y vuelve a correr `npm run dev`. |
