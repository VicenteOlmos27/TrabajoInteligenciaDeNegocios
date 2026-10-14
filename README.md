Kent Foods - Data Warehouse y Proceso ETL (Fase 1)

Descripción del Proyecto
Este proyecto corresponde a la Fase 1 del sistema de Inteligencia de Negocios para la empresa Kent Foods. 
El objetivo principal es la transformación de una base de datos relacional transaccional (OLTP) en un modelo dimensional (OLAP) optimizado para el análisis de ventas, 
rendimiento comercial y gestión logística de envíos.

Estructura del Repositorio
ETL_KentFoods.sln: Solución de Visual Studio que contiene el paquete SSIS (Package.dtsx) con el Control Flow y los Data Flow Tasks configurados.
script_KentFoods_DataWarehouse.sql: Script T-SQL para la creación de la base de datos de destino, sus dimensiones, 
la tabla de hechos, claves primarias, claves foráneas y columnas calculadas.

Objetivos del Negocio e Indicadores Clave (KPIs)
El Data Warehouse fue diseñado para dar soporte analítico a los siguientes indicadores de gestión:
Ventas Totales ($): Evaluación del ingreso monetario global considerando precios unitarios, cantidades y descuentos aplicados.
Productos Vendidos (Unidades): Seguimiento del volumen físico comercializado para el control de inventario y compras.
Ventas por Vendedor: Medición del desempeño comercial individual de los empleados.
Ventas por Producto y Categoría: Identificación de los artículos y líneas de negocio con mayor facturación.
Envíos por Transportista: Control del volumen de despachos asignado a cada empresa de logística.

Modelo Dimensional
Proceso de Negocio: Gestión de Ventas y Envíos de productos alimenticios.
Granularidad: Cada detalle o línea individual de una orden de compra (producto por orden).
<img width="1667" height="508" alt="image" src="https://github.com/user-attachments/assets/182dca7e-856e-43e5-9ace-ee94a4655eea" />

Detalle del Trabajo Técnico Realizado
Durante el desarrollo del proyecto se ejecutaron las siguientes actividades clave:
Análisis e Inspección del Origen:
Se examinó la estructura de la base de datos de origen KentFoods para mapear los campos requeridos por las dimensiones.
Se identificaron discrepancias en los tipos de datos de origen (como campos NCHAR en clientes y consultas DATENAME que devuelven cadenas Unicode).

Diseño e Implementación del Data Warehouse en SSMS:
Se ajustó el script DDL en SQL Server Management Studio para garantizar la coincidencia exacta de tipos de datos entre origen y destino, 
evitando errores de conversión implícita durante la carga.
Se definieron restricciones de integridad referencial (FOREIGN KEY) entre la tabla de hechos y las dimensiones.

Desarrollo del Proceso ETL en SSIS:
Se crearon componentes de lectura Origen de OLE DB conectados a KentFoods y componentes de escritura Destino de OLE DB conectados a KentFoods_DataWarehouse.
Se implementaron consultas T-SQL personalizadas en las fuentes de origen para la extracción limpia de datos:
Empleado: Concatenación de nombre y apellido (NombreCompleto).
Producto: Combinación LEFT JOIN entre la tabla de productos y categorías.
Tiempo: Conversión de fechas a enteros (YYYYMMDD), extracción de año, mes y nombre del mes (DATENAME).
Fact_Ventas: Combinación INNER JOIN entre las tablas de órdenes y detalles de orden.
Orquestación y Resolución de Conflictos:

Se estableció la secuencia de ejecución en el Control Flow, priorizando la carga en paralelo de las 5 dimensiones antes de iniciar la carga de Fact_Ventas.

Se resolvieron errores de compatibilidad de tipos de datos Unicode/no-Unicode (0x80040E21 y DTS_E_OLEDBERROR) 
mediante el alineamiento de esquemas a tipos NVARCHAR y la inclusión de conversiones explícitas (CAST).
