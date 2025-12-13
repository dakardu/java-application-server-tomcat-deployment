# Despliegue de una Aplicación Spring Boot (WAR) en Tomcat
**Sistema:** AlmaLinux  
**Java:** Temurin JDK 21  
**IDE:** Visual Studio Code  
**Servidor:** Apache Tomcat  
**Aplicación:** holaapp (Spring Boot)

---

## 📌 1. Instalación de JDK 21 en AlmaLinux

Se instaló Temurin JDK 21 manualmente:

```bash
java -version
```

Salida esperada:

```
openjdk version "21.0.9" 2025-10-21 LTS
OpenJDK Runtime Environment Temurin-21.0.9+10
OpenJDK 64-Bit Server VM Temurin-21.0.9+10
```

El JDK quedó instalado en:

```
/usr/lib/jvm/jdk-21.0.9+10
```

---

## 📌 2. Configuración de VS Code para JDK 21

En `settings.json` se agregó:

```json
"java.configuration.runtimes": [
  {
    "name": "JavaSE-21",
    "path": "/usr/lib/jvm/jdk-21.0.9+10",
    "default": true
  }
]
```

Después de reiniciar VS Code aparece:

```
Java: Ready (JDK 21)
```

---

## 📌 3. Instalación de Maven en AlmaLinux

```bash
sudo dnf install maven -y
```

Verificación:

```bash
mvn -version
```

---

## 📌 4. Crear Proyecto Spring Boot en VS Code

Desde la paleta de comandos (`Ctrl + Shift + P`):

```
Spring Initializr: Create a Maven Project
```

Parámetros elegidos:

| Parámetro          | Valor               |
|-------------------|---------------------|
| Lenguaje          | Java                |
| Versión Boot      | 3.5.8               |
| Artifact Id       | holaapp             |
| Group Id          | com.demo            |
| Packaging         | WAR                 |
| Java Version      | 21                  |
| Dependencias      | Spring Web          |

---

## 📌 5. Estructura del proyecto

```
holaapp/
 ├── src/main/java/com/demo/holaapp/
 │    ├── HolaController.java
 │    └── DemoApplication.java
 ├── src/main/resources/
 ├── pom.xml
 ├── target/
```

---

## 📌 6. Controlador REST `/hola`

Archivo: `src/main/java/com/demo/holaapp/HolaController.java`

```java
package com.demo.holaapp;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HolaController {

    @GetMapping("/hola")
    public String hola() {
        return "Hola, desde Spring Boot en tomcat!";
    }
}
```

---

## 📌 7. POM configurado para WAR + Tomcat externo

Archivo `pom.xml`:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

  <modelVersion>4.0.0</modelVersion>

  <groupId>com.demo</groupId>
  <artifactId>holaapp</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>

  <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>3.5.8</version>
  </parent>

  <properties>
    <java.version>21</java.version>
  </properties>

  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <scope>provided</scope>
    </dependency>
  </dependencies>

  <build>
    <finalName>holaapp</finalName>
  </build>

</project>
```

---

## 📌 8. Compilar y generar el archivo WAR

Ejecutar desde la carpeta del proyecto:

```bash
mvn clean package
```

Resultado:

```
BUILD SUCCESS
```

Archivo generado:

```
target/holaapp-0.0.1-SNAPSHOT.war
```

---

## 📌 9. Desplegar WAR en Tomcat

Copiar el WAR:

```bash
sudo cp target/holaapp-0.0.1-SNAPSHOT.war /opt/tomcat/webapps/holaapp.war
```

Reiniciar Tomcat:

```bash
sudo systemctl restart tomcat
```

o

```bash
/opt/tomcat/bin/shutdown.sh
/opt/tomcat/bin/startup.sh
```

---

## 📌 10. Probar la aplicación

Abrir en navegador:

```
http://TU_SERVIDOR:8080/holaapp/hola
```

Salida esperada:

```
¡Hola, desde Spring Boot en tomcat!
```

---

## ✔️ Resultado final

Tu entorno ya está completamente operativo:

- VS Code configurado con JDK 21  
- Maven funcionando  
- Spring Boot funcionando en modo WAR  
- Despliegue exitoso en Tomcat  
- Endpoint `/hola` accesible  

---

# 🎉 ¡Aplicación desplegada con éxito!
