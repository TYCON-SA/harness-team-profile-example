---
name: git-workflow
description: Convenciones de git del equipo acme — mensajes de commit, ramas, merges y pull requests. Usar cuando se pregunta por el formato de un commit, cómo nombrar una rama, cómo mergear o qué revisar en un pull request, y antes de commitear o mergear a main.
metadata:
  author: acme-team
  version: "1.0.0"
---

<!-- Para qué sirve este archivo: es una skill del equipo, instrucciones que el asistente carga cuando la tarea
     encaja con la `description:` de arriba. El `name:` tiene que ser el de su carpeta (git-workflow). La capa del
     equipo sólo instala los `.md` de una skill (este SKILL.md y los de references/): nada que se ejecute. Adaptá
     el contenido a cómo trabaja tu equipo; esto es un punto de partida. -->

# Git: convenciones del equipo

Cómo escribimos commits, nombramos ramas y mergeamos en los repos de acme.

## 1. Mensajes de commit (Conventional Commits)

```
<tipo>(<alcance>): <qué cambia, en presente>

[cuerpo opcional: por qué]

[pie opcional: referencias, co-autores]
```

Tipos: `feat` (funcionalidad nueva), `fix` (corrección), `refactor` (sin cambio de comportamiento), `docs`, `test`,
`chore` (mantenimiento), `perf`, `style`.

```bash
feat(pedidos): agregar el filtro por fecha
fix(api): validar el correo antes de guardar
docs: explicar cómo correr las pruebas en la guía
```

Más plantillas: [references/COMMIT_TEMPLATES.md](references/COMMIT_TEMPLATES.md).

## 2. Ramas

- `main`: lo que está en producción. Protegida: no se empuja directo ni se fuerza.
- `dev`: integración.
- Trabajo: `feature/<nombre>`, `fix/<nombre>`, `refactor/<nombre>`, `docs/<nombre>`, siempre desde `dev`.

```
feature/<nombre>  →  dev  (pruebas, integración)  →  main  (merge con resumen)
```

## 3. Merge a main

El merge a `main` lleva un mensaje que resume TODO lo que entra desde el último merge, agrupado por tipo:

```bash
git log origin/main..dev --oneline      # qué entra
git checkout main
git merge dev --no-ff -m "merge: dev -> main - <título>

### Funcionalidades
- feat(pedidos): ...

### Correcciones
- fix(api): ..."
```

**Antes de empujar a main, el proyecto tiene que compilar y sus pruebas pasar.** Si falla, no se empuja: se arregla,
se commitea el arreglo y se vuelve a verificar.

## 4. Pull requests

- Un pull request, un tema. Título con el mismo formato que un commit.
- La descripción dice qué cambia, por qué y cómo se probó.
- No se mergea con pruebas en rojo ni con comentarios sin contestar.

## Lo que no se hace

- Commits genéricos («arreglos», «cambios», «update»).
- `git push --force` sobre `main` o `dev`.
- Saltear los controles de git (`--no-verify`).
- Commits de cientos de archivos mezclando temas.
