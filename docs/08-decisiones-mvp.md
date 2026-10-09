# 08. Decisiones de alcance y diseño del MVP

**Estado:** alcance, roles, reglas de seguimiento, criterios de valoración e historial confirmados. La base técnica sigue siendo una propuesta. La aplicación todavía no está implementada. Una decisión de diseño no constituye evidencia de funcionamiento ni de seguridad.

**Referencia:** [guía del PFM](../INSTRUCCIONES-PFM.md). Las decisiones mantienen Django con modelos, vistas, plantillas, autenticación y persistencia, dos vistas React con datos reales y los cuatro entregables obligatorios.

## Criterio de complejidad

La implementación priorizará las herramientas habituales de Django: entorno virtual, dependencias, modelos y relaciones, consultas ORM, admin, usuarios y grupos, URLs, vistas, plantillas, formularios, ModelForms, decoradores, variables de entorno, mensajes y pruebas unitarias. Se incorporarán únicamente los componentes necesarios para el flujo confirmado y los requisitos del PFM. Las reglas de negocio se explicarán mediante operaciones concretas y comprobables.

## Decisiones de producto confirmadas

| ID | Decisión | Justificación | Consecuencia para la implementación |
| --- | --- | --- | --- |
| D-01 | Una empresa, usuarios internos y datos ficticios para la demostración. | Delimitar un flujo completo de gestión de riesgos que pueda verificarse y explicarse. | No se incorpora entidad de organización ni aislamiento multiempresa. No habrá registro público. |
| D-02 | Registro, evaluación, prioridad, asignación, acciones, monitorización e historial hasta el cierre; registro filtrable y dashboard en React. | Mantener la relación entre el problema, el valor aportado y la integración frontend exigida. | Las operaciones deberán persistir en Django y las vistas React consumirán los mismos datos autorizados. |
| D-03 | Excluir multiempresa, adjuntos, notificaciones, integraciones externas, cumplimiento normativo, auditorías formales y ejecución automática de medidas. | Acotar el MVP a la gestión y seguimiento documentados. | No se crean módulos, servicios ni entidades para estas funciones. |
| D-04 | Administrador, Gestor y Responsable; un rol de negocio por cuenta. | Diferenciar gestión de acceso, coordinación y ejecución. | Se aplicará la [matriz de permisos](03-usuarios.md) a formularios, fichas, historial, API y agregados. |
| D-05 | Cualquier usuario activo con uno de los tres roles puede recibir asignaciones. | Permitir que una persona coordine o ejecute trabajo sin cambiar de rol. | Recibir una asignación no concede permisos adicionales. Las cuentas inactivas no admiten nuevas asignaciones. |
| D-06 | Cierre explícito desde MONITORIZACIÓN, con responsable activo, todas las acciones completas, reevaluación apta, puntuación 1–11 y motivo. Sin excepción por aceptación de riesgo alto ni reapertura. | Mantener un cierre verificable y limitado al flujo del MVP. | Conservar actor, fecha, motivo y evaluación de referencia; bloquear posteriores cambios de negocio. Cerrar no acredita la desaparición del riesgo. |
| D-07 | Acciones nuevas al 0%; progreso y notas de avance solo en EN TRATAMIENTO. | Evitar completar todas las acciones en EVALUADO y bloquear el inicio de tratamiento, que exige una acción pendiente. | En EVALUADO se preparan y asignan acciones. En MONITORIZACIÓN se bloquean; para cambiarlas se vuelve a tratamiento. La restricción afecta a los tres roles. |
| D-08 | Probabilidad e impacto de 1 a 5, horizonte de doce meses, justificación obligatoria y puntuación calculada como probabilidad × impacto. Niveles BAJO 1–5, MEDIO 6–11, ALTO 12–19 y CRÍTICO 20–25. | Valorar con un periodo y criterios comunes y obtener una prioridad verificable. | Django calculará puntuación y nivel. Sin evaluación se mostrará SIN EVALUAR. Los valores no representan porcentajes ni pérdidas económicas. |
| D-09 | Conservar todas las evaluaciones; la última por fecha e identificador determina la prioridad actual. Corregir mediante una nueva evaluación justificada. | Mantener la evolución y explicar la valoración vigente. | No se editan ni eliminan evaluaciones mediante operaciones de negocio. Completar acciones no modifica la puntuación. |
| D-10 | Historial automático de cambios relevantes, con actor, fecha y detalle; nota obligatoria en cada avance. | Explicar quién realizó cada operación y qué trabajo se ha comunicado. | Conservar valores anteriores y nuevos y las referencias pertinentes. Las notas no se sobrescriben y el historial comparte el alcance de acceso de la ficha. |

