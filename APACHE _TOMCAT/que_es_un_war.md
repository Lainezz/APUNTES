
# ¿Qué es un archivo .war?

Un archivo **.war** (Web Application Archive) es un formato de empaquetado utilizado para aplicaciones web basadas en Java. Es similar a un archivo **.jar**, pero está diseñado específicamente para aplicaciones web que se despliegan en servidores de aplicaciones como **Apache Tomcat**, **JBoss**, **WildFly**, entre otros.

El archivo **.war** contiene todos los recursos necesarios para que la aplicación web funcione en un servidor. Estos recursos incluyen:

## Contenido de un archivo .war:
1. **Archivos JSP** (JavaServer Pages): Archivos que contienen código Java embebido y se utilizan para generar contenido dinámico.
2. **Clases compiladas**: Archivos `.class` o paquetes `.jar` que contienen la lógica de negocio de la aplicación.
3. **Archivos estáticos**: Imágenes, CSS, JavaScript, etc.
4. **Archivos de configuración**:
   - **web.xml**: Archivo de descriptor de implementación ubicado en el directorio `WEB-INF`. Define configuraciones de la aplicación, como servlets, filtros, mapeos de URL, y más.
5. **Bibliotecas externas**: Dependencias externas necesarias para la ejecución, generalmente ubicadas en el subdirectorio `WEB-INF/lib`.
6. **Otros recursos**: Archivos de texto, propiedades, plantillas, etc.

## Estructura típica de un archivo .war:
```
miAplicacion.war
├── META-INF/
├── WEB-INF/
│   ├── web.xml
│   ├── lib/
│   │   └── dependencias.jar
│   └── clases/
│       └── paquetes/.class
├── index.jsp
├── estilos/
│   └── estilo.css
└── scripts/
    └── app.js
```

## Características principales:
- **Portabilidad**: Un archivo `.war` es un contenedor independiente que se puede desplegar en cualquier servidor compatible con el estándar de servlets y JSP.
- **Despliegue sencillo**: Se copia el archivo `.war` al directorio de despliegue del servidor de aplicaciones (generalmente `webapps` en Tomcat), y el servidor lo descomprime y configura automáticamente.
- **Escalabilidad**: Permite manejar aplicaciones web robustas y escalables en entornos empresariales.

## Diferencias entre un .jar y un .war:
| Característica    | .jar                          | .war                            |
|-------------------|-------------------------------|---------------------------------|
| **Propósito**      | Aplicaciones generales en Java | Aplicaciones web en Java         |
| **Contenido**      | Código compilado (.class) y recursos generales | JSP, servlets, web.xml, archivos estáticos, etc. |
| **Despliegue**     | Ejecución con `java -jar`      | Despliegue en un servidor de aplicaciones |

En resumen, un **.war** es el formato estándar para distribuir aplicaciones web basadas en Java, facilitando su despliegue y ejecución en servidores compatibles.
