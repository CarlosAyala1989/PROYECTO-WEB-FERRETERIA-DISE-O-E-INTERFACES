### Documento: Relaciones y Cardinalidad de Tablas de la Base de Datos

Este documento describe las interconexiones y la naturaleza de las relaciones (cardinalidad) entre las diferentes tablas del esquema de base de datos diseñado para el sistema de gestión.

**1. Tabla `Usuarios` y Tabla `Pedidos`**
    * **Relación:** Un usuario (con el rol de 'mesero') es responsable de tomar un pedido.
    * **Descripción Detallada:**
        * Un registro en la tabla `Usuarios` (que representa a un mesero) puede estar vinculado a múltiples registros en la tabla `Pedidos`. Esto significa que un mesero puede atender y registrar varios pedidos a lo largo del tiempo.
        * Cada registro en la tabla `Pedidos` debe estar obligatoriamente asociado a un único registro en la tabla `Usuarios`. Esto asegura que cada pedido tenga un mesero responsable asignado.
    * **Cardinalidad:** Uno a Muchos (1 `Usuario` : N `Pedidos`).
        * Desde la perspectiva de `Usuarios` hacia `Pedidos`: Un usuario puede gestionar desde cero hasta muchos pedidos.
        * Desde la perspectiva de `Pedidos` hacia `Usuarios`: Un pedido siempre es gestionado por exactamente un usuario (mesero).
    * **Claves Involucradas:**
        * `Usuarios.id_usuario` (Clave Primaria en `Usuarios`)
        * `Pedidos.id_usuario_mesero` (Clave Foránea en `Pedidos` que referencia a `Usuarios.id_usuario`)

**2. Tabla `Usuarios` y Tabla `Comprobantes`**
    * **Relación:** Un usuario (con el rol de 'cajero' o 'administrador') es responsable de emitir un comprobante de pago.
    * **Descripción Detallada:**
        * Un registro en la tabla `Usuarios` (cajero/administrador) puede estar vinculado a múltiples registros en la tabla `Comprobantes`. Un cajero puede emitir varios comprobantes durante su turno o a lo largo del tiempo.
        * Cada registro en la tabla `Comprobantes` debe estar obligatoriamente asociado a un único registro en la tabla `Usuarios`. Esto asegura que cada comprobante tenga un emisor responsable.
    * **Cardinalidad:** Uno a Muchos (1 `Usuario` : N `Comprobantes`).
        * Desde `Usuarios` hacia `Comprobantes`: Un usuario puede emitir desde cero hasta muchos comprobantes.
        * Desde `Comprobantes` hacia `Usuarios`: Un comprobante siempre es emitido por exactamente un usuario.
    * **Claves Involucradas:**
        * `Usuarios.id_usuario` (Clave Primaria en `Usuarios`)
        * `Comprobantes.id_usuario_cajero` (Clave Foránea en `Comprobantes` que referencia a `Usuarios.id_usuario`)

**3. Tabla `Usuarios` y Tabla `Pedido_Detalles` (Relación Opcional para Preparador)**
    * **Relación:** Un usuario (con el rol de 'cocinero' o 'bartender') es responsable de preparar un ítem específico de un pedido.
    * **Descripción Detallada:**
        * Un registro en la tabla `Usuarios` (cocinero/bartender) puede estar vinculado a múltiples registros en la tabla `Pedido_Detalles`. Un cocinero o bartender puede preparar muchos ítems diferentes.
        * Un registro en la tabla `Pedido_Detalles` puede, opcionalmente, estar asociado a un único registro en la tabla `Usuarios`. Esto permite identificar quién preparó el ítem, pero no es obligatorio (el campo `id_usuario_preparador` puede ser `NULL`).
    * **Cardinalidad:** Uno a Muchos (1 `Usuario` : N `Pedido_Detalles`), donde la participación del `Pedido_Detalle` es opcional.
        * Desde `Usuarios` hacia `Pedido_Detalles`: Un usuario (preparador) puede preparar desde cero hasta muchos ítems de pedido.
        * Desde `Pedido_Detalles` hacia `Usuarios`: Un ítem de pedido puede ser preparado por cero o un usuario.
    * **Claves Involucradas:**
        * `Usuarios.id_usuario` (Clave Primaria en `Usuarios`)
        * `Pedido_Detalles.id_usuario_preparador` (Clave Foránea en `Pedido_Detalles` que referencia a `Usuarios.id_usuario`, permite valores `NULL`)

