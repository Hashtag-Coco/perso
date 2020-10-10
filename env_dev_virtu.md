# Installation d'un environnement de developpement virtuel
Le principe de cet installation va nous permettre de créer, modifier et executer du code sans installer directement sur notre machine les différents packages/serveur web etc.

## Résumer de ce que nous allons mettre en place
Sur notre PC, nous allons installer les logiciels VirtualBox et VSCode. VirtualBox permettra de faire tourner une machine virtuelle ubuntu, cette dernière 
accueillera tout les packages de developpement et serveur web. VSCode nous permettra de modifier/créer et executer le code situé sur l'Ubuntu à partir de 
notre PC. Sur la VM, nous allons installer un serveur SSH, Apache2, PHP et mysql.</br>
Avant de faire la configuration système, nous testerons la connectivité entre l'hôte et la VM.

## 1ère étape, sur l'hôte, installer VirtualBox
Pour télécharger Vbox, il suffit de se rendre sur la page officiel et de télécharger l'installateur **ET** le pack d'extension (ce pack permet d'utiliser certaines 
fonctionnalités à partir des VM) :
```bash
https://www.virtualbox.org/wiki/Downloads
```
[image1]

Pour installer Vbox, il suffit d'exécuter l'exécutable, c'est une installation classique. Pour installer le pack d'extension, dans VirtualBox
il faut cliquer sur **Fichier**, **Paramètres**, **Extensions**, cliquer sur le **+** et ajouter l'extension pack :</br>
[image2][image3]

## 2ème étape, sur l'hôte, installer VSCode et son module SSH :
Pour télécharger VSCode, il suffit de se rendre sur la page officiel et de télécharger l'installateur