# 02. Requisitos

**Estado:** las obligaciones académicas están identificadas y el alcance, roles, criterios de valoración, historial y reglas de seguimiento están confirmados. La base técnica y la gestión de cuentas están decididas; los campos definitivos del modelo y los contratos JSON siguen pendientes. La aplicación todavía no está implementada ni se ha acreditado el cumplimiento de las obligaciones.

**Fuente:** [instrucciones completas del PFM](../INSTRUCCIONES-PFM.md). Los identificadores siguientes son referencias internas; no sustituyen la guía ni alteran su alcance.

## Requisitos técnicos mínimos

| ID | Obligación del PFM | Comprobación prevista | Estado |
| --- | --- | --- | --- |
| PFM-TEC-01 | Backend Django con modelos, vistas, plantillas, autenticación y persistencia de datos. | Recorrer un flujo real autenticado y verificar su persistencia; revisar código, modelos y plantillas. | Pendiente. |
| PFM-TEC-02 | Una o dos vistas React que obtengan datos reales del backend. | Demostrar el consumo del backend en las vistas y en el vídeo. | Pendiente. |
| PFM-TEC-03 | Aplicación funcional en una URL pública. | Comprobar acceso estable desde navegador y mantenerlo durante revisión y defensa. | Pendiente. |
| PFM-TEC-04 | Repositorio GitHub público con README y commits significativos. | Verificar acceso, código completo, instrucciones reproducibles e historial progresivo. | Parcial: repositorio público disponible; código, instrucciones ejecutables e historial de desarrollo pendientes. |

El apartado de entrega admite repositorio público o compartido con el equipo docente; se adopta la condición de repositorio público para cumplir también el requisito técnico mínimo. Docker es opcional y la guía no exige dominio propio.

## Documentación obligatoria

| Apartado de la guía | Ubicación de trabajo | Estado |
| --- | --- | --- |
| Definición del problema: contexto, carencias, impacto y solución | [01-problema.md](01-problema.md) | Necesidad fundamentada en fuentes; escenario ilustrativo hipotético y validación funcional pendiente. |
| Reflexión: aportación y eficiencia | [01-problema.md](01-problema.md) | Beneficios previstos documentados; evidencias pendientes. |
| Tecnologías utilizadas y justificación | [06-arquitectura.md](06-arquitectura.md) | Django y React exigidos; base técnica adoptada, pendiente de implementación. |
| Tipos de usuarios y acciones permitidas | [03-usuarios.md](03-usuarios.md) | Roles y permisos confirmados; implementación pendiente. |
| Casos de uso por usuario, flujos, resultados y errores | [04-casos-de-uso.md](04-casos-de-uso.md) | Flujos documentados; validación en la aplicación pendiente. |
| Seguridad y protección de datos | [07-seguridad.md](07-seguridad.md) | Medidas pendientes de implementar y verificar. |

## Requisitos funcionales de RiskTrack

Las funciones de esta tabla desarrollan el ciclo previsto para RiskTrack.

| ID | Función | Criterio de aceptación | Caso de uso |
| --- | --- | --- | --- |
| RF-01 | Identificar y centralizar riesgos de TI. | Crear un riesgo con título y descripción y consultarlo en el registro tras recargar. | CU-G01. |
| RF-02 | Evaluar probabilidad e impacto. | Guardar enteros entre 1 y 5 según los descriptores y el horizonte de doce meses confirmados; exigir justificación, calcular la puntuación en el backend y conservar cada evaluación. | CU-G02. |
| RF-03 | Priorizar riesgos. | Mostrar puntuación y nivel; 4 × 5 = 20 debe figurar como CRÍTICO. Filtrar y ordenar por prioridad. | CU-G02, CU-G04. |
| RF-04 | Asignar responsables. | Asignar un usuario activo autorizado y mostrarlo en la ficha; rechazar asignaciones inválidas. | CU-G03. |
| RF-05 | Tratar riesgos mediante acciones. | Crear una acción vinculada al riesgo, asignarla y conservar su estado y progreso. | CU-G03, CU-R02. |
| RF-06 | Monitorizar evolución. | Consultar evaluaciones sucesivas, acciones y cambios registrados; una nueva evaluación no elimina la anterior. | CU-G04, CU-R01. |
| RF-07 | Cerrar riesgos. | Permitir el cierre solo desde MONITORIZACIÓN, con responsable activo, acciones completadas, reevaluación apta para cierre de nivel BAJO o MEDIO y motivo; conservar la evaluación utilizada y el historial. | CU-G05. |
| RF-08 | Visualizar exposición. | Mostrar riesgos abiertos por nivel y estado con cifras consistentes con los datos autorizados. | CU-G04. |

