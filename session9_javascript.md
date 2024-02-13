---
title: Session 9 - Javascript, Ajax et JQuery
nav_order: 10
nav_exclude: false
---

<style>
iframe {
	height:300px;
}
</style>

1. TOC
{:toc}

# Javascript : les bases

Nous avons vu rapidement dans
la [session 2](session2_html.html#page-dynamique-javascript) comment
le langage Javascript interagissait avec les éléments d'une page Web
pour fournir une page dynamique (dont le contenu évolue sans recharger
la page).

Javascript peut par exemple:
- changer du contenu HTML :
<script async src="//jsfiddle.net/marie_donnie/z8ovkwua/embed/js,html,css,result/dark/"></script>
- changer la valeur des attributs HTML :
<script async src="//jsfiddle.net/marie_donnie/o2qLkb09/embed/js,html,css,result/dark/"></script>
- changer l'affichage d'éléments grâce à CSS:
<script async src="//jsfiddle.net/marie_donnie/hu3kjaoe/embed/js,html,css,result/dark/"></script>



# Javascript, Ajax et JQuery


Nous allons nous appuyer sur [ces slides](https://0xc0de.fr/courses/Domaine/2018/slides/js-ajax/) pour voir plus en détail Javascript, Ajax et JQuery et comment ils fonctionnent ensemble.


# Exercices

## Trier et filtrer un tableau

Javascript permet donc de modifier des parties de pages en réaction à un évènement.
L'une de ses utilisations consiste à trier ou filtrer un tableau sans recharger la page où il est contenu, ce que nous allons faire maintenant.

Tout d'abord, téléchargez le projet [td_javascript_sort.zip](https://github.com/TeachingAndResearch/ue_web_example/archive/td_javascript_sort.zip) et ouvrez-le dans Pycharm.
Cette archive contient:
- un dossier `database` qui contient les fichiers relatifs à la création d'une base de données sqlite comme vu précédemment. Le modèle ici est simplement des utilisateurs qui ont un `username` et un `age`.
- un dossier `templates`, contenant la seule vue du projet, `index.html.jinja2`, qui va afficher un tableau des utilisateurs de la base de données.
- un dossier `static`, comprenant un fichier javascript `interface.js` à compléter :


```javascript
// Execute the 'onLoad' function after the page has finished loading
$(onLoad)

function onLoad() {
  // 1. On change in the #filter-friends input, compute the table again by
  // filtering rows.

  // Use $().on(...) to register events

  // 2. On click on TH elements, sort the table by the column value.

  // Use $().on(...) to register events
}

function filterRows() {
  // 1. Get the value of the input `filter-friends`

  // Look for `value` or `val` in jQuery

  // 2. Hide all rows whose name do not contain the string `filter`.

  // Hint: use $().filter() to filter elements matching a predicate, and
  // $().hide() to hide elements from the page
}

function sortByField(field) {
  // 1. Sort the rows in place using `Array.sort` and the comparator
  // `compareField` below.

  // 2. Put them back in the table using $().append().
}

// Return a comparator to be used by Array.sort that compares two DOM elements
// based on the value of their attribute named `data-FIELD` using the less-than
// (`<`) operator.
function compareField(field) {
  // See http://devdocs.io/javascript/global_objects/array/sort for how to write
  // a comparator.
  return function(a, b) {
    var na = $(a).data(field)
    var nb = $(b).data(field)
    if (na < nb)
      return -1
    else if (na > nb)
      return 1
    else
      return 0
  }
}
```

### Instructions

1. Divisez vous en deux groupes.
2. Chaque groupe remplit la fonction `onLoad()` qui va appeler :
  - `filterRows` quand le champ `filter-friends` est rempli
  - `sortByField` quand l'un des éléments du haut du tableau est cliqué.
3. Chaque groupe se charge de faire fonctionner les fonctions appelées (`filterRows` et `sortByField`). La [documentation de jQuery](https://api.jquery.com/) sera votre alliée, mais bien sûr, le professeur également.
4. Une fois les deux fonctions remplies, vous expliquerez à l'autre groupe:
  - le fonctionnement de la fonction avec démonstration
  - les choix faits dans différentes approches
  - les difficultés rencontrées, comment elles ont été résolues en expliquant pourquoi le choix final permet le bon fonctionnement.
5. Chaque groupe présente au professeur la fonction de l'autre groupe (et bien sûr, il y aura des questions, comprenez bien ce que les autres ont fait).



## Afficher des données reçues en format JSON par Ajax

Les requêtes AJAX (Asynchronous Javascript And XML) permettent via XMLHttpRequest de manipuler des données (principalement en JSON) pour encore une fois actualiser des morceaux de page sans devoir recharger son entièreté.

Ici, nous allons apprendre à manipuler ces données pour les afficher dynamiquement dans une page.

Téléchargez le projet [td_ajax.zip](https://github.com/TeachingAndResearch/ue_web_example/archive/td_ajax.zip) et ouvrez-le dans Pycharm.

Attendez le reste des instructions :)
