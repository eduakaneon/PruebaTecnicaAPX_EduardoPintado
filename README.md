## README - Proyecto APX - WIKJ123

### Descripción del Proyecto

Este proyecto implementa un servicio para gestionar clientes en una base de datos. Proporciona métodos para insertar un nuevo cliente, buscar un cliente por su ID y verificar si un NUIP ya existe en la base de datos.

### Estructura del Proyecto

El proyecto consta de las siguientes partes principales:

1. **Implementación de Servicio (`WIKJR123Impl`)**: Esta clase contiene la lógica de negocio para insertar y buscar clientes en la base de datos.
  ### Código

```java
public class WIKJR123Impl extends WIKJR123Abstract {

	private static final Logger LOGGER = LoggerFactory.getLogger(WIKJR123Impl.class);

	@Override
	public int executeInsert(CustomerIn customerIn) {
		Map<String, Object> args = new HashMap<>();
		args.put("id", customerIn.getId());
		args.put("nuip", customerIn.getNuip());
		args.put("phone", customerIn.getPhone());
		args.put("address", customerIn.getAddress());
		args.put("full_name", customerIn.getFullName());
		return this.jdbcUtils.update("query.insert",args);
	}
	@Override
	public CustomerOut executeSelect(String id) {
		Map<String,Object> result = new HashMap<>();
		result = this.jdbcUtils.queryForMap("query.select",id);
		CustomerOut customerOut = new CustomerOut();
		customerOut.setId(String.valueOf(result.get("id")));
		customerOut.setNuip(String.valueOf(result.get("nuip")));
		customerOut.setPhone(String.valueOf(result.get("phone")));
		customerOut.setAddress(String.valueOf(result.get("address")));
		customerOut.setFullName(String.valueOf(result.get("full_name")));
		return customerOut;
	}
}
```

2. **Transacción (`WIKJT12301ESTransaction`)**: Esta clase implementa una transacción que utiliza el servicio para insertar un cliente si el NUIP no existe en la base de datos.
### Código

```java
public class WIKJT12301ESTransaction extends AbstractWIKJT12301ESTransaction {

	private static final Logger LOGGER = LoggerFactory.getLogger(WIKJT12301ESTransaction.class);

	@Override
	public void execute() {
		WIKJR123 wikjR123 = this.getServiceLibrary(WIKJR123.class);
		CustomerIn customer = this.getCustomerin();
		int result = wikjR123.executeInsert(customer);
		if(result == 0){
			setCustomerout(null);
		}else{
			CustomerOut customerOut = wikjR123.executeSelect(customer.getId());
			setCustomerout(customerOut);
		}
	}
}

```
3. **Consultas SQL (`query.insert`, `query.select`, `query.selectNuip`)**: Estas consultas SQL están definidas en un archivo de propiedades y se utilizan para realizar operaciones de base de datos como la inserción y selección de clientes.
### Código

```java
query.insert=wikj;INSERT INTO WIKJ.T_WIKJ_HAB_PRUEBAFINAL (id, nuip, full_name, phone, address) VALUES (:id,:nuip,:full_name,:phone,:address);
query.select=wikj;SELECT HAB.id, HAB.nuip, HAB.full_name, HAB.phone, HAB.address FROM WIKJ.T_WIKJ_HAB_PRUEBAFINAL HAB WHERE HAB.id = :id;
```

4. **Dtos
   Estas clases permiten pasar datos de una capa a otra sin exponer directamente los detalles de implementación de cada capa.
   
```java
public class CustomerIn implements Serializable  {
	private String id;
	private String nuip;
	private String fullName;
	private String phone;
	private String address;
// Getters y Setters
```
```java
public class CustomerOut implements Serializable  {
	private String id;
	private String nuip;
	private String fullName;
	private String phone;
	private String address;
// Getters y Setters
```

### Implementación del Servicio

La clase `WIKJR123Impl` proporciona métodos para insertar y seleccionar clientes en la base de datos. Los métodos incluyen:

- `executeInsert`: Inserta un nuevo cliente en la base de datos.
- `executeSelect`: Busca un cliente por su ID en la base de datos.
- `executeSelectNuip`: Verifica si un NUIP ya existe en la base de datos.

### Transacción

La clase `WIKJT12301ESTransaction` implementa una transacción que utiliza el servicio para insertar un cliente si el NUIP no existe en la base de datos. Si el NUIP ya existe, la transacción no realiza ninguna operación. Esto ha sido conseguido mediante la creacion de la tabla en DBeaver con el nuip como campo UNIQUE.

CREATE TABLE WIKJ."T_WIKJ_HAB_PRUEBAFINAL"(
	id NUMBER(5) NOT NULL PRIMARY KEY,
	nuip NUMBER(10) UNIQUE ,
	full_name VARCHAR(50),
	phone VARCHAR(15),
	address VARCHAR(30)
)

### Consultas SQL

Las consultas SQL están definidas en un archivo de propiedades y se utilizan para interactuar con la base de datos. Las consultas incluyen:

- `query.insert`: Consulta SQL para insertar un nuevo cliente.
- `query.select`: Consulta SQL para seleccionar un cliente por su ID.
- `query.selectNuip`: Consulta SQL para verificar si un NUIP ya existe en la base de datos.

### Configuración

El proyecto está configurado para utilizar una base de datos denominada "WIKJ" y una tabla llamada "T_WIKJ_HAB_PRUEBAFINAL" para almacenar la información de los clientes.

### Pruebas de uso
![PruebaTecPostman (2)](https://github.com/eduakaneon/PruebaTecnicaAPX_EduardoPintado/assets/152890267/92d1c8ce-2c3b-40eb-a712-0b9e2ae58e6c)

### Uso
1. Crear una base de datos con el nombre y los campos correspondientes
2. Levantar el entorno local con el comando 'deploy online'
3. Abrir el archivo start online de la carpeta del entorno local
4. Realizar las configuraciones en la interfaz del entorno local asi como la conexion a la base de datos
5. Ir a Postman y realizar las pruebas correpondientes
