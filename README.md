# 🚀 Laboratorio Azure: Infraestructura con Servidor NFS

Este laboratorio guía el diseño de una arquitectura en Azure que permita compartir aplicaciones vía NFS desde un servidor Linux hacia múltiples clientes en modo solo lectura. El objetivo principal es que el participante proponga una solución técnica clara, segura y funcional mediante un diagrama bien estructurado.

---

## 🔍 Detalles del Lab

| 🧩 Elemento            | Descripción                        |
|------------------------|------------------------------------|
| 💻 Plataforma          | Azure                              |
| 📦 Tecnología          | NFS sobre Ubuntu                   |
| 🧠 Nivel               | Intermedio                         |
| ⏱️ Duración Estimada   | 60 minutos                         |
| 📁 Entregable          | Diagrama técnico + validación      |

---

## 🎯 Objetivo

Diseñar una arquitectura en Azure que permita compartir aplicaciones vía NFS desde un servidor Linux hacia múltiples clientes en modo solo lectura. El enfoque principal del laboratorio es:

- Crear un diagrama técnico que represente la infraestructura propuesta
- Identificar los componentes clave y sus configuraciones de red y seguridad
- Entregar el diseño como evidencia de comprensión arquitectónica

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

- Diagrama técnico publicado desde draw.io
- Validación funcional documentada (comandos y resultados)

---

## 🏆 Criterios de Evaluación

| Criterio               | Puntos |
|:-----------------------|:------:|
| Componentes mínimos    | 30%    |
| Seguridad aplicada     | 25%    |
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

## 📌 Ejemplo de Diagrama

📎 [Ejemplo de Diagrama en draw.io](https://raw.githubusercontent.com/jgaragorry/Labs-Azure-SysOps/main/assets/nfs-diagram.png)

---

## 💬 Nota Final

> ¡Creatividad vs funcionalidad! El equilibrio perfecto gana. ¿Quién será el top designer?
