# Installation d'un environnement de developpement web virtuel :
Le principe de cet installation va nous permettre de créer, modifier et executer du code sans installer directement sur notre machine les différents packages/serveur web etc.

## Résumer de ce que nous allons mettre en place :
Sur notre PC, nous allons installer les logiciels VirtualBox et VSCode. VirtualBox permettra de faire tourner une machine virtuelle ubuntu, cette dernière 
accueillera tout les packages de developpement et serveur web. VSCode nous permettra de modifier/créer et executer le code situé sur l'Ubuntu à partir de 
notre PC. Sur la VM, nous allons installer un serveur SSH, Apache2, PHP et mysql.</br>
Avant de faire la configuration système, nous testerons la connectivité entre l'hôte et la VM.

## 1ère étape, sur l'hôte, installer VirtualBox :
Pour télécharger Vbox, il suffit de se rendre sur la page officiel et de télécharger l'installateur **ET** le pack d'extension (ce pack permet d'utiliser certaines 
fonctionnalités à partir des VM) :</br>
<https://www.virtualbox.org/wiki/Downloads></br>
![Pic1](img/1.PNG?raw=true)

Pour installer Vbox, il suffit d'exécuter l'exécutable, c'est une installation classique. Pour installer le pack d'extension, dans VirtualBox
il faut cliquer sur **Fichier**, **Paramètres**, **Extensions**, cliquer sur le **+** et ajouter l'extension pack :</br>
![Pic2](img/2.PNG?raw=true)</br>
![Pic3](img/3.PNG?raw=true)

## 2ème étape, sur l'hôte, installer VSCode et son module SSH :
Pour télécharger VSCode, il suffit de se rendre sur la page officiel et de télécharger l'installateur :</br>
<https://code.visualstudio.com/download></br>
Pour l'installer, idem que Vbox, cliquer sur l'installateur et installer. Pour installer le module SSH, il faut ouvrir VSCode et cliquer sur 
**Extensions**, inscrire dans la barre de recherche **SSH** et installer **Remote - SSH** :</br>
![Pic4](img/4.PNG?raw=true)

Nous verrons plus tard comment se connecter en SSH à la VM.

