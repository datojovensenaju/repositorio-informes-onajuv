# Dato Joven — compendio de proyectos Power BI

Colección de tableros de **Dato Joven** (sitio SharePoint `DATAJOVEN` de MINEDU) sobre la situación de la juventud en el Perú. El repositorio guarda los proyectos en formato **PBIP** (Power BI Project): el reporte y el modelo semántico viven como carpetas de texto (JSON del reporte y TMDL del modelo), listos para abrirse en Power BI Desktop y versionarse en Git.

Hay **57 proyectos**, **124 páginas**, cerca de **3 900 objetos visuales** y **187 medidas DAX**. Cada indicador es un proyecto independiente. No hay un modelo semántico único para todo el observatorio, salvo el tablero consolidado `Indicadores Regionales`.

## Cómo está organizado

```
dato joven - power bi/
├── Datos Demográficos/                  población general y joven
├── DIPROGE/                             productos de gestión
│   ├── RENOJ/                           organizaciones juveniles
│   └── Voluntariado Juvenil/
├── Politica Nacional de la Juventud/    objetivos, indicadores y servicios
└── Indicadores de la Juventud/
    ├── Educacion/                       5 proyectos
    ├── Empleo/                          8 proyectos
    ├── Salud/                           12 proyectos
    ├── Victimizacion y Seguridad Ciudadana/   7 proyectos
    ├── Violencia y Discriminacion/      5 proyectos
    ├── Participacion Ciudadana/         9 proyectos
    ├── Habilidades Digitales/           5 proyectos
    ├── Juventud Migrante/               1 proyecto
    └── Indicadores Regionales/          consolidado de encuestas
```

La carpeta del repositorio sigue los temas del observatorio. La biblioteca de Excel en SharePoint usa una numeración paralela (`1. Educación` … `7. Competencias digitales`). Habilidades digitales en el repo corresponde a la carpeta SharePoint `7. Competencias digitales`.

## Anatomía de un proyecto

Casi todos los proyectos se llaman `project` y tienen esta forma:

```
<Indicador>/
├── project.pbip
├── project.Report/
│   ├── definition.pbir          apunta a ../project.SemanticModel
│   ├── definition/
│   │   ├── report.json
│   │   ├── pages/<id>/page.json y visuals/
│   │   └── bookmarks/
│   └── StaticResources/         tema, imágenes, mapas
└── project.SemanticModel/
    ├── definition.pbism
    └── definition/
        ├── database.tmdl        compatibilityLevel 1606
        ├── model.tmdl           cultura es-MX, consultas en es-PE
        ├── relationships.tmdl
        ├── cultures/es-MX.tmdl
        └── tables/*.tmdl        tablas, columnas, medidas y particiones M
```

Dos excepciones de nombre de archivo:


| Carpeta                                         | Archivo que abre Power BI Desktop                        |
| ----------------------------------------------- | -------------------------------------------------------- |
| `Salud/Salud cronico`                           | `Salud Crónico.pbip`                                     |
| `Educacion/Conclusion de la educacion superior` | `roject.pbip` (falta la `p` inicial en reporte y modelo) |




## Dos familias de modelos



### Encuestas

La mayoría de indicadores de educación, empleo, salud, violencia, victimización, participación y habilidades digitales repiten el mismo molde:

- `datosNacionales` y `datosRegionales`, leídos de hojas con esos nombres dentro de un Excel.
- Columnas habituales: `AÑO`, `POBLACION`, `REGION`, `CATEGORIA`, `INDICADOR`, `VALOR`, `CV`, `FUENTE`.
- Dimensión `Año`, y a veces `Sexo`, `Region Natural` o una tabla de categorías del indicador (`instituciones`, `principalProblema`, `actividadRealizada`, etc.).
- Medidas simples del estilo `MAX` del valor nacional o regional, filtradas por categoría.

El corte geográfico de estas encuestas es nacional y regional. El detalle distrital vive en los registros y en datos demográficos.

### Registros administrativos

Los productos que vienen de padrones o sistemas de registro usan un modelo en estrella: tabla de hechos, dimensiones de ubigeo y dimensiones demográficas, más medidas de conteo, porcentaje y tarjetas.


