---
title: Session 11 - OpenAPI
nav_order: 12
nav_exclude: false
---

<style>
iframe {
  height:300px;
}
</style>

1. TOC
{:toc}

# Documenter son API REST
*(Adapté du cours d'[Hélène COULLON](https://helene-coullon.fr/) ( MA, HDR, IMT Atlantique) « [UE Architectures Distribuées FIL A1 2025-2026 - IMT Atlantique](https://helene-coullon.fr/pages/ue-ad-25-26/) », Section « [REST - Pdf du cours sur REST](https://helene-coullon.fr/download/teachings/ue-ad/cours-25-26/2-api-rest.pdf) »)*

Définir une API claire, ce n’est pas tout : il faut également la **documenter** pour qu’elle soit comprise, adoptée et utilisée correctement par ses utilisateurs (développeurs, équipes, clients...).

## Pourquoi documenter son API ?

- **Source de référence** : une documentation à jour est le contrat qui précise exactement comment interagir avec le serveur, sans ambiguïté.
  - Elle joue un rôle de **spécification** ou de **cahier des charges**.
- **Outil et guide** : elle sert d’aide pour tous ceux qui doivent écrire du code qui consomme l’API.
- **Adoption facilitée** : une API bien documentée aura beaucoup plus de succès car il sera plus facile de l’utiliser et de la maintenir !

Faire l'impasse sur la documentation, c'est s'exposer à des erreurs, un taux d'adoption faible, ou de futurs problèmes de maintenance.

## Pourquoi suivre un standard de documentation ?

Il existe différentes manières de documenter une API. Mais dans la pratique, il est très avantageux de suivre un **standard** pour plusieurs raisons :

- Cela garantit un **niveau d’information standardisé et adéquat**.
- Toute personne familière avec le standard peut comprendre rapidement votre API, sans avoir à deviner comment elle fonctionne.
- Les outils automatiques (générateurs de documentation, validateurs, générateurs de SDK, simulateurs pour les tests, etc.) peuvent analyser et exploiter ces documents pour automatiser de nombreuses tâches.

### Exemples de standards de documentation :

- **OpenAPI** (anciennement Swagger)
  [https://swagger.io/docs/specification/about/](https://swagger.io/docs/specification/about/)
  Le plus répandu aujourd’hui. On décrit l’API dans un fichier YAML ou JSON, qui sert à générer la doc, des clients, voire des serveurs mock.

- **RAML**
  [https://raml.org/](https://raml.org/)
  Également basé sur YAML, avec une philosophie un peu différente.

Adopter un de ces standards facilite grandement la vie de tous, du développeur à l’utilisateur final de l’API.

# Tutoriel sur OpenAPI
*(Adapté du cours d'[Hélène COULLON](https://helene-coullon.fr/) ( MA, HDR, IMT Atlantique) « [UE Architectures Distribuées FIL A1 2025-2026 - IMT Atlantique](https://helene-coullon.fr/pages/ue-ad-25-26/) », Section « [REST - Tutoriel sur OpenAPI](https://helene-coullon.fr/pages/ue-ad-fil-25-26/tuto-openapi) »)*


## Objectif

Nous allons ici voir comment documenter une API en utilisant le standard `OpenAPI`. Nous allons spécifier l'API de notre (micro)service `Movie`.

Le contenu en format YAML qui nous allons utiliser est dans le git repository de la [session 10](session10_rest.html) (`https://github.com/TeachingAndResearch/ue_web_example_session10-rest.git`).
Si vous avez déjà le cloné, vous pouvez continuer à l'utiliser.

Notons que pour des raisons pédagogiques, nous avons d'abord écrit le code de notre (micro)service avant d'écrire et regarder la spécification de notre API. Une bonne pratique serait de procéder dans le sens inverse, même si des corrections peuvent être apportées à la spécification/documentation après implémentation.

Pour une introduction à OpenAPI, vous pouvez consulter :
- [OpenAPI specification (Swagger)](https://swagger.io/docs/specification/about/)
- [Spécification complète OpenAPI](https://swagger.io/specification/)

NOTE: Il existe d'autres standards comme `RAML`, mais `OpenAPI` est très utilisé et utilise le format `YAML`.

Pour éditer votre documentation, nousa vons déjà installé une extension à votre VS Code via Dev Containers: `OpenAPI (Swagger) Editor`. Elle permet d'avoir un affichage graphique local de votre fichier `yaml`.

## Informations et tags

Nous commençons par indiquer la version du standard `OpenAPI` utilisée :

```yaml
openapi: 3.0.3
```

Dans notre cas, nous n'avons pas de serveurs à déclarer. Ensuite, nous remplissons l'objet `info` qui décrit globalement la documentation :

```yaml
info:
  title: Movie API
  summary: This is the API of the Movie service
  description: This is the API of the Movie service, it should be much much much much much much much much much much much much much much much much much much much much much longer
  contact:
    name: Helene Coullon
    url: https://helene-coullon.fr/
    email: helene.coullon@imt-atlantique.fr
```

NOTE: Pour plus de détails sur l'objet `info`, voir la spécification officielle :  [OpenAPI info object](https://swagger.io/specification/#infoObject)

Nous pouvons ensuite définir des `tags` qui permettent de classer les opérations de l'API. Ces tags sont particulièrement utiles pour organiser la documentation :

```yaml
tags:
  - name: admins
    description: Secured Admin-only calls
  - name: developers
    description: Operations available to regular developers
```

Ici, deux catégories d'utilisateurs sont définies : les `admins` et les `developers`, mais vous pouvez créer autant de tags que nécessaire.

## Chemins et composants

### Chemin retournant un JSON

Voici un exemple simple d'un chemin accessible par une requête `GET` à `/json`.
Pour un chemin qui renvoie un contenu JSON, on précise un schéma correspondant aux données retournées.

Explications :
1. La méthode `GET` est associée au chemin `/json`.
2. Le tag `developers` catégorise cette opération.
3. Plusieurs métadonnées accompagnent la requête : `summary`, `operationId`, `description`.
4. La réponse attendue en cas de succès (`200`) est précisée, ici un contenu JSON de type `application/json`.


Exemple :

```yaml
/json:
  get:
    tags:
      - developers
    summary: get the full JSON database
    operationId: get_json
    description: |
      Nothing to do
    responses:
      '200':
        description: full JSON
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/AllMovies'
```

On fait référence ici au composant `AllMovies` dans la section `components/schemas` que nous devons définir. Examinons la définition de ce composant :

```yaml
components:
  schemas:
    AllMovies:
      type: object
      required:
        - movies
      properties:
        movies:
          type: array
          items:
            type: object
            $ref: '#/components/schemas/MovieItem'
```

Ici, `AllMovies` est un objet possédant une propriété `movies` de type tableau (`array`), chaque élément étant un objet décrivant un film (`MovieItem`).

Définissons le schéma `MovieItem` :

```yaml
    MovieItem:
      type: object
      required:
        - title
        - rating
        - director
        - id
      properties:
        title:
          type: string
          example: The Martian
        rating:
          type: integer
          example: 7
        director:
          type: string
          example: Paul McGuigan
        id:
          type: string
          example: 39ab85e5-5e8e-4dc5-afea-65dc368bd7ab
```

Chaque film est donc représenté par un titre (string), une note (integer), un réalisateur (string) et un identifiant (string).

### Chemin avec paramètres

Pour un chemin avec un paramètre dynamique, on utilise la syntaxe `{param}`. Voici un exemple pour récupérer un film par son `id` :

```yaml
/movies/{movieid}:
  get:
    tags:
      - developers
    summary: get the movie by its id
    operationId: get_movie_byid
    description: By passing in the appropriate options, you can get info of a Movie
    parameters:
      - name: movieid
        in: path
        required: true
        description: Movie ID.
        schema:
          type : string
    responses:
      '200':
        description: Movie description
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/MovieItem'
      '404':
        description: not found
```

Points importants :

- Le paramètre `movieid` est inclus dans le chemin et défini comme obligatoire (`required: true`).
- Sa description et son type sont précisés dans la liste `parameters`.
- La réponse 200 contient un JSON conforme au schéma `MovieItem`.
- Une réponse 404 est prévue si aucun film ne correspond à l’ID demandé.

### Requête de type POST

Les requêtes POST sont similaires aux GET mais peuvent contenir un corps (`requestBody`). Voici un exemple pour ajouter un film :

```yaml
/movies/{movieid}:
  get:
    # ...
  post:
    tags:
      - admins
    summary: add a movie item
    operationId: create_movie
    description: Adds a movie to the system
    parameters:
      - name: movieid
        in: path
        required: true
        description: Movie ID.
        schema:
          type : string
    responses:
      '200':
        description: Movie created
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/MovieItem'
      '409':
        description: an existing item already exists
    requestBody:
      content:
        application/json:
          schema:
            $ref: '#/components/schemas/MovieItem'
      description: Inventory item to add
```

On remarque :

- Le corps de la requête (`requestBody`) attend un objet JSON conforme au schéma `MovieItem`.
- Plusieurs réponses possibles suivant le succès ou l’échec (par exemple `409` si le film existe déjà).

### Paramètre dans la requête (`query`)

Enfin, un paramètre peut être passé directement dans la requête, comme ici pour chercher un film par titre :

```yaml
/moviesbytitle:
  get:
    tags:
      - developers
    summary: get the movie by its title
    operationId: get_movie_bytitle
    description: |
      By passing in the appropriate options, you can get Movie info
    parameters:
      - in: query
        name: title
        description: pass a title
        required: true
        schema:
          type: string
    responses:
      '200':
        description: Movie item
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/MovieItem'
      '400':
        description: bad input parameter
```

Ici, la différence principale est que le paramètre `title` est situé dans la query string (`in: query`) et non dans le chemin. Cette approche est classique pour filtrer ou rechercher via l’API.

---

Ainsi, nous avons vu comment décrire et documenter une API REST complète en utilisant le standard OpenAPI. Ce format facilite la maintenance, la compréhension et l’automatisation autour de votre API. N’hésitez pas à expérimenter avec des éditeurs OpenAPI dans votre IDE pour visualiser directement la documentation générée à partir du YAML.

---

## Pour aller plus loin

Nous allons proposer 3 opérations supplémentaires à implémenter. Collez/mergez le fragment YAML ci-dessous dans votre spec OpenAPI existante (dans les sections paths et components), puis implémentez et testez chaque endpoint via Insomnia en suivant précisément les descriptions.

```yaml
# Fragment OpenAPI à ajouter à votre spec existante.
# Ajoutez/mergez les nœuds "paths" et "components" ci-dessous.

paths:
  /movies/{movieid}:
    patch:
      tags:
        - admins
      summary: Mise à jour partielle (PATCH) d’un film
      operationId: patch_movie
      description: |-
        Objectif: permettre une mise à jour partielle d’un film existant.
        Règles à respecter:
        - Champs modifiables: title, rating, director. Le champ id est immuable et ne doit pas être présent dans le payload.
        - Valider les types/contraintes (rating entier borné 0..10, strings non vides).
        - Ignorer les champs absents; ne pas écraser avec null; payload vide => 400.
        Conseils de test simple:
        1) GET /movies/{movieid} pour récupérer le courant.
        2) PATCH avec un payload partiel: vous devez recevoir 200.
      parameters:
        - name: movieid
          in: path
          required: true
          description: Identifiant unique du film (UUID).
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/MoviePatch'
            examples:
              changeTitle:
                summary: Changer uniquement le titre
                value:
                  title: "The Martian (Extended Cut)"
              changeRating:
                summary: Mettre à jour la note
                value:
                  rating: 8
      responses:
        '200':
          description: Film mis à jour. Renvoyer la ressource complète et l’ETag mis à jour.
          headers:
            ETag:
              description: Nouvel ETag de la ressource après mise à jour.
              schema:
                type: string
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MovieItem'
        '400':
          description: Payload invalide (aucun champ, types incorrects, champs non autorisés).
        '404':
          description: Film introuvable pour l’id fourni.
        '415':
          description: Content-Type non supporté (attendu application/json).

  /movies/bulk:
    post:
      tags:
        - admins
      summary: Création en lot (bulk) de films
      operationId: bulk_create_movies
      description: |-
        Objectif: créer plusieurs films en une seule requête.
        Règles:
        - Body = tableau d’objets "NewMovieItem". Le champ id est optionnel; s’il est absent, le serveur génère un id.
        - Détection de conflit: id déjà existant = rejection de tout l'ensemble des ajouts.
        Conseils de test:
        - Envoyer 2-3 films dont un volontairement en conflit.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/BulkCreateRequest'
            example:
              - title: "The Martian"
                rating: 7
                director: "Ridley Scott"
              - id: "39ab85e5-5e8e-4dc5-afea-65dc368bd7ab"
                title: "Sherlock Holmes"
                rating: 8
                director: "Guy Ritchie"
      responses:
        '201':
          description: Tous les films ont été créés.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/BulkResult'
              example:
                summary:
                  processed: 2
                  created: 2
                  conflicts: 0
                  invalid: 0
                results:
                  - index: 0
                    status: created
                    movie:
                      id: "e1f7b6a1-0e8b-4a7d-9d6d-1a2b3c4d5e6f"
                      title: "The Martian"
                      rating: 7
                      director: "Ridley Scott"
                  - index: 1
                    status: created
                    movie:
                      id: "39ab85e5-5e8e-4dc5-afea-65dc368bd7ab"
                      title: "Sherlock Holmes"
                      rating: 8
                      director: "Guy Ritchie"
        '409':
          description: Conflit (aucun film créé).
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/BulkError'
              example:
                error: "conflict"
                details:
                  message: "Doublon détecté sur 1 élément; opération annulée."
                  conflictingIds:
                    - "39ab85e5-5e8e-4dc5-afea-65dc368bd7ab"
        '400':
          description: Payload invalide (non tableau, champs requis manquants, types invalides).
        '415':
          description: Content-Type non supporté (attendu application/json).

  /movies/search:
    get:
      tags:
        - developers
      summary: Recherche multi-critères avec pagination et tri
      operationId: search_movies
      description: |-
        Objectif: filtrer et trier les films existants.
        - Filtres disponibles (tous facultatifs): titleContains, director, minRating, maxRating.
        - Contrainte: si minRating ou maxRating est fourni, ils doivent être des entiers 0..10 et minRating ≤ maxRating.
        - Tri: sortBy ∈ {title, rating, director, id}, sortOrder ∈ {asc, desc}.
        - Retourner 200 avec une liste possiblement vide.
        Conseils de test:
        - Chercher par titre partiel, combiner avec min/max rating, puis tester le tri.
      parameters:
        - in: query
          name: titleContains
          description: Sous-chaîne recherchée dans le titre (recherche insensible à la casse recommandée).
          required: false
          schema:
            type: string
            minLength: 1
        - in: query
          name: director
          description: Filtrer sur le nom exact du réalisateur.
          required: false
          schema:
            type: string
            minLength: 1
        - in: query
          name: minRating
          description: Note minimale (entier 0..10)
          required: false
          schema:
            type: integer
            minimum: 0
            maximum: 10
        - in: query
          name: maxRating
          description: Note maximale (entier 0..10).
          required: false
          schema:
            type: integer
            minimum: 0
            maximum: 10
        - in: query
          name: sortBy
          description: Champ de tri.
          required: false
          schema:
            type: string
            enum: [title, rating, director, id]
            default: title
        - in: query
          name: sortOrder
          description: Ordre de tri.
          required: false
          schema:
            type: string
            enum: [asc, desc]
            default: asc
      responses:
        '200':
          description: Résultats de recherche.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/SearchResults'
              example:
                items:
                  - id: "39ab85e5-5e8e-4dc5-afea-65dc368bd7ab"
                    title: "Sherlock Holmes"
                    rating: 8
                    director: "Guy Ritchie"
        '400':
          description: Paramètres invalides (ex. minRating > maxRating, valeurs hors bornes).
        '415':
          description: Format non supporté.

components:
  schemas:
    MoviePatch:
      type: object
      description: |-
        Payload partiel autorisé pour PATCH.
        - N’envoyez que les champs à modifier.
        - Les champs absents ne sont pas modifiés.
        - Le champ "id" n’est pas autorisé.
      additionalProperties: false
      properties:
        title:
          type: string
          minLength: 1
          example: "The Martian (Extended Cut)"
        rating:
          type: integer
          minimum: 0
          maximum: 10
          example: 8
        director:
          type: string
          minLength: 1
          example: "Ridley Scott"
      minProperties: 1

    NewMovieItem:
      type: object
      description: Données nécessaires à la création d’un film.
      required:
        - title
        - rating
        - director
      properties:
        id:
          type: string
          description: Optionnel. Si absent, le serveur génère un ID.
          example: "39ab85e5-5e8e-4dc5-afea-65dc368bd7ab"
        title:
          type: string
          example: "The Martian"
        rating:
          type: integer
          minimum: 0
          maximum: 10
          example: 7
        director:
          type: string
          example: "Ridley Scott"

    BulkCreateRequest:
      type: array
      description: Tableau d’objets à créer.
      minItems: 1
      items:
        $ref: '#/components/schemas/NewMovieItem'

    BulkItemResult:
      type: object
      description: Résultat unitaire d’un item de la requête bulk.
      properties:
        index:
          type: integer
          description: Index de l’élément dans le tableau d’entrée.
          example: 1
        status:
          type: string
          description: Statut pour cet item.
          enum: [created, exists, conflict, invalid, error]
          example: created
        id:
          type: string
          description: Identifiant du film si disponible.
          example: "39ab85e5-5e8e-4dc5-afea-65dc368bd7ab"
        message:
          type: string
          description: Détail en cas d’échec.
          example: "ID déjà utilisé"
        movie:
          $ref: '#/components/schemas/MovieItem'

    BulkResult:
      type: object
      description: Résumé et détails d’une création en lot.
      required:
        - summary
        - results
      properties:
        summary:
          type: object
          properties:
            processed:
              type: integer
              example: 2
            created:
              type: integer
              example: 1
            conflicts:
              type: integer
              example: 1
            invalid:
              type: integer
              example: 0
        results:
          type: array
          items:
            $ref: '#/components/schemas/BulkItemResult'

    BulkError:
      type: object
      description: Erreur globale pour une opération bulk atomique échouée.
      properties:
        error:
          type: string
          example: "conflict"
        details:
          type: object
          additionalProperties: true
          example:
            message: "Doublon détecté"
            conflictingIds:
              - "39ab85e5-5e8e-4dc5-afea-65dc368bd7ab"

    SearchResults:
      type: object
      description: Résultats paginés pour la recherche.
      required:
        - items
      properties:
        items:
          type: array
          items:
            $ref: '#/components/schemas/MovieItem'
```
