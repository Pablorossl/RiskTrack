# 03. Tipos de usuarios y permisos

**Estado:** confirmados los usuarios internos de una empresa, los tres roles y la matriz de permisos del MVP. Su implementación y verificación siguen pendientes.

**Referencia obligatoria:** [guía del PFM](../INSTRUCCIONES-PFM.md), «4. Definición de tipos de usuarios».

## Roles confirmados

| Rol | Necesidad | Acciones permitidas |
| --- | --- | --- |
| Administrador | Gestionar accesos y mantener el registro de la empresa. | Crear y desactivar usuarios, asignar roles y realizar las operaciones del gestor. |
| Gestor de riesgos | Priorizar y coordinar el tratamiento de riesgos de TI. | Crear y evaluar riesgos, asignar responsables, crear acciones, monitorizar la exposición y cerrar riesgos. |
| Responsable | Tratar y seguir los riesgos o acciones que se le asignan. | Consultar riesgos dentro de su alcance y actualizar el progreso de sus propias acciones. |

Todos los roles requieren autenticación y cada cuenta tendrá un solo rol de negocio, representado mediante un grupo Django. Esta base técnica se adopta en [08-decisiones-mvp.md](08-decisiones-mvp.md); su implementación sigue pendiente.

Se utilizarán el usuario estándar de Django y su admin adaptado para gestionar cuentas. Solo el rol Administrador tendrá acceso ordinario al panel, con los permisos necesarios para cuentas y roles, validando reasignaciones antes de desactivar. La pertenencia al rol no concederá privilegios de superusuario ni permitirá modificar libremente privilegios técnicos. Gestor y Responsable utilizarán las vistas de la aplicación. Estos controles están pendientes de implementación y verificación.

Cualquier usuario activo con uno de estos tres roles podrá ser responsable de un riesgo o encargado de una acción. Recibir una asignación no cambia su rol ni concede permisos adicionales.

Un puesto como «IT Security Manager» describe una función profesional. Laura, del ejemplo, podría tener el rol Responsable o Gestor según las acciones autorizadas; el nombre del puesto no concede permisos.

## Matriz de permisos confirmada

| Operación | Administrador | Gestor | Responsable |
| --- | --- | --- | --- |
| Gestionar usuarios y roles | Sí | No | No |
| Consultar registro, fichas e historial | Todos los riesgos | Todos los riesgos | Solo riesgos asignados a él o con acciones a su cargo |
| Consultar dashboard | Exposición de la empresa | Exposición de la empresa | Resumen limitado a su alcance |
| Crear o editar datos del riesgo | Sí | Sí | No |
| Evaluar o reevaluar | Sí | Sí | No |
| Asignar riesgo o acción | Sí | Sí | No |
| Crear acciones | Sí | Sí | No |
| Actualizar progreso de una acción | Sí | Sí | Solo acciones a su cargo |
| Cambiar estado o cerrar riesgo | Sí | Sí | No |

El alcance de los permisos se aplicará tanto en las plantillas como en los datos que recibe React. El responsable no podrá modificar una acción ajena aunque pueda consultar el riesgo al que pertenece. La actualización del progreso y de las notas de avance solo estará habilitada en EN TRATAMIENTO; en EVALUADO se prepararán y asignarán las acciones.

Cada avance requerirá una nota no vacía y conservará la anterior. Las evaluaciones y entradas del historial serán de consulta; una corrección se registrará como una nueva operación autorizada. Ningún rol dispondrá de operaciones de negocio para editar o borrar registros históricos.

## Gestión de acceso

El administrador creará las cuentas, sin registro público. Solo usuarios activos con uno de los tres roles podrán recibir nuevas asignaciones. Las cuentas se desactivarán mediante `is_active`; no habrá eliminación de usuarios en las operaciones ordinarias.

### Regla operativa adoptada

La desactivación se rechazará mientras la cuenta sea responsable de algún riesgo abierto o encargada de alguna acción sin completar en un riesgo abierto. El formulario mostrará los registros que deben reasignarse. El administrador realizará cada reasignación mediante las vistas de negocio y volverá después al admin para desactivar la cuenta. No se hará una transferencia masiva automática ni se cambiarán estados de riesgo desde el formulario de usuarios.

| Asignación | Tratamiento antes de desactivar |
| --- | --- |
| Responsable de un riesgo en IDENTIFICADO, EVALUADO, EN TRATAMIENTO o MONITORIZACIÓN. | Reasignar a otra cuenta activa con rol válido, conservando el evento. En MONITORIZACIÓN el cambio de responsable exige otra evaluación antes del cierre. |
| Encargado de una acción con progreso inferior al 100% en un riesgo abierto. | Reasignar en EVALUADO o EN TRATAMIENTO. Una acción incompleta en MONITORIZACIÓN sería un estado inconsistente: se rechazará la desactivación y se revisará el riesgo, sin saltarse sus reglas. |
| Encargado de una acción completada, incluso en un riesgo abierto. | Mantener la asignación como referencia del trabajo realizado; no bloquea la desactivación. |
| Asignaciones de un riesgo CERRADO, autorías y entradas del historial. | Conservarlas sin cambios; no bloquean la desactivación. |

Si más adelante se necesita bajar del 100% el progreso de una acción cuyo encargado está inactivo, el gestor o administrador deberá estar en EN TRATAMIENTO y reasignarla primero a una cuenta activa. El avance posterior requerirá su nota obligatoria. Esta comprobación evita dejar trabajo pendiente a cargo de una cuenta inactiva.

Cambiar entre los tres roles conservará las asignaciones, porque cualquiera de ellos puede recibirlas. El nuevo rol determinará inmediatamente el alcance de las siguientes peticiones, incluida una sesión ya iniciada. No se admitirán cuentas de negocio sin rol ni con varios roles. Una reactivación conservará el rol y las asignaciones que todavía existan; no recuperará las transferidas.

El administrador no podrá desactivar su propia cuenta ni cambiar su propio rol, y deberá quedar al menos un Administrador activo. El formulario ofrecerá un único selector de rol y derivará de él los grupos y el acceso al admin. Los privilegios técnicos, los permisos individuales, la edición libre de grupos y las cuentas de superusuario quedarán fuera de la gestión ordinaria. El superusuario se reservará para la configuración técnica inicial y recuperación de acceso.

Las reasignaciones se anotarán en el historial de cada riesgo. Las altas y cambios de cuentas se registrarán mediante el registro del admin de Django, sin contraseñas ni secretos. Al guardar una desactivación se volverán a comprobar las asignaciones dentro de la operación; los cambios concurrentes no deberán permitir asignar trabajo nuevo a una cuenta que se está desactivando. Las reglas están decididas, pero pendientes de implementación y prueba.

La URL de la aplicación será pública para el PFM. El registro de riesgos requerirá acceso autenticado; la demostración utilizará datos ficticios y se facilitará acceso al equipo docente sin publicar secretos en el repositorio.

## Casos de uso por rol

- Administrador: CU-A01 (gestionar acceso) y CU-A02 (reasignar trabajo antes de desactivar).
- Gestor: CU-G01 a CU-G05 (identificar, evaluar, asignar, monitorizar y cerrar).
- Responsable: CU-R01 (consultar su trabajo) y CU-R02 (actualizar progreso).

Los flujos y errores se describen en [04-casos-de-uso.md](04-casos-de-uso.md); las comprobaciones de autorización previstas figuran en [07-seguridad.md](07-seguridad.md).