| Proyecto                             | Hechos principales                                                   | Qué muestra                                                                       |
| ------------------------------------ | -------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Datos Demográficos                   | `factPoblacionGeneral`, `factPoblacionJoven`                         | Población joven, foco distrital, brecha por sexo                                  |
| RENOJ                                | `Representantes`, `Miembros`                                         | Organizaciones acreditadas y miembros de 15 a 29 años                             |
| Voluntariado Juvenil                 | `fact_Voluntarios`, `fact_movilizacion`                              | Voluntarios por origen, sexo, ocupación y movilización                            |
| Política Nacional de la Juventud     | indicadores, lineamientos, servicios, proveedores                    | Ficha de objetivo, valor esperado y obtenido, responsable                         |
| CEM                                  | `CEM`                                                                | Casos de los Centros de Emergencia Mujer                                          |
| MININTER                             | delitos, faltas, violencia familiar y sexual, accidentes de tránsito | Registros de victimización                                                        |
| INPE                                 | `poblacion`                                                          | Población penitenciaria joven                                                     |
| PRONACEJ                             | `pronacej-CJDR`, `pronacej-SOA`, `pronacej-PASPE`                    | Diagnóstico y rehabilitación, orientación al adolescente, seguimiento post egreso |
| CNV, CONADIS, Discapacidad, IPD, VIH | un hecho por registro                                                | Nacidos vivos, discapacidad, deporte, VIH                                         |
| Joven votante                        | padrón nacional y extranjero, jóvenes y total                        | Electores jóvenes                                                                 |
| Diputado joven                       | candidatura general y joven                                          | Candidaturas jóvenes                                                              |
| Trabajadores del sector cultural     | trabajadores culturales, cultura indígena, asistencia a lectura      | Empleo cultural                                                                   |
| Movimiento migratorio                | `fact_movimiento_migratorio`                                         | Movimientos migratorios de jóvenes                                                |


`Indicadores Regionales` junta en un solo modelo las tablas de hecho de las encuestas (educación, trabajo, salud, victimización, violencia y participación) más población general y joven, con ubigeo hasta distrito. Sirve como vista regional cruzada, no reemplaza a los tableros de un solo indicador.

## Páginas, tema y navegación

Cada tablero tiene, como mínimo, una página **web** y una **móvil**.

- Web: **1400 × 1980** px en la gran mayoría.
- Móvil: ancho fijo de **370** px y alto variable según el contenido.

El tema base es `CY26SU02` (un proyecto conserva `CY25SU12`). Los objetos que más se repiten son imágenes, cuadros de texto, tablas, segmentadores, tarjetas, gráficos de anillo, líneas y barras, y mapas de departamentos del Perú (`shapeMap`, con un topojson de departamentos). Algunos registros usan Azure Maps. Hay **76 marcadores**, casi siempre para limpiar filtros entre la vista web y la móvil.

Las dimensiones compartidas salen de dos libros de la carpeta `COMPLEMENTO` de SharePoint:

- `Ubigeo.xlsx`: región, departamento, provincia y distrito.
- `Estandarización.xlsx`: año, mes, sexo, rango etario y región natural.



## Fuentes de datos

Todo se carga en modo **import**. No hay conexiones DirectQuery ni roles de seguridad a nivel de fila. El origen es Excel en el sitio:

`https://mineduperu.sharepoint.com/sites/DATAJOVEN/Documentos compartidos/General/POWER BI - DATO JOVEN/`

```
POWER BI - DATO JOVEN/
├── COMPLEMENTO/                         Ubigeo.xlsx, Estandarización.xlsx
├── DATOS DEMOGRÁFICOS/
├── DIPROGE/                             RENOJ-SETIEMBRE.xlsx, voluntariadoJuvenil.xlsx
├── POLÍTICA NACIONAL DE JUVENTUD/
└── INDICADORES SOBRE JUVENTUD/
    ├── 1. Educación/
    ├── 2. Empleo/                       ENCUESTAS/ y REGISTROS/
    ├── 3. Salud/                        ENCUESTAS/ y REGISTROS/
    ├── 4. Victimización y seguridad ciudadana/
    ├── 5. Violencia y discriminación/
    ├── 6. Participación ciudadana y gobernanza/
    ├── 7. Competencias digitales/
    └── Juventud Migrante/               carpeta leída con SharePoint.Files
```

La partición M típica es `Excel.Workbook(Web.Contents("<url del xlsx>"))`. Movimiento migratorio es la excepción: combina los archivos de una carpeta de SharePoint con `SharePoint.Files`.

Para refrescar hace falta una cuenta con acceso a ese sitio. Las credenciales no están en el repositorio.

## Catálogo



### Datos demográficos y gestión


