# 08. Decisiones de alcance y diseño del MVP

**Estado:** decisiones de producto confirmadas; base técnica y criterios de valoración pendientes de confirmación. La aplicación todavía no está implementada. Una decisión de diseño no constituye evidencia de funcionamiento ni de seguridad.

**Referencia:** [guía del PFM](../INSTRUCCIONES-PFM.md). Las decisiones mantienen Django con modelos, vistas, plantillas, autenticación y persistencia, dos vistas React con datos reales y los cuatro entregables obligatorios.

## Decisiones de producto confirmadas

| ID | Decisión | Justificación | Consecuencia para la implementación |
| --- | --- | --- | --- |
| D-01 | Una empresa, usuarios internos y datos ficticios para la demostración. | Delimitar un flujo completo de gestión de riesgos que pueda verificarse y explicarse. | No se incorpora entidad de organización ni aislamiento multiempresa. No habrá registro público. |
| D-02 | Registro, evaluación, prioridad, asignación, acciones, monitorización e historial hasta el cierre; registro filtrable y dashboard en React. | Mantener la relación entre el problema, el valor aportado y la integración frontend exigida. | Las operaciones persisten en Django y las vistas React consumen los mismos datos autorizados. |
| D-03 | Excluir multiempresa, adjuntos, notificaciones, integraciones externas, cumplimiento normativo, auditorías formales y ejecución automática de medidas. | Acotar el MVP a la gestión y seguimiento documentados. | No se crean módulos, servicios ni entidades para estas funciones. |
| D-04 | Administrador, Gestor y Responsable; un rol de negocio por cuenta. | Diferenciar gestión de acceso, coordinación y ejecución. | Se aplica la [matriz de permisos](03-usuarios.md) a formularios, fichas, historial, API y agregados. |
| D-05 | Cualquier usuario activo con uno de los tres roles puede recibir asignaciones. | Permitir que una persona coordine o ejecute trabajo sin cambiar de rol. | Recibir una asignación no concede permisos adicionales. Las cuentas inactivas no admiten nuevas asignaciones. |
| D-06 | Cierre explícito desde MONITORIZACIÓN, con responsable activo, todas las acciones completas, reevaluación apta, puntuación 1–11 y motivo. Sin excepción por aceptación de riesgo alto ni reapertura. | Mantener un cierre verificable y limitado al flujo del MVP. | Conservar actor, fecha, motivo y evaluación de referencia; bloquear posteriores cambios de negocio. Cerrar no acredita la desaparición del riesgo. |
| D-07 | Acciones nuevas al 0%; progreso y notas de avance solo en EN TRATAMIENTO. | Evitar completar todas las acciones en EVALUADO y bloquear el inicio de tratamiento, que exige una acción pendiente. | En EVALUADO se preparan y asignan acciones. En MONITORIZACIÓN se bloquean; para cambiarlas se vuelve a tratamiento. La restricción afecta a los tres roles. |

## Base técnica recomendada, pendiente de confirmación

| ID | Recomendación | Justificación y efecto |
| --- | --- | --- |
| T-01 | Django con plantillas y formularios para acceso, cuentas y operaciones; sesiones para autenticación. | Mantener el backend y las plantillas exigidos y aplicar las reglas en un único lugar. |
| T-02 | React con Vite para registro y dashboard, servido junto a Django; endpoints JSON de lectura. | Compartir origen y sesión. Las mutaciones se realizarán mediante formularios Django protegidos por CSRF. |
| T-03 | PostgreSQL en desarrollo, pruebas de integridad y producción. | Comprobar las operaciones concurrentes con el mismo motor utilizado en el despliegue. Los bloqueos de filas mediante `select_for_update()` no tienen efecto en SQLite. |
| T-04 | Usuario mínimo basado en `AbstractUser`, definido antes de las primeras migraciones; roles mediante grupos Django. | Reutilizar autenticación y hashing. La aplicación debe garantizar un único grupo de rol, porque Django permite pertenecer a varios grupos. |
| T-05 | Gestión de cuentas mediante formularios propios; panel Django reservado a mantenimiento técnico. | Crear cuentas, cambiar roles y desactivar con las validaciones del negocio. El rol Administrador no convierte automáticamente la cuenta en superusuario ni en personal del panel. |

La documentación oficial explica la definición del [modelo de usuario antes de las migraciones](https://docs.djangoproject.com/en/5.2/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project), los [grupos y permisos](https://docs.djangoproject.com/en/5.2/topics/auth/default/#groups) y el [bloqueo de filas y sus limitaciones por motor](https://docs.djangoproject.com/en/5.2/ref/models/querysets/#select-for-update). Estas referencias no fijan todavía la versión de Django del proyecto.

## Valoración e historial recomendados, pendientes de confirmación

Conservar la escala de probabilidad e impacto de 1 a 5, los descriptores de [02-requisitos.md](02-requisitos.md), el horizonte de doce meses y la justificación obligatoria. Calcular en Django la puntuación como probabilidad × impacto y los niveles BAJO (1–5), MEDIO (6–11), ALTO (12–19) y CRÍTICO (20–25).

Conservar las evaluaciones anteriores y las notas de avance. Cada actualización registrará actor, fecha, acción, progreso anterior, progreso nuevo y nota aportada. La nota se conservará en el evento del riesgo vinculado a la acción, sin sobrescribir notas anteriores ni incorporar un módulo de comentarios. No se interpretará el historial como registro forense inalterable.

## Detalles que se concretarán durante la implementación

- Versiones compatibles de Python, Django, Node.js, React, Vite y PostgreSQL y archivos de dependencias reproducibles.
- Esquemas JSON, filtros, ordenación, paginación y respuestas de error antes de implementar React.
- Orden inequívoco de los eventos de cada riesgo para comprobar la aptitud de una evaluación, incluidos empates de fecha.
- Transacciones, bloqueos y restricciones para que operación e historial se guarden juntos y los cambios concurrentes respeten el cierre.
- Gestión de cuentas que impida desactivar usuarios con asignaciones abiertas sin resolverlas y evite perder el último administrador operativo.
- Proveedor, servidor de aplicación, estáticos, HTTPS, copias y procedimiento de restauración antes del despliegue.

Estos detalles no amplían el alcance funcional y se documentarán con su implementación y comprobación. Los controles de seguridad permanecerán pendientes hasta disponer de evidencias reales.

## Primer hito de desarrollo

Una vez cerradas las decisiones pendientes, crear la base Django y demostrar: inicio de sesión, creación de riesgo, evaluación 4 × 5 = 20 como CRÍTICO, consulta tras recargar y rechazo de entradas y accesos inválidos. Los datos de demostración serán ficticios. Las decisiones se comprobarán progresivamente mediante los criterios de aceptación de [02-requisitos.md](02-requisitos.md).
