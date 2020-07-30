# Kali Linux - Configuración de Driver TP_LINK
Contiene las instrucciones y archivos necesarios para la configuración de la tarjeta de red TP-LINK-TLWN722N v2/v3 (Realtek 802.11n NIC) en Kali Linux 2019.4

**Version**

* SO: Windows 10
* SO VM: Kali Linux 2.0 amd64 (www.kali.org/downloads/)
* VM: Oracle Virtual box 6.0.12 (www.virtualbox.org/)
* VM Extension Pack: VirtualBox 6.0.12 Oracle VM VirtualBox Extension Pack (www.virtualbox .org / wiki / Downloads)
* Núcleo de Kali Linux (Kernel): 5.2.0
* Adaptador USB: TP-LINK TL-WN722N versión 3 o 2 mismo chipset
* Driver: Realtek RTL8188EUS versión 2 chipset y Realtek RTL8188EUS versión 3 chipset

## Configuración del SO

1. Configurar el repositorio principal de Debian para Kali Linux

```bash
nano /etc/apt/sources.list
``` 
dentro copie: ´deb http://http.kali.org/kali kali-rolling main non-free contrib´


2. Actualización del gestor de paquetes

```bash
apt update && apt upgrade
``` 
- Reinicie su computadora
- Verifique el Kernel: digite el comando: ´uname -r´
En mi caso, el kernel es 5.2.0 pero antes es 4.9.0, por lo que el clon git de rtl8188eus no coincidió, por lo que debe hacer coincidir su KERNEL con el chipset rtl8188eus 
Si el Kernel y el chipset no coinciden, entonces obtendrá los siguientes errores en el Paso "5. iwconfig wlan0 mode monitor":
>(Error para inalámbrico solicitar "Modo de configuración" (8B06): SET falló en el dispositivo wlan0; Operación no permitida)
>(Error para la solicitud inalámbrica "Modo de configuración" (8B06): SET falló en el dispositivo wlan0; argumento no válido)
>(ERROR :: solicitud inalámbrica "Modo de configuración" (8B06):SET falló en el dispositivo (nombre del dispositivo); Operación no admitida )


3. Instalar linux-headers

```bash
apt install -y bc linux-headers-amd64
``` 
a veces ya se ha instalado porque ya se actualizó el gestor de paquetes en el Paso 2
>Si sale Error: "no se pudo encontrar el paquete bc" 
solo regresemos al paso 2 apt update && apt upgrade y asegúrese de tener acceso a internet para descargar todas las actualizaciones.


4. Use el controlador del Repo

```bash
cd rtl8188eus-5.3.9
echo "blacklist r8188eu" > /etc/modprobe.d/realtek.conf
make && make install
``` 
>Si sale error: no puede stat 8188eu.ko
Regrese al paso 2


5. Activar modo monitor

```bash
ifconfig wlan0 down
airmon-ng check kill
iwconfig wlan0 mode monitor
ifconfig wlan0 up
``` 
>en este punto si obtiene un error solo reinicie su computadora o maquina virtual, luego comience el paso 5, si el error vuelve nuevamente inicie en el paso 2 apt udate && upgrade para completar las actualizaciones que faltan.

6. Probar la Inyección de paquetes
```bash
aireplay-ng --test wlan0
``` 
Debería mostrar las redes así: 
