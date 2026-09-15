# Team Barrera — Panel de Control

Panel en tiempo real para llevar el control del equipo de Team Barrera: atletas
(presenciales y online), pagos mensuales, rotación de contacto para los
atletas online, y eventos/carreras compartidos entre atletas de distintas
ciudades.

## Versión independiente (`index.html`) — la actual

`index.html` es la app real, hospedada con **GitHub Pages** y con su propia
base de datos en **Firebase (Firestore)**, sin depender de Claude ni de
ninguna cuenta ahí. Es lo que abre el equipo en el día a día.

- Proyecto de Firebase: `teambarrera-38761`
- La conexión usa un inicio de sesión anónimo automático (invisible para
  quien abre la app) + reglas de Firestore que exigen esa sesión — están en
  `firestore.rules` en este mismo repo, para pegar en la consola de Firebase
  (Firestore Database → Reglas).
- La primera vez que la base de datos esté vacía, la propia app siembra el
  roster completo (ver `SEED_DATA` al inicio del `<script>` en `index.html`)
  — es una migración de un solo uso, no se repite si ya hay atletas.

### Publicarla / actualizarla

GitHub Pages sirve `index.html` en la raíz del repo automáticamente en
cuanto Pages está activado (Settings → Pages → Deploy from branch → la rama
correspondiente → `/ (root)`). Cualquier cambio a `index.html` en esa rama
se refleja solo con volver a cargar la página (unos segundos/minutos de
propagación).

## Versión anterior (`team-barrera.html`) — Claude Artifact

Primer intento, publicado como Claude Artifact (usaba la base de datos
integrada de Claude). Se conserva como referencia; ya no es la versión que
usa el equipo.

Enlace: https://claude.ai/artifact/23DbNcZCJpKQ2i4rvhzjHQ

## Modelo de datos (colecciones de Firestore)

**`athletes/<id>`** — un documento por atleta:
- `nombre`, `modalidad` (`Presencial` | `Online`)
- `grupo` (solo presencial: Avanzados, Intermedios 1/2, Principiantes, Triatlón)
- `ciudad` (solo online)
- `mensualidad` (MXN), `diaCobro` (día del mes)
- `meta` (nota libre, opcional)
- `eventoId` (solo online: referencia a `events/<id>`, o `null`)
- `estado` (`Activo` | `Inactivo`)
- `rotationDay` (día de la semana asignado para contacto, solo online)

**`events/<id>`** — un evento/carrera: `{ nombre, createdAt }`. Varios
atletas online pueden compartir el mismo `eventoId`.

**`payments/<YYYY-MM>/entries/<athleteId>`** — existe (`{paid:true, paidAt}`)
si ese atleta ya pagó ese mes; no existe si sigue pendiente.

**`settings/access`** — `{ code }`: el código de acceso compartido que pide
el panel al abrir.
