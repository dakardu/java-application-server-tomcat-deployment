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
    ![Manager](/images/manager.png)

-   Host Manager: `http://IP:8080/host-manager/html`
    ![Host-Manager](/images/host-manager.png)

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

### 8.4 Acceso a la VM mediante Hyper-V NAT para exponer Tomcat (8080)

En entornos donde la máquina virtual usa una tarjeta Wi-Fi en el host, Hyper-V **no permite tráfico entrante** hacia la VM cuando se utiliza un conmutador externo.  
Esto impide acceder a servicios como **Tomcat (8080)** desde Windows.

Para solucionar esto, se configura una red **NAT interna**, que permite:

-   Acceder desde Windows (host) → VM
-   Permitir salida a Internet desde la VM
-   Mantener un entorno aislado y controlado
-   Evitar limitaciones propias del modo Wi-Fi en Hyper-V

---

### 8.4.1 Crear un conmutador virtual interno

Abrir **Administrador de conmutadores virtuales** en Hyper-V:

1. Seleccionar **Nuevo conmutador de red virtual**
2. Tipo de conexión → **Red interna**
3. Nombre recomendado:
    ```
    NAT-SWITCH
    ```
4. Guardar.

---

### 4.4.2 Conectar la VM al conmutador NAT

En los ajustes de la VM:

1. Configuración → **Adaptador de red**
2. Seleccionar:
    ```
    NAT-SWITCH
    ```
3. Aplicar.

---

### 8.4.3 Asignar IP al adaptador virtual en Windows (host)

1. Abrir **Panel de Control → Redes**
2. Buscar el adaptador:
    ```
    vEthernet (NAT-SWITCH)
    ```
3. Abrir **Propiedades → IPv4**
4. Configurar:

```
IP: 192.168.100.1
Máscara: 255.255.255.0
Gateway: (vacío)
DNS: 8.8.8.8
```

---

### 8.4.4 Crear la red NAT en Windows (PowerShell 5.1)

⚠ IMPORTANTE: No funciona en PowerShell 7.  
Debes abrir:

✔ **Windows PowerShell** (icono azul claro)

Ejecutar:

```powershell
New-NetNat -Name NAT_TOMCAT -InternalIPInterfaceAddressPrefix 192.168.100.0/24
```

---

### 8.4.5 Asignar IP estática dentro de AlmaLinux

Identificar la interfaz:

```bash
ip a
```

Configurar:

```bash
sudo nmcli con mod <INTERFAZ> ipv4.addresses 192.168.100.10/24
sudo nmcli con mod <INTERFAZ> ipv4.gateway 192.168.100.1
sudo nmcli con mod <INTERFAZ> ipv4.dns "8.8.8.8"
sudo nmcli con mod <INTERFAZ> ipv4.method manual
sudo nmcli con up <INTERFAZ>
```

Ejemplo:

```bash
sudo nmcli con mod eth0 ipv4.addresses 192.168.100.10/24
sudo nmcli con mod eth0 ipv4.gateway 192.168.100.1
sudo nmcli con mod eth0 ipv4.dns 8.8.8.8
sudo nmcli con mod eth0 ipv4.method manual
sudo nmcli con up eth0
```

---

# 🧪 6. Verificar conectividad

Desde Windows:

```cmd
ping 192.168.100.10
```

Desde la VM:

```bash
ping 8.8.8.8
curl google.com
```

---

# 🚀 7. Acceder a Tomcat desde Windows

```
http://192.168.100.10:8080
```

![Accedemos desde windows](/images/windows.png)

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

# 🧩 Resumen visual

```
Windows Host (NAT Gateway)
   IP: 192.168.100.1
        ↑ NAT
        ↓
VM AlmaLinux (Tomcat)
   IP: 192.168.100.10
   Puerto: 8080
```

✔ Acceso desde Windows → OK  
✔ Acceso a Internet desde VM → OK  
✔ Sin restricciones de Wi-Fi en Hyper-V
Ahora Windows ya puede acceder correctamente al Manager y Host-Manager.

## 10. Resultado Final Esperado

✔ Tomcat Manager operativo  
✔ Host Manager operativo  
✔ Acceso remoto desde Windows  
✔ Roles y usuarios configurados  
✔ Firewall correctamente ajustado

---
