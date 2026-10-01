# PR2_AdminBBDD_Leandro_Kyliam
# Modelo entidad/relación: Viveros Tajinaste S.A.

![Modelo entidad/relación](Practica2ADBD.drawio.png)

# Descripción del modelo

## Entidades

**Vivero.** Cada uno de los viveros de la red de Tajinaste S.A. Se identifica por un código propio y se guarda su georreferenciación.

**Zona.** Cada una de las áreas en las que se divide un vivero (zona exterior, almacén, invernadero...), en las que se almacenan los productos y trabajan los empleados. Tiene su propia georreferenciación. Es una **entidad débil con dependencia en existencia** respecto a Vivero: aunque tiene un identificador propio, una zona no tiene sentido sin el vivero al que pertenece, y si un vivero desaparece, sus zonas también.

**Producto.** Cada uno de los artículos que vende la empresa: plantas, productos de jardinería y artículos de decoración.

**Empleado.** Cada una de las personas que trabajan en la empresa y que son destinadas a las zonas de los viveros.

**Pedido.** Cada uno de los pedidos realizados por los clientes del programa Tajinaste Plus. Se modela como entidad porque tiene identidad propia (un número de pedido) y datos propios, y porque se relaciona de forma independiente con el empleado responsable y con el cliente.

**Cliente_Plus.** Cada uno de los clientes que pertenecen al programa de fidelización Tajinaste Plus. El modelo solo contempla clientes del programa, ya que el enunciado no requiere información de los demás clientes.

## Atributos y dominios

### Vivero

| Atributo | Tipo | Descripción | Dominio | Ejemplos |
|---|---|---|---|---|
| `id_vivero` | Identificador | Código único del vivero | Número entero positivo | 1, 2, 15 |
| `latitud` | Descriptor | Latitud de la ubicación del vivero | Número decimal entre -90 y 90 | 28.4874 |
| `longitud` | Descriptor | Longitud de la ubicación del vivero | Número decimal entre -180 y 180 | -16.3159 |

### Zona

| Atributo | Tipo | Descripción | Dominio | Ejemplos |
|---|---|---|---|---|
| `id_zona` | Identificador | Código único de la zona | Número entero positivo | 1, 7, 42 |
| `latitud` | Descriptor | Latitud de la zona dentro del vivero | Número decimal entre -90 y 90 | 28.4876 |
| `longitud` | Descriptor | Longitud de la zona dentro del vivero | Número decimal entre -180 y 180 | -16.3161 |

### Producto

| Atributo | Tipo | Descripción | Dominio | Ejemplos |
|---|---|---|---|---|
| `id_producto` | Identificador | Código único del producto | Número entero positivo | 1, 230, 1504 |
| `nombre` | Descriptor | Nombre comercial del producto | Texto | "Rosal trepador", "Saco de sustrato 50 L", "Maceta de barro" |
| `tipo` | Descriptor | Categoría del producto | {planta, jardinería, decoración} | "planta", "decoración" |

### Empleado

| Atributo | Tipo | Descripción | Dominio | Ejemplos |
|---|---|---|---|---|
| `id_empleado` | Identificador | Código único del empleado | Número entero positivo | 1, 23, 108 |
| `nombre` | Descriptor | Nombre y apellidos del empleado | Texto | "Ana Pérez González" |

### Pedido

| Atributo | Tipo | Descripción | Dominio | Ejemplos |
|---|---|---|---|---|
| `id_pedido` | Identificador | Número único del pedido | Número entero positivo | 1532, 1533 |
| `fecha` | Descriptor | Fecha en la que se realizó el pedido | Fecha (AAAA-MM-DD) | 2026-03-15 |
| `precio` | Descriptor | Importe total del pedido, en euros | Número decimal mayor o igual que 0, con dos decimales | 45.90, 120.00 |

### Cliente_Plus

