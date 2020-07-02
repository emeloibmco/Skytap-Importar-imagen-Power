
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

## 2. Preparación de máquinas virtuales para importar
¿Sus máquinas virtuales estan basadas en VMware? Si la respuesta es si, siga las instrucciones del numeral A, de lo contrario siga las instrucciones del numeral B.

### A. Máquinas virtuales basadas en VMware
Skytap admite archivos OVA (recomendado), paquetes OVF y archivos VMX. Lea detenidamente los pasos de importación según la herramienta que utilice. **Apague las máquinas virtuales antes de iniciar el proceso.**

VMware workstation:
* Seleccione **Archivo** > **Exportar a OVF**.
* Al seleccionar el nombre del archivo cambie la extensión a .ova y seleccione **Guardar**.

vSphere:
* Seleccione **Acciones** dentro del vSphere Web Client y elija una máquina virtual o vApp.
* Seleccione Exportar plantilla OVF, elija un nombre y ubicación de archivo.
* En el campo **Formato** seleccione preferiblemente el formato OVA.
* Si habilita el campo opciones avanzadas podrá incluir información adicional o configuraciones en la plantilla exportada. 
Si desea más información sobre importación de máquinas virtuales desde vSphere visite la documentación de [VMware vSphere](https://docs.vmware.com/en/VMware-vSphere/5.5/com.vmware.vsphere.vm_admin.doc/GUID-B05A4E9F-DD21-4397-95A1-00125AFDA9C8.html).

Fusion Pro:
* Elija la máquina virtual, seleccione **Archivo > Exportar a OVF** y elija un nombre y ubicación de archivo.
* Seleccione el formato del archivo, preferiblemente el formato OVA y haga clic en **Exportar**.

Si no puede crear archivos de tipo OVA es recomendado comprimir los archivos OVF / VMDK o VMX / VMDK en un archivo 7z, utilizando la herramienta [7-zip](https://www.7-zip.org/).

### B. Máquinas virtuales no basadas en VMware

Algunas imágenes OVA y OVF no están basadas en VMware y no pueden importarse directamente a Skytap. Por ejemplo, las máquinas virtuales VirtualBox no están basadas en VMware, ya que no se exportaron desde un hipervisor VMware (VMware Server, VMware Workstation, VMware ESX, etc.).

Para importar archivo .OVA y .OVF que no sean de VMware a Skytap, debemos convertir esta imagen, para hacerlo usando VMware vCenter Converter lea los sigueintes pasos:

* Si la VM es un archivo OVF, primero conviértalo al formato VMX. Para obtener instrucciones, consulte [Conversión de archivos OVF a VMX para usar con VMware Converter](https://help.skytap.com/Using_OVF_Converter_Tool.html).

* Descargue e instale VMware vCenter Converter en su máquina local, para la descarga lo puede hacer desde [VMware products](https://www.vmware.com/products/converter.html)

* Abra VMware vCenter Converter y haga clic en **Convertir máquina**.
* En **Tipo de origen**, seleccione **VMware Workstation u otra máquina virtual VMware**. Elija la ubicación del archivo VMX u OVA. Haga clic en **Siguiente**.
* En **Seleccionar tipo de destino**, seleccione **VMware Workstation u otra máquina virtual VMware**.
* En **Seleccionar producto VMware**, seleccione **VMware Workstation 11.0**.
* Ingrese un nombre para su VM en el campo **Nombre** y seleccione una ubicación de destino para el archivo convertido. Haga clic en **Siguiente**.
* Para aceptar las opciones predeterminadas, haga clic en **Siguiente**.
* En la página de resumen, verifique la configuración.
* Haga clic en **Finalizar** para comenzar el proceso de conversión. Dependiendo del tamaño de la VM, el proceso de conversión puede tardar más de una hora en completarse.
* **Importe la máquina virtual** convertida en Skytap, Este proceso lo puede realizar siguiendo esta guia siguiendola como si la imagen ya fuera VMware, o si desea información adicionar la puede consultar [aqui](https://help.skytap.com/importing-vms-overview.html).

 
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
