# Explicación sobre las capas: Controller, Service y Repository

En una arquitectura basada en servicios, las capas de **Controller**, **Service**, y **Repository** están diseñadas para mantener una separación de preocupaciones, lo que facilita la mantenibilidad y escalabilidad del código.

## 1. Controller:
El **Controller** es la capa responsable de recibir y manejar las solicitudes HTTP. Su función principal es la de orquestador: recibe los datos, los valida y delega la lógica de negocio a la capa de servicios. Después, recopila el resultado del **Service** y lo envía como respuesta al cliente. Es el punto de entrada para las interacciones con el usuario o API.

- **Responsabilidades**:
    - Recibir y procesar solicitudes HTTP.
    - Validar y transformar los datos de entrada.
    - Delegar la ejecución de lógica de negocio a la capa de **Service**.

## 2. Service:
La capa de **Service** encapsula la **lógica de negocio** de la aplicación. Es el puente entre los controladores y los repositorios, asegurando que todas las reglas de negocio y procesos complejos se gestionen de manera centralizada y coherente. Además, esta capa promueve la reutilización de código, ya que la lógica puede ser usada por diferentes controladores.

- **Responsabilidades**:
    - Implementar la lógica de negocio.
    - Interactuar con el **Repository** para gestionar el acceso a los datos.
    - Realizar validaciones y operaciones que van más allá de un simple CRUD.

## 3. Repository:
El **Repository** abstrae la capa de persistencia de datos, proporcionando una interfaz clara para las operaciones CRUD (Create, Read, Update, Delete). Está aislado de la lógica de negocio y su responsabilidad es única: interactuar directamente con la base de datos. Usualmente, el **Repository** sigue un patrón DAO (Data Access Object) para gestionar el acceso a los datos de manera limpia y eficiente.

- **Responsabilidades**:
    - Ejecutar operaciones CRUD.
    - Abstraer las consultas a la base de datos.
    - Proveer métodos específicos para recuperar o modificar datos.

## Relación Compleja entre las Capas:
1. El **Controller** recibe la solicitud del cliente (por ejemplo, un POST de creación de usuario).
2. Pasa los datos a la capa **Service**, que aplica las reglas de negocio, como verificar si el usuario ya existe.
3. La capa **Service** interactúa con el **Repository** para realizar operaciones sobre la base de datos, como almacenar el nuevo usuario.
4. El **Repository** realiza las consultas directas a la base de datos y devuelve el resultado a la capa **Service**.
5. El **Service** procesa el resultado y lo devuelve al **Controller** para que éste genere la respuesta apropiada para el cliente.

Este modelo promueve el **desacoplamiento** y facilita la **pruebas unitarias** al permitir testear cada capa de manera independiente.
