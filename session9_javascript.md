---
title: Session 9 - Javascript, Ajax et JQuery
excerpt: ""
nav_order: 9
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


Nous allons nous appuyer pour [ces slides](https://0xc0de.fr/courses/Domaine/2018/slides/js-ajax/) pour voir plus en détail Javascript, Ajax et JQuery et comment ils fonctionnent ensemble.


# Trier un tableau

Javascript permet de modifier des parties de pages en réaction à un évènement.
L'une de ses utilisations consiste à trier un tableau sans recharger la page où il est contenu, ce que nous allons faire maintenant.

Tout d'abord, téléchargez le projet [td_javascript_sort.zip](https://github.com/Marie-Donnie/ue_web_example/archive/td_javascript_sort.zip) et ouvrez-le dans Pycharm.
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