## Reglas de evaluación y seguimiento

El criterio definido para el proyecto es **puntuación = probabilidad × impacto**, con clasificación del caso 4 × 5 = 20 como CRÍTICO.

**Decisiones confirmadas del MVP:** una empresa, usuarios internos, tres roles, dos vistas React, cierre desde MONITORIZACIÓN con puntuación de 1 a 11 y las demás condiciones descritas, sin cierre por aceptación de riesgo elevado ni reapertura. Las acciones se crearán con progreso 0% y su progreso solo se actualizará en EN TRATAMIENTO. Están confirmados los descriptores, la escala de 1 a 5, el horizonte de doce meses, los umbrales de prioridad, la conservación de evaluaciones e historial y la nota obligatoria por avance. Estas reglas son internas del proyecto; no representan una política aprobada por una empresa ni controles implementados.

### Horizonte y criterios de valoración

Se valorará la posibilidad de que ocurra la situación descrita durante los **doce meses posteriores a cada evaluación**, considerando los controles existentes en ese momento. Las acciones todavía pendientes no se contabilizarán como medidas efectivas. Cada reevaluación utilizará el mismo horizonte de doce meses desde su propia fecha.

Probabilidad e impacto serán enteros de 1 a 5. La probabilidad expresa una estimación cualitativa de ocurrencia durante ese horizonte; el impacto expresa la gravedad de las consecuencias si ocurre. Los valores no equivalen a porcentajes ni a importes económicos.

| Valor | Probabilidad | Criterio orientativo |
| --- | --- | --- |
| 1 | Muy improbable | La ocurrencia requeriría circunstancias excepcionales y existen controles que dificultan claramente el escenario. |
| 2 | Improbable | El escenario es posible, pero las condiciones conocidas y los controles existentes hacen poco esperable su ocurrencia. |
| 3 | Posible | Existe un escenario creíble de ocurrencia; la información disponible no permite considerarlo improbable ni probable. |
| 4 | Probable | Hay condiciones favorables a la ocurrencia, como debilidades relevantes de control o antecedentes comparables que deben explicarse. |
| 5 | Muy probable | Las condiciones que facilitan la ocurrencia están presentes de forma persistente y los controles resultan ausentes o claramente insuficientes. |

| Valor | Impacto | Consecuencia orientativa |
| --- | --- | --- |
| 1 | Insignificante | Afectación puntual, sin interrupción relevante de la actividad ni compromiso significativo de la información. |
| 2 | Menor | Afectación limitada a una tarea o servicio no esencial, recuperable con los procedimientos habituales. |
| 3 | Moderado | Afectación apreciable a un proceso o servicio, que exige intervención específica para recuperar su funcionamiento. |
| 4 | Grave | Interrupción importante de un proceso esencial o compromiso significativo de información sensible. |
| 5 | Crítico | Afectación que compromete la continuidad de una actividad esencial o produce consecuencias de gran alcance sobre la información de la empresa. |

La justificación será un único campo de texto obligatorio que explique la elección de probabilidad e impacto, las condiciones y controles considerados y la información utilizada. Cuando concurran consecuencias de varios niveles, se elegirá el mayor nivel aplicable y se explicará el criterio. La ausencia de información suficiente no se resolverá asignando automáticamente el valor 1: el riesgo podrá permanecer SIN EVALUAR hasta disponer de una valoración justificable.

Estos descriptores permiten preparar una demostración académica. Su adaptación a tiempos de interrupción, pérdidas u otros límites propios de una empresa requeriría validación con esa organización.

### Puntuación y prioridad

| Puntuación | Nivel confirmado |
| --- | --- |
| 1–5 | BAJO |
| 6–11 | MEDIO |
| 12–19 | ALTO |
| 20–25 | CRÍTICO |

Los intervalos incluyen ambos extremos. Django calculará puntuación y nivel; el cliente no podrá imponer valores derivados independientes. Un riesgo sin evaluación aparecerá como **SIN EVALUAR**, sin puntuación cero ni prioridad artificial. La puntuación permitirá ordenar riesgos dentro de esta escala; no cuantificará pérdidas ni certificará la eficacia de los controles.

