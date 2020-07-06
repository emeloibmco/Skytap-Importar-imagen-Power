
# ☁️ Skytap: Importación de máquinas virtuales Power☁️

En esta guía encontrará el paso a paso detallado para la importación de sus máquinas a Skytap usando FTP.

### Indice:
1. [Acceso a Skytap On IBM Cloud](#1-acceso-a-skytap-on-ibm-cloud)
2. [ Preparar imagen de la máquina Power AIX](#2-preparar-imagen-de-la-máquina-power-aix)
3. [Crear un trabajo de importación en Skytap](#3-crear-un-trabajo-de-importación-en-skytap)
4. [Cargar los archivos vía FTP](#4-cargar-los-archivos-vía-ftp)
5. [Inicio del proceso de análisis e importación](#5-inicio-del-proceso-de-análisis-e-importación)
6. [Notas](#notas-)
7. [Referencias](#referencias-)

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

Para poder exportar la imagen de una máquina AIX sifa los siguientes pasos:

* En la máquina a exportar ingrese a la ruta **/etc/security/limits**  y asegúrese de que la configuración sea la siguiente:
```
default:
         fsize = -1
 root:
         data = -1
         fsize = -1
```     

* Descargue y extraiga los archivos del repositorio con los scripts necesarios para la exportacion:

```
https://github.com/skytap/aix-export
```

* Liste los discos disponibles, para el caso de esta guía se va a exportar hdisk0, que es el disco de arranque identificado como rootvg.
```
lspv
```
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/1%20.png">
</p>

* Prepare el rootvg para exportar usando el siguiente comando, hdisk1 para esta guía será el disco de destino.
```
alt_disk_copy -d hdisk1
```
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/11%20.png">
</p>


* Prepare el disco para exportar
```
exportvg altinst_rootvg
```
<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/2%20.png">
</p>

A continuación encontrará los pasos para crear un Volume Group para almacenar la imagen que se quiere exportar. Al ingresar a los menús desplegados en los siguientes pasos, use las instrucciones que aparecen en la parte inferior del menú para moverse entre las opciones.

1. Corra el siquiente comando, el cual abrirá el menú mostrado. Selecicone la opción resaltada en la imagen.

```
smit vg
``` 

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/3.png">
</p>

 2.  En este nuevo menú selecicone nuevamente la opción resaltada en la imagen.

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/4.png">
</p>

 3.  Añada las caracteríticas del disco teniendo en cuenta que el tamaño de la partición debe ser mayor a la suma total del tamaño de los discos que van a ser exportados.

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/5%20.png">
</p>

* Con los siguientes comandos podemos ver la capacidad total de almacenamiento de cada disco.

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

A continuación encontrará los pasos para crear un file system, que será asignado al disco de destino.

```
smit fs
```

1. Corra el siquiente comando, el cual abrirá el menú mostrado. Selecicone la opción resaltada en la imagen.

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/7%20.png">
</p>

 2.  Añada las caracteríticas del sistema de archivos. Tenga en cuenta que el tamaño del file system debe ser mayor al tamaño total de los discos a exportar.

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/8%20.png">
</p>

* Si el proceso se completó satisfactoriamente encontrará el siguiente mensaje.

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/9.png">
</p>

* Ahora monte el file System que creado en el paso anterior en la ruta del disco de destino, tenga en cuenta que skytapfs es el nombre del file system que se especificó anteriormente.

```
mount /skytapfs
```

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/10%20.png">
</p>

* verifique que el disco este montado en la ruta indicada:

```
lsfs
```

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/12.PNG">
</p>

* En este paso, asegérese de que la imagen creada tenga el mismo tamaño en espacio de almacenamiento del total de los discos que se desean exportar.

```
df -m
```

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/13.PNG">
</p>

* Mediante la utilización del siguiente comando se exporta la imagen del disco, en este proceso se crean dos archivos: un .ovf y un .img. 

```
./export_lpar.ksh hdisk0
```

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/14.PNG">
</p>

* Comprima los archivos .ovf y .img en un .ova mediante el siguiente comando, teniendo en cuenta el nombre de los archivos mostrados en el paso anterior. 

```
tar -cvf - archivo.ovf archivo.img | gzip> archivo.ova
``` 

<p align="center">
<img width="956" alt="Skytapy" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/15.PNG">
</p>

Este archivo .ova es el archivo que deberá cargar mediante FTP a skytap. Se recomienda enviar este archivo por ssh a una máquina donde tenga acceso a herramientas de conexión FTP como winSCP o Filezilla.
 
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

 ## 4. Cargar los archivos vía SFTP
 
 Para este paso necesitará un cliente SFTP, le recomendamos el uso de [WinSCP](https://winscp.net/eng/index.php) para Microsoft y Filezilla para Ubuntu. 
 * En la interfaz de Skytap encontrará las credenciales para la conexión SFTP.

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
<img width="932" alt="Skytap6" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/16.PNG">
</p>

Podrá encontrar su máquina en el panel principal.
<p align="center">
<img width="944" src="https://github.com/emeloibmco/Skytap-Importar-imagen-Power/blob/master/17.PNG">
</p>

## Notas 📑
* Los archivos OVA requieren menos tiempo para cargarse.
* Skytap eliminará la información incluida en el elemento ExtraConfig durante el proceso de importación.

## Referencias 🔗

Encuentre más información sobre el proceso de importación de Skytap en: [Help Skytap](https://help.skytap.com/imports-using-the-vm-imports-page.html).

## Autores ✒️
Equipo IBM Cloud Tech sales Colombia.
