# Visualización de Datos: Estilizando tablas con Python

Este notebook demuestra varias técnicas para estilizar DataFrames de pandas y crear tablas más legibles e informativas. Cubre la extracción, preparación de datos, y varias misiones y desafíos enfocados en problemas de negocio específicos, utilizando las capacidades de estilo de pandas para resaltar información clave.

## Conjuntos de Datos Utilizados

Este notebook utiliza dos conjuntos de datos principales:

1.  **`ventas.csv`**: Contiene datos de ventas, cargados desde un Gist de GitHub. La preparación inicial implicó seleccionar columnas relevantes (`fecha_pedido`, `modo_envio`, `nombre_cliente`, `segmento_cliente`, `ciudad`, `provincia`, `region`, `departamento`, `tipo_producto`, `ventas`, `cantidad`, `ganancia`) y convertir la columna `fecha_pedido` a objetos datetime. Se realizaron inspecciones básicas como `.head()`, `.shape` y `.info()` para comprender su estructura y tipos de datos.

2.  **`tienda_libros.csv`**: Contiene datos de costos de venta de libros y películas, también cargados desde un Gist de GitHub. Para este conjunto de datos, las columnas `fecha_pedido` y `fecha_llegada` se convirtieron a objetos datetime. La exploración inicial de datos incluyó `.head()` y `.info()` para inspeccionar los datos.

## Misión 1: Equipo de Ventas

**Problema de Negocio**: El equipo de ventas necesita identificar a los 10 principales clientes por total de ventas para diseñar estrategias de retención de clientes.

**Manipulación de Datos**: Se agrupó el DataFrame `ventas` por `nombre_cliente` y se calculó la suma de `ventas`. Se utilizó el método `nlargest(10)` para obtener los 10 clientes principales. La Serie resultante se convirtió a un DataFrame, las columnas se renombraron a 'Clientes' y 'Ventas', se añadió una columna 'Ranking' y se estableció como índice.

**Estilo Aplicado**: La columna 'Ventas' se formateó para mostrar valores de moneda con dos decimales y separador de miles usando `estilo.format({'Ventas':'{:,.2f}'})`.

## Misión 2: Equipo Comercial

**Problema de Negocio**: El equipo comercial necesita asociar el total de ventas y el total de ganancias por tipo de producto para enfocarse en estrategias que impulsen los ingresos por cada producto vendido.

**Manipulación de Datos**: Se agrupó el DataFrame `ventas` por `tipo_producto` y se calculó la suma de `ventas` y `ganancia`. El índice se renombró a 'Tipo de producto'.

**Estilo Aplicado**: Los valores se formatearon como moneda (USD con dos decimales) usando `estilo_producto.format('USD {:,.2f}')`. Los valores máximos y mínimos en ambas columnas ('ventas' y 'ganancia') se resaltaron con fondos 'lightgreen' y 'pink' respectivamente usando `highlight_max()` y `highlight_min()`. Se aplicó un `background_gradient(cmap='Greens')` a toda la tabla. Finalmente, los encabezados de tabla se estilizaron para ser en negrita, fuente Arial, centrados y con texto en mayúsculas usando un selector CSS personalizado.

## Misión 3: Equipo de Logística

**Problema de Negocio**: El equipo de logística necesita comprender la distribución de pedidos por región en Brasil para enfocar los esfuerzos en material y mano de obra donde sea más necesario.

**Manipulación de Datos**: La columna `region` del DataFrame `ventas` se utilizó para contar las ocurrencias de cada región usando `value_counts()`. Esto se convirtió en un DataFrame, y la columna se renombró a 'Nº Pedidos', y el índice se renombró a 'Región'. Se calculó una columna 'Porcentaje de Pedidos' convirtiendo los 'Nº Pedidos' a un array de NumPy, calculando los porcentajes y añadiéndolos de nuevo al DataFrame.

**Estilo Aplicado**: La columna 'Porcentaje de Pedidos' se formateó para mostrar como porcentaje con dos decimales usando `estilo_region.format({'Porcentaje de Pedidos':'{:.2f} %'})`. Se aplicó una barra de datos a la columna 'Porcentaje de Pedidos' usando `bar(subset='Porcentaje de Pedidos',vmin=0,vmax=100,color='#9cd3bb')`. Se aplicaron estilos CSS personalizados a los encabezados de tabla (`<th>`) y celdas de datos (`<td>`) para establecer propiedades de fuente, alineación de texto y colores de fondo.

## Misión 4: Equipo de Logística