**4. Tabla `Categorias_Producto` y Tabla `Productos`**
    * **Relación:** Un producto se clasifica dentro de una categoría de producto.
    * **Descripción Detallada:**
        * Un registro en la tabla `Categorias_Producto` puede estar vinculado a múltiples registros en la tabla `Productos`. Una categoría (ej. "Bebidas") puede contener muchos productos diferentes.
        * Un registro en la tabla `Productos` puede, opcionalmente, estar asociado a un único registro en la tabla `Categorias_Producto`. Un producto puede pertenecer a una categoría o a ninguna si el campo `id_categoria_producto` es `NULL`.
    * **Cardinalidad:** Uno a Muchos (1 `Categoria_Producto` : N `Productos`), donde la participación del `Producto` es opcional respecto a la categoría.
        * Desde `Categorias_Producto` hacia `Productos`: Una categoría puede tener desde cero hasta muchos productos.
        * Desde `Productos` hacia `Categorias_Producto`: Un producto pertenece a cero o una categoría.
    * **Claves Involucradas:**
        * `Categorias_Producto.id_categoria_producto` (Clave Primaria en `Categorias_Producto`)
        * `Productos.id_categoria_producto` (Clave Foránea en `Productos` que referencia a `Categorias_Producto.id_categoria_producto`, permite valores `NULL`)

**5. Tabla `Mesas` y Tabla `Pedidos`**
    * **Relación:** Un pedido se origina y está asociado a una mesa específica del establecimiento.
    * **Descripción Detallada:**
        * Un registro en la tabla `Mesas` puede estar vinculado a múltiples registros en la tabla `Pedidos`. Una mesa puede tener varios pedidos a lo largo del tiempo (ej. diferentes clientes en diferentes momentos, o un mismo cliente haciendo varios pedidos).
        * Cada registro en la tabla `Pedidos` debe estar obligatoriamente asociado a un único registro en la tabla `Mesas`. Todo pedido debe originarse en una mesa.
    * **Cardinalidad:** Uno a Muchos (1 `Mesa` : N `Pedidos`).
        * Desde `Mesas` hacia `Pedidos`: Una mesa puede tener desde cero hasta muchos pedidos.
        * Desde `Pedidos` hacia `Mesas`: Un pedido siempre está asociado a exactamente una mesa.
    * **Claves Involucradas:**
        * `Mesas.id_mesa` (Clave Primaria en `Mesas`)
        * `Pedidos.id_mesa` (Clave Foránea en `Pedidos` que referencia a `Mesas.id_mesa`)

