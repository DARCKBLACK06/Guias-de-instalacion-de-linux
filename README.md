---
attachments: [Clipboard_2025-09-18-22-08-46.png, Clipboard_2025-09-18-22-12-09.png, Clipboard_2025-09-18-22-21-23.png, Clipboard_2025-09-18-22-30-41.png, Clipboard_2025-09-18-22-42-39.png, Clipboard_2025-09-18-22-44-06.png, Clipboard_2025-09-18-22-45-51.png, Clipboard_2025-09-18-22-47-09.png, Clipboard_2025-09-18-22-47-46.png, Clipboard_2025-09-18-22-51-01.png, Clipboard_2025-09-18-23-06-15.png, Clipboard_2025-09-18-23-06-51.png, Clipboard_2025-09-18-23-13-05.png, Clipboard_2025-09-18-23-14-25.png, Clipboard_2025-09-18-23-16-14.png, Clipboard_2025-09-18-23-17-05.png, Clipboard_2025-09-18-23-18-09.png, Clipboard_2025-09-18-23-18-51.png, Clipboard_2025-09-18-23-19-53.png, Clipboard_2025-09-18-23-21-57.png, Clipboard_2025-09-18-23-23-13.png, Clipboard_2025-09-18-23-24-53.png, Clipboard_2025-09-18-23-25-51.png, Clipboard_2025-09-18-23-26-57.png, Clipboard_2025-09-18-23-27-51.png, Clipboard_2025-09-18-23-30-22.png, Clipboard_2025-09-18-23-31-02.png, Clipboard_2025-09-18-23-31-51.png, Clipboard_2025-09-18-23-33-03.png, Clipboard_2025-09-18-23-33-36.png, Clipboard_2025-09-18-23-35-48.png, Clipboard_2025-09-18-23-36-45.png, Clipboard_2025-09-18-23-37-43.png, Clipboard_2025-09-18-23-38-11.png, Clipboard_2025-09-18-23-38-46.png, Clipboard_2025-09-18-23-40-22.png]
title: Manual para instalar ArchLinux Minimal y con grub detectando windows
created: '2025-09-19T03:05:54.500Z'
modified: '2025-09-19T04:53:36.987Z'
---

# Manual para instalar ArchLinux Minimal y con grub detectando windows 

Paso 1. Debemos de dirigirnos a la pagina oficial de ArchLinux https://lidsol.fi-b.unam.mx/archlinux/iso/2025.09.01/ y descargar la iso 

![FTP-ISO-DOWNLOAD](@attachment/Clipboard_2025-09-18-22-08-46.png)

Paso 2. Con una herramienta para crear usb booteable y cargar ArchLinux en la usb podemos usar cualquiera de estas herramientas:
* Ventoy
* Rufus
* BalenaEthcher
Paso 3. Particionamos el disco presionando la tecla de Win+X y seleccionando administrador de discos.
![Administracion de discos](@attachment/Clipboard_2025-09-18-22-12-09.png)
Paso 3.1  Seleccionamos el disco a particionar y damos click en reducir volumen y escribimos el tamaño a particionar puede ser desde 20G hasta la capacidad que deseemos para hacerlo se debe escribir siempre el numero de la sigueinte manera ("20000" para gigas y "2000" para Megas)Corregir si estoy mal.
![Administracion de discos-2](@attachment/Clipboard_2025-09-18-22-21-23.png)

Paso 4. Entramos a la bios de nuestra pc y desactivamos el secure boot para poder instalar un operativo distinto a windows (Hay diferentes formas de hacer esto lo recomendado es investigar como desactivar el secure boot para nuestro equipo).

Paso 5. Podemos cambiar el gestor de arranque en la bios para que al encender entre directamente en nuestra USB o tambien podemos seleccionar la unidad de arranque al momento de encender nuestra pc esto se puede lograr presionando la tecla de F9 con un equipo HP o Asus con F8 (Investigar segun el equipo)

(! Nota) Personalmente antes de configurar me gusta cambiar el idioma del teclado al que tenga el dispositivo ya que por defecto esta configurado en ingles si es en español se hace con el sigueinte comando.
 `````bash
 loadkeys la-latin1
 `````

