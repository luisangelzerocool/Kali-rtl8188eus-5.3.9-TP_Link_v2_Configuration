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
dentro copie: `deb http://http.kali.org/kali kali-rolling main non-free contrib`


2. Actualización del gestor de paquetes

```bash
apt update && apt upgrade
``` 
- Reinicie su computadora
- Verifique el Kernel: digite el comando: `uname -r`
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
```bash
17:55:59  Trying broadcast probe requests...
17:55:59  Injection is working!
17:56:01  Found 5 APs
17:56:01  Trying directed probe requests...
17:56:01  B2:E0:3C:BB:75:29 - channel: 1 - 'ALCATEL ONETOUCH POP C7'
17:56:02  Ping (min/avg/max): 2.615ms/41.339ms/152.858ms Power: -62.93
17:56:02  30/30: 100%
17:56:02  20:F3:A3:A0:85:78 - channel: 1 - 'LOS PROFE'
17:56:04  Ping (min/avg/max): 3.976ms/33.561ms/163.852ms Power: -80.50
17:56:04  24/30:  80%
17:56:04  0C:80:63:E9:92:2C - channel: 2 - 'HERRERA'
17:56:05  Ping (min/avg/max): 3.221ms/26.319ms/104.819ms Power: -69.93
17:56:05  30/30: 100%
17:56:05  D8:32:14:D2:51:70 - channel: 1 - 'CARM'
^C/ 9:  88%
``` 

al ejecutar el comando: `iwconfig` mostraría el mode igual a monitor:

```bash
root@kali:~# iwconfig
lo        no wireless extensions.

eth0      no wireless extensions.

wlan0     IEEE 802.11b  ESSID:""  Nickname:"<WIFI@REALTEK>"
          Mode:Monitor  Frequency:2.412 GHz  Access Point: Not-Associated   
          Sensitivity:0/0  
          Retry:off   RTS thr:off   Fragment thr:off
          Encryption key:off
          Power Management:off
          Link Quality=0/100  Signal level=-100 dBm  Noise level=0 dBm
          Rx invalid nwid:0  Rx invalid crypt:0  Rx invalid frag:0
          Tx excessive retries:0  Invalid misc:0   Missed beacon:0
```


## LICENSE

MIT License

Copyright (c) 2020 Luis Angel Vanegas

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
