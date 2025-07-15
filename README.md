markdown
# 🚀 Laboratorio Azure: Infraestructura con Servidor NFS

## 🎯 Objetivo
Diseñar una arquitectura en Azure donde:
- **1 Servidor Linux** comparta aplicaciones via NFS
- **Múltiples VMs cliente Linux** accedan en **solo lectura**
- Identificar componentes críticos y configuraciones de seguridad

## 📋 Requisitos Técnicos
### Componentes obligatorios
```mermaid
graph TD
    A[VNet] --> B[Subred Servidores]
    A --> C[Subred Clientes]
    B --> D[NSG-Servidores]
    C --> E[NSG-Clientes]
    D --> F[Servidor NFS]
    E --> G[VM Cliente 1]
    E --> H[VM Cliente 2]
    F --> I[Disco OS]
    F --> J[Disco Datos]
    J --> K[/File System /apps]
Especificaciones técnicas
Componente	Configuración Mínima
Servidor NFS	Ubuntu 20.04, 2 vCPU, 4GB RAM
VM Cliente	Ubuntu 18.04+, 1 vCPU, 2GB RAM
Discos	OS: 32GB (Premium SSD), Datos: 64GB+
Redes	VNet: 10.0.0.0/16, Subredes: /24
🛠️ Herramientas Necesarias
Diseño de diagramas: draw.io (diagrams.net)

Gestión Azure: Portal Azure o Azure CLI

Conexión SSH: PuTTY (Windows) o Terminal (Linux/macOS)

📝 Fases del Laboratorio
1. Diseño de la Arquitectura (draw.io)
markdown
[ ] Crear diagrama que incluya:
  - VNet con 2 subredes: `servidores` y `clientes`
  - Servidor NFS con 2 discos (OS + Datos)
  - Al menos 2 VMs cliente
  - NSGs aplicados a cada subred
  - Flechas de conexión NFS (puerto 2049)
  - Leyendas explicativas para cada componente
2. Configuración Clave
Reglas NSG Mínimas:

Dirección	Prioridad	Nombre	Puerto	Origen/Destino
Entrada	100	Allow-SSH	22	Internet
Entrada	200	Allow-NFS	2049	VNet
Salida	100	Deny-All	*	Internet
Montaje en Clientes:

bash
sudo mount -t nfs -o ro <IP_NFS>:/apps /mnt/apps
3. Validación Funcional
markdown
[ ] Comprobar acceso solo lectura:
  touch /mnt/apps/test.txt  # Debe fallar
  cat /mnt/apps/app.txt     # Debe funcionar
📤 Entregables
Enlace público al diagrama (.drawio o PNG)
(Usar función "Exportar > Enlace público" en draw.io)

Explicación técnica (en WhatsApp):

markdown
Mi diseño cumple con:
✅ Aislamiento de subredes
✅ Reglas NSG específicas
✅ Acceso solo lectura verificado
El componente más crítico: [Explica tu elección]
⏱️ Tiempo Estimado
⏰ 60 minutos (diseño + documentación)

🏆 Criterios de Evaluación
Criterio	Puntos
Componentes mínimos	30%
Reglas seguridad NSG	25%
Claridad del diagrama	20%
Explicación técnica	15%
Originalidad	10%
💡 Tips Esenciales
bash
# Comando para probar conexión NFS:
showmount -e 10.0.1.4  # Reemplazar con IP del servidor

# Verificar montaje:
df -hT | grep nfs
📚 Recursos Adicionales
Configurar NFS en Ubuntu

Azure NSGs Docs

Plantilla draw.io inicial

📲 Cómo Compartir en WhatsApp
Exporta tu diagrama como PNG o comparte enlace público

Publica en el grupo con formato:

text
[LAB NFS] - Tu Nombre
Diagrama: [ENLACE/ARCHIVO]
Explicación: 
• Cumplimiento: [X]/5 componentes
• Reglas NSG: [Sí/No]
• Dificultad encontrada: [Breve descripción]
⚠️ Fecha Límite: Domingo 23:59 PM
🏆 Premio simbólico: Los 3 mejores diseños serán destacados como "Azure Architects" del mes!

text

## 📌 Ejemplo de Diagrama (Referencia Visual)
![Diagrama NFS Azure](https://i.imgur.com/Y7bklQp.png)

```markdown
💬 **Nota Final:**  
¡Creatividad vs funcionalidad! El equilibrio perfecto gana. ¿Quién será el top designer?
