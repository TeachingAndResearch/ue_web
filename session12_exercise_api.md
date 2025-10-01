---
title: Session 12 - Exercise API REST (Optionnel)
nav_order: 13
nav_exclude: true
---

<style>
iframe {
	height:300px;
}
</style>

1. TOC
{:toc}

# TP Flask, REST et OpenAPI
*(Adapté du cours d'[Hélène COULLON](https://helene-coullon.fr/) ( MA, HDR, IMT Atlantique) « [UE Architectures Distribuées FIL A1 2025-2026 - IMT Atlantique](https://helene-coullon.fr/pages/ue-ad-25-26/) », Section « [REST - Sujet de TP REST](https://helene-coullon.fr/pages/ue-ad-fil-25-26/tp-rest) »)*


Nous allons concevoir la petite application suivante :

Il s'agit d'une application jouet et peu réaliste pour gérer les films et les réservations d'utilisateurs dans un cinéma. Cette application est composée de 4 micro-services :

- `Movie` est le micro-service responsable de la gestion des films du cinéma. Il contient et gère une petite base de données `json` contenant la liste des films disponibles avec quelques informations sur les films.
- `Times` est le micro-service responsable des jours de passage des films dans le cinéma. Il contient et gère une petite base de données `json` contenant la liste des dates avec l'ensemble des films disponibles à cette date.
- `Booking` est le micro-service responsable de la réservation des films par les utilisateurs. Il contient et gère une petite base de données `json` contenant une entrée par utilisateurs avec la liste des dates et films réservés. `Booking` fait appel à `Times` pour connaître et vérifier que les créneaux réservés existent bien puisqu'il ne connait pas lui même les créneaux des films.
- `User` est le micro-service qui sert de point d'entrée à tout utilisateur et qui permet ensuite de récupérer des informations sur les films, sur les créneaux disponibles et de réserver. Il contient et gère une petite base de données `json` avec la liste des utilisateurs. `User` fait appel à `Booking` et `Movie` pour respectivement permettre aux utilisateurs de réserver un film ou d'obtenir des informations sur les films.

![Le schema des composants](assets/img/session12/rest.png)


## Travail à réaliser

1. Proposez des point d'entrée supplémentaires dans le service `Movie` pour récupérer des informations sur les films. Proposez aussi un point d'entrée `help` permettant de connaître l'ensemble des points d'entrée de votre service `Movie`. Mettez à jour la spécification openAPI en conséquence.
2. Testez votre microservice avec `Postman` ou une solution équivalente (https://www.postman.com/).
3. Écrivez le microservice `Times` à partir de la spécification OpenAPI fournie dans le repository (`UE-archi-distribuees-Showtime-1.0.0-resolved.yaml`) et testez votre service (sauvegardez bien vos collections de requêtes).
4. Coder le service `Booking` à partir de la spécification OpenAPI disponible (`UE-archi-distribuees-Booking-1.0.0-resolved.yaml`) et testez votre service (sauvegardez bien vos collections de requêtes).
5. Regarder le contenu du fichier `users.json` et imaginez une spécification openAPI pour le service `User` en conséquence de façon à ce qu'il utilise à la fois les services `Booking` et `Movie`. Des exemples :
    - un point d'entrée permettant d'obtenir les réservations à partir du nom ou de l'ID d'un utilisateur ce qui demandera à interroger le service `Booking` pour vérifier que la réservation est bien disponible à la date demandée
    - un point d'entrée permettant de récupérer les informations des films pour les réservations d'un utilisateur ce qui demandera à interroger à la fois `Booking` et `Movie`
6. Écrivez le microservice correspondant et testez votre service (sauvegardez bien vos collections de requêtes).

IMPORTANT: Pour coder `Booking` et `User` vous aurez besoin d'appeler d'autres services par le biais de leur API REST. Pour cela vous utiliserez le paquet `requests` (déjà présent dans `requirements.txt`) et utilisez la fonction `get` (ou `post`). Voir https://www.w3schools.com/PYTHON/ref_requests_get.asp[ici]
