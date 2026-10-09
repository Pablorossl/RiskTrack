# 07. Seguridad y protección de datos

**Estado:** este documento presenta los controles de seguridad propuestos para el diseño. Ninguno está implementado ni verificado todavía.

**Referencia obligatoria:** [guía del PFM](../INSTRUCCIONES-PFM.md), «6. Seguridad y protección de datos» y criterio «Seguridad e integridad de datos» (15%).

## Datos y accesos a proteger

El registro puede revelar riesgos, debilidades de sistemas, responsables y acciones de seguridad de una empresa. La confidencialidad debe aplicarse a fichas, historial, listados y agregados del dashboard. Las cuentas y nombres de personas pueden constituir datos personales.

La presentación del proyecto y la demostración pública utilizarán datos ficticios. Antes de utilizar datos reales, será necesario concretar finalidad, acceso, conservación y eliminación con la empresa.

## Medidas de seguridad contempladas en la guía

| Medida | Aplicación propuesta en RiskTrack | Comprobación pendiente |
| --- | --- | --- |
| Autenticación | Sesiones Django para todos los roles, sin registro público. | Acceso válido, rechazo de credenciales inválidas, cuenta desactivada y cierre de sesión. |
| Validación | Validar entradas y relaciones en Django y rangos en la base de datos. | Rechazar probabilidad/impacto fuera de 1–5, progreso fuera de 0–100 y transiciones inválidas. |
| CSRF | Mantener protección Django en los formularios y en cualquier futura mutación React basada en sesión. | Rechazar operaciones sin token válido; no modificar datos mediante GET. |
| XSS | Escapado de plantillas y texto en React; evitar renderizar HTML aportado por usuarios. | Introducir texto con etiquetas o scripts y comprobar que no se ejecuta. |
| Hashing de contraseñas | Usar las funciones de contraseñas de Django. | Verificar que el almacenamiento utiliza hash y no texto plano. |
| Secretos | Variables de entorno para clave Django, base de datos y credenciales; ejemplos sin valores sensibles. | Revisar archivos, historial y registros para evitar exposición; comprobar configuración externa. |
| HTTPS | Configurar conexión segura y opciones de cookies y redirección acordes al despliegue. | Verificar certificado, acceso HTTPS y configuración de sesión y CSRF de producción. |
| Copias de seguridad | Copiar la base de datos con almacenamiento protegido y procedimiento de restauración. | Restaurar una copia y comprobar riesgos, evaluaciones, acciones e historial. |

## Autorización y alcance

Se aplicará la [matriz de permisos confirmada](03-usuarios.md) en el backend. El responsable podrá consultar riesgos a su cargo o con acciones asignadas, pero solo actualizará el progreso y las notas de sus propias acciones en EN TRATAMIENTO. Gestor y administrador tendrán alcance de toda la empresa. Estos controles todavía no están implementados.

El diseño previsto filtrará objetos y agregados antes de responder a React y rechazará peticiones directas fuera del alcance autorizado. La verificación deberá comprobar que identificadores, contadores y mensajes no revelen datos ajenos. La autorización se comprobará mediante peticiones directas al backend y revisión de la interfaz.

## Integridad y trazabilidad

Las reglas confirmadas exigen calcular puntuación y nivel en Django, sin aceptar valores derivados enviados por el cliente, y conservar las evaluaciones anteriores. Completar MFA u otra acción no reducirá automáticamente el riesgo: el flujo previsto requerirá una reevaluación. Estos controles todavía no están implementados.

Para mantener la consistencia, se propone guardar el cambio y su evento en una transacción, validar las condiciones de cierre y controlar operaciones concurrentes sobre el mismo riesgo. La comprobación deberá rechazar cierres sin reevaluación apta, con nivel ALTO o CRÍTICO, acciones incompletas o responsable inactivo, según [02-requisitos.md](02-requisitos.md). También deberá impedir cambios de acciones en MONITORIZACIÓN y cambios de datos, evaluaciones o acciones en CERRADO. Las funciones ordinarias no permitirán alterar el historial y las referencias se protegerán al desactivar usuarios. Estos controles siguen pendientes de implementación y prueba.

El historial confirmado como requisito permitirá seguir el proceso de negocio y conservará actor, fecha, cambios y notas de avance obligatorias, sin sustituir los registros anteriores. Su acceso tendrá el mismo alcance que la ficha del riesgo. Deberá comprobarse que una nota vacía no modifica el progreso ni deja un avance parcial. No se plantea como un registro forense inalterable. Su implementación y estas comprobaciones están pendientes; la memoria explicará su alcance y sus limitaciones.

## Producción, conservación y copias

Para producción se propone desactivar el modo de depuración, configurar hosts y orígenes válidos, revisar cookies seguras y permisos de base de datos, y limitar los registros a información necesaria sin credenciales ni contenido sensible.

Quedan pendientes de concretar con el proveedor la frecuencia y retención de copias, la ubicación protegida, el acceso, el cifrado si corresponde y los responsables de restauración. Todavía no se dispone de pruebas que permitan fijar tiempos de recuperación.

La aplicación y su base de datos no contendrán información empresarial real en la demostración. La política para datos reales y cualquier conclusión de cumplimiento legal siguen pendientes de análisis.

## Evidencias para la memoria

Cada medida implementada deberá documentarse con su objetivo de protección, punto de aplicación, resultado de la comprobación y limitaciones. Se dará prioridad a las verificaciones de permisos mediante peticiones directas, datos inválidos, CSRF, representación segura de texto, conservación del historial y restauración.

Todas las evidencias están pendientes. El uso previsto de Django y esta documentación de diseño todavía no acreditan la seguridad ni el cumplimiento legal de la aplicación.