| Proyecto                           | Páginas                                                         |
| ---------------------------------- | --------------------------------------------------------------- |
| `Datos Demográficos`               | Web y móvil                                                     |
| `DIPROGE/RENOJ`                    | Resumen y detalle, cada uno en web y móvil                      |
| `DIPROGE/Voluntariado Juvenil`     | Vista general web y móvil                                       |
| `Politica Nacional de la Juventud` | Indicador por objetivo, y servicio por lineamiento; web y móvil |




### Indicadores de la juventud


| Tema                                | Proyectos                                                                                                                                                                                                                                                                                          |
| ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Educación                           | Años promedio de estudio, asistencia a educación superior, conclusión de secundaria, conclusión de superior, nivel educativo alcanzado                                                                                                                                                             |
| Empleo                              | Actividades que realizan los jóvenes, empleo formal, empleo informal, ingreso promedio mensual, NINI, PEA, tasa de desempleo, trabajadores jóvenes del sector cultural                                                                                                                             |
| Salud                               | Algún seguro de salud, CNV, CONADIS, consumo de alcohol, consumo de cigarrillos, discapacidad, embarazo adolescente, episodio depresivo, IPD, salud crónica, SIS, VIH                                                                                                                              |
| Victimización y seguridad ciudadana | Delincuencia, INPE, MININTER, percepción de inseguridad, percepción de inseguridad en la noche, PRONACEJ, víctimas de hechos delictivos                                                                                                                                                            |
| Violencia y discriminación          | CEM, percepción de discriminación, violencia familiar, violencia física y/o sexual, violencia psicológica                                                                                                                                                                                          |
| Participación ciudadana             | Características más importantes de la democracia, confianza en las instituciones, diputado joven, joven votante, participación juvenil, percepción del funcionamiento de la democracia, percepción de la democracia, percepción de la importancia de la democracia, principales problemas del país |
| Habilidades digitales               | Actividades informáticas, dispositivos de acceso, frecuencia de uso, jóvenes usuarios de internet, lugar de acceso                                                                                                                                                                                 |
| Juventud migrante                   | Movimiento migratorio                                                                                                                                                                                                                                                                              |
| Vista cruzada                       | Indicadores regionales                                                                                                                                                                                                                                                                             |


MININTER publica tres páginas web (delitos, accidentes y violencia) además de la móvil. PRONACEJ publica tres páginas web (CJDR, PASPE y SOA). Joven votante separa padrón nacional y extranjero, cada uno en web y móvil.

## Cómo abrirlo

1. Instalar Power BI Desktop con soporte de proyectos PBIP (formato TMDL).
2. Abrir el archivo `.pbip` del indicador, no la carpeta del modelo por separado.
3. Iniciar sesión con la cuenta organizacional que tiene acceso al sitio `DATAJOVEN`.
4. Actualizar el modelo. La primera actualización descarga los Excel de SharePoint.

Archivos que no se versionan (`.gitignore`):

- `**/.pbi/localSettings.json`
- `**/.pbi/cache.abf`

Esos archivos son de la máquina local. Al clonar el repo, Power BI Desktop los vuelve a crear.

## Estado conocido del repositorio

Varios reportes se copiaron desde una plantilla y conservan el nombre de página del indicador de origen. El contenido hay que leerlo por la carpeta del proyecto, no solo por `displayName` de `page.json`. Casos visibles:

- Percepción de inseguridad, percepción de inseguridad en la noche, víctimas de hechos delictivos y delincuencia muestran el título «Años promedio de estudio».
- Percepción de discriminación muestra el título «Violencia física y/o sexual».
- La página móvil de PRONACEJ, MININTER e INPE se llama «Movimiento Migratorio». La página web de INPE se llama «CJDR».
- IPD, Discapacidad y CONADIS conservan una página llamada «RENOJ-Resumen-Web». Salud crónica conserva el título «Episodio depresivo».

Otros puntos para quien mantenga los modelos:

- `Participacion Ciudadana/Caracteristicas mas importantes asociadas a la democracia` tiene páginas, pero su modelo semántico no incluye la carpeta `definition` con tablas TMDL.
- En varios modelos de encuesta conviven las tablas `datosRegionales` y `Datos Regionales`. La segunda suele ser un catálogo de `Estandarización.xlsx`, no el hecho regional.
- La carpeta `Violencia psciologica` y el archivo `roject.pbip` conservan el nombre con el que se guardó el proyecto.
- Algunas URLs de `Web.Contents` están codificadas y otras van con espacios y tildes literales. Ambas formas aparecen en el mismo tema.
- No hay medidas de tiempo, roles RLS ni grupos de cálculo. La inteligencia de tiempo de Power BI está activada en el modelo, pero el análisis temporal se hace con la dimensión `Año` (y `Mes` en los registros).

