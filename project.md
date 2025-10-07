---
title: Projet 2025 - Marketplace de mode seconde main
nav_order: 14
nav_exclude: false
---

<style>
iframe {
	height:300px;
}
</style>

1. TOC
{:toc}

# Projet 2025 - Marketplace de mode seconde main

> **Travail en équipe :** binômes (une équipe sera un trinôme).  
> **Charge indicative :** ~50 heures/projet.  
> **Période projet (autonomie) :** Sessions 5–10, du **07/10/2025** au **02/12/2025**.
> **Evaluation intermediaire :**: **25/11/2025**.
> **Soutenance (Session 11) :** Examen oral le **10/12/2025**.

---

## 1) Contexte & objectifs

Vous réaliserez une **plateforme de vente d’articles de mode d’occasion**, avec un **front-end Web simple** (HTML/CSS/Bootstrap + JS « vanilla ») et un **back-end** respectant une **spécification OpenAPI (Tier A)** fournie par l’enseignant. Le Tier A constitue le **socle minimal obligatoire**. Des **extensions (Tier B)**, laissées à votre initiative, permettent d’aller au-delà du seuil de validation.

Le Tier A sera **testé automatiquement** contre le fichier OpenAPI fourni. Toute divergence par rapport à la spécification peut faire échouer les tests. Les valeurs monétaires sont exprimées **en centimes d’euro** et l’authentification de test passe par l’entête `X-User-Email` (administrateur pré-créé : `admin@imt.test`). 

Seules les méthodes HTTP `GET`, `POST`, `PUT`, `DELETE` sont employées. 

---

## 2) Périmètre minimum à implémenter (Tier A)

Le Tier A se résume en **capabilités fonctionnelles** (liste ci-dessous) et **endpoints** (référencés par leurs identifiants exacts).  

=> Pour les détails de schémas, codes d’erreurs, contraintes de statut et calculs, **référez-vous exclusivement** au fichier [full_openapi.yaml](/assets/others/full_openapi.yaml) fourni.


### 2.1 Authentification de test & conventions globales
- **Entête obligatoire** sur les endpoints protégés : `X-User-Email`. L’utilisateur `admin@imt.test` possède les droits d’administration.   
- **Monnaie** : centimes d’euro (entiers), **dates** RFC 3339 en UTC, **méthodes HTTP autorisées** : `GET`, `POST`, `PUT`, `DELETE`. 

### 2.2 Paramétrage de la protection acheteur (assurance)
- Lecture/écriture admin de la configuration globale via `GET /api/config/buyer-protection` et `PUT /api/config/buyer-protection`.  
- Le total affiché/acheté inclut `insurance_part_cents = round_half_up(price * ratio/100) + bias_cents`. Les achats stockent un **instantané** des montants. 

### 2.3 Catégories (arborescence à profondeur quelconque)
- Consultation publique : `GET /api/categories`.  
- Administration (création/mise à jour/suppression) : `POST /api/categories`, `PUT /api/categories/{category_id}`, `DELETE /api/categories/{category_id}`.  
- Suppression refusée si la catégorie a des **enfants** ou des **annonces** rattachées (directement ou via descendants). 

### 2.4 Comptes & adresses
- **Comptes** : création publique `POST /api/users` (email = identifiant), suppression **admin** `DELETE /api/users/{email}` (les annonces actives de l’utilisateur deviennent `deleted`).   
- **Adresses (auth)** : `GET /api/addresses`, `POST /api/addresses`, `PUT /api/addresses/{address_id}`, `DELETE /api/addresses/{address_id}`. Le champ `line2` peut être `null`. 

### 2.5 Annonces & navigation
- **CRUD vendeur (auth)** : `POST /api/listings`, `PUT /api/listings/{listing_id}`, `DELETE /api/listings/{listing_id}` (vendeur uniquement tant que `active`). Consultation publique/contrôlée via `GET /api/listings/{listing_id}`.  
- Statuts : `active` (visible/achetable), `sold`, `deleted`. **Photos** : >= 1, au plus une miniature (`is_thumbnail=true`) ; à défaut, la première devient miniature.   
- **Parcours d’exploration public** : `GET /api/browse/listings` avec **filtres** (texte `q`, catégorie + descendants, bornes prix/total) et **tris** (`price`/`total`). Chaque item inclut `insurance_part_cents` et `total_cents` calculés à partir de la configuration courante. 

### 2.6 Crédits & achats
- **Crédits (auth)** : rechargement magique `POST /api/credits/topup` et consultation du **relevé** `GET /api/credits/ledger` (types `topup`, `purchase`, `refund`, `sale_payout`).   
- **Achat (auth)** : `POST /api/purchases` (débit du total, annonce → `sold`, création de transactions acheteur/vendeur, enregistrement adresse instantané). Liste filtrable par rôle : `GET /api/purchases?role=buyer|seller`. Détail : `GET /api/purchases/{purchase_id}`. Déclaration de réception : `POST /api/purchases/{purchase_id}/declare` avec `OK` → `closed`, `NOT_RECEIVED`/`NOT_AS_DESCRIBED` → **remboursement** `(item_price_cents + shipping_cents)` à l’acheteur (l’assurance reste acquise). 

### 2.7 Téléversement et diffusion des photos
- **Upload (auth)** : `POST /api/photos` (form-data `file`, types `image/jpeg|png|webp`, URL absolue en réponse + en `Location`, limite de taille → `413`).  
- **Accès public binaire** : `GET /api/photos/{photo_id}` (retourne l’image binaire, `Content-Type` conforme). Ces URL sont celles à placer dans `Photo.url` des annonces. 

---

## 3) Extensions proposées (Tier B - au-delà du minimum)

