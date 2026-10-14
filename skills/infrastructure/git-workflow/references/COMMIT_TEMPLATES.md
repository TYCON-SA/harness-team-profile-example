<!-- Para qué sirve este archivo: material de apoyo de la skill git-workflow. El asistente lo lee cuando SKILL.md lo
     nombra; no hace falta que tenga frontmatter. Una skill puede tener varios archivos así, siempre `.md`. -->

# Plantillas de commit

## Funcionalidad

```
feat(<alcance>): <qué agrega>

<Por qué hacía falta, en una o dos oraciones.>

Refs: <identificador del pedido o la story>
```

## Corrección

```
fix(<alcance>): <qué estaba mal, dicho como lo que ahora anda>

Causa: <qué lo provocaba>.
Arreglo: <qué se cambió>.
```

## Refactor

```
refactor(<alcance>): <qué se reorganizó>

Sin cambio de comportamiento: <cómo se comprobó (pruebas que siguen verdes)>.
```

## Cambio que rompe compatibilidad

```
feat(<alcance>)!: <qué cambia>

BREAKING CHANGE: <qué tiene que hacer quien lo usa para adaptarse>.
```

## Co-autoría

```
Co-authored-by: Nombre Apellido <correo de la persona>
```
