# 🚀 Apache Tomcat Deployment on Linux (Production-Oriented Setup)

This project demonstrates the installation, configuration and deployment of a Java web application using Apache Tomcat on a Linux server.

It simulates a real-world application server environment with proper service configuration, deployment structure and basic security practices.


## 🎯 What this project demonstrates

- Installation and configuration of Apache Tomcat on Linux
- Deployment of a Java web application (.war)
- Configuration of systemd service for Tomcat
- Directory structure and permissions management
- Basic hardening and service exposure


## 🔧 Contenido del laboratorio

-   📘 [Preparación inicial de AlmaLinux](1-Instalacion-y-configuracion-almalinux.md)
-   ☕ [Instalación de Java 21](2-Instalacion_jdk_21_laboratorio.md)
-   👤 [Creación del usuario tomcat y estructura](3-Laboratorio_tomcat_usuario_estructura.md)
-   🚀 [Instalación de Apache Tomcat 11](4-Instalacion_tomcat_11.md)
-   🛠️ [Habilitación de Manager y Host-Manager](5-Tomcat_manager_hostmanager.md)
-   🌱 [Despliegue de aplicación Spring Boot en Tomcat](6-Documentacion_SpringBoot_Tomcat.md)
-   🔗 [Compartición de archivos Linux ↔ Windows](7-Unidades_compartidas_linux_windows.md)


## 🏗️ Application Flow

Client → Apache Tomcat (Java Application Server) → Web Application (WAR)


## ⚙️ Installation Overview

1. Install Java (OpenJDK)
2. Download and extract Apache Tomcat
3. Configure environment variables (JAVA_HOME)
4. Configure Tomcat users and roles
5. Deploy WAR file
6. Configure systemd service
7. Start and enable Tomcat service


## 📦 Application Deployment

The application is deployed as a `.war` file inside the Tomcat `webapps` directory.

Example:

/opt/tomcat/webapps/myapp.war

Tomcat automatically extracts and deploys the application.


## 🔐 Security Considerations

- Restricted access to Tomcat manager
- Use of dedicated service user
- Controlled permissions on Tomcat directories
- Firewall rules applied to expose only required ports

## 🧪 Validation & Testing

- Accessed application via browser: http://<server-ip>:8080
- Verified Tomcat service status
- Checked application logs inside /logs directory


## ☁️ Cloud Equivalent (Azure)

This architecture can be mapped to Azure using:

- Azure VM (Linux)
- Azure App Service (Java / Tomcat)
- Azure Application Gateway (reverse proxy)


## 💼 Skills Demonstrated

- Linux system administration
- Java application server management (Tomcat)
- Service configuration with systemd
- Web application deployment (WAR)
- Network and service exposure


## 🔄 Future Improvements

- HTTPS configuration (SSL/TLS)
- Reverse proxy with NGINX in front of Tomcat
- Deployment automation with Ansible
- CI/CD pipeline for WAR deployment