> Les extensions ne font **pas** l’objet d’une spécification OpenAPI fournie.  
> Vous devez **concevoir** leur API (ressources, schémas, erreurs) et **implémenter** les endpoints et l’UI correspondants. Une **validation préalable** de l’enseignant est requise pour toute idée hors liste.

- **Favoris** (marquer/déréférencer des annonces, liste dédiée, navigation rapide) - *Difficulté : Facile → Moyen*.  
- **Bundles / paniers par vendeur** (achat groupé multi-articles d’un même vendeur avec mutualisation des frais de port) - *Difficulté : Moyen*.  
- **Offres / négociation** (acheteur propose un prix, vendeur accepte/refuse/counter-offer) - *Difficulté : Moyen → Difficile*.  
- **Messagerie par annonce** (mini-chat vendeur/acheteur lié à une annonce) - *Difficulté : Difficile* (persistance).  
- **Étiquette d’expédition** (QR code avec adresses expéditeur/destinataire + id transaction) - *Difficulté : Moyen*.  
- **Analytics basiques** (volumétrie, min/moy/max, périodes 1/3/6/12 mois, **temps de vente** entre publication et achat, mini-graphes) - *Difficulté : Moyen*.  
- **Génération IA de description** à partir des **photos** (ex. : titres + attributs proposés automatiquement côté vendeur) - *Difficulté : Difficile* (pipeline back-end, appel modèle vision/LLM, UX de validation).  

---

## 4) Organisation du projet & livrables

### 4.1 Calendrier & jalons
- **Période d’autonomie** : Sessions **5–10** (du **07/10/2025** au **02/12/2025**).  
- **Point d’étape obligatoire** à mi-parcours (le **25/11/2025**) : test du Tier A en cours d’intégration (CRUD de base, exploration, rechargement de crédits, un flux d’achat complet, upload photo). L’objectif est d’**anticiper les blocages** techniques ou organisationnels.  
- **Soutenance** (Session 11) : **10/12/2025** - 30 min par binôme (le trinôme dispose d’un temps adapté).

### 4.2 Dépôt, partage et documentation
- Créez un dépôt Git sur le [GitLab IMT Atlantique](https://gitlab-df.imt-atlantique.fr/web-app-2025-2026) et **ajoutez l’enseignant** comme membre :  
  `r17kouts` (GitLab IMT). Ajoutez un **README.md** clair à la racine (installation, lancement, variables, jeu de données, comptes de test, scénarios de vérification).   
- Un **README** bien structuré est requis : comment lancer l’appli, structure BD, choix techniques, et **commande unique** pour exécuter les tests d’API Tier A.  

### 4.3 Aide & interactions
- **Aide pendant les créneaux projet** : support et échanges via un Serveur Discord de l’UE: `UE_WEBAPP_2025_2026` pour questions techniques et retours intermédiaires. 

### 4.4 Pré-provisionnement & contraintes techniques
- L’enseignant va fournir le **fichier OpenAPI**. (Les tests automatiques vérifieront la stricte conformité au Tier A.)  
- Front-end : **HTML/CSS/Bootstrap + JS « vanilla »** par default. Autres acceptés, mais à la responsibilité des membres de l'équipe.  
- Respectez la **sémantique** des statuts (`active|sold|deleted`), des calculs de totaux et des règles d’accès (entête `X-User-Email`, rôle admin). 

---

## 5) Critères d’évaluation

L’évaluation se déroule en **soutenance orale (~30 min)** par équipe (répartition des questions pour apprécier la contribution de chacun·e). Elle porte notamment sur :

1. **Conformité stricte au Tier A** (tests automatiques **passants** contre [full_openapi.yaml](/assets/others/full_openapi.yaml)) : endpoints, schémas, erreurs, calculs de totaux/assurance, statuts des annonces, flux d’achat, upload/serving des photos.   
2. **Qualité du code & architecture** (clarté, séparation des responsabilités, gestion des erreurs, validations côté serveur, cohérence des statuts).  
3. **Modèle de données & persistance** (intégrité, requêtes efficaces).  
4. **Interface & UX** (sobriété, lisibilité, formulaires corrects, navigation et filtres fonctionnels).  
5. **Documentation & démonstration** (README, scripts de démarrage, scénarios de test reproductibles).  
6. **Extensions (Tier B)** : pertinence du besoin, **conception de l’API** (non fournie), robustesse, démonstration.  

> *Un très bon projet est un projet qui, en plus de répondre aux besoins minimaux, serait utilisable par un autre étudiant de l’école (i.e. qui n’aurait pas suivi le module).* 

---

## 6) Références (Tier A - OpenAPI)

- Spécification de référence : [full_openapi.yaml](/assets/others/full_openapi.yaml) (enseignant). Points d’attention :  
  - Authentification de test via `X-User-Email` (admin : `admin@imt.test`).   
  - Catégories (CRUD admin, suppression conditionnelle).   
  - Configuration de la **Buyer-Protection** (formule normative, snapshot à l’achat).   
  - Comptes & adresses (création/suppression admin, `line2` nullable).   
  - Annonces & navigation publique (`/api/browse/listings` : filtres/tri + `total_cents`).   
  - Crédits & achats (ledger, `declare` avec règles de remboursement).   
  - Photos (upload `POST /api/photos`, binaire `GET /api/photos/{photo_id}`). 

---

### Annexes - Rappels utiles

- Les **valeurs monétaires** sont en **centimes d’euro** (entiers). Les dates/horodatages sont des **RFC 3339 UTC**. 
- Les **images d’annonces** utilisent des **URLs absolues** retournées par `POST /api/photos` puis servies en public par `GET /api/photos/{photo_id}`. 
