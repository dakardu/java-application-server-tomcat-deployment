# Instalación de Apache Tomcat 11 en AlmaLinux

Este documento describe el **Punto 3** del laboratorio: descarga, instalación y preparación de Apache Tomcat 11 en un entorno profesional utilizando AlmaLinux.

---

# 1. Descargar Apache Tomcat 11

Descargamos la versión estable actual (11.0.14) desde el repositorio oficial:

```bash
cd /opt
sudo wget https://downloads.apache.org/tomcat/tomcat-11/v11.0.14/bin/apache-tomcat-11.0.14.tar.gz
```

Verificar la descarga:

```bash
ls -l /opt/apache-tomcat-11.0.14.tar.gz
```

---

# 2. Crear la carpeta de instalación

Antes de extraer Tomcat, nos aseguramos que exista el directorio estándar:

```bash
sudo mkdir -p /opt/tomcat
```

---

# 3. Extraer Tomcat en /opt/tomcat

```bash
sudo tar -xzf apache-tomcat-11.0.14.tar.gz -C /opt/tomcat --strip-components=1
```

Explicación:

-   `--strip-components=1` elimina la carpeta raíz interna del .tar.gz y deja los archivos directamente en `/opt/tomcat`.

Verificar:

```bash
ls -l /opt/tomcat
```

Debe mostrar directorios como:

```
bin/  conf/  lib/  logs/  temp/  webapps/
```

---

# 4. Asignar permisos y propietario correctos

Tomcat debe ejecutarse con el usuario de servicio creado previamente:

```bash
sudo chown -R tomcat:tomcat /opt/tomcat
```

---

# 5. Hacer ejecutables los scripts `.sh`

Los scripts del directorio `bin` deben tener el permiso adecuado (`755`):

```bash
sudo chmod 755 /opt/tomcat/bin/*.sh
sudo bash -c 'chmod 755 /opt/tomcat/bin/*.sh'
```

Validar:

```bash
ls -l /opt/tomcat/bin/*.sh
sudo bash -c 'ls -l /opt/tomcat/bin/*.sh'
```

Salida esperada:

```
-rwxr-xr-x tomcat tomcat ... catalina.sh
-rwxr-xr-x tomcat tomcat ... startup.sh
-rwxr-xr-x tomcat tomcat ... shutdown.sh
...
```

---

Este documento corresponde al **Punto 3 del laboratorio**. El siguiente paso será la creación del servicio systemd.

# 6. Crear servicio systemd para Tomcat 11

Crear archivo del servicio:

```bash
sudo nano /etc/systemd/system/tomcat.service
```

Contenido:

```ini
[Unit]
Description=Apache Tomcat 11 Web Application Container
After=network.target

[Service]
Type=forking
User=tomcat
Group=tomcat
Environment="JAVA_HOME=/usr/lib/jvm/jdk-21.0.9+10"
Environment="CATALINA_HOME=/opt/tomcat"
Environment="CATALINA_BASE=/opt/tomcat"
Environment="CATALINA_PID=/opt/tomcat/temp/tomcat.pid"
ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Recargar systemd:

```bash
sudo systemctl daemon-reload
```

Iniciar Tomcat:

```bash
sudo systemctl start tomcat
```

Habilitar en el arranque:

```bash
sudo systemctl enable tomcat
```

Validar estado:

```bash
sudo systemctl status

Tras completar estos pasos:
- Tomcat 11 está correctamente instalado en `/opt/tomcat`.
- Los permisos de ejecución y propiedad son correctos.
- El entorno está listo para configurar el **servicio systemd**, que permitirá controlar Tomcat mediante:
  - `systemctl start tomcat`
  - `systemctl stop tomcat`
  - `systemctl enable tomcat`



```
