
# Actividad: Primeros pasos con Spring Boot - CRUD con Pokémon

**Objetivo:**  
Practicar los conceptos básicos de Spring Boot creando un API REST simple para gestionar una lista de objetos `Pokemon`. La aplicación permitirá realizar operaciones CRUD (Create, Read, Update, Delete) sobre estos objetos mediante un `@RestController`.

---

### Descripción

En esta actividad, crearás un proyecto en Spring Boot donde definirás un API REST para gestionar una lista de Pokémon. Implementarás un `@RestController` que contenga los métodos necesarios para agregar, leer, actualizar y eliminar Pokémon en una lista almacenada en memoria.

### Requisitos previos

- Tener instalado Java y Spring Boot.
- Familiaridad con anotaciones y estructuras básicas de Spring Boot.

### Especificaciones del proyecto

1. **Crear el proyecto Spring Boot**
   - Usa Spring Initializr o IntelliJ IDEA para generar un nuevo proyecto Spring Boot con las siguientes dependencias:
     - `Spring Web`
     - (Opcional) `Spring DevTools` para facilitar el desarrollo.

2. **Definir la clase `Pokemon`**
   - Crea una clase `Pokemon` en el paquete `model`.
   - Define los siguientes atributos:
     - `nPokedex` (Integer): Número de la Pokédex.
     - `nombre` (String): Nombre del Pokémon.
     - `tipo` (String): Tipo de Pokémon (por ejemplo, "Fuego", "Agua").
     - `vidaBase` (Integer): Puntos de vida base del Pokémon.
     - `ataques` (List<String>): Lista de ataques del Pokémon.
   - Implementa los métodos `getters`, `setters`, y un constructor para inicializar los campos.

3. **Implementar el `@RestController`**
   - Crea un controlador en el paquete `controller` llamado `PokemonController`.
   - Usa la anotación `@RestController` para definir este controlador.
   - Define una lista de Pokémon en memoria (por ejemplo, `List<Pokemon> pokemonList = new ArrayList<>();`) como base de datos temporal.

4. **Definir los endpoints REST**
   - Implementa los siguientes endpoints para realizar operaciones CRUD:

     - **Crear un nuevo Pokémon**
       - Endpoint: `POST /pokemon`
       - Método: `createPokemon`
       - Descripción: Recibe un objeto JSON con los datos del Pokémon y lo agrega a `pokemonList`.

     - **Obtener todos los Pokémon**
       - Endpoint: `GET /pokemon`
       - Método: `getAllPokemon`
       - Descripción: Retorna la lista completa de Pokémon.

     - **Obtener un Pokémon por su número en la Pokédex**
       - Endpoint: `GET /pokemon/{nPokedex}`
       - Método: `getPokemonById`
       - Descripción: Retorna un Pokémon específico basado en su `nPokedex`.

     - **Actualizar un Pokémon**
       - Endpoint: `PUT /pokemon/{nPokedex}`
       - Método: `updatePokemon`
       - Descripción: Recibe un objeto JSON con los datos actualizados del Pokémon y actualiza el Pokémon en `pokemonList`.

     - **Eliminar un Pokémon**
       - Endpoint: `DELETE /pokemon/{nPokedex}`
       - Método: `deletePokemon`
       - Descripción: Elimina un Pokémon de `pokemonList` según su `nPokedex`.

5. **Pruebas de la API**
   - Utiliza herramientas como Postman, Insomnia o `curl` para probar cada uno de los endpoints.
   - Asegúrate de que las operaciones CRUD funcionen correctamente.

---

### Ejemplo de JSON para pruebas

**Crear o actualizar un Pokémon**

```json
{
  "nPokedex": 25,
  "nombre": "Pikachu",
  "tipo": "Eléctrico",
  "vidaBase": 35,
  "ataques": ["Impactrueno", "Ataque Rápido", "Rayo"]
}
```

### Criterios de evaluación

- [ ] El proyecto Spring Boot se creó y configuró correctamente.
- [ ] La clase `Pokemon` tiene todos los atributos y métodos necesarios.
- [ ] El `@RestController` implementa correctamente los endpoints CRUD.
- [ ] La API REST responde correctamente a las solicitudes y modifica la lista de Pokémon.
- [ ] Los alumnos realizaron pruebas exitosas de todos los endpoints.

**Nota:** Esta actividad les ayudará a familiarizarse con la estructura de Spring Boot, las anotaciones de un controlador REST y la manipulación de datos en una API REST básica.
