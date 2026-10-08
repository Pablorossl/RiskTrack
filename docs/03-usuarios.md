# 03. Tipos de usuarios y permisos

**Estado:** confirmados los usuarios internos de una empresa, los tres roles y la matriz de permisos del MVP. Su implementación y verificación siguen pendientes.

**Referencia obligatoria:** [guía del PFM](../INSTRUCCIONES-PFM.md), «4. Definición de tipos de usuarios».

## Roles confirmados

| Rol | Necesidad | Acciones permitidas |
| --- | --- | --- |
| Administrador | Gestionar accesos y mantener el registro de la empresa. | Crear y desactivar usuarios, asignar roles y realizar las operaciones del gestor. |
| Gestor de riesgos | Priorizar y coordinar el tratamiento de riesgos de TI. | Crear y evaluar riesgos, asignar responsables, crear acciones, monitorizar la exposición y cerrar riesgos. |
| Responsable | Tratar y seguir los riesgos o acciones que se le asignan. | Consultar riesgos dentro de su alcance y actualizar el progreso de sus propias acciones. |

Todos los roles requieren autenticación y cada cuenta tendrá un solo rol de negocio. La representación técnica mediante grupos Django y la relación con el panel administrativo figuran como recomendaciones pendientes de confirmación en [08-decisiones-mvp.md](08-decisiones-mvp.md).

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

## Gestión de acceso

El administrador crea las cuentas, sin registro público. Solo usuarios activos podrán recibir nuevas asignaciones. El flujo de desactivación conservará el historial y resolverá las asignaciones abiertas mediante reasignación.

La URL de la aplicación será pública para el PFM. El registro de riesgos requerirá acceso autenticado; la demostración utilizará datos ficticios y se facilitará acceso al equipo docente sin publicar secretos en el repositorio.

## Casos de uso por rol

- Administrador: CU-A01 (gestionar acceso) y CU-A02 (reasignar trabajo antes de desactivar).
- Gestor: CU-G01 a CU-G05 (identificar, evaluar, asignar, monitorizar y cerrar).
- Responsable: CU-R01 (consultar su trabajo) y CU-R02 (actualizar progreso).

Los flujos y errores se describen en [04-casos-de-uso.md](04-casos-de-uso.md); las comprobaciones de autorización previstas figuran en [07-seguridad.md](07-seguridad.md).