| Atributo | Tipo | Descripción | Dominio | Ejemplos |
|---|---|---|---|---|
| `id_cliente` | Identificador | Código único del cliente | Número entero positivo | 1, 87, 2301 |
| `nombre` | Descriptor | Nombre y apellidos del cliente | Texto | "Luis Martín Díaz" |
| `fecha_ingreso` | Descriptor | Fecha de alta en el programa Tajinaste Plus | Fecha (AAAA-MM-DD) | 2024-11-02 |
| `bonificaciones` | Multivaluado | Bonificaciones asignadas cada mes según el volumen de compras, en euros | Conjunto de números decimales mayores o iguales que 0 | {5.00, 12.50, 8.75} |

### Atributos de las relaciones

| Relación | Atributo | Tipo | Descripción | Dominio | Ejemplos |
|---|---|---|---|---|---|
| Tarea | `fecha_inicio` | Identificativo de la relación | Fecha en la que empieza el destino del empleado en la zona | Fecha (AAAA-MM-DD) | 2026-06-01 |
| Tarea | `fecha_fin` | Descriptor | Fecha en la que termina el destino. Vacía si el destino sigue vigente | Fecha (AAAA-MM-DD) o vacío | 2026-09-30 |
| Tarea | `tipo_de_tarea` | Descriptor | Tarea o puesto que desempeña el empleado durante ese destino | Texto | "riego", "atención al cliente", "caja", "carga y descarga" |
| Tiene | `cantidad` | Descriptor | Unidades disponibles de un producto en una zona | Número entero mayor o igual que 0 | 0, 25, 340 |

## Relaciones y cardinalidades

En cada relación, la participación (mínimo, máximo) que aparece junto a una entidad indica cuántas ocurrencias de esa entidad se asocian con una ocurrencia de la otra. La cardinalidad de la relación se obtiene a partir de las dos participaciones máximas.

### Está en (Zona – Vivero) · 1:N · dependencia en existencia

Indica a qué vivero pertenece cada zona.

- Una zona pertenece como mínimo a 1 vivero y como máximo a 1 → **(1,1)** junto a Vivero. No puede haber zonas sin vivero.
- Un vivero tiene como mínimo 1 zona y como máximo varias → **(1,n)** junto a Zona.
- Cardinalidad **1:N**. La relación está marcada con **E** porque Zona depende en existencia de Vivero.

### Tiene (Zona – Producto) · N:M

Representa el stock: qué productos hay en cada zona y en qué cantidad. Su atributo `cantidad` depende de la combinación de zona y producto, ya que un mismo producto puede tener cantidades distintas en cada zona.

- Un producto está asignado como mínimo a 1 zona y como máximo a varias → **(1,n)** junto a Zona.
- Una zona contiene como mínimo 1 producto y como máximo varios → **(1,n)** junto a Producto.
- Cardinalidad **N:M**.

### Tarea (Empleado – Zona) · N:M

Registra el histórico de destinos de los empleados: en qué zona trabaja cada empleado, durante qué periodo y desempeñando qué tarea. Permite analizar la productividad de cada zona y de cada empleado a lo largo del tiempo.

- Un empleado, a lo largo del tiempo, trabaja como mínimo en 1 zona y como máximo en varias → **(1,n)** junto a Zona.
- Una zona, a lo largo del tiempo, tiene como mínimo 1 empleado y como máximo varios → **(1,n)** junto a Empleado.
- Cardinalidad **N:M**.

Un mismo empleado puede volver a la misma zona en distintas temporadas, por lo que la pareja empleado-zona puede repetirse. Cada destino se distingue por su `fecha_inicio`, que forma parte de la identificación de la relación.

### Gestiona (Empleado – Pedido) · 1:N

Indica qué empleado es responsable de cada pedido, para medir su capacidad de lograr objetivos de venta.

- Un pedido tiene como mínimo 1 responsable y como máximo 1 → **(1,1)** junto a Empleado. Esto recoge que cada pedido tiene un único responsable.
- Un empleado gestiona como mínimo 0 pedidos y como máximo varios → **(0,n)** junto a Pedido. Puede haber empleados que no gestionen pedidos.
- Cardinalidad **1:N**.

### Hace (Cliente_Plus – Pedido) · 1:N

