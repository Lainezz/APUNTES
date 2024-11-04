# Lección: Introducción al Servidor Web Apache

---

## 1. Introducción a Apache
Apache es uno de los servidores web más populares y ampliamente utilizados en todo el mundo. Su versatilidad, fiabilidad y compatibilidad con diversos sistemas operativos, como Linux y Windows, lo hacen una herramienta esencial para alojar sitios web y aplicaciones. Esta lección cubre las principales características de Apache, su estructura de carpetas y archivos de configuración, y cómo configurar un host virtual.

## 2. Principales Características de Apache

- **Software libre y de código abierto**: Apache es gratuito, lo que permite a los desarrolladores personalizar y modificar el código según sus necesidades.
- **Multiplataforma**: Funciona en varios sistemas operativos, incluyendo Linux, Windows, macOS y más.
- **Modularidad**: Apache utiliza módulos (como `mod_ssl`, `mod_rewrite`, `mod_php`) que permiten añadir funcionalidades según las necesidades del servidor, como soporte para PHP o SSL.
- **Soporte para múltiples protocolos**: Soporta HTTP/1.1, HTTPS (usando SSL/TLS), y otros protocolos web.
- **Compatibilidad con scripts**: Apache permite ejecutar scripts en lenguajes como PHP, Python, Perl, entre otros.
- **Alta configurabilidad**: Ofrece gran flexibilidad a través de sus archivos de configuración, permitiendo ajustar el servidor a distintos casos de uso.

---

## 3. Estructura de Carpetas Importantes en Apache

Al instalar Apache en Debian, las carpetas relacionadas se ubican principalmente en el directorio `/etc/apache2/`. A continuación, se describen las carpetas más importantes:

- **/etc/apache2/apache2.conf**: Este es el archivo de configuración principal de Apache. Contiene directivas generales para el comportamiento del servidor y carga de módulos.
  
- **/etc/apache2/sites-available/**: En esta carpeta se encuentran los archivos de configuración para cada sitio o dominio que el servidor puede gestionar, pero que aún no están habilitados.

- **/etc/apache2/sites-enabled/**: Esta carpeta contiene enlaces simbólicos a los archivos de configuración de sitios activos que están habilitados y actualmente gestionados por Apache. Los sitios habilitados aquí son los que Apache servirá a los clientes.

- **/etc/apache2/mods-available/**: Contiene los módulos disponibles que Apache puede cargar para añadir funcionalidad al servidor.

- **/etc/apache2/mods-enabled/**: Contiene enlaces simbólicos a los módulos que están activados y cargados por Apache.

- **/var/www/**: Este es el directorio donde Apache aloja los archivos del sitio web por defecto. El subdirectorio más común es `/var/www/html`, que es donde puedes colocar los archivos de tu sitio web.

---

## 4. Archivos de Configuración Importantes

- **apache2.conf**: Archivo principal que contiene configuraciones generales del servidor. Aquí puedes definir directivas globales como las políticas de acceso, límites de recursos, entre otras.

- **ports.conf**: Define en qué puertos Apache escucha las peticiones entrantes. Por defecto, escucha en el puerto 80 para HTTP y en el puerto 443 para HTTPS.

- **000-default.conf**: Es el archivo de configuración predeterminado para el sitio web por defecto. Este archivo se encuentra en `sites-available/` y está habilitado por defecto.

- **envvars**: Aquí se definen variables de entorno que afectan al comportamiento del servidor Apache, como el usuario y el grupo bajo los cuales el servidor Apache se ejecuta.

- **log files**: Apache mantiene registros de acceso y error. Los más importantes son:
  - **/var/log/apache2/access.log**: Registra todas las solicitudes que recibe Apache.
  - **/var/log/apache2/error.log**: Registra los errores generados por Apache.

---

## 5. Configuración de un Host Virtual en Apache

Un host virtual permite que un solo servidor Apache gestione múltiples sitios web. Cada sitio tiene su propia configuración, incluso si comparten el mismo servidor físico.

### Pasos para crear un Host Virtual

1. **Crear el directorio del sitio web**  
   Debes crear un directorio para tu sitio web en `/var/www/`. Supongamos que vas a alojar un sitio llamado `ejemplo.com`.
   ```bash
   sudo mkdir /var/www/ejemplo.com
   sudo chown -R $USER:$USER /var/www/ejemplo.com
   ```
2. **Crear un archivo de configuración de Host Virtual** Ahora crea un archivo de configuración en `sites-available/` para tu dominio. El archivo puede llamarse `ejemplo.com.conf`:

```bash
sudo nano /etc/apache2/sites-available/ejemplo.com.conf
```

Dentro de este archivo, debes agregar la configuración del host virtual:

```apache
<VirtualHost *:80>
    ServerAdmin admin@ejemplo.com
    ServerName ejemplo.com
    ServerAlias www.ejemplo.com
    DocumentRoot /var/www/ejemplo.com
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

- ServerAdmin: Dirección de correo del administrador.
- ServerName: El dominio principal del sitio.
- ServerAlias: Un alias para el dominio (por ejemplo, www.ejemplo.com).
- DocumentRoot: La ruta donde están los archivos del sitio web.
- ErrorLog y CustomLog: Definen dónde se guardan los registros de error y de acceso.

3. **Habilitar el nuevo sitio** Una vez que hayas creado el archivo de configuración, habilita el nuevo sitio con el comando:

```bash
sudo a2ensite ejemplo.com.conf
```

4. **Deshabilitar el sitio por defecto (opcional)** Si ya no deseas usar el sitio web predeterminado, puedes deshabilitarlo:

```bash
sudo a2dissite 000-default.conf
```

5. **Reiniciar Apache** Después de realizar estos cambios, reinicia Apache para aplicar la nueva configuración:

```bash
sudo systemctl restart apache2
```

6. **Configurar el archivo hosts (opcional para pruebas locales)** Si quieres probar el sitio localmente sin un dominio real, puedes modificar tu archivo `/etc/hosts` para hacer que el dominio ejemplo.com apunte a tu propia máquina.

```bash
sudo nano /etc/hosts
```

Agrega la siguiente línea:

```bash
127.0.0.1 ejemplo.com
```