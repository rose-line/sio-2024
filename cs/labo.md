# Vulnérabilités web - Labo

## Virtualisation

Nous allons utiliser VMware Workstation Player pour lancer des machines virtuelles. Vous pouvez utiliser une autre solution de virtualisation si vous le souhaitez.

- Téléchargez et installez [VMware Workstation Player](https://www.vmware.com/products/workstation-player.html).

## Kali Linux

Kali Linux est une distribution Linux orientée sécurité. Nous allons l'utiliser pour effectuer des tests d'intrusion sur les applications web.

- Téléchargez la [VM Kali Linux préconfigurée pour VMware](https://www.vmware.com/products/workstation-player.html)

- Vous récupérez un fichier `.7z` que vous décompresserez localement (utilisez le logiciel _7-zip_ si besoin).

- À l'intérieur du répertoire créé, vous trouverez un fichier `.vmx` ; ouvrez-le, cela lancera _VMware Workstation Player_ et chargera la machine virtuelle

- Configurez la VM :

  - 4 vCPU
  - 2 Go de RAM
  - 20 Go de disque dur
  - Réseau en mode NAT

- Identifiants de connexion par défaut : `kali/kali`

- Configuration clavier français - ouvrir un terminal et taper :

```
$ sudo setxkbmap fr
```

- Configuration langue française :

```
$ sudo dpkg-reconfigure locales
```

- Sélectionner `fr_FR.UTF-8` dans l'outil qui s'ouvre

- Puis configurer la langue :

```
$ sudo update-locale LANG=fr_FR.UTF-8 LANGUAGE
```

- Enfin, redémarrer avec :

```
$ reboot
```

- Si le clavier français ne persiste pas après redémarrage, allez dans les paramètres de Kali, section « Clavier » et configurer le clavier français depuis cette interface. Redémarrez ensuite.

- Alternativement, vous pouvez aussi configurer directement la langue du clavier en éditant le fichier `/etc/default/keyboard` et en indiquant `fr` sur la variable `XKBLAYOUT`

- Vérifiez votre connectivité internet :

```
$ ping www.google.com
```

## _Web Security Dojo_

Il nous faut des applications vulnérables à attaquer. Nous allons utiliser _Web Security Dojo_. C'est un _bundle_ (ensemble) d'applications web pour la mise en évidence de vulnérabilités. Il contient notamment les applications _Juice Shop_ et _WebGoat_ développées par la fondation _OWASP_, ainsi que _DVWA_ (_Damn Vulnerable Web Application_). Toutes ces applications web sont conçues pour être des « nids à vulnérabilités » et feront d'excellentes cibles.

- Téléchargez [l'image `.ova`](https://sourceforge.net/projects/websecuritydojo/)

- Double-cliquez sur l'OVA téléchargé

- VMware se lance, donnez un nom et un emplacement pour la VM

- Cliquez « *Retry* » si vous avez un message d'erreur concernant la spécification OVF

- L'importation peut prendre plusieurs minutes

- Configurez la VM avec la même configuration matérielle virtuelle que pour Kali Linux

- Identifiants de connexion par défaut : `dojo/dojo`

Pour lancer _DVWA_ :

- Cliquez sur le bouton en haut à gauche pour afficher les applications disponibles

- Cliquez sur _Targets_ pour afficher la liste des applications vulnérables

- Cliquez sur _Main Target List_, cela ouvre le navigateur web avec une liste d'applications

- Cliquez sur _DVWA_ (notez les identifiants donnés) et connectez-vous

Pour lancer _Juice Shop_ :

- Retournez dans la liste des _Targets_ et cliquez sur *Juice Shop Start* ; patientez (cela peut prendre plusieurs minutes la première fois), le navigateur va ouvrir l'application _Juice Shop_

Pour lancer _WebGoat_ :

- Procédez de même pour trouver le raccourci _WebGoat NG Start_ (de même, il faut patienter jusqu'à l'ouverture par le navigateur)

- Les identifiants de _WebGoat_ sont donnés sur la page d'accueil qui s'ouvre

On va accéder à ces applications depuis la machine hôte plutôt que d'utiliser la VM, ce sera plus confortable. Récupérez les informations suivantes :

- IP de votre machine _Web Security Dojo_
- ports d'accès aux applications _DVWA_, _Juice Shop_ et _WebGoat_ (indiqués dans la barre d'adresse du navigateur)

- En utilisant ces informations, sous votre machine hôte, lancez votre navigateur et ouvrez les pages correspondantes aux trois applications web (utilisez les chemins `/dvwa` pour _DVWA_ et `/WebGoat` pour _WebGoat_)
