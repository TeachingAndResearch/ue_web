---
title: Bonus - Faire vos propres pages web
excerpt: ""
nav_order: 11
---


1. TOC
{:toc}

# Point de vue global

Pour faire vos propres pages web, vous savez maintenant faire une
application en Flask en utilisant HTML, CSS et Javascript. Vous
pourriez ainsi utiliser cette application Flask en tant que serveur
d'une façon ou d'une autre.

Ici, on va apprendre à utiliser [Jekyll](https://jekyllrb.com/), une
application en ruby (aucune connaissance en ruby n'est nécessaire
cependant) pour faire des sites rapidement et facilement via Github ou
Gitlab, à partir d'un format markdown, par exemple, comme ce document,
que vous pouvez observer
[ici](https://github.com/Marie-Donnie/ue_web/blob/gh-pages/bonus_pages.md).


Le markdown a une syntaxe facile à utiliser, que vous connaissez
peut-être via des programmes comme Discord. Vous pouvez trouver des
"cheatsheet"
comme
[celle-ci](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) pour
vous aider,
ou
[la liste des fonctionnalités du Markdown des Github Pages](https://www.markdownguide.org/tools/github-pages/) plus
spécifiquement, pour voir ce qui est supporté par celles-ci
(et [ici](https://docs.gitlab.com/ee/user/markdown.html) pour le
markdown de Gitlab, qui a des fonctionnalités très intéressantes).

Pour simplifier ce cours, nous nous intéresserons à comment faire des
pages sur github pages, mais il faut savoir que vous avez exactement
la même chose chez Gitlab
([qui a d'ailleurs une excellente documentation](https://docs.gitlab.com/ee/user/project/pages/)). De
la même façon, nous pourrions utiliser d'autres générateurs de sites
statiques comme [Hugo](https://gohugo.io/)
ou [Gatsby](https://www.gatsbyjs.com/). Ces générateurs sont dits "de
sites statiques", mais rien n'empêche de mettre du code HTML dans un
fichier Markdown et d'utiliser du Javascript. Ce qui les définit comme
*statique* est le fait qu'ils n'utilisent pas de bases de données à
proprement parlé. Vous pouvez avoir des fichiers yaml qui vont servir
de données pour du templating, mais ça s'arrête là. Jekyll, lui, prend
des fichiers Markdown et va générer les pages HTML qui seront
affichées. Mais Jekyll ne fait que ça ; générer les pages HTML. Ce
n'est pas lui qui va les *exécuter* publiquement, et c'est là qu'on va
avoir besoin de Github/Gitlab Pages.


# Mettons les mains dans le cambouis

Pour information, la documentation de comment faire pour Gitlab est
très complète, vous pouvez la
trouver [ici](https://docs.gitlab.com/ee/user/project/pages/).


On va suivre le déroulement de [cette page](https://pages.github.com/):
1. Créez un dépôt Git appelé "[username].github.io" (avec [username] votre nom sur Github, c'est facile, il est affiché juste à gauche du champ).
2. Les étapes 2 à 5 ne sont pas obligatoires si
vous ne comptez rien éditer en local (mais toujours utiliser l'éditeur
Github).
3. Voilà. Bon, mais vous avez un site vide.


En bas de page, vous avez deux liens utiles :
- [Blogging with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll), une page qui vous redirige vers différents sujets, comme comment créer votre site avec Jekyll, comment le tester localement (vous aurez besoin des étapes 2 à 5), comment ajouter du contenu, un thème, etc.
- [Custom URLs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) qui
  va vous expliquer comment vous pouvez avoir votre nom de domaine à
  vous (exemple: monnom.fr). **Attention, cela implique de payer un nom
  de domaine à côté.**

Si vous savez éditer localement vos pages, tant mieux, vous pourrez
cloner votre dépôt, l'éditer et tester localement votre site.  Vous
pouvez suivre
le
[tutoriel d'installation](https://jekyllrb.com/docs/step-by-step/01-setup/),
et vous aurez besoin de `jekyll build` qui transformera votre contenu
en pages HTML, puis `jekyll serve` pour observer ces pages localement,
comme on le faisait avec Flask et Pycharm.


Sinon, à partir de maintenant, on va voir comment faire tout à partir
de l'interface de Github. Dans votre dépôt, vous devez avoir un
fichier de configuration `_config.yml`. Si vous avez déjà choisi un
thème, il doit être enregistré dedans. Sinon, allez
en
[ajouter un thème à votre site](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll). Ce
thème a un ensemble de règles CSS prédéfinies qui donnent un aspect
correct à votre site sans avoir à le définir vous même.  Ce fichier
doit également contenir à minima le titre de votre site et une
description. Vous pouvez également ajouter des plugins Jekyll par exemple, voici [la doc pour en ajouter](https://jekyllrb.com/docs/plugins/) et [une liste de plugins possibles](https://planetjekyll.github.io/plugins/).


Jekyll utilise [Liquid](https://shopify.github.io/liquid/) pour son templating et Front Matter en tête de fichier pour définir des méta-données en Yaml qui seront utilisables par Liquid.
Ainsi, vous pouvez créer un fichier `index.md` qui aura pour contenu:
```
---
layout: default
title: Accueil
---

Bienvenue !
```
Pour éviter de répéter du code, comme on l'a vu avec les templates mères et filles, Jekyll se sert de [layouts](https://jekyllrb.com/docs/step-by-step/04-layouts/).

Ainsi, vous pouvez créer un dossier `_layouts`
(voir
[ici](pour savoir comment créer un dossier par l'interface Github)) et
créer dedans un fichier `default.html` qui sera le template mère
utilisé pour générer les pages HTML :
```
<!doctype html>
<html lang="fr">
	<head>
		<meta charset="utf-8">
		<title>{{ page.title }}</title>
	</head>
	<body>
		{{ content }}
	</body>
</html>
```

Comme on l'a vu avec Flask, le titre de la page sera remplacé par
celui utilisé dans vos fichiers markdown (la variable `title` du code
au-dessus, et le contenu qui n'est pas dans la partie yaml remplacera
la partie `content`. Notez aussi que le nom du `layout` dans le yaml
est celui du fichier dans le dossier `_layouts`. Vous pouvez donc en
utiliser des différents si besoin pour des pages différentes.
