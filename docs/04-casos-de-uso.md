# 04. Casos de uso

**Estado:** este documento describe los casos de uso previstos a partir del flujo de RiskTrack. Los roles, permisos, criterios de valoración e historial y condiciones de cierre y actualización de progreso están confirmados; los detalles técnicos siguen pendientes. Todavía no se dispone de evidencias de ejecución.

**Referencia obligatoria:** [guía del PFM](../INSTRUCCIONES-PFM.md), «5. Casos de uso». Los roles confirmados se definen en [03-usuarios.md](03-usuarios.md).

## Precondiciones comunes

Para los flujos previstos, la persona que opera la aplicación deberá estar autenticada y activa y tener el rol autorizado. El diseño contempla que Django valide permisos, alcance de datos y entradas antes de persistir cualquier cambio. Las operaciones se refieren al registro de una sola empresa, conforme al alcance confirmado del MVP.

## Administrador

| Caso | Flujo principal | Resultado | Posibles errores |
| --- | --- | --- | --- |
| CU-A01: gestionar acceso | El administrador crea un usuario, asigna uno de los roles y comprueba su acceso. | Usuario persistido con el rol y permisos previstos. | Identificador duplicado, campos inválidos, petición de alguien sin permisos. |
| CU-A02: desactivar y reasignar | Consulta el trabajo abierto del usuario, lo reasigna a una persona activa y desactiva la cuenta. | Cuenta sin acceso; riesgos, acciones e historial conservados y asignaciones abiertas resueltas. | Destinatario inactivo, asignaciones abiertas pendientes o modificación sin autorización. |

## Gestor de riesgos

| Caso | Requisito | Flujo principal | Resultado | Posibles errores |
| --- | --- | --- | --- | --- |
| CU-G01: identificar riesgo | RF-01 | Introduce título y descripción, guarda y abre la ficha desde el registro. | Riesgo persistido en IDENTIFICADO, todavía SIN EVALUAR. | Campos vacíos o inválidos, fallo al guardar. |
| CU-G02: evaluar y priorizar | RF-02, RF-03 | Abre un riesgo y valora probabilidad e impacto para los próximos doce meses con los controles existentes; justifica ambos valores, guarda y consulta la prioridad. | Evaluación histórica y puntuación calculada en Django; 4 × 5 = 20 aparece CRÍTICO. La primera evaluación lleva a EVALUADO; las posteriores conservan el estado. | Valores fuera de 1–5, falta de justificación, riesgo cerrado o sin permiso. |
| CU-G03: asignar y tratar | RF-04, RF-05 | Asigna un responsable activo, crea al menos una acción pendiente con encargado activo y fecha objetivo y solicita iniciar tratamiento. | Riesgo EN TRATAMIENTO con acciones vinculadas y evaluación vigente. | Usuario inactivo, acción inválida o ausencia de evaluación, responsable o acciones pendientes. |
| CU-G04: monitorizar y reevaluar | RF-03, RF-06, RF-08 | Consulta registro y dashboard, revisa acciones e historial y pasa a MONITORIZACIÓN cuando todas las acciones están completadas. Registra una nueva evaluación y, si necesita revisar acciones, solicita volver a tratamiento con motivo. | Exposición coherente y nueva evaluación conservando las anteriores; las acciones solo cambian en los estados habilitados. | Filtros inválidos, acciones incompletas para monitorización, error de obtención de datos o reevaluación no autorizada. |
| CU-G05: cerrar riesgo | RF-07 | Desde MONITORIZACIÓN, comprueba responsable activo y acciones completas; registra una reevaluación posterior a la última entrada en ese estado y a los últimos cambios del riesgo, verifica nivel BAJO o MEDIO y aporta motivo de cierre. | Riesgo CERRADO con fecha, actor, motivo y evaluación de referencia; historial consultable, datos bloqueados para cambios y exclusión del resumen de riesgos abiertos. | Reevaluación no apta, puntuación superior a 11, acciones incompletas, responsable inactivo, falta de motivo, transición inválida o acceso no autorizado. |

## Responsable

| Caso | Requisito | Flujo principal | Resultado | Posibles errores |
| --- | --- | --- | --- | --- |
| CU-R01: consultar trabajo asignado | RF-06 | Abre su registro, consulta un riesgo de su alcance y revisa acciones y evaluaciones. | Ve la información autorizada y su trabajo pendiente. | Sesión caducada o acceso directo a un riesgo fuera de su alcance. |
| CU-R02: actualizar una acción | RF-05, RF-06 | Abre una acción a su cargo en un riesgo EN TRATAMIENTO, informa progreso y nota de avance obligatoria y guarda. | Progreso y estado persistidos y avance registrado con autor, fecha, valores y nota, conservando los anteriores; 100% implica COMPLETADA sin cambiar automáticamente el estado o la evaluación del riesgo. | Progreso fuera de 0–100, nota vacía, acción ajena o riesgo en un estado que no permite actualizar progreso. |

## Demostración principal prevista

Se plantea la siguiente demostración con datos ficticios, pendiente de ejecutar cuando exista la aplicación:

1. El gestor registra «Acceso no autorizado a sistemas críticos».
2. Evalúa probabilidad 4 e impacto 5: Django calcula 20 y nivel CRÍTICO.
3. Asigna a Laura, crea «Implementar MFA para cuentas privilegiadas» con progreso 0% y solicita iniciar tratamiento.
4. Laura consulta su acción y actualiza el progreso hasta completarla, aportando una nota en cada avance. Las notas anteriores quedan disponibles en el historial.
5. El gestor pasa a MONITORIZACIÓN tras comprobar que todas las acciones están completadas y registra una nueva evaluación basada en el resultado del tratamiento. Los nuevos valores no se anticipan: dependen de la evidencia.
6. Si la evaluación es apta para cierre y de nivel BAJO o MEDIO, el responsable está activo y se cumplen las demás condiciones, el gestor cierra con motivo. Si no procede cerrar, mantiene la monitorización o vuelve a tratamiento con motivo.
7. El dashboard refleja el estado actualizado; el historial sigue disponible para consulta en la ficha del riesgo.

## Vistas React del MVP

| Vista | Datos del backend previstos | Relación con los casos |
| --- | --- | --- |
| Registro filtrable | Riesgos, última evaluación, prioridad, responsable y estado. | CU-G01, CU-G02, CU-G04, CU-R01. |
| Dashboard | Totales de riesgos abiertos por nivel y estado y acciones pendientes o vencidas. | CU-G04 y resumen del trabajo del responsable. |

Ambas vistas deberán reflejar el alcance autorizado y mostrar estados de carga, error y ausencia de datos. Los fallos de obtención de datos se mostrarán como errores, sin sustituirlos por cifras ficticias. Los contratos propuestos se describen en [06-arquitectura.md](06-arquitectura.md); el cómputo de acciones del dashboard del Responsable se mantiene pendiente en [08-decisiones-mvp.md](08-decisiones-mvp.md).

## Evidencias pendientes

Las condiciones de valoración, transiciones y cierre se detallan en [02-requisitos.md](02-requisitos.md), distinguiendo decisiones confirmadas y detalles pendientes. La reapertura de riesgos cerrados queda fuera del MVP confirmado.

Una vez implementados estos flujos, se recogerán capturas, diagramas y ejemplos reales para la memoria PDF o Word. El vídeo de hasta cinco minutos presentará el problema, una demostración, las tecnologías y el consumo real de datos mediante React.
