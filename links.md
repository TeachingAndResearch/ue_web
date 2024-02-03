---
title: Documents annexes
excerpt: ""
nav_order: 11
---

# Documents annexes

- slides du cours : [cours](/assets/pdf/slides.pdf)
- slides pre-bootstrap : [html-css](https://0xc0de.fr/courses/Domaine/2018/slides/html-css/)
- slides javascript/ajax : [javascript](https://0xc0de.fr/courses/Domaine/2018/slides/js-ajax/)

# Projet 2023-2024: TBD

- cahier des charges pour le projet : TBD<!--[UE-Web-CdC-2021.pdf](/assets/pdf/UE-Web-CdC-2022.pdf)-->


# Liens utiles

## Les docs

- <https://developer.mozilla.org/en-US/docs/Web> Des infos générales sur le web faites par Mozilla (les standards notamment)
- <https://www.w3schools.com/tags/tag_font.asp> Plein d'infos w3schools pour le html/css/js/python
- <https://flask.palletsprojects.com/en/1.1.x/> La doc de Flask
- <https://jinja2docs.readthedocs.io/en/stable/index.html> Jinja2
- <https://docs.sqlalchemy.org/en/13/orm/index.html> SQL alchemy
- <https://flask-wtf.readthedocs.io/en/stable/form.html#module-flask_wtf.file> La doc pour les formulaires/téléversement de fichiers/tokens/Captcha utilisant flask-wtf
- <https://getbootstrap.com/docs/5.0/> Bootstrap

## Spécifiques

- <https://github.com/github/gitignore/blob/master/Python.gitignore> gitignore tout prêt pour vos projets python

Petits tutos sur comment gérer les utilisateurs en Flask avec Flask-login :
- <https://realpython.com/using-flask-login-for-user-management-with-flask/>
- <https://pythonbasics.org/flask-login/>
- <https://www.digitalocean.com/community/tutorials/how-to-add-authentication-to-your-app-with-flask-login>
- Si aucun des liens n'est utile, vous pouvez aller voir [ce petit exemple](https://github.com/Marie-Donnie/ue_web_example/archive/authentication_example.zip).

## Aller plus loin

- <https://ogp.me/> Doc d'Open Graph Protocol pour les intégrations

### Création de palettes

- <https://coolors.co/989788-51344d-6f5060-a78682-e7ebc5>
- <https://paletton.com/#uid=14w0u0khnof7sCjcitnl+jMrwee>
- <https://color.adobe.com/create/color-wheel>
- <https://flatuicolors.com/>
- <https://www.colourlovers.com/palettes/>

### Accessibilité

- <https://www.gravityforms.com/web-accessibility/>
- <https://www.w3.org/WAI/fundamentals/accessibility-intro/>
- <https://davidmathlogic.com/colorblind/> Spécifiquement pour les daltoniens
- <https://www.color-blindness.com/coblis-color-blindness-simulator/> Tester si une image est visible pour des daltoniens


# Troubleshooting

Quelques guides pour vous aider à débugger, bien sûr, d'autres solutions existent sûrement.


- S'il vous manque une fonction flask, vérifiez que vous avez en en-tête de votre fichier `app.py` :

```python
import flask

app = flask.Flask(__name__)
```

Puis, ajoutez `flask.` avant la fonction. Cela arrive avec les projets pré-générés par Pycharm.

- Accéder aux dev tools sur Firefox et Chromium (et dérivés) : `F12`. C'est aussi ici (entre autres) que vous pouvez trouver le bouton de responsive design.


- Si vous avez des problèmes d'affichage après changements (le changement n'a pas l'air d'apparaître sur la page) :
  - <https://www.howtogeek.com/672607/how-to-hard-refresh-your-web-browser-to-bypass-your-cache/>
  - <https://www.minitool.com/news/clear-cache-for-one-site.htm>
  Vous pouvez aussi désactiver le cache quand les devtools sont utilisés :
  - <https://themightymo.com/how-to-disable-browser-cache-in-firefox-dev-tools/>
  - <https://stackoverflow.com/questions/46341392/chrome-devtools-disable-cache-what-does-it-actually-do-apart-from-nothing-fo>
