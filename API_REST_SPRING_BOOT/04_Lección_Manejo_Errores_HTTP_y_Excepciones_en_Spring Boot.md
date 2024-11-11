# Lección: Manejo de Errores HTTP y Excepciones en Spring Boot

### Introducción

En el desarrollo de una API REST, el manejo de errores es clave para proporcionar a los clientes mensajes de error consistentes y claros. Con Spring Boot, es posible configurar un sistema de manejo de excepciones centralizado mediante el uso de `@ControllerAdvice` y `@ExceptionHandler`. En este ejemplo, veremos cómo crear y organizar excepciones personalizadas para diferentes errores HTTP y cómo implementar un controlador de excepciones (`APIExceptionHandler`) para gestionar todos los errores de manera uniforme.

El proyecto en GitHub proporciona una serie de clases que representan diferentes tipos de excepciones HTTP y un controlador de excepciones. Vamos a profundizar en su propósito y estructura.

---

### 1. Centralización del Manejo de Errores con `APIExceptionHandler`

En Spring Boot, se puede crear un manejador de excepciones global utilizando `@ControllerAdvice`. Esta anotación permite interceptar excepciones lanzadas en cualquier punto de la aplicación y gestionarlas en una clase central, llamada `APIExceptionHandler`.

#### Clase `APIExceptionHandler`

La clase `APIExceptionHandler` es el núcleo del sistema de manejo de errores. Esta clase contiene métodos que manejan distintos tipos de excepciones, cada uno de los cuales está asociado a un tipo de respuesta HTTP específico.

- **Métodos de `APIExceptionHandler`**:
   - Cada método está anotado con `@ExceptionHandler` y maneja una excepción específica, como `BadRequestException`, `NotFoundException`, etc.
   - Cada método devuelve un `ErrorMessage`, que es un objeto de respuesta estructurado que contiene detalles sobre el error (tipo de excepción, mensaje y la ruta que causó el error).
   - La clase también incluye la anotación `@ResponseStatus` para devolver el código de estado HTTP adecuado para cada tipo de error.

````java
@ControllerAdvice
public class APIExceptionHandler {