Indica qué cliente del programa ha realizado cada pedido, para poder realizar campañas entre los clientes Tajinaste Plus.

- Un pedido es realizado como mínimo por 1 cliente y como máximo por 1 → **(1,1)** junto a Cliente_Plus.
- Un cliente realiza como mínimo 0 pedidos y como máximo varios → **(0,n)** junto a Pedido. Un cliente recién incorporado al programa puede no haber hecho todavía ningún pedido.
- Cardinalidad **1:N**.

## Restricciones semánticas

Algunas reglas del enunciado no se pueden expresar con las cardinalidades del diagrama, por lo que las recogemos como restricciones semánticas:

- **Un empleado no puede tener dos destinos a la vez.** Los periodos (`fecha_inicio`, `fecha_fin`) de la relación Tarea de un mismo empleado no pueden solaparse. Las cardinalidades solo reflejan el total a lo largo del tiempo, por lo que no pueden impedir dos destinos simultáneos.
- **La fecha de fin de un destino no puede ser anterior a su fecha de inicio.** Si la fecha de fin está vacía, el destino sigue vigente.
- **Los pedidos registrados son posteriores al ingreso del cliente en el programa.** La fecha de un pedido debe ser igual o posterior a la `fecha_ingreso` del cliente, ya que el enunciado indica que los pedidos se controlan desde su ingreso en Tajinaste Plus.
- **La cantidad de un producto en una zona no puede ser negativa.**
- **Las bonificaciones no pueden ser negativas.**

# Desarrollo del modelo

En este apartado explicamos cómo hemos ido construyendo el modelo: cómo interpretamos el enunciado, qué decisiones tomamos, qué errores cometimos por el camino y cómo los corregimos.

## 1. Primer planteamiento

En la primera lectura identificamos tres entidades: **Vivero**, **Empleado** y **Cliente**, unidas por una única relación **Compra** en la que participaban el vivero, la zona, el empleado que gestionaba la venta y el cliente, con una fecha. Para distinguir a los clientes del programa de fidelización añadimos un atributo booleano `es_plus`. Nuestra idea era que, con este esquema, cualquier información que pidiera el enunciado se podría obtener después con vistas y consultas con `JOIN`.

Este planteamiento tenía un error de enfoque: estábamos pensando en el **nivel lógico** (tablas y consultas) en lugar de en el **nivel conceptual**. El modelo E/R no consiste en preguntarse qué se podrá calcular después, sino en representar fielmente qué cosas existen en el mundo real y cómo se relacionan. Al meter todo en una sola relación mezclábamos hechos que en la realidad son independientes: por ejemplo, un empleado está destinado en una zona aunque no gestione ninguna venta, y con nuestro modelo un empleado sin ventas no habría estado destinado en ningún sitio.

## 2. Lo que faltaba: el stock

Al releer el enunciado vimos que habíamos olvidado su objetivo principal: la empresa quiere "llevar un control del **stock** en los viveros" y saber "de cada producto **cuánto hay disponible en cada zona**". En nuestro primer modelo no existían ni los productos ni las zonas como elementos propios.

## 3. Vivero y Zona

Nuestra segunda idea fue una única entidad Vivero que guardara a la vez los viveros, sus zonas y la latitud y longitud de cada zona. La descartamos por tres motivos:

- El enunciado indica que **tanto el vivero como cada zona** tienen su propia georreferenciación, por lo que no quedaba claro de quién era cada latitud.
- Un vivero tiene **varias** zonas, lo que obligaba a repetir los datos del vivero o a convertir la zona en un atributo multivaluado.
- La zona **se relaciona con otras entidades**: en ella se almacenan productos y trabajan empleados.

Aplicando el criterio de que algo con datos propios y que se relaciona con otros elementos es una entidad, separamos **Vivero** y **Zona** en dos entidades unidas por la relación **Está en**, con cardinalidad **1:N** (una zona pertenece a un único vivero y un vivero tiene una o varias zonas).

Después nos planteamos si Zona debía ser una entidad débil, ya que una zona no existe sin su vivero. Valoramos dos opciones:

