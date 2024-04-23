# Git Workshop

## Introduction

Ce TP est a l'attention des etudiants HETIC.

Dans ce workshop vous allez
  - Installer Git CLI sur votre machine
  - Creer une paire de clef SSH (Secure SHell)
  - Creer un fork de ce repo GitHub
  - Apprendre a lire de la documenation
  - Ajouter, supprimer, modifier des fichiers dans un repo
  - Creer et resoudre des confltits de merge
  - Apprendre a resoudre les conflits

## Rappel sur les commandes de terminal

Liste non exhaustive de commandes de terminal utiles:

### Afficher le repertoire courant

`pwd` = Print Working Directory

`pwd` Affiche le repertoire courant

### Lister un repertoire ou un fichier

`ls` = List

`ls <nom_du_repertoire | nom_du_fichier>`

Exemple:
```
❯ ls .ssh
total 104
 480519 drwx------  11 jdel  staff   352B Apr 13 16:09 .
 263916 drwxr-x---+ 56 jdel  staff   1.8K Apr 23 14:24 ..
1018810 -rw-r--r--@  1 jdel  staff   493B Mar  9 12:44 config
7199345 -rw-------   1 jdel  staff   3.3K Apr  9 17:11 id_rsa
7199346 -rw-r--r--   1 jdel  staff   736B Apr  9 17:11 id_rsa.pub
7419498 -rw-------   1 jdel  staff   8.9K Apr 17 14:08 known_hosts
```

### Les characteres speciaux dans le terminal

- Le charactere `~` signifie votre repertoire utilisateur, equivalent a la variable d'environment $HOME

- Le charactere `.` dans le terminal
  - `.` peut signifier le repertoire courant: `ls ./quelquechose`
  - `.` devant un nom de repertoire en fait un repertoire cache: `.ssh`
  - `.` comme commande est equivalente a `source`: `. ~/.zshrc` = `source ~/.zshrc`

### Afficher le contenu d'un fichier

`cat` = Concatenate

`cat <nom_du_fichier>`

Exemple:
```
❯ cat .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCi99UOjitlzX7DkXQIzb2/Om0d0f7pqY45JNiqw96uh+6zImzzE3aQ6aSPBmGk0Mqdf/NYkrjEi/iMymluhET3urAcE51fGJt2zhPL0xqztQH3jJ7XRodzuWhvNXmPNpiYF15BeD2PLGkOXUy6HuKoU21g9VKyJhJMbSdz2TJHXVHketPngYc/.....+uP7/E5cYQMd4wD0qnhscnUWapcbZKemXeNVynKCv6/i2XQVpv9Vn2iiWrdKu2vJux2KxfWySUi8JPb2crT579/rpvTa5NEvwB3LxnQv9Ol5MbnZzQkPig3XQxT0SteiBOWuFwWE5GQhEU2grKumfPNg8LlJ7JtJcs6bV1ANrfde/mkz9aEqM4+/uULv+Jdy8R56RwSEj+Qm1hylV7VZsaVw== jdel@jwork
```

### Naviguer dans les repertoires

`cd` = Change Directory

Entrer dans le repertoire tp2-git `cd tp2-git`

Remonter d'un repertoire `cd ..`

Revenir a la racine du home `cd`

### Creer un repertoire

`mkdir` = Make Directory

`mkdir <nom_du_repertoire>`

Cree un repertoire dans le repertoire courant

### Ouvrir le finder (Mac uniquement)

`open <nom_du_repertoire>`

### Ouvrir avec VSCode

`code <nom_du_repertoire | nom_du_fichier>`

Sur Mac, pour que cette commande fonctionne, dans VSCode faire CMD + Shift + P puis taper `SHELL`.

Selectionner "Install 'code' command in PATH". Mettre un mot de passe admin. Redemarrer le terminal.

### Supprimer un fichier ou repertoire

`rm` = Remove

