
# Configuración de un Proxy Inverso con Balanceo de Carga utilizando Apache Web Server y Node.js

## Instrucciones para las pruebas de balanceo de carga

Una vez que hayas configurado el proxy inverso y el balanceador de carga, sigue estas instrucciones para realizar las pruebas:

### **1. Verificar la funcionalidad básica del proxy inverso**
   - Asegúrate de que puedes acceder a la aplicación Node.js a través del VirtualHost configurado con HTTPS.
   - Verifica que las peticiones HTTP son redirigidas correctamente al VirtualHost HTTPS.

### **2. Comprobar el balanceo de carga**
   - Asegúrate de que las solicitudes se distribuyen correctamente entre las instancias de la aplicación Node.js.
   - Puedes realizar esta verificación:
     - **Manual**: Configura las instancias de Node.js para devolver información sobre el ID o puerto de la instancia. Realiza múltiples solicitudes al servidor y verifica que las respuestas provienen de diferentes instancias.
     - **Automática**: Usa una herramienta como [ApacheBench (ab)](https://httpd.apache.org/docs/2.4/programs/ab.html) o [cURL](https://curl.se/) para generar múltiples solicitudes concurrentes. Ejemplo con ApacheBench:
       ```bash
       ab -n 100 -c 10 https://tu-dominio.com/
       ```
       Donde:
       - `-n 100`: Número total de solicitudes.
       - `-c 10`: Número de solicitudes concurrentes.

### **3. Realizar pruebas de carga**
   - Utiliza herramientas de prueba de carga para simular tráfico y evaluar el rendimiento del balanceador de carga. Algunas herramientas recomendadas:
     - **JMeter**: Configura un plan de pruebas para enviar múltiples solicitudes al servidor.
     - **Locust**: Configura scripts de carga para probar la capacidad del balanceador.
     - **Siege**:
       ```bash
       siege -c 20 -t 1M https://tu-dominio.com/
       ```
       Donde:
       - `-c 20`: Número de usuarios concurrentes.
       - `-t 1M`: Duración de la prueba (1 minuto).

### **4. Verificar la seguridad del sistema**
   - Asegúrate de que el servidor solo responde a solicitudes HTTPS.
   - Usa herramientas como [SSL Labs](https://www.ssllabs.com/ssltest/) para evaluar la configuración de seguridad SSL/TLS.
   - Comprueba que los encabezados HTTP de seguridad están correctamente configurados. Puedes usar herramientas como [Security Headers](https://securityheaders.com/) o cURL:
     ```bash
     curl -I https://tu-dominio.com/
     ```

### **5. Documentar los resultados**
   - Describe los pasos realizados en cada prueba y sus resultados.
   - Incluye capturas de pantalla o logs de las herramientas utilizadas.
   - Analiza los resultados, indicando si el balanceador de carga y la configuración cumplen con los objetivos esperados.

Estas pruebas serán parte de los criterios de evaluación del proyecto en el apartado **g) Pruebas de funcionamiento y rendimiento**.