Paso 6. Conectaremos nuestro equipo a la red para esto podemos usar Wifi o Ethernet dependiendo de lo que queramos usar usando Ethernet el equipo detectara automaticamente la red y se le asignara una ip podemos probar el acceso a internet haciendo ping a la direccion ip 8.8.8.8.

![Ping con Ethernet](@attachment/Clipboard_2025-09-18-22-30-41.png)

Paso 7. Conexion con wifi para esto debemos de usar unos pasos extra asi que usaremos "iwctl" para eso escribimos 
```````bash
iwctl (entrar en la herramient)
station list (Ver el nombre de la interfaz ejemplo wlan0).
station wlan0 scan
station wlan0 get-networks (escanear y ver redes disponibles)
station wlan0 connect NOMBRE_DE_LA_RED (Conectarnos a una red)
escribir contraseña
exit (Para salir de la herramienta).
```````

Paso 8. Crearemos las particiones usando la herramienta cfdisk para ello escribimos:
cfdisk y nos abrira esta ventana 
![cfdisk](@attachment/Clipboard_2025-09-18-22-42-39.png)
Con las flechas del teclado podemos movernos entre las opciones y seleccionamos y damos enter en la opcion new.
![New](@attachment/Clipboard_2025-09-18-22-44-06.png)

Nos pedira un tamaño para la particion por lo que escribiremos 1G y damos enter creando asi la particion de 1G para nuestro Grub.
![Part1](@attachment/Clipboard_2025-09-18-22-45-51.png)
Despues nos movemos a la opcion de type y buscaremos la opcion que diga Efi System esto para que sea compatible con la particion de windows y asi poder detectarla.
![Efi](@attachment/Clipboard_2025-09-18-22-47-09.png)
![Parti1Complete](@attachment/Clipboard_2025-09-18-22-47-46.png)

Crearemos nuestra particion raiz para ello seleccionamos donde dice free space y damos click en new, seleccionamos el espacio y la dejamos como se ha creado no modificamos en type, finalmente seleccionamos la opcion de write para guardar los cambios escribimos yes y con la opcion Quiet salimos de cfidisk.
![Final](@attachment/Clipboard_2025-09-18-22-51-01.png)
!(Nota) para comprobar que si se han creado las particiones podemos verificar con el comando lsblk
![Particiones](@attachment/Clipboard_2025-09-18-23-06-51.png)
Paso 10. Formatearemos las particiones que hemos creado anteriormente antes de instalar el sistema operativo para ello usaremos mkfs

```````bash
mkfs.fat -F 32 /dev/sda1 (Esta es para formatear la particion de Efi system comprobar la ruta antes de formatear)
```````
![Formatear Efi](@attachment/Clipboard_2025-09-18-23-13-05.png)

```````bash
mkfs.ext4 /dev/sda2 (Esta es para formatear la particion raiz donde instalaremos linux compronar la ruta antes de formatear)
```````
![Formatear raiz](@attachment/Clipboard_2025-09-18-23-14-25.png)
(!Nota) En ocasiones nos dira que la particion tiene informacion daremos click para continuar y seguir con la instalacion.

Paso 11. Usaremos la herramienta de ArchInstall para ello escribimos simplemente archinstall y nos abrira el sigueinte menu.
![ArchInstall](@attachment/Clipboard_2025-09-18-23-16-14.png)

Paso 12. Seleccionamos la primera opcion de lenguage y buscamos el idioma de preferencia.
![lenguaje](@attachment/Clipboard_2025-09-18-23-17-05.png)

Paso 13. En localidades damos click y en lenguaje de teclado buscamos el idioma de este si es en ingles podemos omitir o cambiarlo.
![Menu de localidades](@attachment/Clipboard_2025-09-18-23-18-51.png)
![teclado](@attachment/Clipboard_2025-09-18-23-18-09.png)

Paso 14. En idioma local podemos remplazar el ingles por el español o dejarlo segun nuestra preferencia.
![Idioma](@attachment/Clipboard_2025-09-18-23-19-53.png)

Paso 15. En espejos y repositorios podemos seleccionar la region de la cual se descargara la informacion para ello damos click en seleccione regiones y buscamos la de nuestro pais para seleccionar la opcion damos presionamos la tecla de espacio y enter.
![Region](@attachment/Clipboard_2025-09-18-23-21-57.png)

