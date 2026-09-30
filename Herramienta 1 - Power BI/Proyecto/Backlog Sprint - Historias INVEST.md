# Backlog - (Power BI V2)

## Sprint 1 — Fundamento del modelo + arranque de captura de datos

### HU-00.1 — Conectar las 8 tablas del Excel al modelo único

**Como** equipo de desarrollo, **quiero** conectar Actores, Proyectos, Entidades, Proyecto_Socios, Metas_Indicador, Bateria_Indicadores, Seguimiento_Indicador y TablasODS desde CONSOLIDADO HISTORICO FACULTAD V1.xlsx a un único archivo .pbix, **para** tener una sola fuente de datos en vez de dos .pbix separados.

- **Dado** el Excel fuente disponible, **cuando** se abre el nuevo .pbix, **entonces** las 8 tablas aparecen cargadas y se pueden explorar en el panel de modelo.
- Talla: **M**. Depende de: nada (es la primera historia). Independiente de las demás de este sprint.

### HU-00.2 — Migrar las relaciones (claves) del modelo original al modelo único

**Como** equipo de desarrollo, **quiero** recrear las relaciones entre tablas (codigo_proyecto, codigo_entidad, codigo_indicador, etc.) en el modelo unificado, **para** que los cruces entre Actores/Proyectos/Entidades/Indicadores funcionen igual que en los .pbix actuales.

- **Dado** el modelo con las 8 tablas cargadas (HU-00.1), **cuando** se relacionan por sus llaves, **entonces** un filtro en Proyectos afecta correctamente a Actores, Entidades y Seguimiento_Indicador.
- Talla: **S**. Depende de: HU-00.1.

### HU-00.3 — Consolidar las medidas duplicadas de los dos .pbix

**Como** equipo de desarrollo, **quiero** revisar las medidas de ConsolidadoFING_V5 y de 2630 CONSOLIDADO PROYECTOS SOCIALES FING V2 (Total Proyectos, Total Entidades, los *_Medida, etc.) y dejar una sola versión de cada una en el modelo nuevo, **para** no arrastrar medidas duplicadas o contradictorias.

- **Dado** las medidas de ambos .pbix originales, **cuando** se comparan, **entonces** queda una tabla de medidas única sin duplicados, documentada (qué reemplazó a qué).
- Talla: **S**. Depende de: HU-00.1.

### HU-01 — Tabla de fechas/semestre común (DimSemestre)

**Como** equipo de desarrollo, **quiero** una tabla DimSemestre relacionada a Actores, Proyectos y Seguimiento_Indicador, **para** poder comparar todo en la misma línea de tiempo.

- **Dado** el modelo unificado, **cuando** se filtra por un semestre en DimSemestre, **entonces** Estudiantes, Proyectos e Indicadores se filtran igual, en todas las páginas.
- Talla: **S**. Depende de: HU-00.1, HU-00.2.

### HU-02 — Refresco programado del modelo

**Como** coordinador de la oficina, **quiero** que el .pbix se refresque solo con una periodicidad definida, **para** no depender de refrescos manuales antes de cada reunión.

- **Dado** el modelo publicado en Power BI Service, **cuando** llega la hora programada, **entonces** el modelo se actualiza automáticamente con los datos más recientes del Excel/lista.
- Talla: **S**. Depende de: HU-00.1 y de tener el workspace de Power BI Service creado.

### HU-03.1 — Definir la matriz de niveles de RLS por rol

**Como** equipo de desarrollo, **quiero** documentar qué ve cada rol (oficina, decano, docentes, coordinadores, facultades externas, voluntarios/estudiantes, interlocutores/entidades), **para** tener un criterio único antes de programar los roles de seguridad.

- **Dado** la lista de roles confirmada, **cuando** se documenta la matriz rol→filtro, **entonces** cualquiera del equipo puede leerla y saber qué filas debería ver cada rol sin ambigüedad.
- Talla: **S**. Depende de: nada. Es prerequisito de HU-03.2 y HU-03.3.

