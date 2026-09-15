# Team Barrera — Panel de Control

Panel en tiempo real para llevar el control del equipo de Team Barrera: atletas
(presenciales y online), pagos mensuales y rotación de contacto para los
atletas online.

## Versión en vivo

La app corre publicada como un Claude Artifact (no requiere hosting ni
servidor propio). Los datos viven en una base de datos en tiempo real
asociada al artifact: cualquier cambio hecho desde cualquier dispositivo se
sincroniza al instante en todos los demás.

Enlace: https://claude.ai/artifact/23DbNcZCJpKQ2i4rvhzjHQ

## Este repositorio

`team-barrera.html` es el código fuente de esa misma app (React sin build
step, cargado desde CDN). Se guarda aquí como referencia y respaldo — para
actualizar la versión en vivo hay que volver a publicarlo como Artifact,
editar este archivo no cambia el enlace en vivo por sí solo.

### Modelo de datos (colección `athletes` en el Artifact)

Cada atleta es un documento con:

- `nombre`, `modalidad` (`Presencial` | `Online`)
- `grupo` (solo presencial: Avanzados, Intermedios 1/2, Principiantes, Triatlón)
- `ciudad` (solo online)
- `mensualidad` (MXN), `diaCobro` (día del mes)
- `meta` (carrera u objetivo, opcional)
- `estado` (`Activo` | `Inactivo`)
- `rotationDay` (día de la semana asignado para contacto, solo online)

Los pagos se guardan por periodo en `payments/<YYYY-MM>/entries/<athleteId>`.

Se migró el roster original (169 atletas) desde el archivo estático que
subiste (`Team Barrera - Panel de Control 3.html`) hacia esta base de datos
compartida.
