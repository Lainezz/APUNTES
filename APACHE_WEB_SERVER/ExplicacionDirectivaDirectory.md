
# Directiva `<Directory>` en Apache

La directiva **`<Directory>`** en Apache es una de las más utilizadas para definir configuraciones específicas de un directorio o conjunto de directorios. Permite establecer permisos, opciones y comportamientos para el acceso a los archivos que están dentro de un directorio del sistema de archivos. Su sintaxis es muy flexible y se utiliza para definir qué reglas y restricciones deben aplicarse cuando se accede a un directorio específico.

## Sintaxis básica

```bash
<Directory "/ruta/del/directorio">
    # Configuraciones y directivas
</Directory>
```

- **Ruta del directorio**: Se debe especificar la ruta completa del directorio en el servidor (por ejemplo, `/var/www/html`).
- Entre las etiquetas `<Directory>` y `</Directory>`, se colocan las directivas que aplicarán exclusivamente a ese directorio.

## Propiedades más importantes de la directiva `<Directory>`

### 1. **Options**
La directiva `Options` especifica qué características están habilitadas para el directorio en cuestión. Esta directiva puede aceptar múltiples valores que controlan diversas funcionalidades.

#### Valores comunes de `Options`:
- **Indexes**: Permite generar automáticamente una lista de los archivos del directorio si no existe un archivo `index.html` o equivalente.
   ```bash
   Options Indexes
   ```
   Si está activada y no hay un archivo de índice, el servidor mostrará el contenido del directorio en el navegador.

- **FollowSymLinks**: Permite que el servidor siga enlaces simbólicos (symlinks) presentes en el sistema de archivos.
   ```bash
   Options FollowSymLinks
   ```
   Con esta opción habilitada, el servidor tratará los enlaces simbólicos como si fueran archivos o directorios regulares.

- **ExecCGI**: Permite la ejecución de scripts CGI dentro del directorio.
   ```bash
   Options ExecCGI
   ```
   Esto es importante cuando tienes scripts que deben ejecutarse, como en formularios que devuelven respuestas dinámicas.

- **MultiViews**: Habilita la negociación de contenido, permitiendo al servidor entregar diferentes versiones de un archivo según las preferencias del cliente (idioma, tipo de archivo, etc.).
   ```bash
   Options MultiViews
   ```

Es posible combinar múltiples opciones en una sola línea:
```bash
Options Indexes FollowSymLinks ExecCGI
```

### 2. **AllowOverride**
`AllowOverride` define si las directivas contenidas en un archivo `.htaccess` ubicado en el directorio pueden sobrescribir la configuración definida en el archivo principal de configuración del servidor.

#### Valores comunes:
- **None**: No permite ninguna sobrescritura. El servidor ignorará cualquier archivo `.htaccess` en ese directorio.
   ```bash
   AllowOverride None
   ```
- **All**: Permite que todas las directivas del archivo `.htaccess` sobrescriban las directivas del servidor.
   ```bash
   AllowOverride All
   ```
- También es posible especificar qué tipos de directivas pueden sobrescribirse, como `AuthConfig` (para autenticación) o `FileInfo` (para configuraciones relacionadas con el manejo de archivos).
   ```bash
   AllowOverride AuthConfig FileInfo
   ```

### 3. **Require**
La directiva `Require` se utiliza para controlar el acceso al directorio en función de diversas condiciones, como el usuario, el grupo, o la IP de origen.

#### Ejemplos de `Require`:
- **Require all granted**: Permite el acceso a cualquier cliente.
   ```bash
   Require all granted
   ```
- **Require all denied**: Deniega el acceso a todos los clientes.
   ```bash
   Require all denied
   ```
- **Require ip [dirección IP]**: Limita el acceso al directorio solo a una dirección IP o a un rango de direcciones IP.
   ```bash
   Require ip 192.168.1.0/24
   ```
- **Require user [usuario]**: Restringe el acceso solo a usuarios autenticados. Este valor suele ir acompañado de un sistema de autenticación definido con otras directivas.
   ```bash
   Require user usuario1
   ```

### 4. **Order** (obsoleto en Apache 2.4)
La directiva `Order` se usaba en versiones anteriores de Apache (antes de la 2.4) para controlar el orden en que se aplicaban las reglas de `Allow` y `Deny`. Sin embargo, esta ha sido reemplazada por `Require` en Apache 2.4 y posteriores.

#### Ejemplo antiguo (para referencia):
```bash
Order allow,deny
Allow from all
Deny from 192.168.1.1
```

En Apache 2.4, la equivalencia sería:
```bash
Require all granted
Require not ip 192.168.1.1
```

### 5. **Deny/Allow**
Las directivas `Deny` y `Allow` permiten controlar el acceso de manera granular basándose en la dirección IP del cliente. Aunque han sido reemplazadas por `Require` en Apache 2.4, es común verlas en configuraciones heredadas.

- **Allow from [IP]**: Permite el acceso a los clientes desde una IP específica o rango de IPs.
   ```bash
   Allow from 192.168.1.0/24
   ```

- **Deny from [IP]**: Bloquea el acceso a clientes desde una IP específica.
   ```bash
   Deny from 192.168.1.1
   ```

## Ejemplo completo de `<Directory>`

```bash
<Directory "/var/www/html">
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

- Este ejemplo habilita la opción de listar los archivos del directorio (`Indexes`) y permite seguir enlaces simbólicos (`FollowSymLinks`).
- Se deshabilita el uso de archivos `.htaccess` (`AllowOverride None`).
- Se concede acceso a todos los clientes (`Require all granted`).

## Conclusión
La directiva `<Directory>` en Apache es clave para gestionar el acceso y las funcionalidades de los archivos ubicados en un directorio específico. Permite configurar la seguridad, el comportamiento y los permisos de los archivos del servidor, asegurando que se apliquen las políticas adecuadas a cada parte del sistema de archivos. El uso correcto de las propiedades como `Options`, `AllowOverride`, y `Require` permite un control detallado sobre cómo se manejan los recursos del servidor.