**Problema de Negocio**: El equipo de logística quiere comprender la cantidad estándar de productos solicitados por mes por departamento para optimizar el aprovisionamiento y la asignación de recursos.

**Manipulación de Datos**: Se creó una copia del DataFrame `ventas` y se ordenó por `fecha_pedido`. Se derivó una nueva columna `mes` de `fecha_pedido` para representar 'AAAA - Abr'. Luego se creó una tabla pivote (`ventas_mensuales`), sumando `cantidad` por `departamento` (índice) y `mes` (columnas). El índice se renombró a 'Departamento'.

**Estilo Aplicado**: La tabla pivote se convirtió en un objeto Styler (`estilo_mensual`). Se aplicó `set_sticky(axis='index')` para fijar la columna del índice al desplazarse horizontalmente. Se definieron estilos CSS personalizados para encabezados fijos (`columna_fija_header`), celdas de cuerpo fijas (`columna_fija_body`), encabezados de columna (`columnas`), celdas de tabla generales (`tabla`) y nombres de índice (`indices`), para controlar el posicionamiento fijo, los colores de fondo, las propiedades de fuente y la alineación del texto. Estos estilos se aplicaron usando `set_table_styles()`.

## Misión 5: Informe de Desempeño

**Problema de Negocio**: Proporcionar un informe de rendimiento importante para la empresa, que aclare cómo está funcionando el negocio. Específicamente, comprender la relación entre los tipos de clientes y los métodos de envío de productos basándose en las ventas, destacando qué métodos de envío y tipos de clientes generaron la mayor ganancia.

**Manipulación de Datos**: Se creó una tabla pivote (`df_cliente`) a partir del DataFrame `ventas`, con `segmento_cliente` como índice, `modo_envio` como columnas, y sumando `ventas`. Se añadió una columna 'Total', que representa la suma de ventas en todos los modos de envío para cada segmento de cliente. También se añadió una fila 'Total', que representa la suma de ventas para cada modo de envío y el gran total.

**Estilo Aplicado**: Los valores de la tabla se formatearon para mostrar como moneda con dos decimales usando `compra_cliente.format('{:,.2f}')`. Se aplicaron estilos CSS personalizados a las celdas y encabezados generales de la tabla (`tabla`) para establecer propiedades de fuente, alineación de texto y color de fondo. Los nombres de los índices (`indice`) se estilizaron con fuente cursiva y un color gris. Se utilizaron llamadas adicionales a `set_table_styles()` para añadir un borde superior a la fila 'Total' y a la columna 'Total' para una mejor separación visual. Finalmente, una celda específica (fila 'Total', columna 'Entrega estándar') se resaltó con un fondo gris usando `set_td_classes()` para enfatizar.

## Desafío 1: Países con más productos solicitados

**Objetivo Analítico**: Identificar qué países han solicitado más productos por pedidos para realizar un estudio sobre la distribución y logística de productos.

**Manipulación de Datos**: Se agrupó el DataFrame `tienda` por `pais` y se calculó la suma de `unidades`. Se utilizó el método `nlargest(10)` para obtener los 10 países principales. La Serie resultante se convirtió a un DataFrame, las columnas se renombraron a 'Pais' y 'Unidades Pedidas', se añadió una columna 'Ranking' y se estableció como índice.

**Estilo Aplicado**: Se creó un objeto Styler `s_paises`, pero no se aplicaron explícitamente funciones de estilo específicas (como format, highlight, etc.) a la tabla en el notebook, lo que indica una presentación básica de la lista clasificada.

## Desafío 2: Categorías de productos con más y menos costos

**Objetivo Analítico**: Identificar qué categorías de productos generaron más y menos costos para la tienda para optimizar las estrategias de inventario y precios.

**Manipulación de Datos**: Se agrupó el DataFrame `tienda` por `categoria` y se calculó la suma de `costo_producto`. El índice se renombró a 'Categoría del Producto' y el nombre de la serie a 'Costo del Pedido'.

**Estilo Aplicado**: Los valores se formatearon como moneda (USD con dos decimales) usando `s_costo_producto.format('USD {:,.2f}')`. El costo máximo se resaltó con 'pink' y el costo mínimo con 'lightgreen' usando `highlight_max()` y `highlight_min()`. Los encabezados de tabla (`<th>`) se estilizaron para ser en negrita, fuente Arial y centrados usando un selector CSS personalizado.

## Desafío 3: Cantidad y distribución de pedidos por tipo de descuento

