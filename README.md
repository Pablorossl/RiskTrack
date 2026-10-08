# RiskTrack

RiskTrack es un Proyecto Final de Máster en Desarrollo Full Stack orientado a resolver la dificultad de mantener una visión centralizada, actualizada y trazable de los riesgos tecnológicos de una empresa y de las acciones destinadas a tratarlos. Su objetivo es desarrollar una aplicación web que conecte cada riesgo con sus evaluaciones, responsables, acciones e historial para apoyar la priorización y el seguimiento hasta el cierre documentado.

## Estado del proyecto

Actualmente están documentados el problema, el alcance y una propuesta de diseño. El repositorio contiene este README, las instrucciones del PFM, las instrucciones de trabajo, los documentos de `docs/` y el archivo `.gitignore`. Todavía no contiene código de la aplicación, dependencias, pruebas ni configuración de despliegue.

La documentación distingue las decisiones confirmadas, las propuestas de diseño y las funcionalidades implementadas. Están confirmados el alcance de una empresa con usuarios internos, los tres roles y sus permisos, las dos vistas React y las condiciones de cierre y actualización de progreso. El modelo de datos y los detalles técnicos siguen pendientes de concretar; la documentación no acredita su implementación.

## Marco académico

La referencia académica es [INSTRUCCIONES-PFM.md](INSTRUCCIONES-PFM.md), que conserva la transcripción íntegra de la guía, sus requisitos, procedimiento de entrega, defensa y criterios de evaluación. [AGENTS.md](AGENTS.md) recoge las condiciones de trabajo del repositorio.

El diseño de RiskTrack contempla Django con modelos, vistas, plantillas, autenticación y persistencia, junto con dos vistas React que consuman datos reales del backend. La entrega requiere un despliegue funcional en una URL pública y un repositorio GitHub público con README y commits significativos. Docker es opcional.

## Estructura

La documentación se organiza en la raíz del repositorio y en `docs/`:

```text
RiskTrack/
├── .gitignore
├── AGENTS.md
├── INSTRUCCIONES-PFM.md
├── README.md
└── docs/
    ├── 01-problema.md
    ├── 02-requisitos.md
    ├── 03-usuarios.md
    ├── 04-casos-de-uso.md
    ├── 05-modelo-datos.md
    ├── 06-arquitectura.md
    ├── 07-seguridad.md
    └── 08-decisiones-mvp.md
```

## Documentación

| Archivo | Contenido |
| --- | --- |
| [01-problema.md](docs/01-problema.md) | Fundamento, problema, destinatarios, objetivos, solución, aportación, alcance y comprobaciones previstas. |
| [02-requisitos.md](docs/02-requisitos.md) | Requisitos del PFM, alcance del producto, aceptación y entregables. |
| [03-usuarios.md](docs/03-usuarios.md) | Roles, acciones permitidas y permisos confirmados para el MVP. |
| [04-casos-de-uso.md](docs/04-casos-de-uso.md) | Flujos previstos por rol, resultados y errores. |
| [05-modelo-datos.md](docs/05-modelo-datos.md) | Entidades, relaciones, restricciones y persistencia propuestas. |
| [06-arquitectura.md](docs/06-arquitectura.md) | Tecnologías y justificación, componentes, integración React y despliegue previsto. |
| [07-seguridad.md](docs/07-seguridad.md) | Diseño de autenticación, validación, CSRF/XSS, contraseñas, secretos, HTTPS y copias de seguridad. |
| [08-decisiones-mvp.md](docs/08-decisiones-mvp.md) | Decisiones confirmadas de alcance y negocio, recomendaciones técnicas y consecuencias para la implementación. |

El alcance de RiskTrack consiste en un registro central de riesgos de TI que cubra el ciclo **Identificar → Evaluar → Priorizar → Asignar → Tratar → Monitorizar → Cerrar**.

## Secuencia prevista de desarrollo

La implementación se organizará en cinco fases. Todas están pendientes. El alcance, los roles y las condiciones de cierre y actualización de progreso están confirmados en [02-requisitos.md](docs/02-requisitos.md) y [03-usuarios.md](docs/03-usuarios.md). Los detalles técnicos pendientes se concretarán antes de implementar las funciones que dependan de ellos.

