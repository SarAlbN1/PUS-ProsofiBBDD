# Batería de Indicadores Propuesta — Power BI PROSOFI

*Extraída y ampliada de la sección 13 del documento de Historias de Usuario. No hay una lista de indicadores definida por la oficina ("no me dijeron cuáles"), así que este es un catálogo propuesto para que el profesor/la oficina lo valide (ver pregunta 9 del documento de preguntas). Los marcados con * ya existen como medida en alguno de los dos .pbix actuales; el resto son nuevos.*

## 1. Cómo está organizada

Cada indicador tiene: categoría, qué mide, cómo se calcula, de qué tabla sale, y con qué frecuencia tiene sentido revisarlo. Las tablas fuente son las del Excel CONSOLIDADO HISTORICO: Actores, Proyectos, Entidades, Proyecto_Socios, Metas_Indicador, Bateria_Indicadores, Seguimiento_Indicador, TablasODS.

## 2. Académicos y de cobertura

| Indicador | Cómo se calcula | Fuente | Frecuencia |
|---|---|---|---|
| * Total estudiantes en práctica | Conteo de Actores por semestre/carrera | Actores | Semestral |
| * Total proyectos activos/nuevos/finalizados | Conteo de Proyectos por estado | Proyectos | Semestral |
| Tasa de finalización de práctica | Estudiantes con fecha_fin_practica registrada / total del semestre | Actores | Semestral |
| Docentes encargados activos | Conteo distinto de encargado_docente con proyecto vigente | Proyectos | Semestral |
| Carga por docente | Proyectos o estudiantes por docente encargado | Proyectos, Actores | Semestral |
| Diversidad de carreras participantes | Conteo de carrera_programa distintas por semestre | Actores | Semestral |
| Alcance interfacultades (nuevo rol: facultades externas) | % de estudiantes con carrera_programa fuera de Ingeniería | Actores | Semestral |
| Participantes por tipo de actor | Conteo por tipo_actor (Practicante, Voluntario, etc.) — separa estudiantes PSU/CDIO-3 de voluntarios | Actores | Semestral |

## 3. Equidad

| Indicador | Cómo se calcula | Fuente | Frecuencia |
|---|---|---|---|
| * % Estudiantes mujeres / hombres en práctica | Conteo por sexo / total | Actores | Semestral |

## 4. Territoriales

| Indicador | Cómo se calcula | Fuente | Frecuencia |
|---|---|---|---|
| Entidades y proyectos por localidad/departamento | Conteo agrupado por localidad, departamento | Entidades, Proyectos | Semestral |
| % Cobertura territorial | Localidades/municipios atendidos con proyecto activo / total de referencia | Proyectos | Semestral |
| Mapa de proyectos por tipo | Categorizable por tipo_proyecto, tematica, tipo_actividades — la ubicación real está pendiente de diagnóstico (las coordenadas actuales son placeholder, ver "Alcance por Etapas") | Proyectos | Continuo |

## 5. Alianzas y sostenibilidad (entidades)

| Indicador | Cómo se calcula | Fuente | Frecuencia |
|---|---|---|---|
| Entidades con convenio vigente vs. vencido | Conteo por estado_convenio_javelex | Entidades | Mensual |
| % Convenios próximos a vencer (< 90 días) | fecha_vencimiento_convenio - fecha actual < 90 | Entidades | Mensual |
| Nuevas entidades vinculadas por semestre | Entidades con primer proyecto en el semestre | Entidades, Proyecto_Socios | Semestral |
| Retención de entidades | Entidades con proyecto activo en ≥ 2 semestres consecutivos | Proyecto_Socios | Semestral |
| Duración promedio de convenio | Promedio de duración_convenio | Entidades | Anual |

## 6. Cumplimiento de metas e indicadores de proyecto