## Base técnica recomendada, pendiente de confirmación

| ID | Recomendación | Justificación y efecto |
| --- | --- | --- |
| T-01 | Django con plantillas, formularios y ModelForms para acceso y operaciones; sesiones para autenticación. | Reutilizar las herramientas del framework y mantener las reglas en el backend. |
| T-02 | React con Vite para registro y dashboard, servido junto a Django; dos vistas Django con `JsonResponse` para lectura. | Compartir origen y sesión. React obtiene datos mediante `fetch`; las modificaciones se realizan con formularios Django protegidos por CSRF. |
| T-03 | SQLite para el primer arranque local; PostgreSQL antes de validar concurrencia en tratamiento y cierre y para producción. | Facilitar el inicio del proyecto y comprobar la integridad final con el motor de despliegue. Los bloqueos de filas mediante `select_for_update()` no tienen efecto en SQLite; las primeras comprobaciones locales no acreditarán ese control. |
| T-04 | Modelo `User` estándar de Django y grupos para Administrador, Gestor y Responsable. | El alcance actual utiliza los campos y funciones existentes. No requiere un modelo de usuario personalizado. La aplicación comprobará un único grupo de rol, porque Django permite varios grupos. |
| T-05 | Gestión de cuentas desde el admin de Django, adaptado a la matriz de permisos y a la desactivación con reasignación. | Reutilizar sus formularios de usuarios y contraseñas. Solo Administrador tendrá acceso ordinario al panel y a la gestión de cuentas, con `is_staff` y permisos específicos; el rol no concede `is_superuser`. Se limitará la edición de privilegios técnicos y se impedirán eliminaciones que pierdan historial. |
| T-06 | Vistas basadas en funciones para comenzar y vistas genéricas cuando simplifiquen formularios. | Facilitar la lectura del recorrido URL → vista → formulario/modelo → plantilla. Los permisos se comprobarán en el backend. |
| T-07 | Validaciones en formularios y modelos; métodos de modelo para evaluar, actualizar acciones y cambiar estados. | Reutilizar las mismas reglas en las entradas habilitadas. Los registros de riesgos, evaluaciones, acciones e historial del admin serán de consulta; las operaciones se realizarán por las vistas de negocio para evitar saltarse las reglas. |