| Fase | Trabajo previsto | Comprobación del avance |
| --- | --- | --- |
| 1. Estructura Django y autenticación | Crear la base del proyecto, definir dependencias y configuración, e incorporar plantillas, sesiones y permisos por rol. | Arranque reproducible, inicio y cierre de sesión y rechazo de accesos no autorizados. |
| 2. Riesgos y evaluaciones | Incorporar modelos y migraciones, registro de riesgos, formularios, evaluación y cálculo de prioridad en el backend. | Persistencia tras recargar, rechazo de valores inválidos y cálculo 4 × 5 = 20 como CRÍTICO. |
| 3. Acciones e historial | Incorporar asignaciones, acciones de tratamiento, progreso, evaluaciones sucesivas y transiciones hasta el cierre documentado. | Flujo de tratamiento y cierre con permisos adecuados y conservación del historial. |
| 4. Integración React | Incorporar el registro filtrable y el dashboard mediante datos reales de Django, con estados de carga, error y ausencia de datos. | Ambas vistas reflejan cambios persistidos y respetan el alcance de cada usuario. |
| 5. Despliegue y evidencias | Desplegar en una URL pública con HTTPS, verificar configuración y restauración de copias, y preparar repositorio público, memoria PDF o Word y vídeo de hasta cinco minutos. | Flujo completo en la URL pública, instalación desde un clon limpio y entregables accesibles con evidencias reales. |

Cada fase podrá dividirse en varios commits significativos. La documentación y las comprobaciones de seguridad e integridad se actualizarán junto con las funciones correspondientes. Los avances se marcarán como completados cuando exista implementación y evidencia verificable.

## Instalación y configuración

La aplicación y la elección de versiones y dependencias están pendientes. Todavía no existen comandos de instalación o ejecución verificables. Una vez incorporado el código, este apartado deberá incluir:

1. Requisitos de sistema y versiones de Python, Django, Node.js y React utilizadas.
2. Instalación de dependencias del backend y frontend desde un clon limpio.
3. Variables de entorno mediante archivos de ejemplo sin secretos.
4. Configuración de la base de datos, migraciones y datos de demostración.
5. Comandos de ejecución y comprobación de ambos componentes.

## Uso previsto

El flujo principal previsto consiste en registrar un riesgo, evaluar probabilidad e impacto, calcular su prioridad, asignar un responsable, crear acciones de mitigación y seguir su progreso hasta reducirlo o cerrarlo.

El ejemplo ficticio de demostración es «Acceso no autorizado a sistemas críticos», con probabilidad 4/5, impacto 5/5 y puntuación 20, clasificada como crítica. El riesgo se asigna a Laura, IT Security Manager, y se crea la acción «Implementar MFA para cuentas privilegiadas».

El MVP incluirá dos vistas React: un registro filtrable y un dashboard de exposición calculado a partir de datos persistidos en Django. Cuando exista la aplicación, se incorporarán instrucciones verificadas por rol, acceso a la demostración y capturas reales.

- Repositorio público en GitHub: publicación o confirmación pendiente.
- URL pública de la aplicación: despliegue pendiente.

## Entrega del PFM

| Entregable | Condiciones principales | Estado |
| --- | --- | --- |
| Repositorio GitHub | Código backend y frontend, README de instalación/configuración/uso, estructura coherente y commits progresivos. Se adopta la condición de repositorio público. | Pendiente. |
| Aplicación desplegada | URL pública estable y funcional durante toda la revisión y defensa; no se exige dominio propio. | Pendiente. |
| Memoria técnica | PDF o Word con los seis apartados de la guía, capturas, diagramas y ejemplos; redacción clara, formal y ordenada. | Documentación Markdown inicial; exportación y evidencias pendientes. |
| Vídeo explicativo | Enlace accesible, máximo cinco minutos, problema, demostración, tecnologías y ejemplo real de integración React. | Pendiente. |

Los archivos Markdown constituyen el material de trabajo para la memoria obligatoria en PDF o Word. La defensa tiene un máximo de veinte minutos y la nota mínima de aprobación es 7.0/10. Las condiciones completas de revisión, evaluación y reentrega se conservan en [INSTRUCCIONES-PFM.md](INSTRUCCIONES-PFM.md).
