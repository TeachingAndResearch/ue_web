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

