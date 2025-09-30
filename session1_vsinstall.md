---
title: Session 1 - VS Code, Dev Containers, premier Flask & débogage
nav_order: 1
---

# Objectifs

* Travailler avec **VS Code** et **Dev Containers** (Docker) pour un environnement identique sous Windows/macOS/Linux.
* Créer et exécuter une **application Flask minimale** sur `http://127.0.0.1:5000/`.
* **Récupérer un projet existant** depuis un dépôt Git **contenant déjà** une configuration Dev Container et le tester.
* Utiliser le **débogage VS Code** (breakpoints, step-by-step) sur une app Flask.

---

# Pré-requis

* **Docker Desktop** (ou équivalent) installé **et démarré**.
* **Visual Studio Code** installé.
* Extension **Dev Containers** pour VS Code (éditeur → Extensions → “Dev Containers”).

> **Vérifier Docker est installé (optionnel)**
> Si vous avez un doute, exécutez les commandes ci‑dessous ; elles doivent s’exécuter sans erreur.

**Windows (PowerShell)** ou **macOS (Terminal)**

```powershell
# afficher la version de Docker
docker --version

# tester un conteneur simple
docker run --rm hello-world
```

> Si `docker` n’est pas reconnu, ou si l’image « hello-world » ne s’exécute pas, installez/lancez **Docker Desktop**, puis réessayez.

---

# 1) Ouvrir VS Code et installer *Dev Containers*&#x20;

* Lancer **VS Code**.
* Ouvrir l’onglet **Extensions** (icône “carrés”).
* Rechercher **“Dev Containers”** et cliquer **Installer**.
* Vérifier que **Docker Desktop** tourne (icône baleine visible).

---

# 2) Ajouter une configuration *Dev Container* à un nouveau projet

* Créez un dossier de travail vide, 
* Ouvrir le avec VS Code via **File** → **Open Folder**
* Confiermer la confiance aux auteurs
* Ajoutez un sous-dossier nommé `.devcontainer`
* À l’intérieur du repertoire `.devcontainer` créez le fichier `devcontainer.json` comme ci-dessous. Le résultat attendu est une structure de projet de ce type :

```
mon-projet/
 ├─ .devcontainer/
 │   └─ devcontainer.json
 └─ (futurs fichiers Python, ex. app.py)
```

Copiez-collez ce contenu dans le fichier `devcontainer.json` que vous venez de créer.