:warning: Il n'y a pas de corbeille ni de CTRL + Z !

Supprimer un fichier `rm <nom_du_fichier>`

Supprimer un repertoire `rm -rf <nom_du_repertoire>`

`-r` pour recursif, `-f` pour force

### Creer un alias terminal

`alias g="git"`

Uniquement valable pour la session en cours.

L'ecrire dans votre fichier `~/.zshrc` pour rendre l'alias permanent.

## Etape 1 - Installer le client Git

Suivre les instructions officielles Pour votre platforme specifique.
  - [Installation Linux](https://git-scm.com/download/linux)
  - [Installation Mac](https://git-scm.com/download/mac)
  - [Installation Windows](https://git-scm.com/download/win)

Pour le serveur Git, nous allons utiliser GitHub avec lequel vous etes deja un peu familiers.

## Etape 2 - Creer une paire de clefs SSH

Afin de s'authentifier aupres de GitHub pour pousser du code avec Git, nous allons generer une paire de clefs SSH.

Sur Mac et Linux, ouvrez un terminal.

Sur Windows, ouvrez un terminal Git Bash.

### Verifier si vous avez deja des clefs

Lancer la commande `ls ~/.ssh`. Si vous voyez des fichiers `~/.ssh/id_rsa` et `~/.ssh/id_rsa.pub`, une paire de clef est deja presente, vous pouvez la reutiliser.

Si vous avez deja une paire de clef, vous voulez eviter de reecrire par dessus et la remplacer par une nouvelle, car si vous n'en avez pas fait une copie, la clef perdue ne sera pas recuperable.

Il n'est pas non plus possible d'en regenerer une identique, c'est la tout l'interet de ces paires de clefs cryptographiques.

Allez a l'etape `2b`.

### Creer les clefs

Lancer la commande `ssh-keygen -t rsa -b 4096` qui va generer une clef SSH de type `RSA` de longueur 4096 bits.

Vous pouvez egalement chiffrer cette clef avec un passphrase. Pour les besoins de ce TP, laissez le vide.

La clef sera generee dans une paire de fichiers nommes `~/.ssh/id_rsa` et `~/.ssh/id_rsa.pub`

La clef privee est `~/.ssh/id_rsa`, elle doit absolument rester privee comme son nom l'indique.

Si vous l'ouvres, vous noterez qu'elle commence par un bloc `-----BEGIN OPENSSH PRIVATE KEY-----`.

La clef publique `~/.ssh/id_rsa.pub`, elle, a vocation a etre partagee.

Elle a un format similaire a `ssh-rsa AAAAB3....ZsaVw== user@machine`.

Copiez maintenant la clef publique, nous allons en avoir besoin.

## Etape 2b - Mettre la paire de clef en securite

Votre paire de clefs doit rester dans `~/.ssh`, mais vous pouvez (devez) la copier en lieu sur. Pour ce TP, votre bureau fera l'affaire...

Si vous developpez regulierement et poussez du code sur Git a l'aide du protocole SSH, considerez la sauvegarder dans un password manager, ou meme l'imprimer sur papier et la garder en lieu sur.

Au cas ou vous viendriez a perdre votre clef publique, il est possible de la regenerer si vous possedez toujours la clef privee avec le parametre `-y` de la commande `ssh-keygen`.

Par exemple: `ssh-keygen -y -f ~/.ssh/id_rsa > ~/.ssh/id_rsa.pub`

Note: `> ~/.ssh/id_rsa.pub` redirige la sortie de la commande precedente dans le fichier `~/.ssh/id_rsa.pub`.

## Etape 3 - Dire a GitHub que cette clef vous appartient

Dans l'interface web de GitHub, aller dans les [Settings](https://github.com/settings/profile) (Icone de profile en haut a droite puis clicker sur settings).

Clicker sur [GPG and SSH keys](https://github.com/settings/keys) dans le menu a gauche.

Clicker sur [New SSH key](https://github.com/settings/ssh/new).

Lui donner un titre et copier dans le champ `key` le contenu de votre clef __publique__.

Enfin clicker sur Add SSH key.

## Etape 4 - Utiliser la fonction Fork de GitHub

Une des fonctionnalites de Github s'appelle le `Fork`, fourchette en anglais.

Le fork permet de creer une copie d'un repository vers votre compte.

Appuyer sur le bouton fork en haut a droite, puis `Create fork`.

Vous serez automatiquement rediriges sur votre fork du repository tp2-git.

Maintenant clickez sur le bouton `Code` vert, puis selectionnez `SSH` et copiez l'URL `git@github.com:votrenom/tp2-git.git`.

## Etape 5 - Tester un git clone avec le protocole SSH

Dans votre terminal, a l'aide de la commande `cd` naviguez ou vous souhaitez cloner le repository, puis lancez la commande `git clone git@github.com:votrenom/tp2-git.git`.

La premiere fois que vous vous connectez a GitHub avec le protocole SSH il vous sera demande de valider la clef publique de GitHub.

C'est la responsabilite du client de verifier que la clef publique du serveur ou vous vous connectez est bien celle qui est definie dans [la documentation](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints).

Acceptez en entrant `yes`

```
Cloning into 'tp2-git-fork-test'...
The authenticity of host 'github.com (140.82.121.3)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
remote: Enumerating objects: 12, done.
remote: Counting objects: 100% (12/12), done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 12 (delta 3), reused 12 (delta 3), pack-reused 0
Receiving objects: 100% (12/12), 5.03 KiB | 5.03 MiB/s, done.
Resolving deltas: 100% (3/3), done.
```

Maintenant naviguez dans votre copie locale du repository avec `cd tp2-git`.

## Etape 6 Voir le status

Lancez la commande `git status`.

```
❯ git status
On branch main
nothing to commit, working tree clean
```

Cela signifie que vous etes a jour avec la branche `main` du repository distant.

## Etape 7 Tout d'abord Git config

Avant de pouvoir commit et pousser des changements de code, il est important de configurer Git avec son nom et adresse email.

Le nom et email chosis sont arbitraire, ce qui est problematique. Git n'authentifie pas les utilisateurs !

Cela a entraine la signature des commits avec GPG pour la plupart des gros projets open source.

```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

## Etape 8 Resynchroniser votre fork

Si le repo original tu TP2 (upstream en anglais) recoit des modifications, alors votre fork vous affichera un message similaire a: `This branch is 1 commit behind HETIC-DevOps-2024/tp2-git:main.`

Appuyez sur le bouton `Sync Fork`.

Il est possible re le synchroniser egalement avec le terminal en suivant:
  - [Configurer un upstream](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-repository-for-a-fork)
  - [Synchroniser un fork avec le CLI](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork#syncing-a-fork-branch-from-the-command-line)

## Etape 9 Ajouter du contenu au repository

Ouvrez le fork du repo `tp2-git` avec votre editeur favori, par exemple la commande `code .` pour l'ouvrir avec VSCode.

En parallele, toujours dans un terminal (ou Git Bash sur windows) a l'interieur de votre fork du repo `tp2-git`, lancez la commande `npx create-docusaurus@latest my-website classic` pour creer un site docusaurus.

Choisissez le language Javascript.

En utilisant la commande `ls` vous pouvez voir qu'un nouveau repertoire `my-website` est maintenant present.

```
❯ ls
total 24
8689154 drwxr-xr-x   5 jdel  staff   160B Apr 23 16:20 .
7314245 drwxr-xr-x@  8 jdel  staff   256B Apr 23 18:09 ..
8689155 drwxr-xr-x  12 jdel  staff   384B Apr 23 18:30 .git
8689227 -rw-r--r--   1 jdel  staff   8.2K Apr 23 16:18 README.md
8689787 drwxr-xr-x  16 jdel  staff   512B Apr 23 16:21 my-website
```

### Verifier le status

Lancez la commande `git status` et observez le resultat:

```
❯ git status
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        my-website/

nothing added to commit but untracked files present (use "git add" to track)
```

Git voit bien le nouveau repertoire `my-website` qui est `Untracked` c'est a dire qu'il n'est pas gere par git.

:warning: Il est possible que Git vous montre egalement d'autres fichiers, par exemple si vous etes sur Mac vous verrez sans doute un ou plusieurs fichiers `.DS_Store`.

### Ignorer certains fichiers

Il est possible de dire a Git d'ignorer certains fichiers que l'on ne veut jamais versionner.

C'est interssant entre autre pour:
  - Les fichiers de configuration qui contiennent des donnees sensibles (clefs d'API)
  - Les fichiers indesirables des OS (`.DS_Store` sur Mac)
  - Les repertoires contenant des fichiers temporaires ou de cache (`node_modules`, `cache`, `.npm`)
  - Les resultats de build (`dist`, `build`)

En utilisant VSCode, ajoutez un fichier `.gitignore` a la racine du repo tp2-git (pas dans `my-website` car il y en a deja un) et ajoutez sur une ligne `.DS_Store` afin d'ignorer les fichiers indesirables de Mac.

### Ajouter le site docusaurus

Ajoutez le repertoire `my-website` a git en utilisant la commande `git add my-website`.

Cela a pour effet de dire a Git que vous souhaitez a partir de maintenant tracker les changements du repertoire `my-website`.

:warning: Lors de l'ajout de fichier sur Windows, vous pourrez recevoir un warning similaire a celui ci pour chaque fichier:

`warning: in the working copy of 'my-website/.gitignore', LF will be replaced by CRLF the next time Git touches it`

Ceci est du au fait que Windows traite le charactere de fin de ligne (CRLF) de maniere differente de Mac et Linux (LF).

Normalement cela ne devrait pas poser de souci pour la suite du TP.

Voir l'[article Wikipedia sur la fin de ligne](https://fr.wikipedia.org/wiki/Fin_de_ligne) pour une explication plus detaillee.

### Verifier a nouveau le status

```
❯ git status
On branch main
Your branch is up to date with 'origin/main'.

Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   my-website/.gitignore
        new file:   my-website/README.md
        new file:   my-website/babel.config.js
        new file:   my-website/blog/2019-05-28-first-blog-post.md
        new file:   my-website/blog/2019-05-29-long-blog-post.md
        new file:   my-website/blog/2021-08-01-mdx-blog-post.mdx
        new file:   my-website/blog/2021-08-26-welcome/docusaurus-plushie-banner.jpeg
        new file:   my-website/blog/2021-08-26-welcome/index.md
        [...]
        new file:   my-website/static/img/favicon.ico
        new file:   my-website/static/img/logo.svg
        new file:   my-website/static/img/undraw_docusaurus_mountain.svg
        new file:   my-website/static/img/undraw_docusaurus_react.svg
        new file:   my-website/static/img/undraw_docusaurus_tree.svg
        new file:   my-website/tsconfig.json
```

Git montre maintenant chacun des fichiers qu'il va ajouter au repository. Ces fichiers sont dit indexes, ou pret a etre commit.

### Creer le premier commit

Une fois le travail ajoute a l'index (staged) il est temps de le valider (commit) avec la commande:

```
❯ git commit -m "Ajout de my-website"
[main 9e0dbf8] Ajout de my-website
 41 files changed, 16488 insertions(+)
 create mode 100644 my-website/.gitignore
 create mode 100644 my-website/README.md
 create mode 100644 my-website/babel.config.js
 create mode 100644 my-website/blog/2019-05-28-first-blog-post.md
 create mode 100644 my-website/blog/2019-05-29-long-blog-post.md
 create mode 100644 my-website/blog/2021-08-01-mdx-blog-post.mdx
 create mode 100644 my-website/blog/2021-08-26-welcome/docusaurus-plushie-banner.jpeg
 create mode 100644 my-website/blog/2021-08-26-welcome/index.md
 [...]
 create mode 100644 my-website/static/img/favicon.ico
 create mode 100644 my-website/static/img/logo.svg
 create mode 100644 my-website/static/img/undraw_docusaurus_mountain.svg
 create mode 100644 my-website/static/img/undraw_docusaurus_react.svg
 create mode 100644 my-website/static/img/undraw_docusaurus_tree.svg
```

### Verifier a nouveau le status

```
❯ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

Vous pouvez voir qu'il y'a des changements a push avec le message `Your branch is ahead of 'origin/main' by 1 commit.`

La branche `main` est en avance de 1 commit par rapport au repo qui se trouve sur GitHub nomme `origin`.

### Qu'est ce qu'un remote

Un `remote` signifie un serveur distant. Dans notre cas c'est la copie du repository qui se trouve sur GitHub.

Il est possible d'avoir plusieurs remotes pour un seul repository dans certains cas specifiques.

Lorsque l'on clone un repository, la convention veut que le remote d'ou on a clone le repo s'appelle `origin`.

Vous pouvez voir tous les remotes d'un repository ainsi que leur URL de clone avec 

```
❯ git remote -v
origin  git@github.com:votrenom/tp2-git.git (fetch)
origin  git@github.com:votrenom/tp2-git.git (push)
```

### Push les changements

Il est maintenant temps de push les changements sur GitHub avec `git push origin main`.

La commande signifie que l'on souhaite pousser les changements de notre branche `main` sur le `remote` qui s'appelle `origine`.

:star2: New achievement unlocked ! Felicitations, vous venez d'effectuer votre premier push avec le Git CLI !

## Verifier a nouveau le status

```
❯ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Il n'y a rien aucun fichier modifie, il n'y a pas de commit en attente d'etre push. Retour a la case depart, on peut coder une autre feature.

## Etape 10 Travail collaboratif

Arrangez vous maintenant en groupes de 2 ou 3 pour la suite du TP.

### Invitez vos camarades pour travailler sur votre repo

Dans les settings de votre repo https://github.com/votrenom/tp2-git/settings/access ajoutez un ou deux camarades de classe avec leur identifiant GitHub.

Afin d'eviter les problemes de characteres de fin de ligne evoques plus haut, preferrez un camarade qui utilise le meme OS que vous.

Pour les scenarios suivant, nous utiliserons 3 alias pour les noms de chacuns. 

Le premier developpeur sera Alice, le second Bob, et si il y'en a un troisieme, Charlie

Alice, Bob et Charlie vont tous travailler sur un seul repo, celui d'Alice. 

Alice va donc conserver le fork du repo `tp2-git` qu'elle a deja prepare avec le code de base de docusaurus.

Bob et Charlie eux, __vont oublier leur fork pour le moment__ et vont cloner directement le repository de Alice avec l'URL SSH:

`git clone git@github.com:Alice/tp2-git.git tp2-git-alice` (remplacez `Alice` par le username GitHub de votre camarade).

Notez que la commande git contient un parametre suplementaire `tp2-git-alice` qui specifie dans quel repertoire de sortie cloner le repo de Alice.

Bob et Charlie naviguent dans ce nouveau clone avec `cd tp2-git-alice` et l'ouvrent avec VSCode `code .`.

Alice va lancer un nouveau terminal (ou Git Bash) dans lequel elle va se rendre egalement dans le repo du fork `tp2-git`. 

A l'interieur, elle lance la commande `npm start --prefix my-website` qui va demarrer le serveur local du site docusaurus.

Alice peut se rendre sur http://localhost:3000/ pour voir l'etat de son site docusaurus en temps reel.

### Scenario 1

Alice va copier le `README.md` du `tp2-git` comme un nouveau document nomme `git.md` dans son site docusaurus. Pour se faire, Alice peut utiliser VSCode, ou le terminal avec la commande `cp README.md my-website/docs/git.md`

Immediatement, Alice peut se rendre sur http://localhost:3000/docs/intro et voir que le Git Workshop est maintenant dans le menu de gauche.

#### Git add et commit

Dans le terminal, Alice ajoute le fichier a l'index (stage) `git add my-website/docs/git.md`, le commit `git commit -m "Ajout des instructions Git"`.

Alice, Bob et Charlie observent l'etat de leur repository local.

Alice voit:

```
❯ git status
On branch main
Your branch is ahead of 'origin/main' by 1 commit.
  (use "git push" to publish your local commits)

nothing to commit, working tree clean
```

Bob et Charlie voient:

```
❯ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

Les changements d'Alice ne sont pas encore visibles pour Bob et Charlie car pour l'instant ils ne sont que dans le repo local d'Alice.

Alice peut voir qu'elle a des changements a push dans son `git status` avec le message `Your branch is ahead of 'origin/main' by 1 commit.`

La branche d'alice est en avance de 1 commit par rapport au repo `origin` qui se trouve sur GitHub .

#### Git push

Il est maintenant temps pour Alice de push ses changements pour en faire profiter ses camarades `git push origin main`.

Bob et Charlie ne verront aucune difference si ils font un `git status` immediatement, car leur repo local n'est pas encore au courant qu'il y'a eu des changements sur l'`origin`

#### Git fetch

Git permet de recuperer un status du repo `origin` GitHub en effectuant la commande `git fetch`.

La commande `git fetch` a pour effet de recuperer les derniers changements disponibles sur le remote MAIS ne les applique pas sur la branche en cours. 

Bob et Charlie font maintenant un `git fetch` suivi d'un `git status`.

```
❯ git fetch
❯ git status
On branch main
Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.
  (use "git pull" to update your local branch)

nothing to commit, working tree clean
```

Git annonce que la branche `main` sur laquelle Bob travaille est 1 commit en retard par rapport a la branche main d'`origin`

`Your branch is behind 'origin/main' by 1 commit, and can be fast-forwarded.`

Git dit egalement que la branche peut etre merge en `fast forward`. Cela signifie que Git sait qu'aucun changement present sur `origin/main` ne causera de conflit avec les fichiers sur lequel Bob travaille en ce moment.

Un fetch qui dit que la branch peut etre merge en `fast-forwarded` signifie qu'on peut faire un `git pull` en toute securite.

#### Git merge

Bob et Charlie vont maintenant reintegrer le dernier commit d'Alice dans leur branche `main`.

Mais avant d'executer une operation de merge, Bob doit bien s'assurer que tous les fichiers sur lesquels il a apporte des changements sont touc commit. Dans ce scenario, Bob ne doit pour le moment avoir aucun fichier modifie et peut executer un `git merge` la conscience tranquille:

```
❯ git merge
Updating 9b03ecc..7b0c251
Fast-forward
 my-website/docs/git.md | 364 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 1 file changed, 364 insertions(+)
 create mode 100644 my-website/docs/git.md

❯ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

La commande `git merge` [sans aucun parametre](https://git-scm.com/docs/git-merge/fr#git-merge-ltcommitgt82308203), a pour effet de reintegrer les changements de la branche portant le meme nom `main` sur le remote `origin` dans la branche locale correspondante.

C'est un raccourci pour `git merge origin/main`.

Notons que Git a bien effectue un Fast forward car aucun conflit n'a ete detecte.

#### Git pull

De maniere plus generale, `git pull` combine un `git fetch` suivi d'un `git merge` exactement comme Bob vient de le faire manuellement.

Git `pull` est une maniere plus simple de mettre a jour sa copie locale avec les changements venant d'`origin`.
