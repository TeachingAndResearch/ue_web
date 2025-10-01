---
title: Session 10 - API REST
nav_order: 11
nav_exclude: false
---

<style>
iframe {
	height:300px;
}
</style>

1. TOC
{:toc}

# Avant de démarrer
*(Adapté du cours d'[Hélène COULLON](https://helene-coullon.fr/) ( MA, HDR, IMT Atlantique) « [UE Architectures Distribuées FIL A1 2025-2026 - IMT Atlantique](https://helene-coullon.fr/pages/ue-ad-25-26/) », Section « [REST - Pdf du cours sur REST](https://helene-coullon.fr/download/teachings/ue-ad/cours-25-26/2-api-rest.pdf) »)*

Dans cette session dédiée aux API REST, avant d'attaquer la pratique, nous allons passer en revue les concepts de base essentiels pour bien comprendre l'environnement dans lequel évoluent les API REST.  Ces bases théoriques sont primordiales pour appréhender sérieusement la conception et l’utilisation d’API REST, notamment lorsqu’on cherche à automatiser ou interconnecter des services.


## Rappels sur HTTP

**HyperText Transfer Protocol (HTTP)** est le protocole fondamental qui sous-tend la communication sur le web. Développé en 1989 par Tim Berners-Lee au CERN, il constitue, avec HTML et les URI, la base du World Wide Web.

HTTP opère à la couche **application** du modèle OSI et fonctionne selon le modèle **client/serveur**. Concrètement, cela signifie qu’un client (typiquement votre navigateur ou un client HTTP comme `curl`) envoie une requête à un serveur. Le serveur analyse cette requête, puis renvoie une réponse. Ce dialogue se fonde sur des règles très précises, standardisées, qui permettent l’interopérabilité entre des systèmes très variés, écrits dans des langages différents.

HTTP n’est pas seulement utilisé pour consulter des pages web, mais aussi comme "langue commune" pour tous les logiciels qui doivent échanger des données sur Internet, tout particulièrement dans le domaine des API REST.


## Les méthodes de HTTP

Une requête HTTP typique est constituée de trois éléments principaux :

- **Ligne de requête** : elle indique la méthode voulue, la ressource ciblée (URL) et la version du protocole, par exemple  
  `GET /api/v1/films HTTP/1.1`
- **En-têtes** (headers) : informations optionnelles accompagnant la requête, comme les identifiants de session ou le type de contenu.
- **Corps** (body) : optionnel, présent principalement pour envoyer des données au serveur (par exemple, avec POST ou PUT).

Les **méthodes** HTTP les plus courantes sont :

- **GET** : Demande de récupérer une ressource (typiquement, consulter une page ou obtenir un enregistrement).
- **POST** : Envoi d'informations au serveur (souvent pour créer une nouvelle entité).
- **PUT** : Envoi de données pour mise à jour ou remplacement d'une ressource existante.
- **DELETE** : Demande de suppression d’une ressource.

Dans la conception d'une API REST, savoir choisir la bonne méthode HTTP, et bien comprendre leur sémantique, est essentiel pour obtenir un système robuste et intuitif.


## Les versions de HTTP

L’évolution du protocole HTTP a suivi les évolutions du web.

### HTTP/0.9 (1991)
- C’était une version très rudimentaire, qui permettait uniquement de récupérer le contenu HTML d’une page via GET.
- Après chaque réponse, la connexion était systématiquement fermée côté serveur, ce qui a entraîné des surcoûts pour créer/établir une nouvelle connexion TCP pour chaque requête.

### HTTP/1.0 (1996)
- Introduction du support de tout type de fichier (images, PDF...).
- La connexion TCP/IP était toujours fermée après chaque réponse (problème des surcoûts TCP pas encore réglé).

### HTTP/1.1 (1997)
- Grande avancée ! La connexion peut être maintenue ouverte ("keep-alive"), ce qui permet plusieurs échanges successifs sans se reconnecter à chaque fois.
- Introduction du **pipelining** (envoi de plusieurs requêtes avant d’avoir les réponses).
- Nouvelles méthodes comme POST ou PUT.
- Gestion du caching...
- Amélioration de la performance globale des échanges web.

### HTTP/2 (2015)
- Passage à un format binaire (plus efficace et compact qu’un échange strictement textuel).
- Prise en charge native de requêtes parallèles (multiplexing).
- Compression des en-têtes, réduisant la consommation de bande passante.
- Fonction PUSH : le serveur peut envoyer proactivement des ressources au client avant même que celui-ci les demande (avant, il était toujours obligatoire pour le **client** de demander s'il y avait quelque chose pour lui).

### HTTP/3 (2022)
- Changement fondamental dans le transport : HTTP/3 utilise UDP à la place de TCP, principalement grâce à QUIC, pour améliorer la rapidité et la résilience des connexions.

En pratique, la majorité des API REST reposent encore sur HTTP/1.1 ou HTTP/2 aujourd'hui.


## Formats JSON et YAML

Pour échanger les données via HTTP, il faut des formats standards, textuels, qui puissent être facilement lus, compris, et manipulés par des programmes, tout en restant lisibles pour un humain. Deux grands formats se sont imposés :

### JSON (JavaScript Object Notation)
- Un format de données basé sur la syntaxe de JavaScript, largement adopté pour sa simplicité et sa lisibilité.
- Très utilisé dans les **requêtes/réponses HTTP** et par les **bases de données NoSQL**.
- Largement supporté par la majorité des langages de programmation.

### YAML (YAML Ain’t Markup Language ; ex Yet Another Markup Language)
- Un format conçu pour être encore plus lisible par l’humain.
- Utilisé principalement pour les **fichiers de configuration** et les **spécifications** (par exemple, dans Kubernetes ou Ansible).
- Peut représenter des structures complexes (listes, dictionnaires imbriqués) de façon concise.

Tous deux sont **sérialisables** : on peut facilement convertir une structure de données en string pour l’envoyer sur le réseau, puis la désérialiser côté récepteur.

Par rapport à XML, ils sont plus légers, moins verbeux, et beaucoup plus agréables à manipuler.


## Exemples JSON et YAML

Voici des exemples concrets pour bien visualiser la syntaxe et la différence entre JSON et YAML.

### Exemple de données JSON - Une ressource "film"
```json
{
  "id": 1,
  "titre": "Inception",
  "realisateur": "Christopher Nolan",
  "annee": 2010,
  "acteurs": ["Leonardo DiCaprio", "Ellen Page", "Joseph Gordon-Levitt"]
}
```

### Exemple de liste JSON - Plusieurs films
```json
[
  {
    "id": 1,
    "titre": "Inception"
  },
  {
    "id": 2,
    "titre": "Interstellar"
  }
]
```

### Exemple YAML équivalent d’un film
```yaml
id: 1
titre: "Inception"
realisateur: "Christopher Nolan"
annee: 2010
acteurs:
  - "Leonardo DiCaprio"
  - "Ellen Page"
  - "Joseph Gordon-Levitt"
```

### Exemple YAML - Liste de films
```yaml
- id: 1
  titre: "Inception"
- id: 2
  titre: "Interstellar"
```

On voit immédiatement que YAML est plus concis, et qu'il n'oblige pas à l’utilisation d’accolades ou de crochets, ce qui facilite la lecture visuelle. Mais les deux formats peuvent représenter exactement les mêmes structures de données.


# API REST

Maintenant que nous avons posé les bases techniques, attaquons le sujet central : les API REST.

## Faire communiquer des composants logiciels

Dans une application moderne, il est rare que tous les composants tournent dans le même processus, ou même sur la même machine. Très souvent, différents services, éventuellement écrits dans des langages distincts ou déployés à différents endroits, doivent **échanger des informations**.

Alors, pourquoi ne pas simplement coder "à la main" la gestion du réseau - ouvrir une socket TCP, envoyer des messages en binaire, etc? En pratique, ce serait laborieux, source d’erreurs, difficilement maintenable, et chaque composant devrait réinventer la roue.

Pour **abstraire** ces détails, on s’appuie sur des protocoles standard comme HTTP, et sur des **API** bien définies, qui servent de "contrat" entre les composants. L’usage de **bibliothèques** et de **frameworks** adaptés permet de gérer plus facilement la communication, la sérialisation, la sécurité, etc.

Enfin, pour garantir que les composants sachent "comment parler", il faut une spécification claire et partagée des fonctions de l’API : d’où l’importance de documenter son API de façon rigoureuse.


## Qu'est-ce qu'une API ?

**API** signifie *Application Programming Interface*. C’est un ensemble de règles et de conventions qui définit comment utiliser un logiciel, une bibliothèque ou un service en ligne.

![La notion d'un API](assets/img/session10/api.svg)


Une API, dans ce contexte, représente **un "contrat"** : le serveur annonce quelles opérations il propose (lecture, création, modification, etc) et selon quelles modalités le client peut l’appeler (quelle URL, quelles méthodes HTTP, quelle structure de données).

Par exemple : 
- "Pour obtenir la liste des films, faites : `GET /api/films`"
- "Pour ajouter un film, faites : `POST /api/films` avec le film au format JSON dans le corps"

La notion d'API existe bien au-delà du web, mais dans le domaine des applications distribuées, c'est le protocole HTTP associé à une API REST qui s’est imposé comme standard de communication.


## REST : REpresentational State Transfer

Le concept de REST a été formalisé par Roy Fielding dans sa thèse (2000), dans le but d’exploiter au maximum la **flexibilité** et la **simplicité du web** pour concevoir des logiciels modernes interconnectés.

En quelques principes :
- On pense son application **comme un site web** : chaque type de ressource (film, utilisateur, commande, produit, etc) est identifié par une URL unique.
- Pour interagir avec une ressource, on utilise les **méthodes HTTP** standards (GET, POST, PUT, DELETE...).
- On échange des représentations des ressources (en JSON, YAML ou autre).

L’adresse d’une ressource sert de point d’accès universel. Par exemple :  
`GET http://monapi.example.com/films/42 HTTP/1.1` permet d’obtenir le film d'id 42.

Ce modèle rend les systèmes distribués plus simples à concevoir, à comprendre et à faire évoluer.

Pour un retour aux sources, voir la thèse de Roy Fielding :  
[https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)