```jsonc
{
  // ===============================================
  // Dev Container pour le cours WEBAPP-N
  // - Environnement Python 3.13 (Debian trixie)
  // - Node 20 pour les outils front (ESLint/Prettier/Stylelint/HTMLHint)
  // - Extensions utiles (Python, Docker, YAML, OpenAPI, ESLint, Prettier, Stylelint, Live Share, etc.)
  // - Port 5000 exposé (Flask)
  // ===============================================
  "name": "WEBAPP-N (Python 3.13)",

  // Image de base
  "image": "mcr.microsoft.com/devcontainers/python:3.13-trixie",

  // Ajout d'outils côté conteneur (git, zsh, node 20, etc.)
  "features": {
    "ghcr.io/devcontainers/features/common-utils:2": {},
    "ghcr.io/devcontainers/features/node:1": { "version": "20" }
  },

  // Extensions VS Code installées DANS le conteneur (pour standardiser)
  "customizations": {
    "vscode": {
      "extensions": [
        // Python
        "ms-python.python",
        "ms-python.vscode-pylance",
        "ms-python.black-formatter",

        // Docker / Dev Containers
        "ms-azuretools.vscode-docker",
        "ms-vscode-remote.remote-containers",

        // YAML / OpenAPI
        "redhat.vscode-yaml",
        "42CRUNCH.vscode-openapi",

        // Web (HTML/CSS/JS)
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode",
        "stylelint.vscode-stylelint",
        "ecmel.vscode-html-css",
        "naumovs.color-highlight",

        // Collaboration
        "ms-vsliveshare.vsliveshare"

      ],
      "settings": {
        // Formatage automatique
        "editor.formatOnSave": false,
        "files.trimFinalNewlines": true,
        "files.insertFinalNewline": true,
        "files.trimTrailingWhitespace": true,

        // Python formaté par Black
        "[python]": { "editor.defaultFormatter": "ms-python.black-formatter" },

        // Formatage front par Prettier
        "[javascript]": { "editor.defaultFormatter": "esbenp.prettier-vscode" },
        "[typescript]": { "editor.defaultFormatter": "esbenp.prettier-vscode" },
        "[json]": { "editor.defaultFormatter": "esbenp.prettier-vscode" },
        "[jsonc]": { "editor.defaultFormatter": "esbenp.prettier-vscode" },
        "[markdown]": { "editor.defaultFormatter": "esbenp.prettier-vscode" },
        "[yaml]": { "editor.defaultFormatter": "esbenp.prettier-vscode" },

        // Auto-fix ESLint / Stylelint si possible
        "eslint.format.enable": true,
        "editor.codeActionsOnSave": {
          "source.fixAll.eslint": true,
          "source.fixAll.stylelint": true
        }
      }
    }
  },

  // Après création du conteneur : installer linters/formatters côté container
  "postCreateCommand": "bash -lc 'python -m pip install --upgrade pip && pip install flask black ruff isort && if [ ! -f package.json ]; then npm init -y >/dev/null 2>&1; fi && npm i -D eslint prettier stylelint stylelint-config-standard htmlhint >/dev/null 2>&1 && echo Done'",

  // Expose le port Flask
  "forwardPorts": [5000],
  "portsAttributes": { "5000": { "label": "Flask (dev)" } },

  // Utilisateur du conteneur
  "remoteUser": "vscode"
}
```

Ensuite :

* **View** → **Command Palette...** → **Dev Containers: Reopen in Container**.
* Patientez jusqu’à la fin du build (dépendances auto-installées).

---

# 3) Créer une application Flask minimale

Dans **VS Code (dans l’éditeur, côté conteneur)** :

1. Créez un **nouveau fichier** nommé `app.py` à la racine du projet.
2. Collez le contenu suivant :

```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def home():
    return 'Hello Flask!'

if __name__ == '__main__':
    app.run()
```

Ensuite, dans le **terminal du conteneur** (ouvrez‑le via **Terminal → Nouveau terminal**; vérifiez que la barre d’état affiche **Dev Container** en bas à gauche) :

```bash
# lancer l'application
python app.py
```

* Ouvrir `http://127.0.0.1:5000/` dans le navigateur → **“Hello Flask!”**.

> Astuce : conservez le terminal **dans** le conteneur (badge “Dev Container” en bas à gauche de VS Code).

---

# 4) Récupérer un projet existant (avec Dev Container) depuis Git

1. Palette de commandes → **Dev Containers: Clone Repository in Container Volume…**
2. Collez l’URL du dépôt : 
```
https://github.com/TeachingAndResearch/ue_web_example_session1-git.git
```
3. Le dépôt contiendra un fichier `.devcontainer/devcontainer.json`, alors VS Code utilisera cette configuration **automatiquement**.
4. VS Code va **créer un volume Docker**, **cloner le dépôt à l’intérieur du conteneur** et **ouvrir l’espace de travail** directement **dans** le conteneur. Aucune installation de Git n’est nécessaire sur Windows/macOS.

**Notes de compatibilité**

* **macOS Intel & Apple Silicon (M1/M2/M3)** : les images Dev Containers officielles (dont `mcr.microsoft.com/devcontainers/python:3.13-trixie`) sont **multi‑architecture**. Sur Apple Silicon, elles tirent l’image **arm64**.&#x20;
* **Espace disque** : le clonage en **Container Volume** crée un volume persistant (visible dans Docker Desktop → Volumes). Pensez à le supprimer quand vous n’en avez plus besoin.

---

# 5) Tester le projet récupéré

```bash
# installer des dépendances supplémentaires
python -m pip install -r requirements.txt
```

