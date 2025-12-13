# Instalación y Habilitación de Manager y Host-Manager en Tomcat 11

## 1. Crear usuarios y roles en `tomcat-users.xml`

Editar:

```
sudo nano /opt/tomcat/conf/tomcat-users.xml
```

Agregar:

```xml
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<role rolename="admin-script"/>
<user username="admin" password="S3crEt" roles="manager-gui,admin-gui"/>
```

---

## 2. Crear carpeta para contextos

```
- Estas carpetas hay que verificar por que tomcat 11 al parecer ya las trae
sudo mkdir -p /opt/tomcat/conf/Catalina/localhost/
```

---

## 3. Crear `manager.xml`

```
sudo nano /opt/tomcat/conf/Catalina/localhost/manager.xml
```

```xml
<Context privileged="true">
    <Valve className="org.apache.catalina.valves.RemoteAddrValve"
           allow="^.*$" />
</Context>

<Context privileged="true" antiResourceLocking="false"
         docBase="/opt/tomcat/webapps/manager" />
```

---

## 4. Crear `host-manager.xml`

```
sudo nano /opt/tomcat/conf/Catalina/localhost/host-manager.xml
```

```xml
<Context privileged="true">
    <Valve className="org.apache.catalina.valves.RemoteAddrValve"
           allow="^.*$" />
</Context>

<Context privileged="true" antiResourceLocking="false"
         docBase="/opt/tomcat/webapps/host-manager" />
```

---

## 5. Ajustar permisos

```
sudo chown -R tomcat:tomcat /opt/tomcat/conf/Catalina
sudo chmod -R 755 /opt/tomcat/conf/Catalina
```

---

## 6. Reiniciar Tomcat

```
sudo systemctl restart tomcat
sudo systemctl status tomcat
```

---

## 7. Verificación

-   Manager: `http://IP:8080/manager/html`
-   Host Manager: `http://IP:8080/host-manager/html`

Usuario: **admin**  
Password: **S3crEt**

## 8. Habilitar Acceso Externo en Firewall (OBLIGATORIO)

AlmaLinux bloquea por defecto todos los puertos excepto SSH.

Para permitir acceso desde tu máquina Windows al puerto 8080:

### 8.1 Verificar reglas actuales

```bash
sudo firewall-cmd --list-all
```

### 8.2 Abrir el puerto 8080

```bash
sudo firewall-cmd --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
```

### 8.3 Comprobar que está abierto

```bash
sudo firewall-cmd --list-ports
```

Debe aparecer:

```
8080/tcp
```

Ahora Windows ya puede acceder correctamente al Manager y Host-Manager.

---

## 9. Comprobaciones útiles

### 9.1 Ver si Tomcat escucha la red

```bash
sudo ss -tlnp | grep 8080
```

Debe mostrar:

```
0.0.0.0:8080
```

### 9.2 Probar ping desde Windows

```
ping IP_DEL_SERVIDOR
```

---

## 10. Resultado Final Esperado

✔ Tomcat Manager operativo  
✔ Host Manager operativo  
✔ Acceso remoto desde Windows  
✔ Roles y usuarios configurados  
✔ Firewall correctamente ajustado

---

Fin del documento.
