# Análisis de Ventas TechCore — De datos crudos a dashboard

Proyecto de datos end-to-end sobre las ventas de **TechCore**, una cadena de
tiendas de tecnología con sucursales en varias ciudades. Partí de un export plano
y desprolijo de ~30.000 ventas y lo llevé hasta un **dashboard de Power BI** listo
para tomar decisiones, pasando por la limpieza y el diseño de un modelo relacional.

## Problema que resuelve

El sistema exportaba todas las ventas en una sola tabla ancha (cada fila con hasta
tres productos, marcas mal escritas, precios como texto, fechas sin separar). Así
es imposible analizar el negocio. El proyecto convierte ese archivo en información
útil para responder preguntas como: ¿qué sucursales venden más?, ¿qué marcas y
productos lideran?, ¿cómo evolucionan las ventas en el tiempo?

## Etapas

El proyecto está organizado en tres avances, cada uno una fase del flujo de datos:

### Avance 1 — Limpieza y transformación (`Avance1/`)
Partiendo de `ventas.csv` (datos crudos), con **Power BI / Power Query**:
- Corrección de marcas mal escritas (`Aple` → `Apple`, `Acr` → `Acer`, `Del` → `Dell`…).
- Separación de la fecha en **Año / Mes / Día** y normalización de la hora.
- Tipado de precios y montos, limpieza de teléfonos y campos de texto.
- Resultado: `ventasTransformed.csv`, ya consistente.

### Avance 2 — Modelo relacional (`Avance2/`)
Con **Python (pandas)** transformé la tabla plana en un **modelo relacional**
normalizado, separando dimensiones y hechos:

- Dimensiones: `Vendedores`, `Clientes`, `Ciudades`, `Sucursales`, `Productos`.
- Hechos: `Facturas` y `DetalleFacturas` (una fila por producto vendido).

El modelo se exporta a `modeloVentas.xlsx` (una hoja por tabla). En el camino dejo
análisis intermedios como ventas por marca y top de productos.

### Avance 3 — Dashboard (`Avance3/`)
Dashboard interactivo en **Power BI** sobre el modelo relacional, con métricas de
ventas por sucursal, ciudad, marca, producto y evolución temporal.

## Estructura del repositorio

```
ProyectoVentas_NicolasDiaz/
├── Avance1/
│   ├── ventas.csv                          # Datos crudos (~30k ventas)
│   ├── ventasTransformed.csv               # Datos limpios
│   └── Avance_1_Limpieza_Transformacion.pbix
├── Avance2/
│   ├── Avance_2_Modelo_Relacional.ipynb    # Normalización con pandas
│   └── modeloVentas.xlsx                    # Modelo relacional (dimensiones + hechos)
├── Avance3/
│   └── Avance_3_Dashboard_PowerBI.pbix      # Dashboard final
├── Conclusiones_Recomendaciones.pdf         # Hallazgos y recomendaciones de negocio
└── README.pdf                               # Informe del proyecto
```

## Tecnologías

- **Power BI** — Power Query (limpieza) y dashboard final
- **Python**, **pandas**, **openpyxl** — modelado relacional y exportación a Excel
- **Jupyter Notebook**
- **Excel** como formato del modelo de datos

## Cómo usarlo

1. Revisar `Avance1/` para ver el proceso de limpieza (abrir el `.pbix` o comparar
   `ventas.csv` vs `ventasTransformed.csv`).
2. Ejecutar el notebook de `Avance2/` para regenerar `modeloVentas.xlsx` a partir
   de los datos limpios (ajustar la ruta de lectura al inicio del notebook).
3. Abrir el dashboard de `Avance3/` en Power BI Desktop.
4. Leer `Conclusiones_Recomendaciones.pdf` para el resumen de negocio.