### Evaluación vigente y reevaluación

Cada evaluación conservará valores, justificación, evaluador y fecha de registro asignada por el servidor. La evaluación vigente será la más reciente por fecha y, en caso de empate, por identificador. Las evaluaciones anteriores no se editarán ni eliminarán mediante las operaciones de negocio; una corrección se registrará como una nueva evaluación justificada.

El gestor o administrador podrá reevaluar mientras el riesgo esté abierto. La primera evaluación llevará el riesgo de IDENTIFICADO a EVALUADO; las posteriores actualizarán su puntuación y nivel sin cambiar automáticamente su estado. La puntuación podrá aumentar, mantenerse o disminuir según la valoración registrada.

Una evaluación vigente será **apta para cierre** cuando se registre después de la última entrada en MONITORIZACIÓN y después del último cambio de título, descripción o responsable del riesgo. Cualquier cambio posterior de esos datos exigirá otra evaluación antes del cierre. Si se vuelve a tratamiento y después a monitorización, será necesaria una nueva evaluación en ese nuevo ciclo.

El horizonte de doce meses expresa el periodo analizado; no constituye una fecha de caducidad. El MVP no incorpora vencimiento automático ni revisiones periódicas programadas. La aptitud para cierre se determinará mediante el orden de las operaciones registradas, sin permitir fechas de evaluación introducidas por el cliente.

### Estados y transiciones del riesgo

Los estados del MVP son **IDENTIFICADO, EVALUADO, EN TRATAMIENTO, MONITORIZACIÓN y CERRADO**. La asignación y la priorización son operaciones, no estados adicionales. El gestor o administrador iniciará las operaciones de transición; el responsable de una acción no podrá cambiar por sí mismo el estado del riesgo.

| Origen | Destino | Condiciones de transición |
| --- | --- | --- |
| Creación | IDENTIFICADO | Título y descripción válidos; todavía sin evaluación. |
| IDENTIFICADO | EVALUADO | Registro de la primera evaluación válida en la misma operación. |
| EVALUADO | EN TRATAMIENTO | Evaluación vigente, responsable activo y al menos una acción sin completar; todas las acciones pendientes deberán tener encargado activo y fecha objetivo. |
| EN TRATAMIENTO | MONITORIZACIÓN | Responsable activo, evaluación vigente y al menos una acción; todas las acciones deberán estar COMPLETADAS. |
| MONITORIZACIÓN | EN TRATAMIENTO | Responsable activo, evaluación vigente y motivo documentado de retorno para continuar o revisar el tratamiento. Se conservarán las acciones existentes y el historial. |
| MONITORIZACIÓN | CERRADO | Cumplimiento simultáneo de todas las condiciones de cierre descritas a continuación. |

Las transiciones no enumeradas se rechazarán. Completar acciones no provocará automáticamente el paso a monitorización ni el cierre. Los cambios de estado quedarán registrados con actor, fecha, origen y destino. No se propone un periodo mínimo de permanencia en MONITORIZACIÓN para el MVP; la reevaluación y la justificación deberán explicar el resultado observado.

### Acciones y progreso

El progreso de una acción será un entero entre 0 y 100 y determinará su estado: **PENDIENTE (0%), EN CURSO (1–99%) o COMPLETADA (100%)**. El backend derivará ese estado del progreso. Completar una acción no modificará automáticamente la evaluación del riesgo. Toda acción sin completar deberá tener encargado activo; para bajar del 100% una acción con encargado inactivo se deberá reasignar primero, en EN TRATAMIENTO.

Las acciones se crearán con progreso 0% y estado PENDIENTE. El gestor o administrador podrá prepararlas, asignarlas y editar título, descripción, encargado y fecha objetivo en EVALUADO y EN TRATAMIENTO. La actualización del progreso y las notas de avance solo se permitirá en EN TRATAMIENTO, también para gestor y administrador. El responsable solo podrá actualizar el progreso y las notas de sus propias acciones en ese estado. Así se evita completar todas las acciones en EVALUADO y bloquear la entrada en tratamiento, que exige al menos una acción pendiente.

En MONITORIZACIÓN las acciones quedarán bloqueadas para cambios; el gestor o administrador deberá devolver el riesgo a EN TRATAMIENTO antes de incorporar acciones o revisar su progreso. En CERRADO tampoco se permitirán cambios.

