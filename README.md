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

`pwd` Affiche le repertoire courant

### Lister un repertoire ou un fichier

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

### Les characteres speciauz dans le terminal

- Le charactere `~` signifie votre repertoire utilisateur, equivalent a la variable d'environment $HOME

- Le charactere `.` dans le terminal
  - `.` peut signifier le repertoire courant: `ls ./quelquechose`
  - `.` devant un nom de repertoire en fait un repertoire cache: `.ssh`
  - `.` comme commande est equivalente a `source`: `. ~/.zshrc` = `source ~/.zshrc`

### Afficher le contenu d'un fichier

`cat <nom_du_fichier>`

Exemple:
```
❯ cat .ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCi99UOjitlzX7DkXQIzb2/Om0d0f7pqY45JNiqw96uh+6zImzzE3aQ6aSPBmGk0Mqdf/NYkrjEi/iMymluhET3urAcE51fGJt2zhPL0xqztQH3jJ7XRodzuWhvNXmPNpiYF15BeD2PLGkOXUy6HuKoU21g9VKyJhJMbSdz2TJHXVHketPngYc/.....+uP7/E5cYQMd4wD0qnhscnUWapcbZKemXeNVynKCv6/i2XQVpv9Vn2iiWrdKu2vJux2KxfWySUi8JPb2crT579/rpvTa5NEvwB3LxnQv9Ol5MbnZzQkPig3XQxT0SteiBOWuFwWE5GQhEU2grKumfPNg8LlJ7JtJcs6bV1ANrfde/mkz9aEqM4+/uULv+Jdy8R56RwSEj+Qm1hylV7VZsaVw== jdel@jwork
```

Remonter d'un repertoire `cd ..`

Revenir a la racine du home `cd`

### Creer un repertoire

`mkdir <nom_du_repertoire>`

Cree un repertoire dans le repertoire courant

### Ouvrir le finder (Mac uniquement)

`open <nom_du_repertoire>`

### Ouvrir avec VSCode

`code <nom_du_repertoire | nom_du_fichier>`

Sur Mac, dans VSCode CMD + Shift + P puis taper `SHELL`.

Selectionner "Install 'code' in command PATH". Mettre un mot de passe admin. Redemarrer le terminal.

### Supprimer un fichier ou repertoire

:warning: Il n'y a pas de corbeille ni de CTRL + Z !

Supprimer un fichier `rm <nom_du_fichier>`

Supprimer un repertoire `rm -rf <nom_du_repertoire>`

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

## Tout d'abord Git config

Le nom et email chosis sont arbitraire, ce qui est problematique

Cela a entraine la signature des commits avec GPG pour la plupart des gros projets open source.

```
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com
```

## Resynchroniser votre fork

Si le repo original tu TP2 (upstream en anglais) recoit des modifications, alors votre fork vous affichera un message similaire a: `This branch is 1 commit behind HETIC-DevOps-2024/tp2-git:main.`

Appuyez sur le bouton `Sync Fork`.

Il est possible re le synchroniser egalement avec le terminal en suivant:
  - https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-repository-for-a-fork
  - https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork#syncing-a-fork-branch-from-the-command-line

## Travail collaboratif

Arrangez vous maintenant en groupes de 2 ou 3 pour la suite du TP.

To be continued
