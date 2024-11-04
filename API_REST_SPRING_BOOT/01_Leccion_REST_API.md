
# Lección: Arquitectura REST para Servicios Web

### 1. Introducción a REST
**Representational State Transfer (REST)** es un estilo de arquitectura de software que se utiliza principalmente para el diseño de servicios web. Fue propuesto por Roy Fielding y se basa en los principios de HTTP para la comunicación entre cliente y servidor. A diferencia de otros estilos arquitectónicos, REST es sencillo y no requiere capas adicionales para funcionar. 

El propósito principal de REST es permitir que los sistemas se comuniquen de manera simple y directa, usando operaciones básicas de HTTP. REST es un estándar para crear servicios web que sean escalables, mantenibles y que permitan la interoperabilidad entre sistemas.

### 2. Principios de REST
Para que una API sea considerada RESTful, debe cumplir con ciertos principios:

1. **Relación Cliente-Servidor**: El cliente y el servidor se comunican mediante solicitudes HTTP. El servidor proporciona recursos, mientras que el cliente los consume.
2. **Sin Estado**: En cada solicitud, el cliente envía toda la información necesaria, ya que el servidor no almacena el estado de ninguna solicitud anterior.
3. **Caché**: REST permite que las respuestas sean cacheadas para mejorar el rendimiento.
4. **Interfaz Uniforme**: Todas las interacciones con el servidor se realizan de la misma manera, facilitando la comprensión y el uso de la API.
5. **Sistema en Capas**: El diseño RESTful permite la introducción de capas entre el cliente y el servidor, como cachés, proxies y balanceadores de carga.

---

### 3. Operaciones CRUD en REST

Las APIs RESTful utilizan los métodos HTTP para realizar las operaciones CRUD (Crear, Leer, Actualizar, Eliminar):

| Operación | Método HTTP | Descripción                              |
|-----------|-------------|------------------------------------------|
| Crear     | POST        | Crear un nuevo recurso                  |
| Leer      | GET         | Obtener un recurso o una lista de ellos |
| Actualizar| PUT/PATCH   | Modificar un recurso existente          |
| Eliminar  | DELETE      | Borrar un recurso                       |

#### Ejemplos de CRUD en REST
Supongamos que estamos desarrollando una API para gestionar un sistema de **pedidos**.

1. **Crear un nuevo pedido** (POST):  
   - **Solicitud**:  
     ```http
     POST /api/pedidos
     ```
   - **Cuerpo**:
     ```json
     {
       "cliente": "Juan Pérez",
       "producto": "Laptop",
       "cantidad": 1
     }
     ```
   - **Respuesta**: Código 201 (Creado) con el nuevo recurso o referencia al recurso.

2. **Obtener un pedido específico** (GET):  
   - **Solicitud**:
     ```http
     GET /api/pedidos/1
     ```
   - **Respuesta**:
     ```json
     {
       "id": 1,
       "cliente": "Juan Pérez",
       "producto": "Laptop",
       "cantidad": 1
     }
     ```
   - Código de respuesta 200 (OK).

3. **Actualizar un pedido** (PUT):  
   - **Solicitud**:
     ```http
     PUT /api/pedidos/1
     ```
   - **Cuerpo**:
     ```json
     {
       "cantidad": 2
     }
     ```
   - **Respuesta**: Código 200 (OK) con el recurso actualizado o una confirmación de la operación.

4. **Eliminar un pedido** (DELETE):  
   - **Solicitud**:
     ```http
     DELETE /api/pedidos/1
     ```
   - **Respuesta**: Código 204 (No Content) si se eliminó exitosamente.

---

### 4. Buenas Prácticas para APIs RESTful

Una API bien diseñada no solo implementa correctamente los principios de REST, sino que también sigue buenas prácticas para hacer que sea más intuitiva, segura y fácil de mantener.

#### 4.1 Seguridad: Uso de SSL
Utilizar HTTPS es fundamental para proteger la comunicación entre cliente y servidor. SSL (Secure Sockets Layer) encripta los datos, impidiendo que sean interceptados.

#### 4.2 Documentación: OpenAPI
La documentación es clave para que los desarrolladores puedan entender cómo usar una API. **OpenAPI** es un estándar que facilita la generación de documentación legible y proporciona herramientas como **Swagger**, que permite interactuar con la API directamente desde la documentación.