**6. Tabla `Pedidos` y Tabla `Pedido_Detalles`**
    * **Relación:** Un pedido se desglosa en uno o más ítems o productos específicos, que son los detalles del pedido.
    * **Descripción Detallada:**
        * Un registro en la tabla `Pedidos` puede estar vinculado a múltiples registros en la tabla `Pedido_Detalles`. Un pedido típicamente contiene varios productos.
        * Cada registro en la tabla `Pedido_Detalles` debe estar obligatoriamente asociado a un único registro en la tabla `Pedidos`. Cada ítem de producto solicitado forma parte de un pedido específico.
    * **Cardinalidad:** Uno a Muchos (1 `Pedido` : N `Pedido_Detalles`).
        * Desde `Pedidos` hacia `Pedido_Detalles`: Un pedido puede tener desde uno hasta muchos detalles (ítems). (Asumiendo que un pedido no puede estar vacío).
        * Desde `Pedido_Detalles` hacia `Pedidos`: Un detalle de pedido siempre pertenece a exactamente un pedido.
    * **Claves Involucradas:**
        * `Pedidos.id_pedido` (Clave Primaria en `Pedidos`)
        * `Pedido_Detalles.id_pedido` (Clave Foránea en `Pedido_Detalles` que referencia a `Pedidos.id_pedido`)
    * **Flujo de Estados en `Pedido_Detalles`:**
        * Cuando un mesero toma un pedido, se crea un registro en `Pedidos` (ej. `estado_pedido = 'PENDIENTE'`).
        * Por cada producto solicitado, se crea un registro en `Pedido_Detalles` con `estado_item = 'SOLICITADO'`. La aplicación mostraría estos ítems 'SOLICITADOS' al personal de cocina/bar.
        * El personal de cocina/bar cambiaría `estado_item` a `'EN_PREPARACION'`, luego a `'LISTO_PARA_ENTREGA'`, y finalmente a `'ENTREGADO_A_MESERO'`.
        * El mesero, al recibirlo y llevarlo al cliente, cambiaría `estado_item` a `'ENTREGADO_A_CLIENTE'`.
        * El `estado_pedido` en la tabla `Pedidos` se actualizaría en la lógica de la aplicación basándose en el estado de todos sus `Pedido_Detalles`. Por ejemplo, si todos los ítems están `'ENTREGADO_A_CLIENTE'`, el `estado_pedido` podría pasar a `'SERVIDO'`.

**7. Tabla `Productos` y Tabla `Pedido_Detalles`**
    * **Relación:** Un ítem específico en un detalle de pedido corresponde a un producto del catálogo.
    * **Descripción Detallada:**
        * Un registro en la tabla `Productos` puede estar vinculado a múltiples registros en la tabla `Pedido_Detalles`. Un mismo producto puede ser solicitado en muchos pedidos diferentes o varias veces.
        * Cada registro en la tabla `Pedido_Detalles` debe estar obligatoriamente asociado a un único registro en la tabla `Productos`. Cada ítem de un pedido debe ser un producto existente.
    * **Cardinalidad:** Uno a Muchos (1 `Producto` : N `Pedido_Detalles`).
        * Desde `Productos` hacia `Pedido_Detalles`: Un producto puede aparecer en desde cero hasta muchos detalles de pedido.
        * Desde `Pedido_Detalles` hacia `Productos`: Un detalle de pedido siempre corresponde a exactamente un producto.
    * **Claves Involucradas:**
        * `Productos.id_producto` (Clave Primaria en `Productos`)
        * `Pedido_Detalles.id_producto` (Clave Foránea en `Pedido_Detalles` que referencia a `Productos.id_producto`)

**8. Tabla `Pedidos` y Tabla `Comprobantes`**
    * **Relación:** Un comprobante de pago (boleta o factura) se emite para un pedido específico.
    * **Descripción Detallada:**
        * Un registro en la tabla `Pedidos` está asociado, como máximo, a un único registro en la tabla `Comprobantes`. Se asume que un pedido genera un solo comprobante.
        * Un registro en la tabla `Comprobantes` debe estar obligatoriamente asociado a un único registro en la tabla `Pedidos`. Todo comprobante se emite en base a un pedido.
    * **Cardinalidad:** Uno a Uno (1 `Pedido` : 1 `Comprobante`). Esta relación se refuerza mediante una restricción `UNIQUE` en la clave foránea `Comprobantes.id_pedido`.
    * **Claves Involucradas:**
        * `Pedidos.id_pedido` (Clave Primaria en `Pedidos`)
        * `Comprobantes.id_pedido` (Clave Foránea en `Comprobantes` que referencia a `Pedidos.id_pedido`, y que además tiene una restricción `UNIQUE`)

Este detalle de relaciones y cardinalidades es fundamental para entender el flujo de datos, asegurar la integridad de la información y para el desarrollo de consultas y la lógica de la aplicación.
