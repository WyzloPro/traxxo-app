# Traxxo App

Experiencia del cliente de Traxxo — `app.traxxo.ai`.

Sitio estático servido en Vercel. Login + dashboard del cliente con datos de GSC, GA4 y Semrush.

## Fase actual: waitlist-only (login únicamente)

El **registro público está cerrado** hasta tener ≥4 proyectos en producción. Las nuevas solicitudes entran por la [landing](https://www.traxxo.ai) → tabla `public.leads_notify` y se gestionan manualmente desde [`traxxo-admin`](https://github.com/WyzloPro/traxxo-admin).

La pantalla de login muestra solo la pestaña **Iniciar sesión** (la pestaña de Registro está oculta y un banner avisa de que el acceso es por invitación).

## Relación con otros repos

- [`traxxo-landing`](https://github.com/WyzloPro/traxxo-landing): landing pública y waitlist (`www.traxxo.ai`)
- [`traxxo-admin`](https://github.com/WyzloPro/traxxo-admin): operación interna y edge functions (`admin.traxxo.ai`)
- `traxxo-app` (este repo): experiencia del cliente

## Estructura

```
traxxo-app/
├── index.html        # login + onboarding (registro oculto en fase actual)
├── dashboard.html    # dashboard del cliente
├── perfil.html       # gestión de cuenta y conexiones (GSC, GA4, Semrush)
└── favicon.svg
```

## Backend

- **Auth**: Supabase Auth (email + password). SMTP transaccional vía Resend.
- **Edge Functions de cliente** versionadas en [`traxxo-admin`](https://github.com/WyzloPro/traxxo-admin) bajo `supabase/functions/`:
  - `collect-gsc`, `collect-ga4`, `collect-semrush` — ingesta de datos
  - `list-gsc-sites`, `list-ga4-properties`, `validate-semrush-key` — onboarding
  - `build-dashboard-snapshot` — snapshot semanal del dashboard

## Deploy

Push a `main` → Vercel auto-deploya en `app.traxxo.ai`.

## Reabrir el registro

Cuando llegue el momento (≥4 proyectos en producción):

1. Restaurar la pestaña Registro en el componente de auth (quitar el `display:none` y el banner)
2. Activar **leaked password protection** en Supabase Auth dashboard
3. Considerar abrir signup gradualmente (códigos de invitación, dominios permitidos, etc.)