**Objetivo Analítico**: Informar la cantidad y distribución de pedidos por tipo de descuento para ayudar a los departamentos de la empresa a comprender la demanda de productos durante cada promoción.

**Manipulación de Datos**: La columna `tipo_descuento` del DataFrame `tienda` se utilizó para contar las ocurrencias de cada tipo de descuento usando `value_counts()`. Esto se convirtió en un DataFrame, y la columna se renombró a 'Nº Pedidos', y el índice se renombró a 'Tipo de Descuento'. Se calculó una columna 'Distribución de Pedidos' convirtiendo los 'Nº Pedidos' a un array de NumPy, calculando los porcentajes y añadiéndolos de nuevo al DataFrame.

**Estilo Aplicado**: Se definieron estilos CSS personalizados para los encabezados de tabla (`encabezado`) y las celdas de datos (`celdas`) para establecer propiedades de fuente, alineación de texto y colores de fondo. La columna 'Distribución de Pedidos' se formateó para mostrar como porcentaje con dos decimales usando `estilo_demanda.format({'Distribucion de Pedidos':'{:.2f} %'})`. Se aplicó una barra de datos a la columna 'Distribución de Pedidos' usando `.bar(subset='Distribucion de Pedidos',vmin=0,vmax=100,color='#9cd4bd',height=50,width=50)`.

## Desafío 4: Tiempo medio de entrega de pedidos

**Objetivo Analítico**: Proporcionar una visualización que muestre el tiempo medio de entrega de pedidos durante los meses de 2013 a 2015 por país, para analizar el rendimiento del departamento de transporte de la empresa durante esos años y formular planes de mejora.

**Manipulación de Datos**: Se hizo una copia del DataFrame `tienda`. Luego se filtró para incluir solo pedidos realizados entre '2013-01-01' y '2015-12-31'. Se creó una columna 'Mes' a partir de `fecha_pedido` en formato 'AAAA - Mes abreviado'. El `demora_entrega` (retraso en la entrega) en días se calculó restando `fecha_pedido` de `fecha_llegada`. Se creó una tabla pivote (`entregas`), promediando `demora_entrega` por `pais` (índice) y `Mes` (columnas).

**Estilo Aplicado**: Los valores de la tabla se formatearon para mostrar con dos decimales usando `s_entregas.format('{:,.2f}')`. Se aplicó `set_sticky(axis="index")` para asegurar que la columna del país permanezca visible al desplazarse horizontalmente. Se aplicaron estilos CSS personalizados para fijar la primera columna de encabezado (`thead tr th:nth-child(1)`) y la primera columna del cuerpo (`tbody tr th:nth-child(1)`) con posicionamiento fijo y fondo blanco para una mejor legibilidad.

## Desafío 5: Gastos de envío por tipo de cliente y país

**Objetivo Analítico**: Visualizar los costos de envío de cada producto (costo total) por tipo de cliente, para comprender qué tipo de producto generó los mayores costos de envío para cada tipo de cliente. Además, mostrar la distribución de estos costos por país de envío, permitiendo comprender también los gastos por país.

**Manipulación de Datos**: Se creó una tabla pivote (`tabla_cliente`) a partir del DataFrame `tienda`, con `categoria` como índice, `tipo_cliente` y `pais` como columnas jerárquicas, sumando `costo_envio`. Se añadieron márgenes para las filas y columnas 'Total'. Los nombres de los ejes se ajustaron para mayor claridad. Se definió una función personalizada `resaltar_celdas` para aplicar colores de fondo condicionales a celdas específicas (por ejemplo, la fila 'Total' para 'B2B' en 'Perú', la fila 'Total' para 'B2C' en 'Paraguay', y la categoría 'Libro' en la columna 'Total').

**Estilo Aplicado**: Los valores de la tabla se formatearon para mostrar con dos decimales usando `estilo_cliente.format('{:,.2f}')`. Se aplicaron estilos CSS personalizados para añadir un borde izquierdo a la columna que sigue a todos los países para 'B2B' y 'B2C' (es decir, antes de la columna 'Total' para cada tipo de cliente), un borde izquierdo antes de la columna 'Total' final, para establecer propiedades de fuente, alineación de texto y color de fondo para celdas y encabezados generales, y para estilizar los nombres de los índices con fuente cursiva. Se añadió un borde superior a la fila 'Total' para separación visual. Finalmente, se usó `set_sticky(axis='columns')` (implícitamente, estilizando la primera columna) para asegurar que la columna `categoria` permanezca visible al desplazarse horizontalmente, logrado a través de estilos personalizados `columna_fija_header` y `columna_fija_body`.