## 3ème étape, sur l'hôte, installation de la VM :
Pour installer la VM, nous nous rendons sur le site officiel et téléchargeons le fichier .iso :</br>
<https://ubuntu.com/download/desktop></br>
Maintenant que nous avons le fichier iso, nous allons ouvrir VirtualBox et ajouter une VM, pour cela, cliquer sur **Nouvelle**, en **nom** : **Ubuntu - dev**, **Type** : **Linux**, 
**Version** : **Ubuntu (64-bit)**, la taille de mémoire en **2048** (cela représente 2 GO alloué, ici tout dépend du type de dev que l'on souhaite faire),
on décide de **Créer un disque dur virtuel maintenant**. On laisse le type de fichier en **VDI (VirtualBox Disk Image)**, le stockage idem on laisse en 
**Dynamiquement alloué** (si on alloue 50Go à la VM et qu'elle utilise seulement 5 Go, elle ne prendra pas les 45Go restants pour elle). En emplace du fichier et 
taille nous laissons par défaut les valeurs (10Go me suffit emplement pour moi). </br>
![Pic5](img/5.PNG?raw=true)
</br>
Et voila notre VM est créée, lorsque l'on va la démarrer (**démarrage**, **démarrage normal**) nous aurons une pop up pour aller chercher l'iso d'Ubuntu. Après cela,
il faut simplement installer Ubuntu (les tuto sont légions sur l'internet).</br>
![Pic6](img/6.PNG?raw=true)

## 4ème étape, sur la vm, configuration Ubuntu :
Sur mon Ubuntu, j'ai un utilisateur test/test et root/toor (pour changer le mdp de root, c'est ci-dessous), nous allons installer les packages nécéssaires pour le SSH et LAMP (également des modules de PHP) :
```bash
sudo apt install -y openssh-server apache2 php php-mysql php-curl php-gd php-intl php-json php-mbstring php-xml php-zip mysql-server
sudo apt install -y phpmyadmin
sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config # allow root en ssh
```
Pour modifier le mot de passe de root :
```bash
$ sudo /bin/bash
$ passwd
```
Maintenant que Ubuntu est configuré, il faut faire la config réseau et la config VSCode.

## 5ème étape, sur l'hôte, réseau privé pour l'hôte et la vm :
Nous allons créer un réseau seulement pour l'hôte et la vm, de ce fait, seul notre PC aura accès à la VM.</br>
Pour ce faire, après avoir VirtualBox, cliquer sur **Fichier**, **Gestionnaire de réseau hôte**, **Créer**, adresse IPv4 : **172.16.0.1**, puis dans l'onglet **Serveur DHCP**, mettre le serveur en .100 et 2 à 10 en adresses distribuées :</br>
![Pic7](img/7.PNG?raw=true)</br>
![Pic8](img/8.PNG?raw=true)</br>
![Pic9](img/9.PNG?raw=true)</br>
![Pic10](img/10.PNG?raw=true)
</br>
Maintenant, il faut associer la VM à ce réseau, clique droit sur la VM, **Configurations**, **Réseau**, **Mode d'accès réseau** : **Réseau privé hôte**, en Nom il faut mettre la carte que l'on a configuré précédemment :</br>
![Pic11](img/11.PNG?raw=true)</br>
![Pic12](img/12.PNG?raw=true)
</br>

## 6ème étape, vérifier la connectivité :
Maintenant en rallumant la VM, elle prend une adresse IP entre 172.16.0.2 et 172.16.0.10, l'hôte étant en 172.16.0.1, nous allons tester le ping entre les deux :
### Sur la vm :
```bash
$ ip a s
$ ping 172.16.0.1
```
### Sur l'hôte :
```bash
C:/> ping 172.16.0.3
```
La connection entre les deux s'effectue bien, nous pouvons même accéder depuis l'hôte au site web dispo de la VM en rendant visite à <http://172.16.0.3/>.
/!\ Attention, je rappel que sur un Windows 10 et serveur 2016, le port de réponse ICMP est désactivé par défaut. Donc à ce stade, si vous n'avez jamais fait l'ouverture de ces ports, c'est normal que le ping du Windows au Linux fonctionne
mais que l'inverse ne fonctionne pas. (Généralement à l'école on désactive le pare-feu, ce n'est absolument pas la solution, la bonne solution est l'ouverture du port ICMP en écoute.).

## 7ème et dernière étape, sur l'hôte, configuration de VSCode :
On lance VSCode, cliquer sur **Remote Explorer**, **Add new**, on met la commande ssh : **ssh root@172.16.0.3**, on laisse par défaut la première ligne on appuit juste
 sur entrer.</br>
![Pic13](img/13.PNG?raw=true)</br>
![Pic14](img/14.PNG?raw=true)</br>
![Pic15](img/15.PNG?raw=true)
</br>
Maintenant il suffit juste de cliquer sur le répertoire **Connect to Host in New Window** (il demandera le mot de passe de l'utilisateur qu'on a spécifié plus tôt) :</br>
![Pic16](img/16.PNG?raw=true)
</br>
(pour accéder au terminal, il suffit d'aller sur l'onglet **view** puis **terminal** de la nouvelle fenetre qui s'est ouverte) :
![Pic17](img/17.PNG?raw=true)

## Pour Créer un fichier, l'éditer et voir son résultat :
Sur le terminal de VSCode, nous allons dans /var/www/html pour créer le fichier :
```bash
$ cd /var/www/html
$ touch mapage.php
```
Puis nous l'ouvrons dans VSCode, cliquer sur **File**, **Open File** et entrer le chemin jusqu'au fichier :</br>
![Pic18](img/18.PNG?raw=true)</br>
![Pic19](img/19.PNG?raw=true)
</br>
Nous remplissons alors le fichier avec du code php :</br>
![Pic20](img/20.PNG?raw=true)
</br>
Après avoir sauvegardé (ctrl + s), nous nous rendons sur <http://172.16.0.3/mapage.php> pour voir si cela fonctionne :</br>
![Pic21](img/21.PNG?raw=true)
</br>
Et voila, le code php est exécuté et fonctionne bien.

## Transfert de fichier de l'hôte à la vm :
Maintenant, nous allons transférer un dossier contenant plusieurs pages web depuis notre windows 10 sur la VM à l'aide de WINSCP.</br>
Tout d'abord il faut télécharger WINSCP :</br>
<https://winscp.net/eng/download.php></br>
Maintenant nous allons l'exécuter et basculer le dossier **hello** de mon bureau vers le /var/www/html de la VM.</br>
Après avoir exécuté, il faut remplir les champs **Nom d'hôte**, **Nom d'utilisateur** et **Mot de passe** pour permettre au logiciel de se 
connecter sur la VM en SSH :</br>
![Pic22](img/22.PNG?raw=true)</br>
</br>
Maintenant, sur la droite nous allons naviguer jusqu'a **/var/www/html** et sur la gauche à l'emplacement où se trouve mon dossier hello :</br>
![Pic23](img/23.PNG?raw=true)
</br>
Nous avons juste à Drag&Drop le dossier **hello** de gauche à droite pour le copier sur la VM.</br>
Après la copie nous vérifions que les pages sont accessibles :</br>
![Pic24](img/24.PNG?raw=true)