### HU-31.1 — Formulario de Microsoft Forms para altas de Actores

**Como** equipo de desarrollo, **quiero** un formulario de Forms con los campos mínimos de un Actor nuevo (documento, nombre, correo, carrera, proyecto), **para** que PROSOFI pueda capturar altas sin editar el Excel directamente.

- **Dado** el formulario publicado, **cuando** alguien de PROSOFI lo llena y envía, **entonces** la respuesta queda registrada en Forms con todos los campos requeridos completos (validación básica de campos obligatorios).
- Talla: **S**. Depende de: nada. Se puede construir en paralelo a HU-00.x.

### HU-31.2 — Flujo de Power Automate: Forms de Actores → Excel

**Como** equipo de desarrollo, **quiero** un flujo que tome cada respuesta del formulario de Actores (HU-31.1) y la escriba como fila nueva en la hoja Actores del Excel, **para** que la alta quede reflejada en la fuente de datos sin intervención manual.

- **Dado** una respuesta nueva en el formulario de Actores, **cuando** se dispara el flujo, **entonces** aparece una fila nueva en la hoja Actores del Excel con los datos capturados, en menos de 5 minutos.
- Talla: **S**. Depende de: HU-31.1.

---

## Sprint 2 — RLS completo, captura de proyectos, geocodificación, home

### HU-03.2 — Implementar y probar RLS para roles internos

**Como** equipo de desarrollo, **quiero** programar los roles de seguridad de oficina, decano, docentes y coordinadores de prácticas según la matriz de HU-03.1, **para** que cada uno vea solo lo que le corresponde.

- **Dado** un usuario de prueba por cada rol interno, **cuando** abre el tablero, **entonces** ve exactamente las filas que la matriz de HU-03.1 define para su rol (verificado con al menos 1 usuario por rol).
- Talla: **M**. Depende de: HU-03.1, HU-00.2.

### HU-03.3 — Implementar y probar RLS para roles externos

**Como** equipo de desarrollo, **quiero** programar los roles de seguridad de facultades externas, voluntarios/estudiantes e interlocutores/entidades, **para** que su vista quede acotada a su propia participación.

- **Dado** un usuario de prueba por cada rol externo, **cuando** abre el tablero, **entonces** solo ve su propio proyecto/entidad/participación, nunca datos de otros.
- Talla: **M**. Depende de: HU-03.1, HU-00.2.

### HU-31.3 — Formulario de Microsoft Forms para altas de Proyectos

**Como** equipo de desarrollo, **quiero** un formulario de Forms con los campos mínimos de un Proyecto nuevo (título, entidad, temática, dirección/municipio, docente encargado), **para** capturar altas de proyectos sin editar el Excel directamente.

- **Dado** el formulario publicado, **cuando** se llena y envía, **entonces** la respuesta queda registrada con todos los campos obligatorios completos.
- Talla: **S**. Depende de: nada (independiente de HU-31.1/31.2, es su propio par formulario+flujo).

### HU-31.4 — Flujo de Power Automate: Forms de Proyectos → Excel

**Como** equipo de desarrollo, **quiero** un flujo que tome cada respuesta del formulario de Proyectos (HU-31.3) y la escriba como fila nueva en la hoja Proyectos del Excel, **para** que la alta quede reflejada en la fuente de datos.

- **Dado** una respuesta nueva en el formulario de Proyectos, **cuando** se dispara el flujo, **entonces** aparece una fila nueva en la hoja Proyectos del Excel, en menos de 5 minutos.
- Talla: **S**. Depende de: HU-31.3.

### HU-31.5 — Diagnóstico: por qué las coordenadas de Proyectos no sirven

**Como** equipo de desarrollo, **quiero** documentar con evidencia por qué las coordenadas actuales de Proyectos no se pueden usar en un mapa (longitud = latitud repetida, sin campo de dirección exacta en Proyectos), **para** explicárselo al director y preguntarle cómo conseguir la ubicación real, sin comprometernos todavía a una forma técnica de resolverlo.

