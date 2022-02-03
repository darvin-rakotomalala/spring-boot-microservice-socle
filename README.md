## Spring Boot - Microservice - Structure de projet
Dans ce repo, nous allons voir les bonnes pratiques dans la structure du projet Spring Boot. Nous découvrirons les différentes couches de Microservice et comment nous pouvons tirer parti de ces couches 
pour créer une structure de projet de Spring Boot propre.

### Architecture globale des couches
---
![archi_globale_socle](https://user-images.githubusercontent.com/75081354/152366192-6d607f66-f971-4c70-bfbd-90149ee8eb4c.jpg)

<br/>

### Explication
**Couche contrôleur** :
* Il contiendra la définition des API Rest et le corps de la requête.
* Seuls les appels d'API doivent invoquer la couche contrôleur.

**Couche de service** :
* Il ne prendra que les données de la couche contrôleur et les transférera vers la couche référentiel.
* Il contiendra également la logique métier et modélisera les données pour la couche référentiel.
* Il prendra également les données de la couche référentiel et les renverra à la couche contrôleur.
* Seule la couche contrôleur doit invoquer la couche service.

**Couche de Repository Layer** :
* Il interagira avec la base de données sous-jacente.
* La couche de service ne doit invoquer que la couche de référentiel.

**Quelques terminologies importantes**<br/>
Il y a quelques terminologies et concepts que vous devriez vous familiariser avant de passer à la section suivante.<br/>

**DTO**
* Les DTO (objets de transfert de données) sont les objets ou les classes utilisés pour transférer des données entre les couches via la couche de service.
* La couche de service accepte uniquement les données sous la forme de DTO.
* Toutes les données renvoyées à la couche contrôleur seront sous la forme de DTO.

**Modèle**
* Les modèles sont l'objet utilisé par la couche de référentiel pour appeler la procédure stockée de la base de données ou effectuer des opérations CRUD sans utiliser la procédure stockée.

**Mappeur**
* Les mappeurs sont utilisés pour convertir la forme des données lorsqu'elles sont transférées entre les couches. Il existe deux types de mappeurs :
	* Model Mapper : cela mappera toutes les données au modèle.
	* DTO Mapper : cela mappera toutes les données aux DTO.

**Erreur - Exception**
* Gestion des erreurs techniques : Les Checked Exceptions sont les exceptions que nous pouvons généralement prévoir et planifier à l'avance dans notre application.
* Gestion des erreurs fonctionnelles : Les Unchecked Exceptions sont les exceptions qui se produisent généralement en raison d'une erreur humaine plutôt que d'une erreur environnementale.

![structure_projet](https://user-images.githubusercontent.com/75081354/152366257-de6e20de-c715-41df-b6b1-d698a5fecd1a.PNG)

**La structure des packages**<br/>
Ce socle est formé de 5 grands packages :<br/>
- Package `donnee` composé de 3 sous packages : <br/>
	- `domainobject` - pour les classes entités<br/>
	- `dto` - pour les classe DTO (on peut avoir un sous package `commun`).<br/>
	- `commun` - pour les classe commun (ex: AuditModel).<br/>
- Package `service` composé de 3 sous packages: <br/>
	- `repository` - pour la communication avec la base de données<br/>
	- `metier` - est un package middleware entre Controller et Repository pour le traitement de la logique métier.<br/>
	- `businessdelegate` - est utilisé pour accèder aux autres microservices (en utilisant Open Feign)<br/>
- Package `infrastructure` - définit un contrôleur pour gérer la couche Web et les APIs Rest.<br/>
- Package `commun` composé de 2 sous packages <br/>
	- `config` - pour la configuration globale de l'application(ex: Security et Swagger).<br/>
	- `mapper` - pour mapper les entités DO to DTO et DTO to DO (ex: en utilisant MapStruct)<br/>
- Package `contrainte` - est un package pour la gestion des erreurs (techniques et fonctionnelles) et `validateur`.

*Avoir ces packages sera notre première étape vers une séparation claire des limites de travail et un code plus propre. Chaque package a sa propre fonctionnalité définie et il n'y a aucun chevauchement entre les packages.*

**Définition du modèle pour le corps de la demande**<br/>
Les points de terminaison REST (API) peuvent effectuer différentes actions. Ces actions sont appelées verbes HTTP. Vous trouverez ci-dessous les verbes HTTP les plus couramment utilisés.
* **POST** : utilisé pour enregistrer l'enregistrement dans la base de données.
* **PUT** : utilisé pour mettre à jour les enregistrements existants dans la base de données.
* **GET** : utilisé pour récupérer les données de la base de données.
* **DELETE** : Utilisé pour supprimer un enregistrement de la base de données.

Voilà ! dans ce repo nous avons vu les meilleurs pratiques dans la structure de projet Spring Boot.
