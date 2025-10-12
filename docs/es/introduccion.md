# Introducción a Git

## ¿Qué es Git?

Git es un **sistema de control de versiones distribuido** diseñado para rastrear
cambios en archivos y coordinar el trabajo entre múltiples desarrolladores.
Fue creado por Linus Torvalds en 2005 para gestionar el desarrollo del kernel
de Linux.

### Características principales

- **Distribuido:** Cada desarrollador tiene una copia completa del historial del
  proyecto.
- **Rápido y eficiente:** Está optimizado para trabajar con proyectos de cualquier
  tamaño.
- **Seguro:** Utiliza hashes SHA-1 para garantizar la integridad de los datos.
- **Flexible:** Soporta múltiples flujos de trabajo y estrategias de ramificación.
- **Offline:** Permite trabajar sin conexión a internet.

### ¿Por qué usar Git?

1. **Control de Cambios:**

   Git te permite registrar cada cambio realizado en tu código, quién lo hizo y cuándo.
   Puedes ver el histórico completo de tu proyecto en cualquier momento.

2. **Colaboración:**

   Múltiples desarrolladores pueden trabajar en el mismos proyecto simultáneamente
   sin pisarse los cambios entre sí. Git facilita la integración de cambios
   diferentes personas.

3. **Ramificación**

   Puedes crear ramas (branches) para trabajar en nuevas características sin
   afectar el código principal. Luego puedes fusionar (merge) los cambios
   cuando estén listos.

4. **Recuperación de Errores:**

   Si cometes un error, puedes revertir cambios o volver a una versión anterior
   del código fácilmente.

5. **Rastrabilidad:**

   Cada cambio incluye información sobre quién lo hizo, cuándo y por qué (a través
   del mensaje del commit). Esto es crucial para auditorías y mantenimiento.

6. **Experiencia Laboral:**

   Git es una herramienta estándar en la industria. Aprenderlo es esencial para cualquier
   desarrollador de software.

### Conceptos Básicos

**Repositorio (Repository):**

Un repositorio es una carpeta que contiene todos los archivos de tu proyecto y
el historial completo de cambios. Existen dos tipos:

- **Repositorio Local:** Está en tu máquina.
- **Repositorio Remoto:** En un servidor (GitHub, GitLab, Bitbucket, etc.).

**Commit:**

Un commit es una "fotografía" del estado de tu proyecto en un momento específico.
Contiene:

- Los cambios realizados.
- Un mensaje descriptivo.
- Autor y fecha del cambio.
- Un identificador único (hash).

**Rama (Branch):**

Una rama es una línea independiente de desarrollo. La rama por defecto se llama `main`
o `master`. Las ramas permiten trabajar en características nuevas sin afectar el
código principal.

**Push y Pull:**

- **Push:** Enviar tus commits locales al repositorio remoto.
- **Pull:** Descargar cambios del repositorio remoto a tu copia local.

**Fusión (Merge):**

Combinar cambios de una rama a otra. Por ejemplo, fusionar una rama de características
con la rama principal.

**Conflictos:**

Ocurre cuando Git no puede fusionar automáticamente dos cambios porque afectan las
mismas líneas de código.

### Plataformas de Repositorios remotos

**GitHub:**

- La plataforma más popular para alojar repositorios Git.
- Propiedad de Microsoft.
- Gratuita para repositorios públicos y privados.
- Excelente para colaboración y proyectos de código abierto.

**GitLab:**

- Alternativa open-source a GitHub.
- Ofrece tanto alojamiento en la nube como auto-hospedaje.
- Características avanzadas de CI/CD integradas.
- Popular en entornos empresariales.

**Bitbucket:**

- Propiedad de Atlassian.
- Integración con otras herramientas Atlassian (Jira, Confluence).
- Gratuita para equipos pequeños.
- Popular en entornos empresariales.

### Flujo de Trabajo Básico

```txt
1. Clonar (Clone) -> Descargar repositorio remoto a tu máquina

2. Crear Rama (Branch) -> Crear una rama para trabajar en una nueva característica

3. Modificar Archivos -> Hacer cambios en el código

4. Preparar Cambios (Stage) -> Marcar archivos para el próximo commit

5. Hacer Commit -> Guardar los cambios con un mensaje descriptivo

6. Hacer Push -> Enviar commits al repositorio remoto

7. Crear Pull Request -> Solicitar que cambios sean revisados e integrados

8. Merge -> Fusionar cambios en la rama principal
```

### Próximos Pasos

Una vez entiendas estos conceptos básicos, puedes:

1. [Instalar Git](/docs/es/instalacion.md) - Configurar Git en tu máquina.
2. [Configuración Inicial](/docs/es/configuracion-inicial.md) - Primeros pasos después de instalar.
3. [Conceptos Básicos](/docs/es/conceptos-basicos.md) - Profundiza en los términos clave.

### Recursos Adicionales

- [Documentación Oficial de Git](https://git-scm.com/doc)
- [GitHub Guides](https://guides.github.com/)
- [Pro Git Book](https://git-scm.com/book/en/v2)
