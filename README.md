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

:info: `> ~/.ssh/id_rsa.pub` redirige la sortie de la commande precedente dans le fichier `~/.ssh/id_rsa.pub`.

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

Dans votre terminal, a l'aide de la commande `cd` naviguez ou vous souhaitez cloner le repository, puis lancez la commande `git clone git@github.com:jdel/tp2-git.git`.

```
❯ git clone git@github.com:jdel/tp2-git.git
Cloning into 'tp2-git'...
remote: Enumerating objects: 12, done.
remote: Counting objects: 100% (12/12), done.
remote: Compressing objects: 100% (8/8), done.
remote: Total 12 (delta 3), reused 12 (delta 3), pack-reused 0
Receiving objects: 100% (12/12), 7.97 KiB | 7.97 MiB/s, done.
Resolving deltas: 100% (3/3), done.
```

Maintenant naviguez dans votre copie locale du repository avec `cd tp2-git`.

## Voir le status

Lancez la commande `git status`.

```
❯ git status
On branch main
nothing to commit, working tree clean
```

Cela signifie que vous etes a jour avec la branche `main` du repository distant.

## Travail collaboratif

Arrangez vous maintenant en groupes de 2 ou 3 pour la suite du TP.

To be continued