| Indicador | Cómo se calcula | Fuente | Frecuencia |
|---|---|---|---|
| * % Cumplimiento promedio de metas por proyecto | Promedio de porcentaje de cumplimiento | Seguimiento_Indicador | Según periodicidad del indicador |
| Indicadores en riesgo | % cumplimiento por debajo de lo esperado a mitad del periodo | Seguimiento_Indicador | Según periodicidad |
| % Proyectos con seguimiento registrado | Proyectos con ≥1 registro en Seguimiento_Indicador / total proyectos | Proyectos, Seguimiento_Indicador | Semestral |
| Reporte oportuno de avance | % de registros de Seguimiento_Indicador entregados dentro del periodo esperado (vs. atrasados) | Seguimiento_Indicador | Según periodicidad |
| Indicadores huérfanos | codigo_indicador usado en Metas/Seguimiento que no existe en Bateria_Indicadores (catálogo maestro) | Metas_Indicador, Bateria_Indicadores | Continuo (calidad de datos) |

## 7. Impacto ODS (Objetivos de Desarrollo Sostenible)

La hoja `TablasODS` del Excel ya trae desarrollados con sub-indicadores solo **6 ODS**: 1 (Fin de la pobreza), 4 (Educación de calidad), 7 (Energía asequible y no contaminante), 10 (Reducción de las desigualdades), 13 (Acción por el clima) y 16 (Paz, justicia e instituciones sólidas). Se propone que sean **estos 6 los ODS prioritarios** a reportar (validar en pregunta 9), sin dejar de mostrar los demás si aparecen como "ODS Principal" de algún proyecto.

| Indicador | Cómo se calcula | Fuente | Frecuencia |
|---|---|---|---|
| * Proyectos por ODS principal | Conteo de Proyectos por ODS Principal, con ícono de TablasODS | Proyectos, TablasODS | Semestral |
| Concentración/diversificación ODS | % de proyectos por cada uno de los 6 ODS prioritarios (evita que el portafolio se concentre en uno solo) | Proyectos, TablasODS | Semestral |
| ODS por línea de trabajo | Cruce de tematica/tipo_actividades con ODS Principal, para ver qué tipo de proyecto aporta a cuál ODS | Proyectos, TablasODS | Semestral |

**Cómo abordarlo:** cada proyecto ya tiene (o debería tener) un ODS Principal asignado manualmente al diligenciar el Excel; el tablero solo agrega y visualiza. Si faltan proyectos sin ODS asignado, ese hueco se reporta como indicador de calidad de datos (sección 9).

## 8. Documentales — ARL y vinculación (dependen de campos nuevos, ver Épica 9 del documento de Historias de Usuario)

| Indicador | Cómo se calcula | Fuente | Frecuencia |
|---|---|---|---|
| % Estudiantes con ARL vigente antes de iniciar práctica | arl_afiliada = Sí y fecha_afiliacion_arl ≤ fecha_inicio_practica | Actores (campo nuevo) | Continuo |
| Estudiantes próximos a iniciar sin ARL | fecha_inicio_practica próxima y arl_afiliada = No | Actores (campo nuevo) | Continuo |
| % Participantes no estudiantes con firma de vinculación | estado_firma_vinculacion = Sí / total de participantes no estudiantes del proyecto | Entidades/Interlocutores (campo nuevo) | Continuo |
| Documentación pendiente por proyecto | Cruce de ARL de estudiantes + firma de vinculación de no-estudiantes por código_proyecto | Actores, Entidades (campos nuevos) | Continuo |

## 9. Calidad de datos (soporte, no se muestra al decano)

| Indicador | Cómo se calcula | Fuente | Frecuencia |
|---|---|---|---|
| % Completitud de campos críticos | % no vacío en estado, fecha_inicio/fin, latitud/longitud, correo, ODS Principal | Todas | Continuo |
| Registros con valores placeholder | Conteo de "NULOS" u otros valores sospechosos en titulo_proyecto, carrera_programa | Actores, Proyectos | Continuo |

## 10. Siguiente paso

Este catálogo es un punto de partida amplio a propósito ("todos los indicadores posibles"). Antes de construirlos todos en Power BI, se recomienda que el profesor/la oficina marque cuáles quedan para la primera versión del tablero y cuáles se dejan para después — ver pregunta 6 del documento de preguntas.
