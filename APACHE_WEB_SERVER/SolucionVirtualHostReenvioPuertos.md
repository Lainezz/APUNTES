
# Configuración de Servidor Apache en VirtualBox con Redireccionamiento de Puertos

### Problema:
Estás intentando acceder a un servidor Apache configurado en una máquina virtual (VM) mediante Oracle VirtualBox. Usas una red NAT y realizas un redireccionamiento de puertos desde tu ordenador host a la VM. Sin embargo, no puedes acceder a los *virtual hosts* de Apache por nombres de dominio, ya que la resolución de estos nombres no está configurada para redirigir las solicitudes hacia la máquina virtual.

### Solución Paso a Paso:

### 1. Redireccionamiento de Puertos en VirtualBox
Asegúrate de que el redireccionamiento de puertos esté correctamente configurado en VirtualBox:
- **IP del host (tu ordenador):** 192.168.1.207 (La que tengas en tu ordenador)
- **Puerto del host (en tu ordenador):** 8000 (o cualquier otro puerto que elijas)
- **IP de la VM:** 10.0.2.15 (IP de tu máquina virtual)
- **Puerto de la VM:** 80 (donde corre Apache)

### 2. Modificar el Archivo `/etc/hosts` en tu Ordenador Local
Para que puedas acceder a los dominios configurados en Apache desde el navegador en tu ordenador, debes editar el archivo `hosts` de tu ordenador local. Este archivo redirige las peticiones a la IP correcta:

#### En Linux/macOS:
Edita el archivo `/etc/hosts` y agrega las siguientes líneas:

```
192.168.1.207   misitio.com
192.168.1.207   otrodominio.com
```

#### En Windows:
El archivo `hosts` se encuentra en `C:\Windows\System32\drivers\etc\hosts`. Ábrelo con permisos de administrador y agrega las mismas líneas:

```
192.168.1.207   misitio.com
192.168.1.207   otrodominio.com
```

Esto hará que las solicitudes a `misitio.com` y `otrodominio.com` se redirijan a la IP de tu ordenador (192.168.1.207), donde el redireccionamiento de puertos enviará las solicitudes a la VM.

### 3. Configuración de Virtual Hosts en Apache (en la VM)
En la máquina virtual, configura los *virtual hosts* de Apache con el `ServerName` adecuado:

#### Virtual Host para `misitio.com`
```apache
<VirtualHost *:80>
    ServerName misitio.com
    DocumentRoot /var/www/misitio
    ErrorLog ${APACHE_LOG_DIR}/misitio_error.log
    CustomLog ${APACHE_LOG_DIR}/misitio_access.log combined
</VirtualHost>
```

#### Virtual Host para `otrodominio.com`
```apache
<VirtualHost *:80>
    ServerName otrodominio.com
    DocumentRoot /var/www/otrodominio
    ErrorLog ${APACHE_LOG_DIR }/otrodominio_error.log
    CustomLog ${APACHE_LOG_DIR}/otrodominio_access.log combined
</VirtualHost>
```

Asegúrate de que los directorios `DocumentRoot` apunten al lugar correcto y los archivos del sitio estén allí.

### 4. Reiniciar Apache
Después de hacer cambios en la configuración de Apache, reinicia el servidor Apache para aplicar los cambios:

```bash
sudo systemctl restart apache2
# o
sudo systemctl restart httpd
```

### 5. Acceso desde el Navegador
Ahora puedes acceder a los sitios usando las siguientes URLs en tu navegador:
- `http://misitio.com:8000`
- `http://otrodominio.com:8000`

### Resumen:
- Modifica el archivo `hosts` de tu **ordenador local** para redirigir los dominios a la IP de tu ordenador.
- Configura el redireccionamiento de puertos correctamente en VirtualBox.
- Configura los *virtual hosts* en Apache en la máquina virtual.
- Usa las URLs correctas en el navegador con el puerto redirigido.

Siguiendo estos pasos, deberías poder acceder a los *virtual hosts* en tu servidor Apache dentro de la máquina virtual desde tu ordenador local.
