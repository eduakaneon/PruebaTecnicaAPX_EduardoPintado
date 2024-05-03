## README - Proyecto APX

### Descripción del Proyecto

Este proyecto implementa un servicio para gestionar clientes en una base de datos. Proporciona métodos para insertar un nuevo cliente, buscar un cliente por su ID y verificar si un NUIP ya existe en la base de datos.

### Estructura del Proyecto

El proyecto consta de las siguientes partes principales:

1. **Implementación de Servicio (`WIKJR123Impl`)**: Esta clase contiene la lógica de negocio para insertar y buscar clientes en la base de datos.
  ### Código

```java

```

2. **Transacción (`WIKJT12301ESTransaction`)**: Esta clase implementa una transacción que utiliza el servicio para insertar un cliente si el NUIP no existe en la base de datos.
### Código

```java

```
3. **Consultas SQL (`query.insert`, `query.select`, `query.selectNuip`)**: Estas consultas SQL están definidas en un archivo de propiedades y se utilizan para realizar operaciones de base de datos como la inserción y selección de clientes.
### Código

```java

```
### Implementación del Servicio

La clase `WIKJR123Impl` proporciona métodos para insertar y seleccionar clientes en la base de datos. Los métodos incluyen:

- `executeInsert`: Inserta un nuevo cliente en la base de datos.
- `executeSelect`: Busca un cliente por su ID en la base de datos.
- `executeSelectNuip`: Verifica si un NUIP ya existe en la base de datos.

### Transacción

La clase `WIKJT12301ESTransaction` implementa una transacción que utiliza el servicio para insertar un cliente si el NUIP no existe en la base de datos. Si el NUIP ya existe, la transacción no realiza ninguna operación.

### Consultas SQL

Las consultas SQL están definidas en un archivo de propiedades y se utilizan para interactuar con la base de datos. Las consultas incluyen:

- `query.insert`: Consulta SQL para insertar un nuevo cliente.
- `query.select`: Consulta SQL para seleccionar un cliente por su ID.
- `query.selectNuip`: Consulta SQL para verificar si un NUIP ya existe en la base de datos.

### Configuración

El proyecto está configurado para utilizar una base de datos denominada "WIKJ" y una tabla llamada "T_WIKJ_HAB_PRUEBAFINAL" para almacenar la información de los clientes.

### Uso