- **Dado** la tabla Proyectos, **cuando** se revisan los campos latitud/longitud y dirección, **entonces** queda un hallazgo documentado (con ejemplos concretos) listo para incluir en el correo/pregunta al director.
- Talla: **S**. Depende de: nada. **No implica** construir ninguna solución de geocodificación — eso queda pendiente de lo que responda el director (ver documento de preguntas).

### HU-04 — Página de inicio con KPIs y navegación

**Como** cualquier usuario, **quiero** una página de inicio con KPIs generales (Total Proyectos, Total Entidades, Total Estudiantes en Práctica, Total Docentes Encargados) y accesos directos a cada sección, **para** orientarme sin buscar entre páginas sueltas.

- **Dado** el modelo unificado (Sprint 1), **cuando** abro el tablero, **entonces** veo los 4 KPIs con el dato correcto del semestre activo y puedo navegar a Estudiantes, Proyectos, Entidades, Indicadores/ODS y Consulta desde ahí.
- Talla: **S**. Depende de: HU-00.1, HU-00.2, HU-01.

### HU-05 — Slicer de semestre sincronizado

**Como** usuario, **quiero** filtrar todo el tablero por semestre desde la página de inicio, **para** no repetir el filtro en cada página.

- **Dado** el slicer de semestre en Inicio, **cuando** cambio el semestre seleccionado, **entonces** todas las páginas del tablero se filtran igual, sin tener que repetir el filtro.
- Talla: **S**. Depende de: HU-01, HU-04.

---

## Sprint 3 — Estudiantes y Proyectos

### HU-06 — Listado de estudiantes filtrable

**Como** coordinador, **quiero** ver el listado de estudiantes en práctica filtrable por carrera, semestre, tipo de actor y asignatura, **para** tener un único listado en vez de Excel + dos páginas separadas.

- **Dado** el modelo con RLS activo, **cuando** aplico cualquier combinación de esos 4 filtros, **entonces** la tabla muestra solo los estudiantes que cumplen el filtro, sin errores de conteo.
- Talla: **S**. Depende de: HU-00.x, HU-03.2/HU-03.3.

### HU-07 — Distribución por sexo

**Como** coordinador, **quiero** ver el % de estudiantes mujeres/hombres en práctica, **para** reportarlo en indicadores de equidad.

- **Dado** el listado de HU-06, **cuando** abro el visual de distribución por sexo, **entonces** los porcentajes suman 100% y coinciden con el total filtrado.
- Talla: **S**. Depende de: HU-06.

### HU-08 — Evolución histórica de estudiantes (multianual)

**Como** decano, **quiero** ver la evolución del número de estudiantes en práctica por semestre a lo largo de varios años, **para** sustentar el crecimiento del programa.

- **Dado** el histórico completo cargado, **cuando** abro el gráfico de tendencia, **entonces** se ven todos los semestres disponibles en el Excel, no solo el activo.
- Talla: **S**. Depende de: HU-01, HU-06.

### HU-09 — Vista de estudiantes acotada por docente (RLS aplicado)

**Como** profesor coordinador, **quiero** ver únicamente los estudiantes asignados a mis proyectos, **para** no ver el resto de la facultad.

- **Dado** un usuario con rol de profesor, **cuando** entra a la página de Estudiantes, **entonces** solo ve los estudiantes de sus propios proyectos (encargado_docente = su usuario).
- Talla: **S**. Depende de: HU-06, HU-03.2.

### HU-10 — Listado de proyectos filtrable

**Como** coordinador, **quiero** un listado de proyectos con temática, tipo de actividad, docente encargado y estado, **para** monitorear la cartera completa.

- **Dado** el modelo unificado, **cuando** aplico cualquiera de esos filtros, **entonces** la tabla muestra solo los proyectos que cumplen el filtro.
- Talla: **S**. Depende de: HU-00.x, HU-03.2/HU-03.3.