    @ExceptionHandler(BadRequestException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ResponseBody
    public ErrorMessage handleBadRequest(HttpServletRequest request, Exception exception) {
        return new ErrorMessage(exception, request.getRequestURI(), "Bad Request");
    }

    @ExceptionHandler(NotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    @ResponseBody
    public ErrorMessage handleNotFound(HttpServletRequest request, Exception exception) {
        return new ErrorMessage(exception, request.getRequestURI(), "Not Found");
    }

    // Otros métodos para diferentes excepciones
}
````

---

### 2. Definición de Excepciones HTTP

En este proyecto, se han creado varias clases que representan diferentes tipos de excepciones HTTP, como `BadRequestException`, `UnauthorizedException`, `ForbiddenException`, y `ConflictException`. Estas clases permiten lanzar excepciones específicas que luego serán gestionadas por `APIExceptionHandler`.

#### Clases de Excepciones Específicas

1. **BadRequestException**: Se utiliza para manejar errores de solicitud malformada.
   - Clase base para excepciones como `MalformedHeaderException` y `FieldInvalidException`.
   - Esta clase incluye una descripción de error HTTP (400) y extiende de `RuntimeException`.
````java
public class BadRequestException extends RuntimeException {
    public static final String DESCRIPTION = "Bad Request Exception (400)";
    public BadRequestException(String detail) {
        super(DESCRIPTION + ". " + detail);
    }
}

````

2. **UnauthorizedException**: Se lanza cuando el usuario no está autenticado y se requiere autenticación.
   - Código de estado HTTP asociado: 401.
````java
public class UnauthorizedException extends RuntimeException {
    public static final String DESCRIPTION = "Unauthorized Exception (401)";
    public UnauthorizedException(String detail) {
        super(DESCRIPTION + ". " + detail);
    }
}
````
   
3. **ForbiddenException**: Señala que el usuario no tiene los permisos necesarios, a pesar de estar autenticado.
   - Código de estado HTTP asociado: 403.
````java
public class ForbiddenException extends RuntimeException {
    public static final String DESCRIPTION = "Forbidden Exception (403)";
    public ForbiddenException(String detail) {
        super(DESCRIPTION + ". " + detail);
    }
}
````

4. **NotFoundException**: Utilizada cuando el recurso solicitado no existe o no se encuentra.
   - Código de estado HTTP asociado: 404.
````java
public class NotFoundException extends RuntimeException {
    public static final String DESCRIPTION = "Not Found Exception (404)";
    public NotFoundException(String detail) {
        super(DESCRIPTION + ". " + detail);
    }
}
````

5. **ConflictException**: Se utiliza para errores que implican un conflicto, como duplicados en la base de datos.
   - Código de estado HTTP asociado: 409.
````java
public class ConflictException extends RuntimeException {
    public static final String DESCRIPTION = "Conflict Exception (409)";
    public ConflictException(String detail) {
        super(DESCRIPTION + ". " + detail);
    }
}
````

Cada una de estas clases incluye un mensaje de descripción, lo cual facilita que el manejador de excepciones asocie la excepción con el mensaje de error adecuado.

---

### 3. Jerarquía de Excepciones y Especialización

Para organizar los errores de manera más estructurada, el proyecto utiliza una jerarquía de excepciones. Por ejemplo, `BadRequestException` actúa como una clase base para otros errores más específicos relacionados con solicitudes incorrectas.

#### Ejemplos de Excepciones Especializadas

1. **MalformedHeaderException**: Indica que la cabecera de la solicitud está mal formada.
   - Hereda de `BadRequestException` y, por lo tanto, se gestiona como un error HTTP 400.
````java
public class MalformedHeaderException extends BadRequestException {
    public static final String DESCRIPTION = "Malformed Header Exception";
    public MalformedHeaderException(String detail) {
        super(DESCRIPTION + ". " + detail);
    }
}
````

2. **FieldInvalidException**: Se usa cuando un campo específico de la solicitud es inválido.
   - También hereda de `BadRequestException`, asegurando que se maneje como un error 400.
````java
public class FieldInvalidException extends BadRequestException {
    public static final String DESCRIPTION = "Field Invalid Exception";
    public FieldInvalidException(String detail) {
        super(DESCRIPTION + ". " + detail);
    }
}
````

3. **FieldAlreadyExistException**: Señala que un campo único, como un email, ya existe en la base de datos.
   - Hereda de `ConflictException`, asociándola al código de error 409.
````java
public class FieldAlreadyExistException extends ConflictException {
    public static final String DESCRIPTION = "Field Already Exists Exception";
    public FieldAlreadyExistException(String detail) {
        super(DESCRIPTION + ". " + detail);
    }
}

````

La jerarquía permite que, al lanzar una excepción específica, la aplicación devuelva el tipo de error HTTP adecuado. Esto simplifica el código del controlador y asegura respuestas coherentes.

---

### 4. Clase `ErrorMessage` para Respuestas de Error Uniformes

La clase `ErrorMessage` estructura las respuestas de error, proporcionando detalles que el cliente de la API puede utilizar para entender la causa del error. Esta clase es utilizada por `APIExceptionHandler` para devolver mensajes de error consistentes.

#### Estructura de `ErrorMessage`

````java
public class ErrorMessage {
    private String exception;
    private String message;
    private String path;

    public ErrorMessage(Exception exception, String path) {
        this.exception = exception.getClass().getSimpleName();
        this.message = exception.getMessage();
        this.path = path;
    }

    // Getters y setters
}

````

- **exception**: El tipo de excepción que ocurrió.
- **message**: El mensaje descriptivo del error.
- **path**: La URL donde ocurrió el error, útil para depuración.

Esta estructura asegura que todos los errores se envíen en el mismo formato, independientemente de su tipo o causa.

---

### Ejemplo Completo de `APIExceptionHandler` con Métodos para Todas las Excepciones

Aquí está un ejemplo completo de `APIExceptionHandler` que captura todas las excepciones definidas anteriormente y devuelve un `ErrorMessage` adecuado.

```java
@ControllerAdvice
public class APIExceptionHandler {

    @ExceptionHandler(BadRequestException.class)
    @ResponseStatus(HttpStatus.BAD_REQUEST)
    @ResponseBody
    public ErrorMessage handleBadRequest(HttpServletRequest request, Exception exception) {
        return new ErrorMessage(exception, request.getRequestURI());
    }

    @ExceptionHandler(NotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    @ResponseBody
    public ErrorMessage handleNotFound(HttpServletRequest request, Exception exception) {
        return new ErrorMessage(exception, request.getRequestURI());
    }

    @ExceptionHandler(ConflictException.class)
    @ResponseStatus(HttpStatus.CONFLICT)
    @ResponseBody
    public ErrorMessage handleConflict(HttpServletRequest request, Exception exception) {
        return new ErrorMessage(exception, request.getRequestURI());
    }

    @ExceptionHandler(UnauthorizedException.class)
    @ResponseStatus(HttpStatus.UNAUTHORIZED)
    @ResponseBody
    public ErrorMessage handleUnauthorized(HttpServletRequest request, Exception exception) {
        return new ErrorMessage(exception, request.getRequestURI());
    }

    @ExceptionHandler(ForbiddenException.class)
    @ResponseStatus(HttpStatus.FORBIDDEN)
    @ResponseBody
    public ErrorMessage handleForbidden(HttpServletRequest request, Exception exception) {
        return new ErrorMessage(exception, request.getRequestURI());
    }

    @ExceptionHandler(FatalErrorException.class)
    @ResponseStatus(HttpStatus.INTERNAL_SERVER_ERROR)
    @ResponseBody
    public ErrorMessage handleFatalError(HttpServletRequest request, Exception exception) {
        return new ErrorMessage(exception, request.getRequestURI());
    }
}

```

---

### Conclusión

Este sistema de manejo de errores centralizado permite que una API Spring Boot maneje y responda a errores de manera uniforme y coherente. Con `APIExceptionHandler`, todas las excepciones se gestionan en un solo lugar, y con la jerarquía de excepciones, se pueden personalizar las respuestas para cubrir una variedad de escenarios de error. Este enfoque asegura que el cliente reciba mensajes de error claros y específicos, mejorando la experiencia de uso y facilitando la depuración.