### Historial y notas de avance

Cada avance exigirá una nota de texto no vacía que explique el trabajo realizado o la corrección, también al alcanzar el 100%. El backend rechazará notas vacías o formadas solo por espacios, sin modificar el progreso ni registrar un avance parcial. Cada nota se conservará junto con acción, actor, fecha del servidor y progreso anterior y nuevo; los avances posteriores no la sobrescribirán.

El modelo de historial se incorporará junto a Riesgo y Evaluación desde el primer flujo de negocio. Cada operación y sus eventos se guardarán conjuntamente. El historial se registrará automáticamente para creación y edición del riesgo, evaluaciones, asignaciones, creación y edición de acciones, avances, transiciones y cierre. Conservará actor, fecha y detalle del cambio, con valores anteriores y nuevos y referencias a acción o evaluación cuando corresponda. Las entradas anteriores no se editarán ni borrarán mediante operaciones de negocio. Se mantendrán disponibles al cerrar el riesgo o desactivar usuarios y tendrán el mismo alcance de consulta que la ficha. El detalle de las operaciones figura en [08-decisiones-mvp.md](08-decisiones-mvp.md).

### Condiciones de cierre

El cierre requerirá una petición explícita del gestor o administrador autenticado y activo. Django deberá comprobar conjuntamente:

1. El riesgo está en MONITORIZACIÓN y tiene un responsable activo.
2. Existe al menos una acción asociada y todas están COMPLETADAS, con progreso 100%.
3. Existe una evaluación vigente apta para cierre según el orden de operaciones definido anteriormente.
4. La puntuación vigente está entre **1 y 11**, correspondiente a nivel **BAJO o MEDIO**.
5. Se aporta un motivo no vacío que explique las medidas realizadas, la valoración posterior y la razón del cierre.

El límite de 11 es una decisión de diseño del MVP; no acredita un nivel de tolerancia aprobado por una empresa. Un riesgo ALTO o CRÍTICO permanecerá abierto, aunque todas sus acciones estén completadas. El gestor podrá mantenerlo en monitorización o volver a tratamiento para revisar las medidas; no se contempla una excepción de cierre por aceptación de riesgo elevado.

La operación conservará fecha, actor, motivo y referencia a la evaluación utilizada. El cambio de estado y su evento se guardarán conjuntamente, comprobando que las condiciones siguen cumpliéndose ante operaciones concurrentes. Un error dejará el riesgo abierto sin registrar un cierre parcial.

Un riesgo CERRADO quedará disponible para consulta, conservará su historial y se excluirá del resumen de riesgos abiertos. Sus datos, evaluaciones y acciones no podrán modificarse mediante las operaciones de negocio. La reapertura queda fuera del MVP confirmado; incorporarla requeriría revisar estas reglas y los casos de uso. El cierre representa una decisión documentada y no demuestra que el riesgo haya desaparecido.

### Ejemplos de aceptación pendientes de verificar

| Situación | Resultado esperado |
| --- | --- |
| Primera evaluación con probabilidad 4 e impacto 5. | Puntuación 20, nivel CRÍTICO y estado EVALUADO. |
| Valor fuera de 1–5 o justificación vacía. | Rechazo sin guardar evaluación ni cambiar el estado. |
| Crear una acción en EVALUADO. | Acción PENDIENTE con progreso 0%, preparada para iniciar tratamiento. |
| Actualizar progreso o notas de avance en EVALUADO, incluso como gestor o administrador. | Rechazo sin modificar la acción ni registrar un avance. |
| Última acción pasa a 100% en EN TRATAMIENTO. | Acción COMPLETADA; el riesgo permanece EN TRATAMIENTO hasta una transición explícita. |
| Avance con nota vacía o formada solo por espacios. | Rechazo sin modificar progreso ni guardar un avance parcial. |
| Segundo avance válido de una acción. | Se conserva la nota anterior y se añade el nuevo avance con autor, fecha y valores. |
| Nueva evaluación justificada de un riesgo abierto. | Se conserva la evaluación anterior y la nueva pasa a ser la vigente. |
| Cierre con acciones completas y puntuación 10, pero evaluación anterior a la última entrada en MONITORIZACIÓN. | Rechazo por falta de reevaluación apta para cierre. |
| Cierre con puntuación 10 y reevaluación posterior a monitorización, seguido de un cambio de descripción. | Rechazo hasta registrar otra evaluación posterior a ese cambio. |
| Cierre con reevaluación apta de puntuación 12 o 20. | Rechazo por nivel ALTO o CRÍTICO. |
| Cierre con reevaluación apta de puntuación 10, responsable activo, acciones completas y motivo. | Cierre registrado con evaluación de referencia e historial conservado. |
| Actualización de una acción en MONITORIZACIÓN o petición de cierre desde el rol Responsable. | Rechazo por estado o permiso, respectivamente. |