### HU-11 — Proyectos agrupados por estado

**Como** coordinador, **quiero** ver los proyectos agrupados por estado (Nuevo, En ejecución, Finalizado), **para** identificar cuáles necesitan seguimiento o cierre.

- **Dado** el listado de HU-10, **cuando** abro el visual de conteo por estado, **entonces** se ve una categoría aparte para "sin estado registrado" (dato vacío), sin perderlos del conteo total.
- Talla: **S**. Depende de: HU-10.

### HU-12 — Mapa de proyectos con filtros

**Como** usuario, **quiero** ver los proyectos en un mapa filtrable por tipo de proyecto y otros atributos, **para** entender la cobertura territorial.

- **Dado** el modelo con las coordenadas que existan hoy (placeholder, ver HU-31.5), **cuando** abro el mapa, **entonces** puedo filtrar por tipo_proyecto, tematica y tipo_actividades, y ver cada proyecto ubicado con la información disponible.
- Talla: **S**. Depende de: HU-10. La precisión real de la ubicación queda pendiente de lo que se defina con el director (HU-31.5 es solo diagnóstico, no la resuelve); el mapa se construye igual con lo que haya.

### HU-13 — Ficha de proyecto para interlocutor

**Como** interlocutor de entidad, **quiero** ver el detalle de mi(s) proyecto(s): objetivos, duración, fechas, docente encargado y estudiantes asignados, **para** hacer seguimiento sin pedirlo por correo.

- **Dado** un usuario con rol de interlocutor, **cuando** entra a la ficha de su proyecto, **entonces** ve todos esos campos y solo de su(s) propio(s) proyecto(s).
- Talla: **S**. Depende de: HU-10, HU-03.3.

---

## Sprint 4 — Entidades e Indicadores/ODS

### HU-14 — Listado de entidades filtrable

**Como** coordinador, **quiero** un listado de entidades con sector económico, CIIU, localidad y estado del convenio, **para** saber con quién se tiene relación activa.

- **Dado** el modelo unificado, **cuando** aplico cualquiera de esos filtros, **entonces** la tabla muestra solo las entidades que cumplen el filtro.
- Talla: **S**. Depende de: HU-00.x, HU-03.2.

### HU-15 — Alerta de convenios próximos a vencer

**Como** coordinador, **quiero** una alerta de convenios que vencen pronto, **para** gestionar la renovación a tiempo.

- **Dado** la fecha_vencimiento_convenio de cada entidad, **cuando** faltan menos de 90 días para esa fecha, **entonces** la entidad aparece marcada con un semáforo/alerta visual en el listado de HU-14.
- Talla: **S**. Depende de: HU-14.

### HU-16 — Proyectos por entidad

**Como** coordinador, **quiero** ver cuántos y cuáles proyectos tiene cada entidad, **para** identificar relación recurrente vs. puntual.

- **Dado** la relación Entidades↔Proyecto_Socios, **cuando** selecciono una entidad, **entonces** veo el conteo y el listado de sus proyectos asociados.
- Talla: **S**. Depende de: HU-14, HU-10.

### HU-17 — Entidades por sector y localidad

**Como** decano, **quiero** ver entidades agrupadas por sector económico y localidad, **para** entender el perfil de las organizaciones atendidas.

- **Dado** el listado de HU-14, **cuando** abro el visual de distribución, **entonces** se ve el conteo de entidades por sector_economico y por localidad, correctamente sumado.
- Talla: **S**. Depende de: HU-14.

### HU-18 — % de cumplimiento de metas por proyecto

**Como** coordinador, **quiero** ver el % de cumplimiento de cada indicador por proyecto, **para** saber qué proyectos van atrasados.

- **Dado** Seguimiento_Indicador cargado, **cuando** abro el visual, **entonces** veo el % de cumplimiento (valor_avance/meta_indicador) por proyecto, filtrable por familia_indicador.
- Talla: **S**. Depende de: HU-00.x.

