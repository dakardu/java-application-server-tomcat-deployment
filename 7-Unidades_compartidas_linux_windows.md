# Guía de Unidades Compartidas entre Linux (AlmaLinux) y Windows 11

Este documento describe **todos los métodos disponibles** para compartir carpetas y archivos entre Windows 11 y AlmaLinux, especialmente en entornos virtualizados con **Hyper-V**.

Incluye:
- Compartición mediante **SMB/Samba** (acceso desde el Explorador de Windows).
- Acceso mediante **SFTP/SSH** (seguro y sin configurar Samba).
- Uso de **SSHFS-Win** para montar carpetas Linux en Windows como unidades.

---

# 1. Compartir Carpeta de Linux usando SMB (Samba)
Este método permite acceder a Linux desde Windows escribiendo:
```
\\IP_DE_LINUX\compartida
```

## 1.1 Crear Carpeta a Compartir
```bash
sudo mkdir -p /opt/compartida
sudo chmod 777 /opt/compartida
```

## 1.2 Instalar Samba
```bash
sudo dnf install -y samba samba-common samba-client
```

## 1.3 Crear Usuario Samba
```bash
sudo smbpasswd -a TU_USUARIO
```

## 1.4 Editar `/etc/samba/smb.conf`
Añadir al final:
```ini
[compartida]
   path = /opt/compartida
   browseable = yes
   writable = yes
   read only = no
   guest ok = no
   create mask = 0666
   directory mask = 0777
```

## 1.5 Activar y Reiniciar Servicios
```bash
sudo systemctl enable --now smb nmb
sudo systemctl restart smb nmb
```

## 1.6 Configurar Firewall
```bash
sudo firewall-cmd --permanent --add-service=samba
sudo firewall-cmd --reload
```

## 1.7 Acceder desde Windows
En el Explorador de Windows:
```
\\IP_DE_LA_VM\\compartida
```
Ejemplo:
```
\.168.1.50\\compartida
```

### 1.8 Habilitar Escritura desde Windows (Permisos SELinux + Linux)
En sistemas basados en RHEL/AlmaLinux, incluso con permisos 777, Samba **no puede escribir** en carpetas fuera de `/home` cuando SELinux está en modo *enforcing*.

#### 1.8.1 Verificar estado de SELinux
```bash
sestatus
```
Si aparece:
```
SELinux status: enabled
Current mode: enforcing
```
entonces SELinux está bloqueando la escritura.

#### 1.8.2 Permitir que Samba escriba en cualquier directorio
```bash
sudo setsebool -P samba_export_all_rw on
```

#### 1.8.3 Reiniciar Samba
```bash
sudo systemctl restart smb nmb
```

#### 1.8.4 Confirmar permisos correctos de la carpeta
```bash
sudo chmod -R 777 /opt/compartida
```
Con esto, Windows podrá **crear, modificar y eliminar** archivos dentro de `/opt/comparti
En el Explorador de Windows:
```
\\IP_DE_LA_VM\compartida
```
Ejemplo:
```
\\192.168.1.50\compartida
```

---

# 2. Compartir Archivos mediante SFTP (SSH)
Este método no requiere Samba y funciona incluso sin interfaz gráfica.

## 2.1 Verificar que SSH está Activo
```bash
sudo systemctl status sshd
```
Si no está activo:
```bash
sudo systemctl enable --now sshd
```

## 2.2 Acceder desde Windows con WinSCP
1. Abrir **WinSCP**.
2. Protocolo: **SFTP**.
3. IP del servidor Linux.
4. Usuario y contraseña.
5. Navegar hasta:
```
/opt/compartida
```

Listo. Transferencia segura y directa.

---

# 3. Montar una Carpeta Linux en Windows con SSHFS-Win
Permite montar carpetas vía SSH como si fueran unidades Windows.

## 3.1 Instalar WinFSP
https://github.com/winfsp/winfsp/releases

## 3.2 Instalar SSHFS-Win
https://github.com/billziss-gh/sshfs-win/releases

## 3.3 Acceder desde el Explorador
En la barra del explorador escribir:
```
\\sshfs\TU_USUARIO@IP\opt\compartida
```
Ejemplo:
```
\\sshfs\almalinux@192.168.1.50\opt\compartida
```

## 3.4 Opción GUI (recomendada)
Instalar:
https://github.com/billziss-gh/sshfs-win-manager

Permite montar `/opt/compartida` como unidad `Z:`.

---

# 4. Comparación de Métodos
| Método | Acceso desde Explorador | Seguridad | Complejidad |
|--------|--------------------------|-----------|-------------|
| **Samba (SMB)** | ✔ Sí | Buena | Media/Alta |
| **SSHFS-Win** | ✔ Sí | Muy Alta | Media |
| **SFTP (WinSCP)** | ✖ No | Muy Alta | Muy Baja |

---

# 5. Recomendación según Escenario
- **Laboratorio / Seguridad alta:** SFTP o SSHFS.
- **Compatibilidad con empresas / Windows Server:** Samba.
- **Necesitas montar una unidad Windows:** SSHFS-Win.

---

Documento completo para gestionar unidades compartidas entre AlmaLinux y Windows 11. Indica si deseas añadir capturas o más ejemplos.