Estos ejemplos describen el comportamiento requerido para el MVP; su ejecución y sus evidencias siguen pendientes.

## Gestión de cuentas

Se adopta la [regla operativa de cuentas](03-usuarios.md): antes de desactivar hay que reasignar los riesgos abiertos y las acciones sin completar de riesgos abiertos. Las acciones completadas, los riesgos cerrados y las autorías conservarán sus referencias. Un cambio de rol conservará asignaciones y aplicará los nuevos permisos en cada petición. Se mantendrá un rol por cuenta y al menos un Administrador activo, sin autodesactivación ni cambio del propio rol.

| Situación | Resultado esperado, pendiente de verificar |
| --- | --- |
| Desactivar una cuenta con riesgo abierto a su cargo o acción sin completar. | Rechazo con indicación de las asignaciones que deben resolverse. |
| Cuenta con acciones completadas como únicas asignaciones, incluso en MONITORIZACIÓN. | Desactivación permitida si cumple las reglas de acceso administrativo; acciones e historial sin cambios. |
| Reasignar el responsable de un riesgo en MONITORIZACIÓN. | Evento de reasignación y exigencia de otra evaluación antes del cierre; acciones bloqueadas sin cambios. |
| Bajar del 100% una acción cuyo encargado está inactivo. | Rechazo hasta reasignarla a una cuenta activa en EN TRATAMIENTO. |
| Cambiar Gestor por Responsable en una cuenta con sesión iniciada. | Asignaciones conservadas y siguientes peticiones limitadas a los permisos de Responsable. |
| Asignación simultánea a una desactivación. | No queda trabajo abierto asignado a una cuenta inactiva; una de las operaciones debe rechazarse tras comprobar el estado vigente. |

## Requisitos no funcionales y límites del MVP

| ID | Requisito de diseño | Comprobación prevista |
| --- | --- | --- |
| RNF-01 | Usuarios autenticados y autorización por rol y alcance de datos. | Rechazar lecturas y cambios no autorizados incluso mediante peticiones directas. |
| RNF-02 | Persistencia e integridad del registro. | Validar rangos, relaciones y transiciones; verificar que recargar no pierde datos. |
| RNF-03 | Dos vistas React con datos reales: registro y dashboard. | Cambiar un riesgo en Django y comprobar el resultado actualizado en ambas vistas. |
| RNF-04 | Exposición pública estable con datos ficticios de demostración. | Verificar URL HTTPS y acceso durante revisión y defensa. |
| RNF-05 | Instalación, configuración y uso reproducibles. | Seguir el README desde un clon limpio cuando exista la aplicación. |

El MVP confirmado comprende una empresa, usuarios internos y dos vistas React: registro filtrable y dashboard. No habrá registro público, multiempresa, adjuntos, notificaciones ni integraciones externas. También quedan fuera los módulos de cumplimiento normativo, las auditorías formales y la ejecución automática de medidas técnicas. La demostración utilizará datos ficticios. Los objetivos numéricos de rendimiento todavía no están definidos. El análisis de seguridad que requiere la guía se desarrolla en [07-seguridad.md](07-seguridad.md), con las medidas previstas.

## Entregables y evaluación

La entrega debe incluir cuatro elementos imprescindibles: repositorio GitHub con todo el código, aplicación desplegada, memoria PDF o Word y vídeo enlazado de hasta cinco minutos.

| Criterio de evaluación | Peso |
| --- | --- |
| Correctitud técnica del backend (Django) | 30% |
| Integración parcial de React (consumo de datos) | 15% |
| Arquitectura, estructura y buenas prácticas | 15% |
| Seguridad e integridad de datos | 15% |
| Documentación y presentación | 15% |
| Reflexión y valor aportado | 10% |

La guía completa contiene las condiciones de revisión, autoría, defensa de hasta veinte minutos, feedback y reentrega que también deben respetarse.

La cobertura documental no acredita por sí sola la implementación ni el cumplimiento final.
