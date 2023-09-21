# TP0 PREPARATION - Création compte gitlab.com et configuration Git en local

> **Objectifs du TP**
> * Créer, si ce n'est pas déjà fait, un compte sur gitlab.com
> * Configurer les paramètres globaux de Git en local
> 

> Apparté: Pour une meilleure lisibilité des fichiers .md, je vous conseille de télécharger VS Code. Une fois le fichier .md ouvert, la combinaison de touches "*Ctrl - maj - V*"  vous permettra d'ouvrir une version mise en forme et beaucoup plus facilement lisible.

# Création d'un compte sur gitlab

Aller sur le site [https://gitlab.cloudbakery.fr/](https://gitlab.cloudbakery.fr/) et cliquer sur `Register`. En dehors de ce cours, je vous incite à visiter [https://gitlab.com/](https://gitlab.com/), qui est le site officiel de Gitlab (nécessitant une carte bancaire valide pour utiliser les fonctionnalités de CI/CD, raison pour laquelle nous ne l'utiliserons pas dans ce contexte). [https://gitlab.cloudbakery.fr/](https://gitlab.cloudbakery.fr/) est une version self-host de Gitlab mise à votre disposition le temps de ce cours. L'utilisation de fonctionnalités de CI/CD est gratuite mais la plateforme sera supprimée d'ici quelques semaines/mois. 

Remplir le formulaire de création de compte, 
- sur [https://gitlab.com/](https://gitlab.com/) après avoir soumis le formulaire, consultez votre boite email et **cliquez sur le lien d'activation reçu de la part de gitlab pour activer votre compte**. Vous devez avoir un compte activé pour pouvoir utiliser les runners partagés. 
- sur [https://gitlab.cloudbakery.fr/](https://gitlab.cloudbakery.fr/), si vous voyez que votre compte n'est pas validé après quelques refresh du navigateur, indiquez moi que votre compte est créé pour que je l'approuve manuellement.

> Votre nom d'utilisateur gitlab (username) est important car vous l'utiliserez tout au long des TP pour vous connecter sur gitlab, pensez donc à le noter.
 

# Configuration de Git sur votre poste local

## Configuration de l'environnement local pour annoter les commits

La première chose à faire sera d'installer git. Je vous renvoie sur ce lien pour trouver le bon moyen d'installation selon votre environnement de travail: 
https://git-scm.com/book/fr/v2/Démarrage-rapide-Installation-de-Git

Il est cependant nécessaire de personnaliser votre environnement Git pour indiquer à Git comment annoter vos commits par exemple. 
Vous ne devriez avoir à réaliser ces réglages qu’une seule fois ; ils persisteront lors des mises à jour. Vous pouvez aussi les changer à tout instant en relançant les mêmes commandes.

> Git propose la commande `git config` pour vous permettre de voir et modifier les variables de configuration qui contrôlent le comportement de Git. Ces variables peuvent être stockées dans trois endroits différents :
> - Fichier `/etc/gitconfig` : Contient les valeurs pour tous les utilisateurs et tous les dépôts du système. Si vous passez l’option `--system` à git config.
> - Fichier `~/.gitconfig` : Spécifique à votre utilisateur. Vous pouvez forcer Git à lire et écrire ce fichier en passant l’option `--global`.
> - Fichier `config` dans le répertoire Git (c’est-à-dire `.git/config`) du dépôt en cours d’utilisation : spécifique au seul dépôt en cours.
> 
> Sur les systèmes Windows, Git cherche le fichier `.gitconfig` dans le répertoire $HOME (%USERPROFILE% dans l’environnement natif de Windows)

```bash
$ git config --global user.name "<Prenom Nom>" # exemple: git config --global user.name "Thomas PETETIN"
$ git config --global user.email <votre email> # exemple: git config --global user.email thomas.petet1@myges.fr
$ git config --global color.ui true
```

Exemple de résutat de fichier `~/.gitconfig`

```ini
[core]
	editor = /usr/bin/vim
[user]
	email = tpetetin1@myges.fr
	name = Thomas PETETIN
[ui]
	color = true
```


# Installation de votre clé SSH dans votre compte Gitlab

Si vous essayez de cloner ou pousser du code sur Gitlab (ne le faites pas) vous obtiendrez probablement l'erreur suivante :
```bash
git push -u origin --all
Warning: Permanently added 'gitlab.com,35.231.145.151' (ECDSA) to the list of known hosts.
git@gitlab.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

Cette erreur est liée au fait qu'on utilise le protocole `git` pour envoyer les données du dépôt sur gitlab. Celui-ci se base à son tour sur le protocole SSH. Pour s'authentifier sur gitlab, il est nécessaire d'avoir configuré au préalable sur gitlab sa clé SSH.

Vous devrez donc générer votre clé SSH si ce n'est pas déjà fait afin de l'importer sur gitlab.com, pour cela je vous renvoie sur ce tuto : 
https://docs.gitlab.com/ee/ssh/ (sur Windows vous pouvez utiliser PuTTY mais ssh est censé être présent sur tous les PC Windows depuis quelques années)

Depuis l'interface gitlab, cliquez alors sur le bouton de votre profil en haut à droite de la sidebar, puis cliquez sur `Settings`.

- la clé privée, que vous ne devez jamais sortir de votre machine se trouve dans `~/.ssh/id_ed25519`
- la clé publique, que vous pouvez utiliser sans soucis se trouve dans `~/.ssh/id_ed25519.pub`

Peu importe si le format de votre clé difére un peu, que ce soit "id_rsa" ou autre.

Vous pouvez afficher votre clé publique avec cette commande : 

```bash
$ cat ~/.ssh/id_ed25519.pub
ssh-ed25519 AAAAC3... thomas@PC-01
```
En remplaçant `cat` par `type`sous Windows

Puis allez dans la page `SSH Keys` :

Copier-coller maintenant **la partie publique** de **votre clé SSH** dans cette interface puis valider. 
Il est maintenant temps de retester d'envoyer votre dépôt sur Gitlab.

Les prérequis sont réalisés, bravo !!

# Création de votre premier projet

Maintenant que vos identifiants sont configurés en local et en remote, vous allez pouvoir créer votre premier projet Git (qui ne vous servira pas par la suite mais c'est un autre problème...).

Depuis l'interface [Gitlab](https://gitlab.cloudbakery.fr), créez un nouveau projet vierge. Faites attention à décocher la case "Initiate with a Readme File".

Une fois le projet créé, s'il est bien vide, il vous donnera certaines instruction selon que :
* Télécharger ce projet (vide) sur votre poste local,
* Uploader un répertoire local vers ce repository distant,
* Uploader un repository local sur ce repository distant.

Une fois que vous aurez choisi une des 3 options, vous pouvez vous amuser avec une des nombreuses commandes qu'offre Git, telles que :
* git init
* git push
* git commit
* git add
* ... (n'hésitez pas à regarder la doc, ça ne coûte rien et ça donne des bonnes habitudres :)