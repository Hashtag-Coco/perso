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
[image1]

Pour installer Vbox, il suffit d'exécuter l'exécutable, c'est une installation classique. Pour installer le pack d'extension, dans VirtualBox
il faut cliquer sur **Fichier**, **Paramètres**, **Extensions**, cliquer sur le **+** et ajouter l'extension pack :</br>
[image2][image3]

## 2ème étape, sur l'hôte, installer VSCode et son module SSH :
Pour télécharger VSCode, il suffit de se rendre sur la page officiel et de télécharger l'installateur :</br>
<https://code.visualstudio.com/download></br>
Pour l'installer, idem que Vbox, cliquer sur l'installateur et installer. Pour installer le module SSH, il faut ouvrir VSCode et cliquer sur 
**Extensions**, inscrire dans la barre de recherche **SSH** et installer **Remote - SSH** :</br>
[image4]

Nous verrons plus tard comment se connecter en SSH à la VM.

## 3ème étape, sur l'hôte, installation de la VM :
Pour installer la VM, nous nous rendons sur le site officiel et téléchargeons le fichier .iso :</br>
<https://ubuntu.com/download/desktop></br>
Maintenant que nous avons le fichier iso, nous allons ouvrir VirtualBox et ajouter une VM, pour cela, cliquer sur **Nouvelle**, en **nom** : **Ubuntu - dev**, **Type** : **Linux**, 
**Version** : **Ubuntu (64-bit)**, la taille de mémoire en **2048** (cela représente 2 GO alloué, ici tout dépend du type de dev que l'on souhaite faire),
on décide de **Créer un disque dur virtuel maintenant**. On laisse le type de fichier en **VDI (VirtualBox Disk Image)**, le stockage idem on laisse en 
**Dynamiquement alloué** (si on alloue 50Go à la VM et qu'elle utilise seulement 5 Go, elle ne prendra pas les 45Go restants pour elle). En emplace du fichier et 
taille nous laissons par défaut les valeurs (10Go me suffit emplement pour moi). </br>
[image5]
Et voila notre VM est créée, lorsque l'on va la démarrer (**démarrage**, **démarrage normal**) nous aurons une pop up pour aller chercher l'iso d'Ubuntu. Après cela,
il faut simplement installer Ubuntu (les tuto sont légions sur l'internet).</br>
[image6]

## 4ème étape, configuration Ubuntu :
Sur mon Ubuntu, j'ai un utilisateur test/test, nous allons installer les packages néc éssaires pour le SSH et LAMP (également des modules de PHP) :
```bash
sudo apr install -y openssh-server apache2 php php-mysql php-curl php-gd php-intl php-json php-mbstring php-xml php-zip mysql-server phpmyadmin 
```
Maintenant que Ubuntu est configuré, il faut faire la config réseau et la config VSCode.

##