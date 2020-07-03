
# ☁️ Skytap: Importación de máquinas virtuales Power☁️

En esta guía encontrará el paso a paso detallado para la importación de sus máquinas a Skytap usando FTP.

### Indice:
1. [Acceso a Skytap On IBM Cloud](#1-acceso-a-skytap-on-ibm-cloud)
2. [Preparación de máquinas virtuales para importar](#2-preparación-de-máquinas-virtuales-para-importar)
- [Máquinas virtuales basadas en VMware](#a-máquinas-virtuales-basadas-en-VMware)
- [Máquinas virtuales no basadas en VMware](#b-máquinas-virtuales-no-basadas-en-VMware)
3. [Crear un trabajo de importación en Skytap](#3-crear-un-trabajo-de-importación-en-Skytap)
4. [Cargar los archivos vía FTP](#4-cargar-los-archivos-vía-FTP)
5. [Inicio del proceso de análisis e importación](#5-inicio-del-proceso-de-análisis-e-importación)
6. [Notas](#Notas-)
7. [Referencias](#Referencias-)

## 1. Acceso a Skytap On IBM Cloud
Para acceder a sus recursos en Skytap, primero ingrese a su cuenta de [IBM cloud](https://cloud.ibm.com/login), en caso de no tener una cuenta [cree una de forma gratuita](https://cloud.ibm.com/registration) y proporcione a IBM el correo de su cuenta para que le conceda acceso a su servicio de Skytap.
Luego de esto ingrese en **Servicios** y en **Skytap On IBM Cloud**.

<p align="center">
<img width="956" alt="Skytapx" src="https://github.com/emeloibmco/Skytap-Importar-una-imagen/blob/master/Imagen1.png">
</p>

Finalmente presione el botón **Launch Skytap On IBM Cloud** que lo redirigirá a la interfaz de Skytap, donde se harán las configuraciones de los pasos siguientes.

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-una-imagen/blob/master/Imagen.png">
</p>

## 2. Preparar imagen de la máquina Power AIX 

Para poder exportar la imagen de una máquina AIX se deben seguir los siguientes pasos:



### Descargue y extraiga los archivos del repositorio con los Scripts necesarios para la exportacion:

```
https://github.com/skytap/aix-export
```

### Estos son los discos disponibles, se va a exportar hdisk0
```
lspv
```
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/1%20.png">
</p>

### Usamos el siguiente comando con hdisk1 como disco de destino
```
alt_disk_copy -d hdisk1
```
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/11%20.png">
</p>


### C
```
exportvg altinst_rootvg
```
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/2%20.png">
</p>

### Con el siguiente comando creamos un Volume Group para almacenar la imagen que se quiere exportar 
```
smit vg
```
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/3.png">
</p>
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/4.png">
</p>
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/5%20.png">
</p>

### Con los siguientes comandos podemos ver la capacidad total de almacenamiento de cada disco

**Disco 0:**

```
bootinfo -s hdisk0
```

**Disco 1:**

```
bootinfo -s hdisk1
```
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/6%20.png">
</p>

### Mediante la utilización del siguiente comando, asignamos un file system al disco de destino:

```
smit fs
```

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/7.png">
</p>

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/8%20.png">
</p>

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/9.png">
</p>

### Ahora debemos montar ese file System que creamos en el paso anterior en la ruta del disco de destino:
```
mount /skytapfs
```
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/10%20.png">
</p>

### verificamos que el disco este montado en la ruta indicada:
```
lsfs
```
### En este paso, debemos asegurarnos que la imagen creada tenga el mismo tamaño en espacio de almacenamiento a la maquina que se desea exportar.
```
df -m
```
### Mediante la utilización del siguiente comando exportamos la imagen del disco,en este proceso se crean dos archivos: un .ovf y un .img. 
```
./export_lpar.ksh hdisk0
```
### Teniendo los archivos .ovf y .img debemos crear uno nuevo que resulta de comprimir estos dos:
```
tar -cvf - archivo.ovf archivo.img | gzip> archivo.ova
``` 
**NOTA: Los archivos .ovf y .img son los que se han gererado anteriormente, mientras que el archivo .ova es el que deseamos.**

 
 ## 3. Crear un trabajo de importación en Skytap
 
 Para cada archivo cree un **trabajo de importación** dentro de Skytap mediante los siguientes pasos:
 
 * Ingrese en **Environments** ubicado en la barra superior, luego en **VM imports** de clic en el botón **Create VM import job**.
  
  
 <p align="center">
<img width="956" alt="Skytap2" src="https://github.com/emeloibmco/Skytap-Importar-una-imagen/blob/master/Imagen2.png">
</p>
 


 * Ingrese la información sobre su importación y al finalizar de clic en el botón **Save import job**.

 
 <p align="center">
<img width="929" alt="Skytap5" src="https://github.com/emeloibmco/Skytap-Importar-una-imagen/blob/master/Imagen3.png">
</p>

 .
 ## 4. Cargar los archivos vía FTP
 
 Para este paso necesitará un cliente FTP, le recomendamos el uso de [WinSCP](https://winscp.net/eng/index.php) para Microsoft.
 * En la interfaz de Skytap encontrará las credenciales para la conexión FTP.
 


<p align="center">
<img width="929" alt="skytap1" src="https://github.com/emeloibmco/Skytap-Importar-una-imagen/blob/master/Imagen4.png">
</p>
 
 
 * Ingrese en su cliente FTP con las credenciales proporcionadas por Skytap, ingrese en la carpeta **UPLOAD** y arrastre el archivo.

 <p align="center">
<img width="805" alt="skytap3" src="https://github.com/emeloibmco/Skytap-Importar-una-imagen/blob/master/Imagen5.png">
</p>
 

 
  ## 5. Inicio del proceso de análisis e importación
  
Una vez el proceso de carga del archivo en WinSCP termine haga clic en el botón **Create environment**, Skytap iniciará un proceso de análisis de la carpeta **UPLOAD** para luego importar las máquinas virtuales y crear un entorno virtual con su configuración de red.

<p align="center">
<img width="932" alt="Skytap6" src="https://github.com/emeloibmco/Skytap-Importar-una-imagen/blob/master/Imagen6.png">
</p>

Podrá encontrar su máquina en el panel principal.
<p align="center">
<img width="944" src="https://github.com/mariolarte19/Skytap-Importacion-de-maquinas-virtuales-/blob/master/Skytap9.PNG">
</p>

 
## Notas 📑
* Los archivos OVA requieren menos tiempo para cargarse.
* Skytap eliminará la información incluida en el elemento ExtraConfig durante el proceso de importación.

## Referencias 🔗

Encuentre más información sobre el proceso de importación de Skytap en: [Help Skytap](https://help.skytap.com/imports-using-the-vm-imports-page.html).

## Autores ✒️
Equipo IBM Cloud Tech sales Colombia.
