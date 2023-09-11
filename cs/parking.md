## API de gestion de parking privé

Vous devez écrire une application de gestion d'un parking privé. On ne s'occupera ici que de l'API métier (on laissera de côté l'UI). On a retenu les contraintes et besoins suivants avec le client pour la première itération du projet :

- L'application doit garder trace des immatriculations de l'ensemble des véhicules autorisés à stationner sur le parking privé.
- Il faut aussi savoir, à tout moment, quelles sont les immatriculations des véhicules actuellement sur le parking.
- Il n'y pas de limite au nombre de véhicules autorisés à stationner sur le parking.
- Cependant, il y a bien entendu une capacité maximale pour le parking, qui est donnée.
- Au lancement, la liste des immatriculations autorisées doit être vide.
- La liste des véhicules actuellement sur le parking aussi.
- On doit pouvoir ajouter l'immatriculation d'un véhicule à la liste des immatriculations autorisées (doublons interdits).
- On doit pouvoir enregistrer l'entrée d'un véhicule, identifié par son immatriculation, sur le parking.
- On doit pouvoir enregistrer la sortie d'un véhicule (toujours identifié par son immatriculation).
- On doit pouvoir savoir si un véhicule donné est sur le parking ou pas.
- On doit pouvoir savoir si le parking est plein ou pas.
- On doit pouvoir connaître le nombre de véhicules actuellement sur le parking.
- On doit pouvoir afficher la liste des véhicules actuellement sur le parking.
- On doit pouvoir afficher la liste des véhicules autorisés.
- On doit pouvoir afficher le taux de remplissage actuel du parking.

L'ensemble des opérations se fera en mémoire (pas de persistance en BDD).

On testera cette API de manière systématique et automatisée à l'aide du framework de test JUnit.

Dans un second temps, on passera en mode TDD (*Test-Driven Development* : développement piloté par les tests). En TDD, on écrit les tests *avant* d'implémenter quoi que ce soit de fonctionnel : on n'écrit aucun nouveau code tant qu'un test ne met pas en évidence sa nécessité.
