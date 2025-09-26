---
Date: 2024-09-22
Title: 🖥️ Conventional Commits
Description: Una guía rápida y con ejemplos sobre la utilidad de los conventional commits en el desarrollo de software (hecho con IA).
Image: portadas/commit.png
---

# Conventional Commits: una buena práctica
Los **Conventional Commits** son una convención para escribir mensajes de commit de forma clara
y estandarizada.
La idea es que tanto las personas como las herramientas puedan entender fácilmente qué cambios
se hicieron y por qué.

Adoptar Conventional Commits tiene varias ventajas:
- Facilita entender qué cambió en el proyecto solo mirando el historial.
- Permite a las herramientas generar automáticamente changelogs y documentación.
- Asegura consistencia en los mensajes de commit, lo que hace más fácil colaborar.

El formato básico de un commit sigue esta estructura:
- **Type**:
  el tipo de cambio (ej:
  feat, fix, docs, chore).
- **Scope**:
  el alcance del cambio (ej:
  un archivo, un módulo, todo el proyecto).
- **Description**:
  una breve descripción del cambio.
- **Breaking change**:
  sección opcional para indicar si el commit introduce cambios incompatibles.

## Tipos más comunes
Aquí algunos ejemplos de los tipos de commit más usados:

1. **feat**:
   nueva funcionalidad _Ejemplo:_ `feat(user):
   agregar sistema de login`

2. **fix**:
   corrección de errores _Ejemplo:_ `fix(database):
   solucionar bug de consultas lentas`

3. **style**:
   cambios de estilo (no funcionales) _Ejemplo:_ `style(code):
   formatear código de JavaScript`

4. **refactor**:
   cambios internos de código sin modificar la lógica externa _Ejemplo:_ `refactor(auth):
   simplificar lógica de autenticación`

5. **chore**:
   tareas de mantenimiento o soporte _Ejemplo:_ `chore(deploy):
   configurar pipeline de CI/CD`

6. **ci**:
   cambios en procesos de integración continua _Ejemplo:_ `ci(cron):
   agregar tarea cron diaria para limpiar caché`

7. **perf**:
   mejoras de rendimiento _Ejemplo:_ `perf(db):
   indexar columna de productos para optimizar consultas`

8. **build**:
   cambios en la configuración de compilación o dependencias _Ejemplo:_ `build(webpack):
   actualizar configuración de webpack`

9. **revert**:
   revertir un commit previo _Ejemplo:_ `revert(1234):
   deshacer cambio introducido en la versión anterior`

10. **test**:
    añadir o actualizar pruebas _Ejemplo:_ `test(unit):
    actualizar tests unitarios para nueva API`

11. **docs**:
    cambios en la documentación _Ejemplo:_ `docs(contributing):
    añadir guía de contribución`

## Consejos prácticos
Si querés asegurarte de seguir el formato correctamente, podés usar un hook de git o
herramientas como [Commitizen](https://commitizen.github.io/cz-cli/), que te guían paso a paso
al escribir el mensaje.

## Recursos útiles
- Sitio oficial de Conventional Commits:
  [https://www.conventionalcommits.org/en/v1.0.0/](https://www.conventionalcommits.org/en/v1.0.0/)
- Documentación de Commitizen:
  [https://commitizen.github.io/cz-cli/](https://commitizen.github.io/cz-cli/)
- Buenas prácticas para escribir commits:
  [https://blog.devgenius.io/writing-good-commit-messages-with-conventional-commits-8a40e99da2de](https://blog.devgenius.io/writing-good-commit-messages-with-conventional-commits-8a40e99da2de)
