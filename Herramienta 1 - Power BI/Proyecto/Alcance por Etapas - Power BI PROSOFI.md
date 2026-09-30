# Alcance — Power BI V2 (etapa única)

*Este documento cubre solo la Herramienta 1 — Power BI. La App de Gestión Administrativa (Herramienta 2) tiene su propia carpeta ("Herramienta 2 - App Gestion Administrativa (Javeriana Impacta)"), con un documento aparte de solo diseño y estructura ("Diseño y Estructura - App Gestion Administrativa.md") — no se mezcla su alcance ni su planeación aquí.*

*El Power BI **nunca** se conecta directamente a una base de datos institucional en vivo — eso es exclusivo de la App. Por eso Power BI V2 **no tiene dos etapas**: es una sola etapa, siempre alimentada por el Excel (vía Forms + Power Automate), cuyo último hito es publicarla en la página web de PROSOFI.*

---

## Objetivo

Unificar los dos reportes .pbix existentes en un solo modelo, construir el catálogo de indicadores (ver documento "Batería de Indicadores Propuesta - PROSOFI.md"), dejar un tablero funcional para visualización y consulta, y publicarlo en la página web de PROSOFI como último hito.

## Fases dentro de esta única etapa

1. **Modelo de datos unificado** (HU-00 a HU-03) a partir de **CONSOLIDADO HISTORICO FACULTAD V1.xlsx** (Actores, Proyectos, Entidades, Proyecto_Socios, Metas_Indicador, Bateria_Indicadores, Seguimiento_Indicador, TablasODS).
2. **Páginas y funcionalidades** (HU-04 a HU-30): inicio, estudiantes, proyectos (mapa categorizable por tipo de proyecto y otros atributos — la ubicación real está pendiente de diagnóstico, ver más abajo), entidades, indicadores/ODS, consulta individual, calidad de datos, reportes ejecutivos, ARL/firma de vinculación (si se agregan esos campos al Excel), e histórico multianual visible en todas las secciones.
3. **Flujo de recolección de datos con Forms + Power Automate (HU-31):** un formulario de captura (Microsoft Forms) conectado a un flujo de **Power Automate** que escribe automáticamente cada respuesta como fila nueva en el Excel CONSOLIDADO HISTORICO (o en una lista de SharePoint espejo del mismo). El Power BI se conecta a ese mismo Excel/lista y se refresca con el flujo ya corrido. Esto resuelve cómo alimentar el modelo fácilmente y cómo manejar las altas de datos sin que eso choque con que Power BI es de solo lectura — Forms + Power Automate son la "puerta de entrada" de datos, Power BI solo los visualiza.
4. **Publicación interna** en Power BI Service (workspace de PROSOFI) con seguridad a nivel de fila (RLS) para todos los roles confirmados: oficina PROSOFI, decano, docentes, coordinadores de prácticas profesionales, facultades externas, voluntarios/estudiantes e interlocutores/entidades — cada uno con una versión del tablero acorde a su nivel (audiencia: toda la comunidad javeriana interna activa).
5. **Publicación en la página web de PROSOFI (último hito de esta etapa):** confirmado que la página será **pública, sin login**, así que se usa **"Publish to Web"**. Se coordina con la oficina de Comunicaciones (contacto: Camilo, en su momento). El tablero sigue alimentado por el mismo Excel/lista de SharePoint — **no implica conectar ninguna base de datos institucional en vivo**, es solo publicar/hospedar la visualización.

## Coordenadas de proyectos — diagnóstico (sin solución comprometida)

Las coordenadas actuales de Proyectos no sirven para ubicar en el mapa, por dos razones verificadas en el Excel:

1. El campo longitud trae exactamente el mismo número que el campo latitud en varios proyectos (ej. 4.316107698 en ambos) — geográficamente imposible, es un valor de relleno, no una coordenada real.
2. La tabla Proyectos no tiene un campo de dirección exacta, solo departamento y municipio (dato muy general para ubicar un punto). La dirección exacta solo existe en la tabla Entidades, y no siempre está diligenciada.

Esto no es solo un problema de automatización — depende de que exista un dato real de ubicación por proyecto, que hoy no se está capturando. Se dejó como pregunta abierta al director (ver correo) antes de comprometer una forma técnica de resolverlo.

## Fuera de alcance (permanentemente, no es un paso futuro de esta herramienta)

- Conexión en vivo a bases de datos institucionales de la universidad (estudiantes, EPS/ARL, directorio Azure AD) — eso no ocurre nunca en Power BI.
- Autenticación institucional real (Azure AD/SSO) contra el directorio de la universidad para el login del tablero — la RLS se implementa a nivel de Power BI Service, no contra el directorio institucional.
- Cualquier CRUD completo (editar/eliminar registros existentes con pantallas, validaciones cruzadas, etc.) — se resuelve con Forms + Power Automate (HU-31) para las altas simples que necesita Power BI; un CRUD completo queda fuera de esta herramienta.

## Dependencias

- Licencia de Power BI Pro/Premium para dar acceso directo con RLS a todos los roles — confirmado que hay que gestionarla con la DTI (ver correo al director).
- Validación del catálogo de indicadores y ODS prioritarios (documento "Batería de Indicadores Propuesta") — confirmado que se comparte con todos los interesados (oficina, decano, etc.) para revisión colectiva antes de fijarlo.
- Si se quiere HU-27/HU-28/HU-29/HU-30 (ARL y firma de vinculación), primero hay que agregar esos campos al Excel fuente — no existen hoy.
- Política de habeas data (Ley 1581) para los datos sensibles que se muestran en el tablero — pendiente de respuesta del director/DTI.
- Para el hito final (publicación web): coordinación con la oficina de Comunicaciones ya en marcha (contacto: Camilo).

## Riesgo

Bajo en general (modelo, indicadores, captura de datos no dependen de la DTI). El único punto que puede necesitar aval externo es el hito final de publicación en el dominio web oficial de la universidad — pero es una aprobación de hosting/seguridad liviana, nada comparable a pedir acceso a bases de datos institucionales sensibles.

## Nota: por qué Power Automate + Forms y no Power Apps para alimentar el Excel

| | Power Automate + Microsoft Forms | Power Apps |
|---|---|---|
| Qué es | Un formulario simple + un flujo que escribe la respuesta como fila nueva en el Excel/lista. | Una app con pantallas, para buscar, editar y borrar registros existentes, no solo crear nuevos. |
| Facilidad/tiempo de armar | Más fácil y rápido — se arma en horas, sin curva de aprendizaje. | Más lento de construir — requiere aprender fórmulas de Power Apps y diseñar pantallas. |
| Sirve para | Altas nuevas (un estudiante nuevo, un proyecto nuevo), marcar Sí/No (ARL afiliada, firma de vinculación) — que es todo lo que necesita el Power BI. | Gestión completa con edición/borrado y validaciones cruzadas — fuera del alcance de esta herramienta. |
| Licenciamiento | Suele venir incluido en el M365 educativo de la universidad, sin trámite adicional con la DTI. | Requiere plan de Power Apps aparte. |
| No sirve bien para | Editar o borrar un registro que ya existe. | Una captura rápida y puntual — sobredimensionado para solo un formulario de alta. |

**Recomendación aplicada:** para lo que necesita Power BI V2 — alimentar el Excel con altas y marcas simples (ARL, firma de vinculación) — **Power Automate + Forms es lo más fácil y es lo que se usa (HU-31)**.
