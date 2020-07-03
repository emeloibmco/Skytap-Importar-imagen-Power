
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

### C
```
lspv
```
### C
```
alt_disk_copy -d hdisk1
```
### C
```
exportvg altinst_rootvg
```
### C
```
lspv
```
### C
```
smit vg
```
### C
```
bootinfo -s hdisk0
```
### C
```
bootinfo -s hdisk1
```
### C
```
smit fs
```
### C
```
mount /skytapfs
```
### C
```
lsfs
```
### C
```
df -m
```
### C
```
./export_lpar.ksh hdisk0
```
 
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
