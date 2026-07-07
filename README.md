# fam-listas 🛒

PWA de **listas compartidas familiares** (compras, tareas y pendientes) con sincronización en vivo.

**App:** https://angulodev.github.io/fam-listas/

## Stack

- Single-file `index.html` — vanilla JS, sin build, hash-based routing
- **Supabase** (proyecto `cms-mdd`, ref `mseiqleparjnchmgnwwa`): Auth con Google OAuth, Postgres con RLS, Realtime
- Backend: módulo `fam_*` del repo [cms-mdd](https://github.com/angulodev/cms-mdd) (migraciones 004 y 005)
- PWA: `manifest.json` + `sw.js` (shell cache-first, datos siempre por red)

## Cómo funciona

1. Entras con Google → el trigger `handle_new_user()` crea tu `sys_user`; la app completa nombre/avatar desde los metadatos de Google.
2. Creas una lista → el trigger te hace `owner` y se genera un **código de 6 caracteres**.
3. Compartes el código → el otro usa "Unirme" → `fn_fam_join_by_code()` lo agrega como `editor`.
4. Todo cambio en ítems se propaga en vivo vía Realtime a todos los miembros.

La **publishable key** en el código es pública por diseño: todo el acceso real lo controla el RLS (aislamiento por membresía de lista).

## Roles

`owner` (creador: puede eliminar la lista) · `editor` (CRUD de ítems) · `viewer` (solo lectura)

---
Autores: Francisco Angulo (angulodev) + Claude (socio técnico)
