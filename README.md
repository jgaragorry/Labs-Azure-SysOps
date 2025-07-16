# 🚀 Laboratorio Azure: Infraestructura con Servidor NFS

## 🎯 Objetivo
Diseñar una arquitectura en Azure donde:
- **1 Servidor Linux** comparta aplicaciones vía NFS
- **Múltiples VMs cliente Linux** accedan en **solo lectura**
- Se identifiquen componentes críticos y configuraciones de seguridad

---

## 📋 Requisitos Técnicos

### 🔧 Componentes Obligatorios
![Diagrama de referencia](https://raw.githubusercontent.com/jgaragorry/Labs-Azure-SysOps/main/assets/nfs-diagram.png)

**Leyenda del diagrama:**
- **VNet** con 2 subredes: `servidores (10.0.1.0/24)` y `clientes (10.0.2.0/24)`
- **NSG-Servidores**: Grupo de seguridad para servidor NFS
- **NSG-Clientes**: Grupo de seguridad para VMs cliente
- **Servidor NFS**: VM Ubuntu con disco de datos para `/apps`
- **VMs Cliente**: Al menos 2 VMs Linux montando el NFS

### 📐 Especificaciones Técnicas Mínimas

| Componente       | Configuración Mínima                  |
|------------------|---------------------------------------|
| **Servidor NFS** | Ubuntu 20.04, 2 vCPU, 4GB RAM         |
| **VM Cliente**   | Ubuntu 18.04+, 1 vCPU, 2GB RAM        |
| **Discos**       | OS: 32GB (Premium SSD), Datos: 64GB+  |
| **Redes**        | VNet: 10.0.0.0/16, Subredes: /24      |

---

## 🛠️ Herramientas Necesarias

- **Diseño de diagramas**:  
  - [draw.io](https://app.diagrams.net/)  
  - Microsoft Visio

- **Gestión Azure**:  
  - [Portal Azure](https://portal.azure.com)  
  - Azure CLI (opcional)

- **Conexión SSH**:  
  - Windows: [PuTTY](https://www.putty.org/)  
  - Linux/macOS: Terminal integrada

---

## 📝 Fases del Laboratorio

### 1️⃣ Diseño de la Arquitectura

```markdown
[ ] Crear diagrama que incluya:
  - VNet con 2 subredes: `servidores` y `clientes`
  - Servidor NFS con 2 discos (OS + Datos)
  - Al menos 2 VMs cliente
  - NSGs aplicados a cada subred
  - Conexiones NFS (puerto 2049)
  - Leyendas explicativas para cada componente
```

---

### 2️⃣ Configuración Clave

#### 🔐 Reglas NSG mínimas para servidor NFS

```markdown
Dirección    Prioridad    Nombre       Protocolo    Puerto    Origen
Entrada      100          Allow-SSH    TCP          22        Internet
Entrada      110          Allow-NFS    TCP          2049      10.0.2.0/24
Salida       100          Allow-All    Cualquiera   *         Internet
```

#### 🧰 Comandos para configurar el servidor NFS

```bash
# Crear directorio apps
sudo mkdir /apps

# Instalar servidor NFS
sudo apt install nfs-kernel-server

# Configurar exportación (solo lectura)
echo "/apps 10.0.2.0/24(ro,sync,no_subtree_check)" | sudo tee -a /etc/exports
sudo exportfs -arv
```

---

### 3️⃣ Validación Funcional

```bash
# En cliente Linux:
sudo mount -t nfs -o ro <IP_NFS>:/apps /mnt/apps

# Pruebas:
echo "test" > /mnt/apps/test.txt      # Debe FALLAR (solo lectura)
cat /mnt/apps/aplicacion.txt         # Debe funcionar (lectura)
```

---

## 📤 Entregables

Enlace público al diagrama (usar función "Publicar" en draw.io)

Explicación técnica en WhatsApp con formato:

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
|------------------------|--------|
| Componentes mínimos    | 30%    |
| Reglas seguridad NSG   | 25%    |
| Claridad del diagrama  | 20%    |
| Explicación técnica    | 15%    |
| Originalidad           | 10%    |

---

## 💡 Tips Esenciales

```bash
# Comando para probar conexión NFS desde cliente:
showmount -e 10.0.1.4  # Reemplazar con IP del servidor

# Verificar montaje:
df -hT | grep nfs

# Solución error "Access denied":
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

![Ejemplo](https://raw.githubusercontent.com/jgaragorry/Labs-Azure-SysOps/main/assets/nfs-diagram.png)

---

## 💬 Nota Final

> ¡Creatividad vs funcionalidad! El equilibrio perfecto gana. ¿Quién será el top designer?