### HU-19 — Proyectos por ODS principal

**Como** decano, **quiero** ver a qué ODS contribuye cada proyecto, con el ícono oficial, **para** reportar el impacto social en el lenguaje institucional.

- **Dado** TablasODS relacionada a Proyectos, **cuando** abro el visual, **entonces** veo el conteo de proyectos por cada uno de los 6 ODS prioritarios, con su ícono correspondiente.
- Talla: **S**. Depende de: HU-00.x.

### HU-20 — Línea de tiempo de avance por indicador

**Como** coordinador, **quiero** ver la evolución del avance de un indicador en el tiempo, **para** ver la trayectoria del proyecto, no solo la última medición.

- **Dado** varios registros de Seguimiento_Indicador para un mismo indicador, **cuando** selecciono ese indicador, **entonces** veo un gráfico de línea con cada fecha_registro y su valor_avance.
- Talla: **S**. Depende de: HU-18.

### HU-21 — Validación de indicadores huérfanos

**Como** equipo de desarrollo, **quiero** un reporte de codigo_indicador usados en Metas/Seguimiento que no existan en Bateria_Indicadores, **para** evitar indicadores mal escritos o huérfanos.

- **Dado** las 3 tablas de indicadores cargadas, **cuando** corro la validación, **entonces** obtengo la lista de códigos que no calzan contra el catálogo maestro (idealmente 0).
- Talla: **S**. Depende de: HU-00.x.

---

## Sprint 5 — Consulta, calidad de datos y reporte ejecutivo

### HU-22 — Buscador de estudiante (ficha unificada)

**Como** coordinador o profesor, **quiero** buscar un estudiante por documento o nombre y ver su ficha completa, **para** resolver consultas puntuales rápido.

- **Dado** un número de documento o nombre válido, **cuando** lo busco, **entonces** veo proyecto, entidad, docente y semestre de ese estudiante en una sola pantalla.
- Talla: **S**. Depende de: HU-06.

### HU-23 — Ficha de proyecto seleccionado (drill-through)

**Como** coordinador, **quiero** seleccionar un proyecto y ver su entidad, socios, docente, indicadores y estudiantes en una ficha, **para** preparar reuniones sin cruzar Excel a mano.

- **Dado** un proyecto seleccionado, **cuando** hago drill-through, **entonces** veo entidad, socios, docente, indicadores y estudiantes de ese proyecto en una sola vista.
- Talla: **S**. Depende de: HU-10, HU-14, HU-18.

### HU-24 — Medida de completitud de campos críticos

**Como** equipo de desarrollo, **quiero** un % de campos vacíos por tabla clave (estado, fechas, coordenadas, correo, ODS Principal), **para** priorizar qué pedir que se diligencie mejor.

- **Dado** el modelo cargado, **cuando** abro la página de calidad de datos, **entonces** veo el % de completitud por cada campo crítico listado.
- Talla: **S**. Depende de: HU-00.x. Visible solo para el equipo de desarrollo/coordinador (usa RLS de HU-03.2).

### HU-25 — Detección de valores placeholder

**Como** equipo de desarrollo, **quiero** una lista de registros con "NULOS" u otros valores sospechosos en titulo_proyecto/carrera_programa, **para** limpiarlos antes de mostrarlos al decano.

- **Dado** el modelo cargado, **cuando** corro la detección, **entonces** obtengo la lista de filas sospechosas con su tabla y campo.
- Talla: **S**. Depende de: HU-00.x.

### HU-26 — Página de resumen ejecutivo exportable

**Como** coordinador, **quiero** exportar un resumen con los KPIs y el impacto ODS del semestre, **para** la reunión final con profesores y decano.

- **Dado** los KPIs (HU-04) y el visual de ODS (HU-19) construidos, **cuando** exporto la página de resumen a PDF, **entonces** el PDF trae los KPIs y el mapa/distribución ODS legibles.
- Talla: **S**. Depende de: HU-04, HU-19.

