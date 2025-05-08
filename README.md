---
title: "Administración de Redes Linux - Entregable 7: PXE"
---

## Administración de Redes Linux

# Entregable 7: PXE

<!-- markdownlint-disable MD053 -->
[![CC BY-SA 4.0][shield-cc-by-sa]][cc-by-sa]
[![GITT][shield-gitt]][gitt]
[![Administración de Redes Linux][shield-lna]][lna]

## Introducción

Este documento contiene el registro del desarrollo de la actividad, incluyendo
las instrucciones principales, las decisiones, y los resultados.

## 1. Configuración de un servidor para instalar Debian con configuración cero

### 1.1. Añadir interfaces en red interna.

Para este paso, al tratarse de una máquina virtual QEMU administrada desde
`virt-manager`, los pasos son distintos a los de una máquina administrada con
VirtualBox.

#### Creación de la red interna

El primer paso será crear una red interna, de nombre `virbr1` por ejemplo. Se
puede hacer usando `sudo virsh net-create --file <filename>`, donde
`<filename>` es el nombre de un archivo de configuración de red en XML. El
siguiente es un ejemplo de un archivo que crea esta red `virbr1`:

```xml
<network>
  <name>virbr1</name>
  <bridge name='virbr1' stp='on' delay='0'/>
</network>
```

#### Asignación de la red interna a la máquina virtual

Una vez creada la red interna, se puede asignar a la máquina virtual. Lo más
sencillo es hacerlo desde la interfaz gráfica de `virt-manager`. Desde el menú
'Details' de la máquina virtual, haciendo click en 'Add Hardware':

![Details menu in Virt-Manager](img/1.1.1-virt-manager-details.png)

En el menú emergente, seleccionamos 'Network' en el panel izquierdo y el nombre
de nuestra nueva red interna (`virbr1`) en el desplegable de 'Network source':

![Add Hardware menu in Virt-Manager](img/1.1.2-virt-manager-add-hardware.png)

En la opción 'Device model' podemos seleccionar 'Hypervisor default' o la
opción que se ajuste a nuestras necesidades. Para finalizar, hacemos click en
'Finish' para guardar los cambios.

> **Nota:** dado que no hemos configurado un servidor DHCP interno ni un
> router, la máquina no obtendrá una dirección IP automáticamente, ni podría
> acceder a Internet si su única interfaz estuviera conectada a esta red.

#### Creación de la Máquina Virtual cliente

Vamos a crear una máquina virtual nueva con las siguientes características:

* Nombre: linuxpxe
* Tipo: Linux
* Versión: Other Linux (64-bits)
* Tamaño de memoria: 1 GB
* Tamaño de almacenamiento: 8 GB
* Interfaz de red: `virbr1`

La forma más sencilla es a través de la interfaz gráfica de `virt-manager`.
Desde la pantalla principal, haciendo click en 'Create a new virtual machine',
avanzaremos por el asistente de creación de máquinas virtuales eligiendo las
opciones anteriores.

![Create a new VM in Virt-Manager (1/5)](img/1.1.3.1-new-mode.png)
![Create a new VM in Virt-Manager (2/5)](img/1.1.3.2-new-os.png)
![Create a new VM in Virt-Manager (3/5)](img/1.1.3.3-new-cpu-ram.png)
![Create a new VM in Virt-Manager (4/5)](img/1.1.3.4-new-disk.png)
![Create a new VM in Virt-Manager (5/5)](img/1.1.3.5-new-overview-network.png)

El menú 'Network selection' del último paso está oculto por defecto. Para
seleccionar una red diferente a la predeterminada, debemos hacer click en el
texto 'Network selection' para revelar la opción y seleccionar la red interna
en el desplegable.

[shield-cc-by-sa]: https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg
[shield-gitt]:     https://img.shields.io/badge/Degree-Telecommunication_Technologies_Engineering_|_UC3M-eee
[shield-lna]:       https://img.shields.io/badge/Course-Linux_Networks_Administration-eee

[cc-by-sa]: https://creativecommons.org/licenses/by-sa/4.0/
[gitt]:     https://uc3m.es/bachelor-degree/telecommunication
[lna]:       https://aplicaciones.uc3m.es/cpa/generaFicha?est=252&plan=445&asig=18467&idioma=2
