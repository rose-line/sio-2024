# Injection de commandes

## Mise en place du laboratoire de test

### Burp Suite Community Edition

_Burp Suite_ est un logiciel de test d'intrusion. Il permet de tester les vulnérabilités des applications web.

La _Community Edition_ est gratuite. Si vous utilisez Kali Linux, elle est déjà pré-installée.

https://portswigger.net/burp/communitydownload

### Compte _PortSwigger_

Les labos en ligne de _PortSwigger_ (l'éditeur de _Burp Suite_) nécessitent une inscription au site. Une fois votre adresse email renseignée, un email est envoyé avec les instructions pour activer votre compte.

https://portswigger.net/users/register

### Accès aux labs _PortSwigger_

Pour plus de facilité, on va utiliser le navigateur intégré à _Burp Suite_ pour accéder aux labs. Cela permet de ne pas avoir à configurer de proxy dans le navigateur pour intercepter les requêtes.

- Ouvrez Burp Suite
- Sous _Proxy/Intercept_, cliquez sur _Open Browser_
- Sur le navigateur _Burp_, connectez-vous à votre compte _PortSwigger_

Vous êtes alors prêts à commencer les laboratoires ci-dessous.

## Intercepter une requête avec _Burp Suite_

_Burp Proxy_ est un composant de _Burp Suite_ qui permet d'intercepter les requêtes/réponses HTTP/S entre le navigateur et l'application web.

- Sous _Proxy/HTTP history_, vous voyez passer toutes les requêtes effectuées depuis le navigateur de _Burp Suite_
- Sous _Proxy/Intercept_, cliquez sur _Intercept is off_ (pour obtenir _Intercept is on_)
- Effectuez une requête quelconque depuis le navigateur de _Burp Suite_
- Sous _Proxy/Intercept_, vous voyez que la requête a été interceptée et est actuellement bloquée
- Vous pouvez consulter tranquillement la requête (et surtout la modifier) avant de la transmettre
- Lorsque votre requête est prête, cliquez sur _Forward_ pour la transmettre à l'application web
- N'oubliez pas de désactiver l'interception quand vous n'en avez plus besoin (_Intercept is on_ => _Intercept is off_), sinon vous ne pourrez plus naviguer sur le site sans cliquer sur _Forward_ à chaque requête !

## Utiliser _Burp Repeater_

Par la suite, si vous souhaitez juste tester des requêtes et voir les réponses correspondantes sans avoir besoin de voir le résultat sur le site, vous pouvez utiliser le _repeater_ :

- Sous _Proxy/HTTP history_, sélectionnez une requête
- Clic-droit, _Send to Repeater_
- Cliquez sur l'onglet _Repeater_
- Vous pouvez consulter la requête et la modifier/renvoyer autant de fois que vous le souhaitez (bouton _Send_), et consulter la réponse correspondante

## Pentest 1 - Injection de commandes basée sur le résultat (_in-band_)

[Lien vers le labo]([https://portswigger.net/academy/labs/launch/28bda028c1ba840a8f4fccaaad3739fdc8d8e2d195f348c0db992cbcd6971cbf?referrer=%2fweb-security%2fos-command-injection%2flab-blind-time-delays](https://portswigger.net/academy/labs/launch/2b08de150e353dc1c0057519f466a55a0ca4a7901b2469d56ef7d2a80eddeaa6?referrer=%2fweb-security%2fos-command-injection%2flab-simple))

Ce laboratoire contient une vulnérabilité d'injection de commande shell dans la procédure de vérification de stock du produit.

L'application exécute une commande shell contenant les identifiants du produit et du magasin fournis par l'utilisateur, et renvoie la sortie brute de la commande dans sa réponse.

Pour résoudre le laboratoire, vous devez parvenir à exécutez sur le système la commande `whoami` pour déterminer le nom de l'utilisateur actuel.

- Localisez le formulaire permettant de consulter le stock d'un produit
- Utiliser _HTTP history_ pour examiner la requête qui permet de récupérer le stock
- Utiliser l'interception et/ou le _repeater_ pour pouvoir modifier la requête
- Trouver comment injecter la commande `whoami` (vous devez obtenir le résultat à l'intérieur de la réponse : injection _in-band_)
- Le labo sera automatiquement marqué comme réussi dès que vous aurez effectué une requête qui exploite la vulnérabilité

BONUS : trouver l'emplacement du script shell qui s'exécute et récupérer son code

## Pentest 2 - Injection de commandes aveugle avec délai (_blind injection_)

[Lien vers le labo](https://portswigger.net/academy/labs/launch/28bda028c1ba840a8f4fccaaad3739fdc8d8e2d195f348c0db992cbcd6971cbf?referrer=%2fweb-security%2fos-command-injection%2flab-blind-time-delays)

Ce laboratoire contient une vulnérabilité d'injection de commande shell dans la procédure de _feedback_ (« donner votre avis »).

L'application exécute une commande shell contenant les détails fournis par l'utilisateur. La sortie de la commande n'est pas renvoyée dans la réponse. Donc vous n'avez pas de retour direct sur l'exécution éventuelle de la commande. L'objectif est de réaliser une injection de commande aveugle avec délai.

Dans ce laboratoire, vous devez démontrer la possibilité d'injection en forçant un délai de 10 secondes avant l'envoi de la réponse. Ce délai effectif que vous imposez prouve que l'injection fonctionne, même si on ne « voit » pas le résultat.

- Localisez le formulaire de _feedback_
- Étudiez la requête envoyée avec le formulaire et trouvez comment injecter une commande qui va forcer un délai de 10 secondes

## Pentest 3 - Injection de commande aveugle avec redirection (_blind injection_)

[Lien vers le labo](https://portswigger.net/academy/labs/launch/794c7ab29027c16b9a3fab84326c6f313a8af27a96f1833828edd38b26a4aca7?referrer=%2fweb-security%2fos-command-injection%2flab-blind-output-redirection)

Ce laboratoire contient une vulnérabilité d'injection de commande shell dans la procédure de _feedback_. C'est la même vulnérabilité que dans le labo précédent. Nous avons donc déjà une preuve de concept : nous savons comment exploiter la vulnérabilité.

La vulnérabilité doit cette fois être exploitée pour exfiltrer des informations. Même si la sortie d'une commande n'est pas renvoyée dans la réponse, vous pouvez utiliser la *redirection de sortie* pour capturer la sortie de la commande. Il vous faut pour cela un emplacement accessible en lecture et en écriture par l'application web. Il y a un tel répertoire sur le serveur : `/var/www/images/`.L'application sert les images du catalogue de produits à partir de cet emplacement.

Vous pouvez donc rediriger la sortie de la commande injectée vers un fichier dans ce répertoire, puis utiliser l'URL de chargement de l'image pour récupérer le contenu du fichier via _Burp Suite_

Pour résoudre le laboratoire, vous devez exécuter la commande `whoami` et récupérez la sortie via une redirection.

### Pentest 4 - Injection de commande aveugle et interaction avec un serveur distant (_out-of-band interaction_)

[Lien vers le labo]()

On considère maintenant qu'il est impossible d'utiliser un répertoire sur le serveur pour stocker et exfiltrer les données.

Il existe une autre technique pour l'exfiltration : l'interaction avec un serveur distant. Cette technique consiste à injecter une commande qui va déclencher une requête vers un serveur distant contrôlé par l'attaquant. On peut ensuite récupérer les données via ce serveur.

Dans ce laboratoire, il faut prouver que l'exfiltration _out-of-band_ est possible en contactant un serveur distant.

Malheureusement, ce laboratoire nécessite la version professionnelle de _Burp Suite_ pour être résolu.

### Pentest 5 - Injection de commande aveugle et exfiltration de donnéesinteraction avec un serveur distant (_out-of-band interaction_)

[lien vers le labo(https://portswigger.net/academy/labs/launch/6e6e115e1981ae822b7e41fcaccbc1cb96a5d85b63b42b80e3e6d35565209752?referrer=%2fweb-security%2fos-command-injection%2flab-blind-out-of-band)

