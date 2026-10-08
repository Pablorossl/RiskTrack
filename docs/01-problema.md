# 01. Definición del problema y objetivos

**Estado:** planteamiento del problema y objetivos del proyecto documentados. La solución está pendiente de implementación y validación. El escenario de aplicación es hipotético y los beneficios descritos son previstos.

## Contexto y fundamento

La gestión de riesgos tecnológicos requiere relacionar las situaciones que pueden afectar a los sistemas de una empresa con su valoración, las decisiones de tratamiento y el seguimiento de las medidas adoptadas. En el ámbito de la seguridad de la información, INCIBE describe un proceso que incluye valoración, tratamiento y revisión continuada de los riesgos. Esta referencia aporta fundamento al seguimiento del riesgo a lo largo del tiempo. [INCIBE, *Gestión de riesgos: una guía de aproximación para el empresario*, apartados 3.3 y 4.2](https://www.incibe.es/sites/default/files/contenidos/guias/doc/guia_ciberseguridad_gestion_riesgos_metad.pdf).

El marco NIST CSF 2.0 también sitúa la comprensión, evaluación, priorización y comunicación de los riesgos de ciberseguridad entre los propósitos de la gestión. Su planteamiento permite justificar la importancia de disponer de información útil para decidir qué atender y comunicar su situación. [NIST, *The Cybersecurity Framework (CSF) 2.0*](https://www.nist.gov/publications/nist-cybersecurity-framework-csf-20).

Estas fuentes fundamentan la parte de seguridad de la información del proyecto. Su aplicación al diseño de RiskTrack es una propuesta propia: no implica implementar íntegramente esos marcos ni acreditar conformidad con ellos.

El escenario de partida de RiskTrack es una empresa cuyo registro de riesgos, evaluaciones y acciones se distribuye entre hojas de cálculo, documentos y comunicaciones. Se trata de un supuesto de diseño, sin atribución a una organización concreta ni a entrevistas o estudios realizados. En este escenario, la dificultad aparece cuando la información se mantiene por separado y deja de existir una relación clara entre el riesgo identificado, su evaluación vigente y el trabajo destinado a tratarlo.

## Formulación del problema

**El problema que aborda RiskTrack es la dificultad para mantener una visión centralizada, actualizada y trazable de los riesgos tecnológicos de una empresa y de las acciones destinadas a tratarlos.**

Un registro aislado puede indicar que existe un riesgo sin permitir conocer con claridad qué prioridad tiene, quién debe actuar, qué medidas están pendientes o cómo ha cambiado su valoración. Cuando estos datos dependen de consultas a varios archivos o personas, reconstruir la situación exige conciliar información que puede estar incompleta o corresponder a momentos diferentes.

La necesidad funcional consiste en conectar esa información durante todo el ciclo de gestión. Para cada riesgo, la herramienta deberá facilitar la consulta de su descripción, sus evaluaciones, el responsable asignado, las acciones de tratamiento y los cambios de estado. La conservación de evaluaciones anteriores permitirá distinguir la valoración actual del recorrido seguido hasta alcanzarla.

## Carencias del escenario y consecuencias previstas

| Carencia analizada | Consecuencia posible sobre la gestión | Necesidad que debe cubrir la aplicación |
| --- | --- | --- |
| Información repartida entre archivos y mensajes. | Consultas repetidas y dudas sobre qué información está actualizada. | Registro central con riesgos, evaluaciones y acciones relacionadas. |
| Valoraciones sin criterios explícitos o sin justificación conservada. | Dificultad para comparar riesgos y explicar su prioridad. | Evaluación con probabilidad, impacto, justificación y cálculo consistente. |
| Responsabilidades separadas del registro. | Dificultad para saber quién coordina el riesgo o ejecuta cada acción. | Asignación visible de responsables y encargados. |
| Acciones sin seguimiento vinculado al riesgo. | Dificultad para conocer qué tratamiento está pendiente y su progreso. | Acciones asociadas al riesgo, con estado, progreso y fecha objetivo. |
| Sustitución de valoraciones anteriores al actualizar. | Pérdida de contexto para explicar la evolución del riesgo. | Conservación de evaluaciones sucesivas y cambios relevantes. |
| Resúmenes elaborados por separado. | Posibles diferencias entre la visión global y el detalle del registro. | Resúmenes calculados a partir de los mismos datos autorizados. |

Estas consecuencias son hipótesis del escenario de aplicación. No constituyen incidentes observados ni resultados de una evaluación empresarial. Su impacto sobre el tiempo de trabajo o la calidad de las decisiones todavía no se ha medido.

## Destinatarios y necesidades de uso

El MVP se dirige a usuarios internos que participan en la gestión de riesgos de TI de una empresa. El gestor necesita priorizar riesgos y coordinar su tratamiento; el responsable necesita consultar el trabajo asignado y comunicar su progreso; el administrador necesita gestionar accesos y roles. Esta distribución está confirmada para el proyecto y se desarrolla en [03-usuarios.md](03-usuarios.md).

La información disponible para cada usuario deberá corresponder a sus permisos. El registro y el resumen de exposición deberán respetar ese mismo alcance para evitar que una vista agregada muestre información que el usuario no pueda consultar en detalle.

## Objetivo general

Diseñar, desarrollar y validar una aplicación web que centralice la identificación, evaluación, priorización, asignación, tratamiento y seguimiento de los riesgos tecnológicos de una empresa, proporcionando información coherente y trazable para apoyar su gestión hasta el cierre documentado.

El resultado esperado es una herramienta operativa para organizar y consultar el proceso de gestión. La eficacia de las medidas técnicas que se registren dependerá de su ejecución y verificación fuera de la aplicación.

## Objetivos específicos

| ID | Objetivo | Relación con los requisitos | Comprobación prevista |
| --- | --- | --- | --- |
| OE-01 | Centralizar el registro y la consulta de riesgos de TI. | RF-01. | Crear un riesgo y recuperar sus datos tras recargar la aplicación. |
| OE-02 | Facilitar una evaluación y priorización consistentes mediante probabilidad e impacto. | RF-02, RF-03. | Calcular la puntuación en el backend, mostrar el nivel correspondiente y rechazar valores inválidos. |
| OE-03 | Vincular los riesgos con responsables y acciones de tratamiento. | RF-04, RF-05. | Consultar las asignaciones y actualizar el progreso de una acción con permisos adecuados. |
| OE-04 | Conservar la evolución del riesgo y documentar su cierre. | RF-06, RF-07. | Reevaluar sin perder valoraciones anteriores y comprobar las condiciones de transición y cierre. |
| OE-05 | Facilitar una visión de los riesgos abiertos y del trabajo pendiente. | RF-08. | Contrastar los totales del dashboard con el registro autorizado que los origina. |
| OE-06 | Aplicar autenticación y autorización de acuerdo con los roles y las asignaciones. | RNF-01. | Verificar el acceso permitido y el rechazo de peticiones directas fuera del alcance autorizado. |

Los identificadores RF y RNF remiten a [02-requisitos.md](02-requisitos.md). Las comprobaciones están pendientes y no acreditan funcionalidades implementadas.

## Solución propuesta y justificación

RiskTrack se plantea como una aplicación web con un registro persistente que relacione riesgos, evaluaciones, usuarios, acciones e historial. El flujo previsto es **Identificar → Evaluar → Priorizar → Asignar → Tratar → Monitorizar → Cerrar**. Cada operación deberá conservar la relación con el riesgo correspondiente y aplicar las reglas de validación y acceso definidas para el proyecto.

La interfaz permitirá consultar el detalle de cada riesgo y una visión conjunta de los riesgos abiertos. El registro filtrable y el dashboard de exposición se proponen como dos vistas React que consumirán datos reales del backend Django. Las tecnologías y su justificación se desarrollan en [06-arquitectura.md](06-arquitectura.md).

La aportación del diseño consiste en mantener conectadas la valoración, la responsabilidad y la ejecución del tratamiento. El registro central permitirá consultar esas relaciones, mientras que el historial conservará el contexto de los cambios. Las decisiones sobre la prioridad y la suficiencia de las medidas seguirán correspondiendo a las personas autorizadas.

## Reflexión: aportación y eficiencia

Se prevé simplificar la consulta de información, el cálculo de puntuaciones y la preparación de resúmenes. La aplicación calculará los valores derivados y agrupará los registros, mientras que la identificación del riesgo, la valoración de probabilidad e impacto y la justificación del cierre requerirán intervención humana.

| Tarea | Mejora esperada | Evidencia por obtener |
| --- | --- | --- |
| Consultar la situación de un riesgo. | Reunir evaluación, responsable y tratamiento en una ficha relacionada. | Recuperar toda esa información en el caso de demostración. |
| Priorizar el trabajo. | Calcular puntuación y nivel con las reglas definidas. | Verificar el cálculo y la ordenación del registro. |
| Coordinar acciones. | Mostrar encargados, fechas objetivo y progreso junto al riesgo. | Recorrer la asignación y actualización de una acción. |
| Revisar la evolución. | Conservar valoraciones anteriores y cambios de estado. | Comparar evaluaciones sucesivas sin sobrescribir el historial. |
| Consultar la exposición registrada. | Obtener resúmenes de riesgos abiertos desde los datos persistidos. | Contrastar el dashboard con los registros de origen. |

Estas comprobaciones permitirán evaluar el funcionamiento de la solución. La demostración funcional, por sí sola, no permitirá cuantificar ahorro de tiempo, mejoras de productividad o reducción de incidentes. Esos beneficios requerirían una evaluación adicional en condiciones de uso reales.

## Alcance y límites del MVP

El alcance confirmado comprende el registro central, la evaluación, la prioridad, la asignación, las acciones, la monitorización y el cierre. El producto mínimo viable (MVP) cubre una empresa, usuarios internos y dos vistas React, con datos ficticios para la demostración. Los roles y las condiciones de cierre y actualización de progreso están confirmados; los detalles pendientes se mantienen identificados como propuestas en los documentos de diseño.

Quedan fuera del MVP la gestión de múltiples empresas, los adjuntos, las notificaciones, los módulos de cumplimiento normativo, las auditorías formales, las integraciones externas y la ejecución automatizada de medidas técnicas. La aplicación documentará y seguirá las acciones que se registren; su implantación en sistemas externos queda fuera del alcance.

La puntuación prevista servirá como criterio de priorización dentro del modelo definido. No representa una probabilidad estadística ni una estimación monetaria de pérdidas. Asimismo, el cierre documentará una decisión del flujo de gestión y no acreditará por sí mismo la desaparición del riesgo.

## Caso ilustrativo

El siguiente caso ficticio permite relacionar el problema con el funcionamiento previsto:

1. El gestor registra «Acceso no autorizado a sistemas críticos», con la descripción de la situación que debe tratarse.
2. Introduce probabilidad **4** e impacto **5**, en las escalas propuestas de 1 a 5, junto con su justificación. La puntuación calculada es **4 × 5 = 20**, clasificada como **CRÍTICO** según el criterio definido para el ejemplo.
3. Asigna el riesgo a **Laura — IT Security Manager** y crea la acción «Implementar MFA para cuentas privilegiadas», con encargado y fecha objetivo.
4. La persona encargada actualiza el progreso de la acción. Su finalización queda registrada sin modificar automáticamente la valoración del riesgo.
5. El gestor pasa a monitorización tras completar las acciones y registra una nueva evaluación, justificada a partir del resultado del tratamiento, conservando la anterior. Los nuevos valores quedan pendientes de esa valoración; no se presupone una reducción.
6. El gestor revisa las [condiciones confirmadas de cierre](02-requisitos.md), incluida una reevaluación apta de nivel BAJO o MEDIO. Si procede cerrar, documenta el motivo y conserva el historial para consulta; en caso contrario, mantiene la monitorización o vuelve a tratamiento.

Laura es un personaje ficticio. El puesto profesional no determina sus permisos: las operaciones autorizadas dependerán del rol asignado en la aplicación.

El caso permitirá verificar que evaluación, asignación, tratamiento y evolución permanecen relacionados y son consultables por usuarios autorizados. Su ejecución y las evidencias para la memoria siguen pendientes.
