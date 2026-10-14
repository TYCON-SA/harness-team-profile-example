# Perfil del equipo — ejemplo

[English](README.en.md)

Un **perfil del equipo** de ejemplo para el arnés, armado para una organización inventada: **acme**. Sirve para que tu
organización vea cómo se arma el suyo, lo copie y lo adapte. Ningún nombre de acá es real.

## Qué es un perfil del equipo

Es un repo **privado** de tu organización, chico, con lo que comparte todo el equipo: la configuración, dónde vive cada
repo, y los revisores y skills propios. Cada máquina que instala el arnés con `--team` lo baja, lo valida y lo pone
encima del arnés público. Cuando el perfil cambia, el cambio le llega a cada máquina en su próxima actualización.

## Qué trae este ejemplo

```
team-layer.json                                         la declaración del perfil (obligatoria)
perfil-equipo.json                                      la configuración del equipo
team-repos.json                                         dónde vive cada repo del arnés de la organización
packs/acme/pack.json                                    un paquete de revisores: cuándo corre cada uno
agents/acme-logging-reviewer.md                         un revisor del equipo
skills/infrastructure/git-workflow/SKILL.md             una skill: las convenciones de git del equipo
skills/infrastructure/git-workflow/references/*.md      material de apoyo de la skill
README.md, README.en.md                                 esto (no se instala)
```

| Archivo | Para qué sirve |
|---|---|
| `team-layer.json` | Dice que este repo es un perfil del equipo: su nombre y la versión mínima del arnés que lo entiende. Sólo admite `schema`, `name` y `requires`, sin comentarios. |
| `perfil-equipo.json` | Qué integraciones están prendidas, cómo se actualizan las máquinas, qué paquetes de revisores corren y cuánto instala el arnés. Sólo valores del equipo: un correo, un chat o un permiso lo invalida. |
| `team-repos.json` | Dónde vive cada repo: de dónde instalan las máquinas (`public`), este repo (`profile`) y el de las tareas programadas (`operations`). |
| `packs/acme/pack.json` | Agrupa los revisores del equipo y dice cuándo entra cada uno en la revisión de un cambio. |
| `agents/acme-logging-reviewer.md` | Un revisor: un agente de sólo lectura que el pipeline lanza sobre un cambio, con su catálogo de reglas. |
| `skills/infrastructure/git-workflow/` | Instrucciones que el asistente carga cuando la tarea encaja con su descripción. |

Cada archivo trae un comentario corto: en los `.json`, la clave `__doc` (las claves que empiezan con `__` son
comentarios); en los `.md`, un comentario HTML; en `pack.json`, su `description` y la `note` del revisor.

## Cómo copiarlo y adaptarlo

1. **Creá un repo privado** en tu organización, en GitHub o en Azure DevOps, con acceso sólo para tu equipo. Por
   ejemplo `harness-team-profile`.
2. **Copiá estos archivos** a ese repo.
3. **Cambiá `acme` por el nombre de tu organización** en `team-layer.json` (`name`), `team-repos.json` y
   `perfil-equipo.json` (`ado.org`, si usás Azure DevOps; si no, borrá el bloque).
4. **Ajustá `team-repos.json`** con tus repos reales. Si está en GitHub: `{ "platform": "github", "org", "repo" }`; en
   Azure DevOps: `{ "platform": "azure", "org", "project", "repo" }`. También lo podés editar con la página
   `harness.js team-config`, que valida lo que escribís.
5. **Elegí la configuración** en `perfil-equipo.json`. Empezá con `install.profile: minimal` y `update.apply: ask`.
6. **Quedate con lo que te sirve.** El revisor, el paquete y la skill son ejemplos: renombralos con tu prefijo (si
   cambiás el nombre de un archivo o carpeta, cambiá también su `name:`) o borralos. Un perfil puede tener sólo
   `team-layer.json`, `perfil-equipo.json` y `team-repos.json`.
7. **Instalá con el perfil**, desde el arnés público:

   ```bash
   node scripts/setup/harness.js install --version latest --team <tu-org>/harness-team-profile
   ```

   En Azure DevOps, `--team` lleva la dirección del repo: `https://dev.azure.com/<org>/<proyecto>/_git/<repo>`. El
   permiso para leer un repo privado se guarda una vez con `harness.js config set github.token` (o `azure.token`), que
   lo pide sin mostrarlo.

## Lo que el arnés acepta en un perfil

El arnés revisa el perfil entero antes de instalarlo. **Si un solo archivo no cumple, no instala nada del perfil** y
dice cuál y por qué.

- **Rutas admitidas:** `perfil-equipo.json`, `team-repos.json`, `packs/<paquete>/pack.json`, `agents/<nombre>.md`, los
  `.md` de una skill bajo `skills/` (con su `SKILL.md`, hasta tres carpetas por debajo de `skills/`) y los `.md` de la
  raíz de `onboarding/` (el texto que un dev nuevo le pega al asistente). En la raíz pueden ir además `README*`,
  `LICENSE*` y `.gitignore`, que no se instalan.
- **Nada que se ejecute:** ni guiones, ni hooks, ni enlaces, ni archivos ejecutables. El frontmatter de un agente o una
  skill no puede traer `hooks:` ni `mcpServers:`.
- **Nombres:** el `name:` de un agente es el nombre de su archivo; el de una skill, el de su carpeta; el de un paquete,
  el de su carpeta. Ninguno puede llamarse como un agente, una skill o un paquete que ya trae el arnés.
- **Topes:** hasta 300 archivos y 1 MB en total.

## Probarlo

Con el perfil subido, una instalación con `--team` dice si lo aceptó. En una máquina ya instalada,
`node scripts/setup/harness.js doctor` dice si el perfil está entero («capa del equipo … todo en su lugar») y `harness.js version --json` (clave `team`) de qué commit vino.
