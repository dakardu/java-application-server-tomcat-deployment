# Laboratorio Tomcat – Creación de Usuario de Servicio y Estructura de Directorios

Este documento describe el **Punto 2** del laboratorio: la creación de un usuario dedicado para Tomcat y la estructura estándar de directorios para su despliegue profesional.

---

# 1. Crear Usuario y Grupo Tomcat
Para evitar riesgos de seguridad, Tomcat no debe ejecutarse como *root*. Se crea un usuario sin shell y sin directorio home.

```bash
sudo groupadd tomcat
sudo useradd -M -s /bin/nologin -g tomcat tomcat
```

Verificar:
```bash
id tomcat
```

Salida esperada:
```
uid=XXX(tomcat) gid=XXX(tomcat) groups=XXX(tomcat)
```

---

# 2. Crear Estructura de Directorios
Estructura estándar para un servidor Tomcat:

```
/opt/tomcat
/opt/tomcat/conf
/opt/tomcat/logs
/opt/tomcat/temp
/opt/tomcat/webapps
```

Crear directorios:
```bash
sudo mkdir -p /opt/tomcat/{conf,logs,temp,webapps}
```

---

# 3. Asignar Permisos Correctos
El usuario y grupo **tomcat** deben ser propietarios del directorio.

```bash
sudo chown -R tomcat:tomcat /opt/tomcat
sudo chmod -R 755 /opt/tomcat
```

Verificar:
```bash
ls -ld /opt/tomcat
ls -l /opt/tomcat
```

Salida esperada:
```
drwxr-xr-x tomcat tomcat /opt/tomcat
```
Y subdirectorios propiedad de `tomcat:tomcat`.

---

# Estado Final
El sistema queda preparado con:
- Usuario de servicio seguro para Tomcat
- Estructura profesional en `/opt/tomcat`
- Permisos correctos para instalación del servidor

Este documento deja el entorno listo para continuar con el **Punto 3: Instalación de Tomcat**.

Indica si deseas crear también el documento para la instalación de Tomcat 10 o Tomcat 11.