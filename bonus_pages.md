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
- [Blogging with Jeykll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll), une page qui vous redirige vers différents sujets, comme comment créer votre site avec Jekyll, comment le tester localement (vous aurez besoin des étapes 2 à 5), comment ajouter du contenu, un thème, etc.
- [Custom URLs](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) qui
  va vous expliquer comment vous pouvez avoir votre nom de domaine à
  vous (exemple: monnom.fr). **Attention, cela implique de payer un nom
  de domaine à côté.**