- **Dependencia en identificación:** la zona se identificaría por un discriminante (como "Almacén", que se repite en varios viveros pero no dentro del mismo) junto con el identificador del vivero.
- **Dependencia en existencia:** la zona mantiene su propio identificador, pero no tiene sentido sin el vivero al que pertenece.

Elegimos la **dependencia en existencia**: Zona conserva su identificador propio `id_zona`, lo que simplifica su uso en el resto de relaciones (Tarea y Tiene), y la dependencia de su vivero queda reflejada como entidad débil y con la marca **E** en la relación Está en.

## 4. El stock como relación

Inicialmente pensamos en el stock como una tabla con producto, cantidad e identificador de zona. Al razonarlo en términos conceptuales, vimos que el stock no es una cosa con identidad propia, sino la información de "cuánto hay de este producto en esta zona". La cantidad no depende solo del producto ni solo de la zona, sino de la **combinación** de ambos. Por eso lo modelamos como la relación **Tiene** entre **Zona** y **Producto**, con el atributo propio `cantidad`, y cardinalidad **N:M**: una zona puede albergar muchos productos y un producto puede estar en muchas zonas.

En una de las versiones del diagrama colocamos `cantidad` como atributo de Producto. Lo corregimos, ya que así habría representado una única cantidad para cada producto en toda la empresa, en lugar de la cantidad disponible en cada zona.

## 5. Errores de notación en los primeros diagramas

En los primeros diagramas cometimos varios errores que fuimos corrigiendo:

- **Poníamos claves foráneas como atributos de las relaciones** (`id_empleado`, `id_cliente` e `id_vivero` dentro de los rombos). En el E/R, la línea que une una relación con una entidad ya indica quién participa; en una relación solo deben aparecer sus atributos propios. Los identificadores se movieron a sus entidades.
- **`id_vivero` en la relación de los empleados era además redundante**: el empleado trabaja en una zona y la zona ya pertenece a un vivero, así que guardar también el vivero podía dar lugar a datos contradictorios.
- **Unimos Zona y Vivero con una línea sin rombo.** Entre dos entidades siempre debe haber una relación con nombre.
- **Usábamos "M:M" y "1:M"** en lugar de la notación correcta **N:M** y **1:N**.
- **Nombrábamos entidades en plural** ("Productos"), cuando una entidad representa un tipo de elemento, por lo que debe ir en singular.

## 6. Los empleados y el histórico de destinos

El párrafo de los empleados fue el que más nos costó entender. En un primer momento lo interpretamos como que un empleado podía estar en varias zonas a la vez, siempre que fueran de viveros distintos. Era justo al revés:

- *"Pueden ser destinados a diferentes viveros según la época del año"* significa que, **a lo largo del tiempo**, un empleado pasa por varios viveros.
- *"Nunca van a tener dos destinos"* significa que, **en un momento dado**, solo está en un vivero.
- *"En cada vivero que desempeñe una tarea lo hará en una zona"* significa que, en cada destino, trabaja en una única zona.
- El *"seguimiento del histórico del puesto"* obliga a guardar todos los destinos pasados, no solo el actual, para poder relacionar la productividad de cada zona y de cada empleado con quién trabajaba allí en cada momento.

Con esta interpretación modelamos la relación **Tarea** entre **Empleado** y **Zona** con los atributos `fecha_inicio`, `fecha_fin` y `tipo_de_tarea`. Este último recoge la tarea o puesto que desempeña el empleado en ese periodo, que eran las dos palabras que el enunciado destacaba. Como el modelo guarda el histórico, la cardinalidad se razona a lo largo del tiempo: un empleado trabaja en muchas zonas y una zona tiene muchos empleados, es decir, **N:M**.

Al analizarla detectamos un caso que había que tener en cuenta: un mismo empleado puede volver a la misma zona en otra temporada, de forma que la misma pareja empleado-zona aparece más de una vez. Lo que distingue un destino de otro es la **fecha de inicio**, por lo que la marcamos como parte de la identificación de la relación.