Paso 16. En configuracion del disco damos click y selccionamos la opcion de particionado, aqui seleccionamos particion manual.
![Particion Man](@attachment/Clipboard_2025-09-18-23-23-13.png)

Nos aparecera el disco y las particiones que este contiene (Como las imagenes son de una maquina virtual no existe otro sistema pero aqui tambien estaran las particiones disponibles), seleccionamos nuestro disco.
![Disco](@attachment/Clipboard_2025-09-18-23-24-53.png)

Seleccionamos la particion de 1G del grub y damos click en asignar punto de montaje.
![ASIG](@attachment/Clipboard_2025-09-18-23-25-51.png)
Escribimos /boot y damos enter para asignarlo.
```````bash
/boot
```````
![boot](@attachment/Clipboard_2025-09-18-23-26-57.png)

Seleccionamos la sigueinte particion y en punto de montaje escribimos unicamente " / " para indicar que es la raiz del sistema donde se instalara.
```````bash
/
```````
![/](@attachment/Clipboard_2025-09-18-23-27-51.png)

Finalmente nos quedara asi indicandonos cual es la particion de booteo y la particion raiz despues damos click en confirmar y salir para guardar los cambios.
![Fin part](@attachment/Clipboard_2025-09-18-23-30-22.png)

Paso 17. En gestor de arranque seleccionamos grub.
![Grub](@attachment/Clipboard_2025-09-18-23-31-02.png)

Paso 18. En nombre de host podemos dejarlo por defaulto o remplazarlo 
![host](@attachment/Clipboard_2025-09-18-23-31-51.png)

Paso 19. En autentication crearemos la contraseña root y el usuario para acceder antes de guardar el usuario no olvidar seleccionar la opcion si de superusuario, confirmamos y salimos.
![root](@attachment/Clipboard_2025-09-18-23-33-03.png)
![usuarios](@attachment/Clipboard_2025-09-18-23-33-36.png)

Paso 20. En perfil seleccionamo tipo seleccionamos el entorno grafico de nuestra preferencio o dejarlo en minimal sin entorno grafico (Si se tiene alguna configuracion dotfiles se recomienda instalar la version minial)
![Entorno](@attachment/Clipboard_2025-09-18-23-35-48.png)

Paso 21. En nucleos se recomienda marcar 2 opciones la predeterminada y la version de linux-lts este paso es opcional.
![Lts](@attachment/Clipboard_2025-09-18-23-36-45.png)

Paso 22. En configuracion de red podemos selecionar la manual o marcar la opcion de usar la configuracion de la instalacion este paso es opcional 
![red](@attachment/Clipboard_2025-09-18-23-37-43.png)

Paso 23. Finalmente damos click en instalar y damos click en si.
![Instalacion](@attachment/Clipboard_2025-09-18-23-38-11.png)
![SI](@attachment/Clipboard_2025-09-18-23-38-46.png)
![Instalacion3](@attachment/Clipboard_2025-09-18-23-40-22.png)

Paso 24. instalamos los siguientes paquetes.
```````bash
sudo pacman -S  bash-completion efibootmgr os-prober ntfs-3g nano fastfetch lolcat git iwd 
```````
Paso 25. Montamos la particion de windows para que pueda ser detectada por os-prober para ello ejecutamos los siguientes comandos.
```````bash
sudo mkdir -p /mnt/Win
sudo mount -t ntfs-3g /dev/partición de Windows-efi /mnt/win
```````
Paso 26. Editaremos el archivo de grub usando nano para ello ejecutamos:
```````bash
sudo nano /etc/default/grub
```````
y descomentamos la linea #GRUB_DISABLE_OS_PROBER=false presionamos Ctrl+o para guardar y Ctrl+x para salir.

Paso 27. Escribimos el siguiente comando para regenerar la configuracion del grub con la configuracion nueva.
```````bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```````

Con esto terminamos la instalacion de ArchLinux en dual boot con windows para salir escribimos exit y escribimos "shutdown now" para apagar el equipo y poder retirar la usb.

Video de instalacion de archlinux 
https://www.youtube.com/watch?v=wNGiNTkbe68