#### 4.3 Versionado
A medida que una API evoluciona, es importante que los clientes sepan en qué versión están trabajando. Una práctica común es incluir la versión en la URL, como:
   ```http
   https://api.example.com/v1/pedidos
   ```

#### 4.4 URI’s Autodescriptivas
Las URI’s deben ser sencillas, claras y descriptivas, representando recursos en plural y evitando nombres de acciones. Ejemplos:
   - Correcto: `/api/usuarios`
   - Incorrecto: `/api/get-usuarios`

#### 4.5 Uso de ID’s Seguros
Para evitar que los usuarios puedan predecir o acceder a información de otros usuarios, los identificadores deben ser únicos y difíciles de deducir. En lugar de usar números incrementales como ID, se pueden usar UUIDs (Identificadores Únicos Universales).

#### 4.6 Control de Tamaño de Respuesta y Compresión
Es importante limitar la cantidad de datos que el servidor envía de vuelta al cliente. Usar compresión (gzip) y limitar la cantidad de datos ayuda a mejorar el rendimiento. 

#### 4.7 Gestión de Errores
Las respuestas de error deben ser claras y contener un mensaje detallado. Por ejemplo:
   - **Solicitud**:
     ```http
     GET /api/pedidos/999
     ```
   - **Respuesta**:
     ```json
     {
       "error": "No se encontró el pedido con ID 999",
       "code": 404
     }
     ```

---

### 5. Diseño de URI’s y Estructuras de Consulta

#### Jerarquía de Recursos en URI
Las URI’s deben reflejar la jerarquía de los recursos y deben utilizarse de manera consistente:
- **URI base**: `/api`
- **Recurso**: `/api/usuarios`
- **Sub-recurso**: `/api/usuarios/{id}/pedidos`

#### Filtrado y Búsqueda
Para búsquedas o filtros, usa parámetros de consulta:
   - **Ejemplo de Búsqueda**:
     ```http
     GET /api/pedidos?cliente=Juan
     ```
   - **Ejemplo de Filtrado**:
     ```http
     GET /api/pedidos?fecha=2024-10-31
     ```

#### Paginación y Ordenación
Para reducir la carga de datos, la API puede paginar las respuestas. Los parámetros comunes para la paginación son `page` y `size`:
   - **Ejemplo de Paginación**:
     ```http
     GET /api/pedidos?page=1&size=10
     ```

---

### 6. Buenas Prácticas para CRUD en REST
Las operaciones CRUD en REST no solo se asocian a los métodos HTTP correctos, sino que deben gestionarse de acuerdo con los principios de REST:

1. **POST** (Crear):  
   La creación de un recurso debe devolver el código **201 (Creado)**, junto con el recurso o una referencia al recurso creado.

2. **GET** (Leer):  
   La operación de lectura es idempotente (múltiples solicitudes darán el mismo resultado) y debe devolver un código **200 (OK)**.

3. **PUT** (Actualizar Total):  
   Usado para actualizar un recurso completo, PUT es idempotente y debe devolver **200 (OK)** o **204 (Sin Contenido)** si es exitoso.

4. **PATCH** (Actualizar Parcial):  
   PATCH es similar a PUT, pero solo modifica los campos especificados del recurso. También devuelve **200 (OK)** o **204 (Sin Contenido)**.

5. **DELETE** (Eliminar):  
   La operación de eliminación también es idempotente y generalmente devuelve **204 (Sin Contenido)** cuando el recurso se elimina.

---

### 7. Ejercicio Práctico

**Ejercicio**: Diseñar una serie de URI’s para un sistema de gestión de una biblioteca con los recursos: `libros`, `autores`, `temas`.
   
   - **Obtener todos los libros**:
     ```http
     GET /api/libros
     ```
   - **Buscar libros por autor**:
     ```http
     GET /api/libros?autor=Gabriel%20García%20Márquez
     ```
   - **Crear un nuevo autor**:
     ```http
     POST /api/autores
     ```
   - **Eliminar un tema de la biblioteca**:
     ```http
     DELETE /api/temas/novela
     ```

Con esta lección, los estudiantes tienen una introducción completa al diseño de una API RESTful, con principios básicos, buenas prácticas y ejemplos claros para aplicar el conocimiento.