La condición de que un empleado nunca tenga dos destinos a la vez no se puede expresar con cardinalidades, porque estas solo reflejan el total a lo largo del tiempo, así que la recogemos como restricción semántica.

## 7. Los pedidos

Esta fue la parte a la que más vueltas le dimos. El enunciado indica que se controlan los pedidos que gestiona cada empleado, *"teniendo en cuenta que cada pedido sólo tiene un responsable"*.

Nuestro primer intento fue mantener **Compra** como relación entre Empleado y Cliente, pensando en identificarla por empleado, cliente y fecha. Al comprobarlo con filas de ejemplo vimos el problema: dos filas con el mismo cliente y la misma fecha pero distinto empleado podían ser dos pedidos distintos o el mismo pedido con dos responsables, y no había forma de saberlo. Si quitábamos el empleado de la identificación, garantizábamos un solo responsable, pero entonces un cliente no podía hacer dos pedidos el mismo día, algo que sí puede ocurrir.

En un intento posterior pusimos participaciones **(1,1)** en ambos lados de Compra con cardinalidad **1:1**, pero eso significaba que cada empleado gestionaba un único pedido en toda su vida y cada cliente compraba una sola vez. También llegamos a marcar `fecha` y `precio` como identificadores, aunque el precio no identifica nada.

La clave fue entender la diferencia entre una **relación** (un vínculo entre elementos) y una **entidad** (algo con identidad propia a lo que se puede señalar). Un pedido tiene número, fecha e importe, y se puede hablar de "el pedido 1532": es una entidad. Al convertir **Pedido** en entidad con su propio identificador `id_pedido`, la relación Compra se separó en dos:

- **Gestiona**, entre Empleado y Pedido, con cardinalidad **1:N**: un pedido tiene exactamente un responsable (1,1) y un empleado puede gestionar muchos pedidos.
- **Hace**, entre Cliente y Pedido, con cardinalidad **1:N**: un pedido pertenece a un único cliente (1,1) y un cliente puede hacer muchos pedidos.

Así, la condición de un único responsable deja de ser un problema de claves y pasa a expresarse directamente con la participación máxima 1 del lado del empleado.

## 8. Los clientes Tajinaste Plus

Nuestro modelo recogía la pertenencia al programa con el atributo `es_plus`, pero con la relación Hace conectada a todos los clientes el modelo permitía registrar pedidos de clientes que no son Plus, mientras que el enunciado solo controla los pedidos de los clientes del programa. Además, faltaban datos que el enunciado menciona: la **fecha de ingreso** en el programa y las **bonificaciones mensuales**.

Nos surgió la duda de si sería necesario guardar los pedidos de todos los clientes para controlar el stock. Concluimos que el stock que pide el enunciado es la cantidad disponible de cada producto en cada zona, que ya recoge la relación Tiene, y que nuestros pedidos no están relacionados con los productos, por lo que guardar los pedidos de todos los clientes no aportaría información sobre el stock.

Planteamos dos alternativas: que el modelo solo contemple clientes Plus, o una jerarquía con Cliente como entidad general y Cliente Plus como subtipo con la fecha de ingreso, las bonificaciones y la relación con los pedidos.

Elegimos la primera opción: el modelo **solo contempla clientes del programa Tajinaste Plus**. El enunciado no pide guardar ninguna información de los clientes que no pertenecen al programa, así que una jerarquía habría añadido una entidad general sin atributos ni relaciones propias. Por ello sustituimos la entidad Cliente por **Cliente_Plus** y eliminamos el atributo `es_plus`, que ya no aportaba nada porque todos los clientes del modelo lo son. Como consecuencia, la relación **Hace** solo admite pedidos de clientes Plus, que es justo lo que indica el enunciado.

A esta entidad le añadimos los datos propios del programa:

- **Fecha de ingreso** en el programa, necesaria porque los pedidos se controlan "desde su ingreso en el programa".
- **Bonificaciones**, que se asignan mensualmente en función del volumen de compras. Como un mismo cliente acumula una bonificación por cada mes, las representamos como un **atributo multivaluado**.
