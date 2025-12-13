# Laboratorio: Instalación y Configuración de Apache Tomcat en AlmaLinux

## 1. Preparación Inicial de la VM

-   **Sistema Operativo:** AlmaLinux
-   **Hipervisor:** VMware Workstation
-   **Objetivo:** Instalar y configurar Apache Tomcat en un entorno de laboratorio.

---

## 2. Verificación de Conexión a Internet

Antes de comenzar, verificamos que la VM tiene acceso a Internet:

```bash
ping -c 3 google.com
```

**Resultado esperado:** respuestas exitosas.

---

## 3. Limpieza y Actualización de Repositorios

Limpiamos la caché antigua y generamos una nueva:

```bash
sudo dnf clean all
sudo dnf makecache
```

**Descripción:**

-   `dnf clean all` elimina todos los metadatos locales.
-   `dnf makecache` descarga nuevamente la metadata de repos.

---

## 4. Actualización Completa del Sistema

Actualizamos todos los paquetes disponibles:

```bash
sudo dnf update -y
```

**Notas:**

-   Si se actualiza el kernel, será necesario reiniciar.

---

## 5. Reinicio del Sistema (si aplica)

```bash
sudo reboot
```

**Motivo:** Aplicar el nuevo kernel o servicios actualizados.

---

## 6. Comprobación Posterior a la Actualización

Verificamos que el sistema está actualizado:

```bash
rpm -qa --last
```

Ver versión actual del sistema:

```bash
cat /etc/almalinux-release
```

---

## 7. Activar Repositorio EPEL (opcional) Si planeas instalar Java, Tomcat u otras herramientas, conviene activar EPEL:

```bash
sudo dnf install epel-release -y
sudo dnf makecache
```

**Motivo:** Contar con paquetes adicionales útiles para entornos de laboratorio.

---
