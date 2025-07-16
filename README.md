# 🚀 Laboratorio Azure: Infraestructura con Servidor NFS

Este laboratorio guía la implementación de una arquitectura NFS en Azure, ideal para entornos educativos o pruebas de infraestructura. Aprenderás a configurar un servidor NFS en Linux, aplicar reglas de seguridad con NSGs y validar el acceso desde múltiples clientes en modo solo lectura.

---

## 🔍 Detalles del Lab

| 🧩 Elemento            | Descripción                        |
|------------------------|------------------------------------|
| 💻 Plataforma          | Azure                              |
| 📦 Tecnología          | NFS sobre Ubuntu                   |
| 🧠 Nivel               | Intermedio                         |
| ⏱️ Duración Estimada   | 60 minutos                         |
| 📁 Entregable          | Diagrama + Validación funcional    |

---

## 🎯 Objetivo

- Implementar **1 servidor Linux** que comparta aplicaciones vía NFS
- Conectar **múltiples VMs cliente Linux** en modo **solo lectura**
- Identificar componentes críticos y aplicar configuraciones de seguridad

---

## 📋 Requisitos Técnicos

### 🔧 Componentes Obligatorios

![Diagrama de referencia](https://raw.githubusercontent.com/jgaragorry/Labs-Azure-SysOps/main/assets/nfs-diagram.png)

**Leyenda del diagrama:**
- VNet con 2 subredes: `servidores (10.0.1.0/24)` y `clientes (10.0.2.0/24)`
- NSG-Servidores: Grupo de seguridad para servidor NFS
- NSG-Clientes: Grupo de seguridad para VMs cliente
- Servidor NFS: VM Ubuntu con disco de datos para `/apps`
- VMs Cliente: Al menos 2 VMs Linux montando el NFS

### 📐 Especificaciones Técnicas Mínimas

| Componente       | Configuración Mínima                  |
|:-----------------|:--------------------------------------|
| **Servidor NFS** | Ubuntu 20.04, 2 vCPU, 4GB RAM         |
| **VM Cliente**   | Ubuntu 18.04+, 1 vCPU, 2GB RAM        |
| **Discos**       | OS: 32GB (Premium SSD), Datos: 64GB+  |
| **Redes**        | VNet: 10.0.0.0/16, Subredes: /24      |

---

## 🛠️ Herramientas Necesarias

- [draw.io](https://app.diagrams.net/) para diseño de arquitectura
- [Portal Azure](https://portal.azure.com) o Azure CLI
- SSH: [PuTTY](https://www.putty.org/) o terminal integrada

---

## ✅ Tareas del Laboratorio

### 1️⃣ Diseño de la Arquitectura

- [ ] Crear VNet con 2 subredes: `servidores` y `clientes`
- [ ] Configurar servidor NFS con 2 discos (OS + Datos)
- [ ] Implementar al menos 2 VMs cliente
- [ ] Aplicar NSGs a cada subred
- [ ] Validar conexiones NFS (puerto 2049)
- [ ] Agregar leyendas explicativas en el diagrama

---

### 2️⃣ Configuración del Servidor NFS

#### 🔐 Reglas NSG mínimas

| Dirección | Prioridad | Nombre      | Protocolo | Puerto | Origen        |
|:----------|:----------|:------------|:----------|:-------|:--------------|
| Entrada   | 100       | Allow-SSH   | TCP       | 22     | Internet      |
| Entrada   | 110       | Allow-NFS   | TCP       | 2049   | 10.0.2.0/24   |
| Salida    | 100       | Allow-All   | *         | *      | Internet      |

#### 🧰 Comandos de configuración

```bash
# Crear directorio de aplicaciones
sudo mkdir /apps

# Instalar servidor NFS
sudo apt install nfs-kernel-server

# Configurar exportación en solo lectura
echo "/apps 10.0.2.0/24(ro,sync,no_subtree_check)" | sudo tee -a /etc/exports
sudo exportfs -arv
```

---

### 3️⃣ Validación Funcional

#### En cliente Linux:

```bash
# Montar NFS en modo solo lectura
sudo mount -t nfs -o ro <IP_NFS>:/apps /mnt/apps

# Pruebas
echo "test" > /mnt/apps/test.txt      # Debe FALLAR (solo lectura)
cat /mnt/apps/aplicacion.txt         # Debe funcionar (lectura)
```

---

## 📤 Entregables

- Enlace público al diagrama (usar función "Publicar" en draw.io)
- Explicación técnica en WhatsApp con el siguiente formato:

```text
[LAB NFS] - Tu Nombre
Diagrama: [URL]
Cumplimiento: 
- Componentes: 5/5 
- NSGs: Reglas NFS y SSH configuradas
- Dificultad: [Breve descripción]
Componente crítico: NSG por su rol en la seguridad
⏱️ Tiempo Estimado
⏰ 60 minutos (diseño + documentación)
```

---

## 🏆 Criterios de Evaluación

| Criterio               | Puntos |
|:-----------------------|:------:|
| Componentes mínimos    | 30%    |
| Reglas seguridad NSG   | 25%    |
| Claridad del diagrama  | 20%    |
| Explicación técnica    | 15%    |
| Originalidad           | 10%    |

---

## 💡 Tips Esenciales

```bash
# Probar conexión NFS desde cliente
showmount -e 10.0.1.4  # Reemplazar con IP del servidor

# Verificar montaje
df -hT | grep nfs

# Solución error "Access denied"
sudo chown nobody:nogroup /apps
```

---

## 📚 Recursos Adicionales

- [Configurar NFS en Ubuntu](https://ubuntu.com/server/docs/service-nfs)
- [Documentación Azure NSGs](https://learn.microsoft.com/en-us/azure/virtual-network/network-security-groups-overview)
- [Plantilla draw.io inicial](https://app.diagrams.net/)

---

## 📲 Instrucciones para Compartir

Exporta tu diagrama como PNG o comparte enlace público

Publica en el grupo de WhatsApp con este formato:

```text
[LAB NFS] - Tu Nombre
Diagrama: [ENLACE]
Explicación: 
• Cumplimiento: [X]/5 componentes
• Reglas NSG: [Sí/No]
• Dificultad: [Breve descripción]
⚠️ Fecha Límite: Domingo 23:59 PM
🏆 Reconocimiento: Los 3 mejores diseños serán destacados como "Azure Architects"!
```

---

## 📌 Ejemplo de Diagrama

📎 [Ejemplo de Diagrama en draw.io](https://raw.githubusercontent.com/jgaragorry/Labs-Azure-SysOps/main/assets/nfs-diagram.png)

---

## 💬 Nota Final

> ¡Creatividad vs funcionalidad! El equilibrio perfecto gana. ¿Quién será el top designer?