La documentación oficial describe el [usuario estándar y la autenticación](https://docs.djangoproject.com/en/5.2/topics/auth/default/), la [adaptación del admin](https://docs.djangoproject.com/en/5.2/ref/contrib/admin/), las [respuestas JSON](https://docs.djangoproject.com/en/5.2/ref/request-response/#jsonresponse-objects) y el [bloqueo de filas y sus limitaciones por motor](https://docs.djangoproject.com/en/5.2/ref/models/querysets/#select-for-update). Estas referencias no fijan todavía la versión de Django del proyecto.

## Valoración e historial confirmados

Se adopta la escala de probabilidad e impacto de 1 a 5, los descriptores de [02-requisitos.md](02-requisitos.md), el horizonte de doce meses y la justificación obligatoria. Django calculará la puntuación como probabilidad × impacto y los niveles BAJO (1–5), MEDIO (6–11), ALTO (12–19) y CRÍTICO (20–25).

### Criterios de valoración

- Probabilidad: estimación cualitativa de que ocurra la situación durante los próximos doce meses, considerando los controles existentes.
- Impacto: gravedad de las consecuencias si la situación ocurre. Se justificará el mayor nivel aplicable cuando concurran varias consecuencias.
- Justificación: un campo de texto obligatorio que explique la probabilidad, el impacto y las condiciones consideradas.
- Registro: cada evaluación conservará riesgo, probabilidad, impacto, justificación, evaluador y fecha del servidor. La puntuación y el nivel se calcularán en Django.
- Valoración actual: la última evaluación por fecha e identificador. Sin evaluación se mostrará SIN EVALUAR, sin asignar puntuación cero.
- Corrección: una nueva evaluación justificada conservará la anterior. La puntuación podrá aumentar, mantenerse o disminuir.
- Reevaluación: completar una acción no cambiará la puntuación. El gestor o administrador deberá registrar una nueva valoración; el cierre seguirá las condiciones confirmadas.

El horizonte establece el periodo de valoración; no programa revisiones ni caducidad automática. Los niveles son criterios internos de priorización del MVP y no expresan porcentajes, pérdidas económicas ni una política empresarial validada.

### Historial

Se conservarán todas las evaluaciones y se registrarán los cambios relevantes mediante el modelo Evento del riesgo previsto. Cada entrada identificará el riesgo, el actor, la fecha del servidor, el tipo y el detalle del cambio. Si corresponde a una acción o evaluación, conservará también esa referencia.

| Operación | Información que se conservará |
| --- | --- |
| Crear riesgo | Autor y datos iniciales. |
| Editar título o descripción | Campos modificados, valor anterior y valor nuevo. |
| Evaluar o reevaluar | Referencia a la evaluación con sus valores y justificación. |
| Asignar o reasignar | Riesgo o acción y responsable anterior y nuevo. |
| Crear o editar una acción | Referencia a la acción y datos iniciales o campos modificados con valores anteriores y nuevos. |
| Registrar un avance | Acción, progreso anterior, progreso nuevo y nota explicativa. |
| Cambiar estado | Estado anterior y nuevo; motivo cuando lo exija la transición. |
| Cerrar | Actor, fecha, motivo y referencia a la evaluación que justifica el cierre. |

Se exigirá una nota no vacía al registrar cada avance, también al completar una acción, para explicar qué se ha realizado o corregido. La nota se conservará en el evento vinculado a la acción, sin sobrescribir notas anteriores ni incorporar un módulo de comentarios. La validación y el registro están pendientes de implementación.

Las entradas anteriores serán de consulta y no se editarán ni borrarán mediante operaciones de negocio. Una corrección conservará el registro previo y añadirá la operación correspondiente. El historial de cada riesgo tendrá el mismo alcance de acceso que su ficha y seguirá disponible cuando se cierre el riesgo o se desactive un usuario. No se interpretará como registro forense inalterable.

### Comprobaciones de aceptación previstas

- Una evaluación 4 × 5 produce puntuación 20 y nivel CRÍTICO.
- Se rechazan valores fuera de 1–5 y justificación vacía sin guardar una evaluación parcial.
- Una nueva evaluación conserva la anterior y pasa a ser la vigente.
- Completar una acción conserva la puntuación hasta registrar una reevaluación explícita.
- Cada avance conserva acción, actor, fecha, valores y nota; una nota nueva no sustituye la anterior.
- Un avance con nota vacía o formada solo por espacios se rechaza sin modificar el progreso ni registrar un avance parcial.
- La ficha y el historial rechazan el acceso de un responsable fuera de su alcance.
- El cierre conserva la evaluación utilizada y permite consultar todo el recorrido.

Estas comprobaciones están pendientes de implementación y ejecución.

## Detalles que se concretarán durante la implementación

- Versiones compatibles de Python, Django, Node.js, React, Vite y PostgreSQL y archivos de dependencias reproducibles.
- Esquemas JSON, filtros, ordenación, paginación y respuestas de error antes de implementar React.
- Cómputo de acciones pendientes y vencidas en el dashboard del Responsable: concretar si contará todas las acciones de los riesgos visibles o solo las asignadas a esa cuenta, manteniendo el alcance de consulta confirmado.
- Orden inequívoco de los eventos de cada riesgo para comprobar la aptitud de una evaluación, incluidos empates de fecha.
- Uso acotado de transacciones y, donde sea necesario, bloqueo de filas del ORM para guardar operación e historial juntos y respetar el cierre ante cambios concurrentes. Son mecanismos internos de Django; no se añade infraestructura de coordinación.
- Adaptaciones de los formularios y permisos del admin para gestionar cuentas, validar un solo rol, resolver asignaciones antes de desactivar y limitar cambios de privilegios técnicos.
- Definición operativa de las asignaciones abiertas que deberán resolverse antes de desactivar una cuenta y tratamiento de los cambios de rol. La gestión de cuentas deberá respetar el bloqueo de acciones en MONITORIZACIÓN y CERRADO; cualquier flujo de reasignación se documentará antes de implementarlo.
- Proveedor, servidor de aplicación, estáticos, HTTPS, copias y procedimiento de restauración antes del despliegue.

Estos detalles no amplían el alcance funcional y se documentarán con su implementación y comprobación. Los controles de seguridad permanecerán pendientes hasta disponer de evidencias reales.

## Primer hito de desarrollo

Tras concretar las decisiones técnicas necesarias para el primer hito, se creará la base Django y se demostrarán el inicio de sesión, la creación de riesgo, la evaluación 4 × 5 = 20 como CRÍTICO, la consulta tras recargar y el rechazo de entradas y accesos inválidos. Los datos de demostración serán ficticios. Las decisiones se comprobarán progresivamente mediante los criterios de aceptación de [02-requisitos.md](02-requisitos.md). La preparación de la entrega se controlará mediante [09-verificacion-pfm.md](09-verificacion-pfm.md).
