# Instalación de JDK 21 LTS en AlmaLinux

Este documento describe el proceso completo y validado para instalar **OpenJDK 21 LTS** en una máquina AlmaLinux, siguiendo buenas prácticas de administración de sistemas.

---

# 1. Descargar JDK 21 LTS desde Adoptium
Las versiones de Adoptium utilizan nombres de archivo que cambian según el release, por lo que es necesario obtener la URL correcta desde la API oficial.

Ejemplo de URL obtenida desde:
```
https://api.adoptium.net/v3/assets/latest/21/hotspot?architecture=x64&image_type=jdk&os=linux
```

Archivo descargado:
```
OpenJDK21U-jdk_x64_linux_hotspot_21.0.9_10.tar.gz
```

Descarga realizada desde `/opt`:
```bash
cd /opt
sudo wget https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.9%2B10/OpenJDK21U-jdk_x64_linux_hotspot_21.0.9_10.tar.gz
```

---

# 2. Crear directorio estándar para instalaciones Java
```bash
sudo mkdir -p /usr/lib/jvm
```

---

# 3. Extraer el JDK en /usr/lib/jvm
```bash
sudo tar -xzf /opt/OpenJDK21U-jdk_x64_linux_hotspot_21.0.9_10.tar.gz -C /usr/lib/jvm
```

Verificar:
```bash
ls /usr/lib/jvm
```
Salida esperada:
```
jdk-21.0.9+10
```

---

# 4. Registrar JDK 21 en alternatives
```bash
sudo alternatives --install /usr/bin/java java /usr/lib/jvm/jdk-21.0.9+10/bin/java 1
sudo alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk-21.0.9+10/bin/javac 1
```

---

# 5. Seleccionar Java 21 como versión predeterminada
```bash
sudo alternatives --config java
```
Si solo aparece una versión:
- Basta con **presionar Enter**.

---

# 6. Validar instalación
```bash
java -version
```
Salida esperada:
```
openjdk version "21.0.9" ...
```

---

# 7. (Opcional) Configurar JAVA_HOME
Crear archivo de entorno:
```bash
sudo nano /etc/profile.d/jdk21.sh
```
Agregar:
```bash
export JAVA_HOME=/usr/lib/jvm/jdk-21.0.9+10
export PATH=$JAVA_HOME/bin:$PATH
```
Aplicar cambios:
```bash
source /etc/profile.d/jdk21.sh
```

---

# Estado Final
Con esto, el sistema AlmaLinux queda con **Java 21 LTS instalado, configurado y operativo**, listo para:
- Instalación de Tomcat
- Desarrollo y ejecución de aplicaciones Java
- Configuración de servicios en systemd

Indica si deseas continuar documentando el siguiente paso del laboratorio (creación del usuario de servicio para Tomcat).

