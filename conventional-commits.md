---
Date: 2024-09-22
Title: 🖥️ Conventional Commits
Description: Un resumen (hecho con IA) sobre la utilidad y como utilizar los conventional commits segun cada caso.
Image: portadas/commit.png
---

# Conventional commits, a good practice
## Definition
Conventional Commits is a specification for writing commit messages in a way that is easy for
humans and machines to understand.
It defines a standard format for commit messages, which consists of the following parts:

![imagen](/media/http1.0.png)

### Parts
- _Type_:
  The type of commit, such as feat, fix, docs, or chore.
- _Scope_:
  The scope of the change, such as a-file, a-module, or the-project.
- _Description_:
  A brief description of the change.
- _Breaking change_:
  An optional section to indicate if the commit introduces a breaking change.

### Usage example
1. feat:
   Nueva funcionalidad
   - Ejemplo:
     "feat(user):
     Agregar sistema de login"
2. fix:
   Corrección de errores
   - Ejemplo:
     "fix(database):
     Solucionar bug de consultas lentas"
4. style:
   Cambios estilísticos (sin impacto funcional)
   - Ejemplo:
     "style(code):
     Formatear código de JavaScript"
5. refactor:
   Refactorings internos
   - Ejemplo:
     "refactor(auth):
     Simplificar lógica de autenticación"
7. chore:
   Tareas administrativas
   - Ejemplo:
     "chore(deploy):
     Configurar CI/CD pipeline"
8. ci:
   Cambios en procesos de integración continua
   - Ejemplo:
     "ci(cron):
     Agregar tarea cron diaria para limpieza de cache"
9. perf:
   Optimización de rendimiento
   - Ejemplo:
     "perf(db):
     Indexar columna de productos para mejorar consultas"
10. build:
    Cambios en scripts de construcción
    - Ejemplo:
      "build(webpack):
      Actualizar configuración de webpack"
11. revert:
    Revertir cambios anteriores
    - Ejemplo:
      "revert(1234):
      Deshacer cambio de versión anterior"
12. test:
    Actualización de tests existentes
    - Ejemplo:
      "test(unit):
      Actualizar tests unitarios para nueva API"
15. docs:
    Actualización de documentación
    - Ejemplo:
      "docs(contributing):
      Añadir guía de contribución"

Estos tipos de títulos ayudan a categorizar claramente los cambios en el proyecto, facilitando
la revisión y comprensión del historial de desarrollo.
To use conventional commits, you can use a git hook or a tool like commitizen:
https://commitizen.github.io/cz-cli/ to help you write the commit messages in the correct
format.

## Benefits
Here are some of the benefits of using conventional commits:

It makes it easier for humans to understand the changes that have been made to a project.
It makes it easier for machines to parse commit messages and generate change logs, changelogs,
and other documentation.
It helps to ensure that commit messages are consistent and easy to find.

If you are working on a project that uses conventional commits, I encourage you to follow the
specification and write clear and concise commit messages.
It will make your project easier to maintain and understand for everyone involved.

## Aditional resources
Here are some additional resources that you may find helpful:

- Conventional Commits website:
  [https://www.conventionalcommits.org/en/v1.0.0/](https://www.conventionalcommits.org/en/v1.0.0/)
- Commitizen documentation:
  [https://commitizen.github.io/cz-cli/](https://commitizen.github.io/cz-cli/)
- Writing good commit messages:
  [https://blog.devgenius.io/writing-good-commit-messages-with-conventional-commits-8a40e99da2de](https://blog.devgenius.io/writing-good-commit-messages-with-conventional-commits-8a40e99da2de)