---

## Sprint 6 — ARL y firma de vinculación (bloqueado por cambio en el Excel fuente)

### HU-EN-1 — Agregar campos de ARL y vinculación al Excel fuente (prerrequisito)

**Como** equipo de desarrollo, **quiero** agregar las columnas arl_afiliada, fecha_afiliacion_arl a Actores y estado_firma_vinculacion/fecha_firma/link a Entidades (o una tabla de Interlocutores), **para** tener dónde capturar esta información antes de poder visualizarla.

- **Dado** el Excel fuente actual, **cuando** se agregan esas columnas, **entonces** el modelo las puede cargar sin romper las relaciones existentes.
- Talla: **S**. Depende de: acuerdo con PROSOFI sobre quién diligencia esos campos (ver documento de preguntas). **Bloquea** a HU-27 a HU-30.

### HU-27 — Estudiantes sin ARL vigente antes de iniciar

**Como** coordinador, **quiero** ver qué estudiantes no tienen ARL vigente antes de su fecha de inicio de práctica, **para** evitar que empiecen sin cobertura.

- **Dado** arl_afiliada y fecha_afiliacion_arl cargados, **cuando** fecha_afiliacion_arl es posterior a fecha_inicio_practica (o está vacía), **entonces** el estudiante aparece en la lista de pendientes.
- Talla: **S**. Depende de: HU-EN-1.

### HU-28 — Alerta de estudiantes próximos a iniciar sin ARL

**Como** coordinador, **quiero** una alerta de estudiantes que inician pronto sin ARL registrada, **para** gestionar la afiliación a tiempo.

- **Dado** la lista de HU-27, **cuando** fecha_inicio_practica está a menos de 15 días, **entonces** el estudiante aparece resaltado como urgente.
- Talla: **S**. Depende de: HU-27.

### HU-29 — Estado de firma de vinculación de no-estudiantes

**Como** coordinador, **quiero** ver qué interlocutores/dueños de entidad tienen firmada la vinculación, **para** saber quién falta antes de iniciar el proyecto.

- **Dado** estado_firma_vinculacion cargado, **cuando** abro el visual por entidad/proyecto, **entonces** veo Sí/No y, si existe, el link al documento firmado.
- Talla: **S**. Depende de: HU-EN-1.

### HU-30 — Documentación pendiente por proyecto (ARL + vinculación)

**Como** coordinador, **quiero** un listado único que cruce ARL de estudiantes y firma de vinculación de no-estudiantes por proyecto, **para** tener un solo punto de control.

- **Dado** HU-27 y HU-29 construidas, **cuando** abro la vista combinada, **entonces** veo por código_proyecto si falta ARL, falta firma, o está completo.
- Talla: **S**. Depende de: HU-27, HU-29.

---

## Resumen por sprint

| Sprint | Foco                                                                        | # historias |
| ------ | --------------------------------------------------------------------------- | ----------- |
| 1      | Modelo base + RLS (diseño) + arranque Forms/Automate (Actores)             | 8           |
| 2      | RLS implementado + Forms/Automate Proyectos + geocodificación + Inicio     | 8           |
| 3      | Estudiantes + Proyectos (páginas completas)                                | 8           |
| 4      | Entidades + Indicadores/ODS                                                 | 8           |
| 5      | Consulta + calidad de datos + reporte ejecutivo                             | 5           |
| 6      | ARL y firma de vinculación (bloqueado hasta que el Excel tenga los campos) | 5           |

*Los sprints 3, 4 y 5 se pueden reordenar o correr en paralelo entre sí si hay más de una persona desarrollando — no tienen dependencias cruzadas entre ellos, solo dependen del modelo base (Sprint 1-2). El Sprint 6 solo puede arrancar cuando PROSOFI confirme quién diligencia los campos nuevos del Excel (ver documento de preguntas/correo al director).*