```bash
# lancer l'application (exemple simple)
python app.py
```

* Vérifier la page d’accueil dans le navigateur&#x20;
* Ouvrir `http://127.0.0.1:5000/` dans le navigateur.
* Arretez l'execution en tappant Ctrl+C dans le terminal.

---

# 6) Déboguer avec VS Code (breakpoints)

Le plus simple pour débuter : un **launch** “Python: Fichier actuel”.

1. Ouvrir `app.py`.
2. Placer un **point d’arrêt** (clic dans la gouttière à gauche d’une ligne) sur la ligne 19 (`return flask.render_template("homepage.html.jinja2",`)
3. Cliquez sur le bouton **Run or Debug** (icône “triangle” en haut droite).
4. Choisir **Python Debugger: Debug Python File** et lancer.
5. Ouvrir `http://127.0.0.1:5000/` dans le navigateur.

> À l’arrêt sur un breakpoint : survoler les variables, utiliser **Step Over / Step Into / Continue**.

* Arretez la deboggage en cliquant sur le rectangle rouge en haut (**Stop**)

### Configuration (optionnelle) `launch.json`

> À mettre dans `.vscode/launch.json` si vous voulez une config dédiée.

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: app.py",
      "type": "python",
      "request": "launch",
      "program": "${workspaceFolder}/app.py",
      "console": "integratedTerminal",
      "justMyCode": true
    }
  ]
}
```

* Ouvrir l’onglet **Run or Debug** (icône “triangle” en haut droite) pour trouver la configuration faite, nommée « Python: app.py ».

### Auto-rechargement (dev server Flask)

C'est possible de configurer le serveur en mode auto-rechargement, qui permet la prise en compte des changement dans les fichiers du project automatiquement (il faut juste faire un refresh de ma page dans le navigateur) sans arret et redemarrage du serveur.

Il faut mettre les variables d'environment comme suite :

```bash
# activer le mode debug Flask (rechargement automatique)
export FLASK_APP=app.py
export FLASK_DEBUG=1
```

Puis lancer `flask run` (à la place de `python app.py`):

```bash
flask run
```

* Naviguer à `http://127.0.0.1:5000/`.
* Modifiez le fichier `templates/homepage.html.jinja2` en ajoutant après ligne 3 la nouvelle ligne 4 :
```text
    <h1> So dynamic, Much WOW </h1>
```
* À chaque sauvegarde, le serveur se relance et vos modifications s’appliquent.
* Faite un refresh de la page dans le navigateur et le nouvel texte va apparaitre.

> Note : en mode `flask run`, pour déboguer:
> * Ouvrir l’onglet **Run or Debug**
> * Selectionner l'option **Python Debugger..** → **Python Debugger: Flask**


---

# 7) Critères de réussite

* Le conteneur **se reconstruit** et **s’ouvre** (aucune erreur bloquante).
* L’app Flask minimale **répond** sur `http://127.0.0.1:5000/`.
* Un **breakpoint** est atteint en débogage VS Code.
* Un projet cloné **avec** `.devcontainer/` tourne et s’ouvre dans VS Code Dev Container.

---

# 8) Problèmes fréquents & dépannage

* **Docker Desktop non lancé** → le build Dev Container échoue ou ne démarre pas.
* **Port 5000 occupé** → changer de port ou fermer le processus en cours.
* **Dossier incorrect** ouvert dans VS Code → le bouton “Reopen in Container” n’apparaît pas.
* **Proxy/réseau** → l’installation des dépendances peut échouer ; réessayer plus tard ou configurer le proxy.
* **Breakpoints non atteints** → vérifier que vous exécutez bien **dans le conteneur** et que vous lancez via “Python: Fichier actuel”.

---

# 9) Pour aller plus loin (facultatif)

* Utiliser **Black** (déjà présent dans notre conteneur) pour automatiquement formatter un fichier python:
    * Ouvrir `app.py`
    * Cliquez avec le alternatif bouton du souris (alt-click) dans le document
    * Cliquez sur **Format Document** dans le menu

---
