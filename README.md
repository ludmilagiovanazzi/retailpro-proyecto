# RetailPro — Análisis de ventas de tecnología

Proyecto de análisis de datos realizado en el marco de un curso. Trabaja con una empresa ficticia, **RetailPro**, que vende productos de tecnología, y recorre el flujo completo de un análisis: desde el diseño y la carga de una base de datos relacional hasta la construcción de un modelo en Power BI.

## Descripción del proyecto

El objetivo es transformar los datos transaccionales de ventas en información útil para el equipo comercial. Para eso, el proyecto:

- Modela y carga los datos de ventas en una base relacional en **PostgreSQL** (`Ventas_Tech_DB`).
- Responde preguntas de negocio con **SQL**: facturación, cantidad de pedidos, ticket promedio y ranking de productos, entre otras.
- Combina las tablas con **JOINs** para obtener una vista integrada de clientes, productos, categorías y ventas.
- Limpia y transforma los datos con **Power Query** y construye un **modelo en estrella** en **Power BI Desktop**.
- Calcula indicadores con **medidas DAX** para el análisis y la visualización.

## Herramientas

| Herramienta | Uso en el proyecto |
|---|---|
| **PostgreSQL** | Motor de base de datos relacional |
| **pgAdmin 4** | Administración de la base y ejecución de scripts SQL |
| **Power BI Desktop** | Modelado de datos y visualización |
| **Power Query** | Limpieza y transformación de datos (ETL) |
| **DAX** | Medidas e indicadores del modelo |

## Fuentes de datos

El proyecto usa dos fuentes distintas:

- **Base SQL `Ventas_Tech_DB`** (módulos 3 a 5): datos de ejemplo cargados por script, con 10 ventas de marzo de 2024.
- **Archivo `Pipeline_ETL_Dataset.xlsx`** (módulos 6 y 8): dataset provisto por el curso, con 50 ventas de 2023 y 2024 y problemas de calidad intencionales (duplicados y nulos) para practicar la limpieza en Power Query.

## Modelo de datos

La base `Ventas_Tech_DB` tiene cuatro tablas:

| Tabla | Contenido |
|---|---|
| `ventas` | Tabla de hechos: cada venta con fecha, cliente, producto, cantidad y precio unitario |
| `clientes` | Datos de los clientes (nombre, email, ciudad y fecha de registro) |
| `productos` | Catálogo de productos con su categoría, precio, stock y estado |
| `categorias` | Categorías de productos |

Relaciones principales:

- `ventas.id_cliente` → `clientes.id_cliente`
- `ventas.id_producto` → `productos.id_producto`
- `productos.id_categoria` → `categorias.id_categoria`

En Power BI, `Fact_Ventas` funciona como tabla de hechos y `Dim_Clientes`, `Dim_Productos`, `Dim_Categorias` y `Dim_Fechas` como dimensiones de un modelo en estrella.

## Estructura del proyecto

El proyecto está organizado en un repositorio por módulo:

| Módulo | Repositorio | Contenido |
|---|---|---|
| M3 | [ventas-tech-db](https://github.com/ludmilagiovanazzi/ventas-tech-db) | Script que crea las tablas y carga los datos |
| M4 | [m4_consultas_negocio.sql](https://github.com/ludmilagiovanazzi/m4_consultas_negocio.sql) | Consultas de negocio |
| M5 | [m5_consultas_joins.sql](https://github.com/ludmilagiovanazzi/m5_consultas_joins.sql) | Consultas con JOIN |
| M6 | [pipeline-etl-m6](https://github.com/ludmilagiovanazzi/pipeline-etl-m6) | Pipeline ETL en Power Query (`Pipeline_ETL_Giovanazzi_Ludmila.pbix`) |
| M8 | [checkpoint2-m8](https://github.com/ludmilagiovanazzi/checkpoint2-m8) | Modelo en estrella y medidas DAX (`Giovanazzi_Ludmila_Checkpoint2.pbix`) |

## Cómo ejecutar los scripts SQL

### Requisitos

- PostgreSQL instalado.
- pgAdmin 4 (o la consola `psql`).

### Con pgAdmin 4

1. **Crear la base de datos.** En el panel izquierdo, hacé clic derecho en *Databases* → *Create* → *Database…* y poné como nombre `Ventas_Tech_DB`.
2. **Abrir el Query Tool.** Seleccioná la base `Ventas_Tech_DB` y abrí *Tools* → *Query Tool*. Verificá que arriba de la pestaña diga `Ventas_Tech_DB` y no `postgres`.
3. **Crear las tablas y cargar los datos.** Abrí `ventas_tech_db.sql` y ejecutalo completo con **F5**.
4. **Ejecutar las consultas de análisis.** Abrí `m4_consultas_negocio.sql` y después `m5_consultas_joins.sql`. Corré cada consulta por separado: seleccioná el bloque y presioná **F5**.

> **Importante:** ejecutá `ventas_tech_db.sql` siempre primero, porque las consultas de los módulos 4 y 5 dependen de las tablas que crea ese script. Ese script empieza con `DROP TABLE`, así que volver a ejecutarlo borra y recrea las tablas.

## Cómo abrir los archivos de Power BI

Los archivos `.pbix` no se conectan a PostgreSQL: su origen es el Excel `Pipeline_ETL_Dataset.xlsx`.

1. Descargá `Pipeline_ETL_Dataset.xlsx` y guardalo en una carpeta fija.
2. Abrí el `.pbix` con **Power BI Desktop**.
3. Si aparece un error de origen de datos, actualizá la ruta del Excel en *Inicio* → *Transformar datos* → *Configuración de origen de datos* → *Cambiar origen*.
4. Hacé clic en **Actualizar** para recargar los datos.

- `Pipeline_ETL_Giovanazzi_Ludmila.pbix`: pasos de limpieza y transformación en Power Query (duplicados, nulos, tipos de datos y Merge).
- `Giovanazzi_Ludmila_Checkpoint2.pbix`: modelo en estrella con relaciones 1:N, tabla calendario `Dim_Fechas`, tabla `_Medidas` con 5 medidas DAX y una página de validación. El dashboard final se construye en los módulos siguientes.

## Autora

**Ludmila Giovanazzi** — Analista de datos
