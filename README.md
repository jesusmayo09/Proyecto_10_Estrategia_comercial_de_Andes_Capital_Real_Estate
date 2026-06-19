# 🏢 Estrategia Comercial — Andes Capital Real Estate

Proyecto de análisis de datos y visualización construido en **Power BI**, que evalúa el desempeño comercial del sector inmobiliario: ventas, rentabilidad, comportamiento de clientes y recurrencia de compra.

---

## 🧾 Descripción general

El dashboard fue diseñado para apoyar decisiones estratégicas del negocio, respondiendo preguntas en cuatro frentes:

- **Desempeño general:** ingreso total, propiedades vendidas, precio promedio, comisión total.
- **Análisis comercial:** qué tipo de propiedad, segmento de cliente y canal de venta generan más ingresos.
- **Análisis temporal:** evolución de ventas, crecimiento año contra año (YoY) y desempeño acumulado.
- **Cohortes de clientes:** si los clientes vuelven a comprar después de su primera compra, y qué cohortes son más rentables en el tiempo.

---

## 🗂️ Modelo de datos

El proyecto sigue un **esquema estrella**, con la siguiente estructura:

| Tabla | Tipo | Descripción |
|---|---|---|
| `hecho_ventas_propiedades` | Hechos | Transacciones de venta: precio, cliente, propiedad, canal, fecha. |
| `dim_clientes` | Dimensión | Información y segmentación de clientes. |
| `dim_propiedades` | Dimensión | Características de las propiedades (tipo, tamaño, ciudad, etc.). |
| `Dim_Fecha` | Dimensión (calendario) | Tabla de fechas creada con `CALENDAR` y `ADDCOLUMNS`, con jerarquía Año → Mes. |

**Relaciones:** todas configuradas como 1 a muchos (1:\*), con dirección de filtro simple (single) y activas — `hecho_ventas_propiedades` como tabla central conectada a las tres dimensiones.

**Preparación de datos:**
- `fecha_venta` convertida a formato **Date**.
- `porcentaje_comision` formateado como **porcentaje**.
- Validación de nulos y de duplicados en las claves primarias de `dim_clientes` y `dim_propiedades`.

---

## 🧮 Medidas y columnas calculadas (DAX)

### Medidas base
- `Ingreso Total` — suma de `precio_venta`.
- `Cantidad Venta` — conteo de ventas.
- `Ticket Promedio` — ingreso total / cantidad de ventas.
- `Comision Total` — suma de comisiones generadas.

### Medidas con contexto de filtro (participación %)
Calculadas usando `CALCULATE` para remover el filtro de la categoría y obtener el total general como denominador:
- `Ingresos Tipo Propiedad` — participación de ingresos por tipo de propiedad.
- `Ingresos Canal Venta` — participación de ingresos por canal de venta.
- `Ingresos Segmento Cliente` — participación de ingresos por segmento de cliente.

> Estas medidas se usan como **tooltips** en los gráficos de barras de la página "Análisis Comercial".

### Inteligencia de tiempo
- `Ventas Ano24` y `Ventas Ano Anterior` — comparación de ventas año contra año.
- `Crecimiento YoY %` — crecimiento porcentual interanual.

### Columnas calculadas para cohortes
Creadas en la tabla `hecho_ventas_propiedades`:
- **Primera compra por cliente** — fecha mínima de compra de cada cliente.
- `Cohorte` — mes de adquisición (primera compra) del cliente.
- `Mes Venta` — mes en que ocurre cada transacción, usado para identificar recompra.
- `Retencion %` — porcentaje de clientes que vuelven a comprar después de su mes de cohorte.

---

## 🖥️ Estructura del reporte (4 páginas)

### 1️⃣ Overview
KPIs principales en tarjetas:
- Ingreso Total
- Cantidad Venta
- Ticket Promedio
- Comisión Total
- Crecimiento YoY %

Visuales:
- **Gráfico de líneas** — "Ingresos Durante el Periodo 23-24" (tendencia temporal por año/mes).
- **Gráfico de columnas** — "Ingresos por Ciudad".
- **Gráfico de líneas** — "Comparación Ingresos Periodo 23-24" (Ventas Año 24 vs. Ventas Año Anterior).
- **Slicer** de Año para filtrar toda la página.

### 2️⃣ Análisis Comercial
- **Gráfico de columnas** — "Ingresos por Tipo de Propiedad" (con tooltip de % de participación).
- **Gráfico de columnas** — "Ingresos por Canal de Venta" (con tooltip de % de participación).
- **Gráfico de columnas** — "Ingresos por Segmento de Clientes" (con tooltip de % de participación).
- **Tabla** con formato condicional (semáforo): tipo de propiedad, Ingreso Total, Cantidad Venta y Ticket Promedio.

### 3️⃣ Análisis de Cohorte
- **Matriz/pivot table** — "Retención % de Clientes":
  - Filas → `Cohorte` (mes de primera compra)
  - Columnas → `Mes Venta`
  - Valores → `Retencion %`

### 4️⃣ Cantidad Ventas
- **Gráfico de líneas** — evolución de la cantidad de ventas (conteo de `id_venta`) por año y mes.

---

## 📝 Resumen ejecutivo

**Hallazgos clave**
- El ingreso total del periodo 2023–2024 fue de **$6bn**, generado por **9K** ventas.
- El tipo de propiedad que genera mayor ingreso es **Casa**.
- La ciudad con mayor volumen de ventas es **Ciudad de México**.
- El canal de venta más eficiente en ingresos es **Corredor**.

**Métricas principales**
- Ingreso Total: **$6bn**
- Cantidad de Ventas: **9K**
- Ticket Promedio: **$707.35K**
- Comisión Total: **$201M**

**Insights accionables**
- El segmento **Primera vez** concentra la mayor parte del ingreso (**$3.8bn**).
- Las cohortes con mayor recurrencia de compra son: 2023-01, 2023-02, 2023-03, 2023-08, 2023-09, 2024-03 y 2024-04.
- Las ventas muestran un crecimiento de **11.14% YoY**.

**Recomendaciones estratégicas**
- Priorizar la comercialización de propiedades tipo **Casa**.
- Fortalecer el canal de ventas **Corredor**, que presenta mayor participación en el revenue.
- Implementar estrategias de retención enfocadas en mejorar la recompra en las cohortes más recientes.

---

## 🛠️ Herramientas utilizadas

- **Power BI Desktop** — modelado de datos, medidas DAX y construcción del dashboard.
- **DAX** — `CALCULATE`, `CALENDAR`, `ADDCOLUMNS` para tabla de fechas e inteligencia de tiempo.
- **Jupyter Notebook** — documentación del proceso: requerimientos, modelado y resumen ejecutivo.

---

## 👤 Autor

Jesús — Data Analyst Junior
