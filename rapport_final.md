---
title: "Rapport Final SAE 2.03"
author: [MIREY Kellian, TAS Atilla]
date: "3-26-2023"
subject: "Markdown/Pandoc"
keywords: [Rapport, Markdown, Pandoc]
lang: "fr"

toc: true
toc-own-page: true
colorlinks: true
titlepage: true

titlepage-logo: "ressources/Logo-IUT-de-Lille.png"
page-background: "ressources/background-page.png"

header-includes:
- |
  ```{=latex}
  \usepackage{tcolorbox}

  \newtcolorbox{info-box}{colback=cyan!5!white,arc=0pt,outer arc=0pt,colframe=cyan!60!black}
  \newtcolorbox{warning-box}{colback=orange!5!white,arc=0pt,outer arc=0pt,colframe=orange!80!black}
  \newtcolorbox{error-box}{colback=red!5!white,arc=0pt,outer arc=0pt,colframe=red!75!black}
  ```
pandoc-latex-environment:
  tcolorbox: [box]
  info-box: [info]
  warning-box: [warning]
  error-box: [error]
...

[`pandoc-latex-environments`]: https://github.com/chdemko/pandoc-latex-environment/
[`tcolorbox`]: https://ctan.org/pkg/tcolorbox

# Préparation d'une machine virtuelle Debian

Nous avons créé une machine virtuelle avec son système d'exploitation Debian 11 et l'environnement graphique MATE, et au moins 2 utilisateurs et quelques logiciels de départ.

## Caractéristiques

| Nom de la machine | sae203 |
|---|---|
*Type* | Linux |
*Version* | Debian 11 64-bit |
*Mémoire vive (RAM)* | 2048 Mo |
*Disque dur* | 20 Go |

## Installation

À l'aide de l'interface graphique de VirtualBox:

- [ ] Nom de la machine : `serveur`

- [ ] Domaine : `-`

- [ ] Pays / Langue : `France`

- [ ] Miroir : <http://debian.polytech-lille.fr>

- [ ] Proxy : <http://cache.univ-lille.fr:3128>

- [ ] Compte administrateur :
  - [x] login : `root`
  - [x] mot de passe : `root`

- [ ] Compte utilisateur :
  - [x] nom : `User`
  - [x] login : `user`
  - [x] mot de passe : `user`

- [ ] 1 seule partition pour tout le disque

- [ ] Logiciels de démarrage :
    - [x] environnement de bureau Debian
    - [x] MATE
    - [x] serveur web
    - [x] serveur ssh
    - [x] utilitaire usuels du système

::: warning
**Warning**: Redémarage du système sans le fichier iso d'installation.
:::

## Préparation du système

### Accès `sudo` pour user

Pour simplifier la gestion du système, nous avons ajouter à `user` le groupe `sudo`

::: info
Passage en mode console : `Ctrl`+`Alt`+`F1`

Connexion en root: login = `root` et mot de passe = `root`
:::

```shell
sudo adduser user sudo
```

![ ](img/adduser_user_sudo.png)

::: warning
**Warning**: Rechargement de la session pour appliquer les changements.
:::

### Installation des suppléments invités

- Montage du CD
  
```shell
sudo mount /dev/cdrom/mnt
```

- Installation des suppléments :

```shell
sudo /mnt/VBoxLinuxAdditions.run
```

## Culture générale

### Configuration matérielle dans VirtualBox

#### 64-bit

- **Q: Que signifie “64-bit” dans “Debian 64-bit” ?**

- **R:** Représente le traitement des données et instructions que peut faire le système, ici, cela serait par paquets de 64 bits.

- **Q: Quelle est la configuration réseau utilisée par défaut ?**

- **R:** La configuration réseau utilisée par défaut:

![ ](img/network.png)

- **Q: Quel est le nom du fichier XML contenant la configuration de votre machine ?**

- **R:** Le nom du fichier est sae203.vbox.

- **Q: Sauriez-vous le modifier directement ce fichier pour mettre 2 processeurs à votre machine ?**

- **R:** Oui, il suffit de modifier la valeur de la variable "CPU count" qui est par défaut à 1 et la mettre à 2.

![ ](img/cpu_count.jpg){width="500px"height="auto"}

### Installation OS de base

#### Un fichier iso bootable

- **Q: Qu’est-ce qu’un fichier iso bootable ?**

- **R:** Un fichier image créé à partir d'un CD ou d'un DVD auquel on peut faire démarrer une machine dessus.

#### MATE / GNOME

- **Q: Qu’est-ce que MATE ? GNOME ?**
- **R:** Environnement de bureau
  
**Source :** [MATE](https://wiki.debian.org/fr/MATE), [GNOME](https://wiki.debian.org/fr/Gnome)

#### Serveur web

- **Q: Qu’est-ce qu’un serveur web ?**
- **R:** Un logiciel qui comprend les URL et le protocole HTTP.
  
**Source :** [Mozilla](https://developer.mozilla.org/fr/docs/Learn/Common_questions/Web_mechanics/What_is_a_web_server)

#### Serveur SSH

- **Q: Qu’est-ce qu’un serveur ssh ?**
- **R:** Un protocole qui facilite les connexions sécurisées entre deux systèmes à l'aide d'une architecture client/serveur et permet aux utilisateurs de se connecter à distance à des systèmes hôte de serveurs.

#### Serveur mandataire

- **Q: Qu’est-ce qu’un serveur mandataire ?**
- **R:** Un serveur informatique qui a pour fonction de relayer des requêtes entre un poste client et un serveur.

### Préparation du système

#### Sudo

- **Q: Comment peux-ton savoir à quels groupes appartient l’utilisateur user ?**

- **R:** Grâce à la commande :

```shell
groups
```

![ ](img/groups.png)

### Suppléments invités

#### Version du noyau Linux

- **Q: Quel est la version du noyau Linux utilisé par votre VM ?**

- **R:** La version du noyau Linux utilisé est 11.6.0

#### Utilités suppléments invités

- **Q: À quoi servent les suppléments invités ?**

- **R:** Ils contiennent des pilotes de périphériques et des applications système qui optimisent le système d'exploitation ou améliore l'interaction entre la machine hôte et invitée ; Par exemple la connexion automatisée ou l'intégration du pointeur de la souris.

**Sources :** [Oracle](https://docs.oracle.com/cd/E26217_01/E35193/html/qs-guest-additions.html)

#### Mount ?

- **Q: À quoi sert la commande `mount` ?**

- **R:** Sert à monter un système de fichier sur un répertoire donné en paramètre ce qui rendra le système de fichier accessible.
  
**Sources :** [IBM](https://www.ibm.com/docs/fr/power8?topic=commands-mount-command)

# Installation Debian automatisée par pré-configuration

## À propos de la distribution Debian

- **Q: Qu’est-ce que le Projet Debian ? D’où vient le nom Debian ?**

- **R:** C'est l'association d'individus qui ont eu pour but de créer un système d'exploitation libre. Le nom Debian vient des noms du créateur de Debian, Ian Murdock, et de sa femme, Debra.

**Source :** [Debian](https://www.debian.org/intro/about)

### La maintenance

\ Il existe 3 durées de prise en charge (support) de ces versions : la durée minimale, la durée en support long terme (LTS) et la durée en support long terme étendue (ELTS).

- **Q: Quelle sont les durées de ces prises en charge ?**

- **R:** La durée minimale est la date de publication de la prochaine version stable + 1 an
    La durée en support long terme (LTS) est d'au moins 5 ans et 5 ans de plus pour une ELTS.

**Sources :** Debian
    [durée minimale](https://wiki.debian.org/fr/DebianReleases)
    [LTS](https://wiki.debian.org/fr/LTS)
    [ELTS](https://wiki.debian.org/fr/LTS/Extended)

---

- **Q: Pendant combien de temps les mises à jour de sécurité seront-elles fournies ?**

- **R:** Environ une année après que la version stable suivante a été publiée.

**Source :** [Debian](https://www.debian.org/security/faq.fr.html)

### Nom générique, nom de code et version

- **Q: Combien de version au minimum sont activement maintenues par Debian ?**

- **R:** Au moins trois versions: stable, testing, unstable

**Source :** [Debian](https://www.debian.org/releases/index.fr.html)

---

- **Q: D’où viennent les noms de code données aux distributions ?**

- **R:** Les noms de code proviennent des personnages des films *"Toy Story"*.

**Source :** [Debian](https://www.debian.org/doc/manuals/debian-faq/ftparchives#sourceforcodenames)

---

- **Q: Combien et lesquelles sont prises en charge par la version Bullseye ?**

- **R:**   9 architectures:
  - AMD64 & Intel 64
  - Intel x86-based
  - ARM
  - ARM avec matériel FPU
  - ARM 64 bits
  - MIPS 64 bits
  - MIPS 32 bits
  - Power Systems
  - IBM S/390 64 bits

**Source :** [Debian](https://www.debian.org/releases/stable/i386/ch02s01.fr.html)

### Première version avec un nom de code

- **Q: Quelle a était le premier nom de code utilisé ?**

- **R:** Nom de code: Buzz

---

- **Q: Quand a-t-il été annoncé ?**

- **R:** Annoncé le 17 juin 1996.

---

- **Q: Quelle était le numéro de version de cette distribution ?**

- **R:** Sous le numéro de version Debian GNU/Linux 1.1.

**Source :** [Debian](https://wiki.debian.org/fr/DebianBuzz)

### Dernière nom de code attribué

- **Q: Quel est le dernier nom de code annoncée à ce jour ?**

- **R:** Nom de code: Bullseye

---

- **Q: Quand a-t-il été annoncé ?**

- **R:** Annoncé le  14 août 2021.

---

- **Q: Quelle est la version de cette distribution ?**

- **R:** La première version de la distribution est la version 11.0 et sa dernière mise à jour est la version 11.6.

**Source :** [Debian](https://wiki.debian.org/fr/DebianTrixie)

## Installation préconfigurée

Etant donné que l'image iso d'installation *Debian 11.6* posséde une option avancée qui permet d'automatiser l'installation, rien n'est à faire.
Après avoir récupéré l'archive `autoinstall.zip`;

::: box
Executé la commande suivante:

```shell
sed -i -E "s/(--iprt-iso-maker-file-marker-bourne-sh).*$/\1=$(cat
/proc/sys/kernel/random/uuid)/" S203-Debian11.viso
```

:::

Et inséré le fichier `S203_Debian11.viso` dans le lecteur optique de la machine virtuelle,
Il ne restait plus qu'à démarrer la machine virtuelle et laisser l'installation se dérouler.

Pour modifier notre configuration dans le fichier `preseed-fr.cfg`:

::: info
Ajout droit sudo:

```shell
d-i passwd/user-default-groups string audio cdrom video sudo
```

:::

::: info
Installation MATE ainsi que différénts paquets:

```shell
tasksel tasksel/first multiselect standard ssh-server mate-desktop
d-i pkgsel/include string git, sqlite3, curl, bash-completion, neofetch
```

:::

# Gitea

## Préliminaire

::: info
Après avoir executé les commandes suivantes:

```shell
git config --global user.name "Kellian Mirey"
git config --global user.email "kellian.mirey.etu@univ-lille.fr"
git config --global init.defaultBranch "master"
```

:::

### Configuration globale de git

- **Q: Qu’est-ce que le logiciel git-gui ?**

- **R:** git-gui est l'interface graphique intégré à la commande git via la commande suivante:
  `` git gui [<command>] [<arguments>] ``

**Source:** <https://git-scm.com/docs/git-gui>

---

- **Q: Qu’est-ce que le logiciel gitk ?**

- **R:** gitk est une interface graphique de git permettant de visualiser l'historique des commits sous forme de graphe. `` gitk & `` dans le même répertoire que le projet.

**Source:** <https://git-scm.com/docs/gitk>

---

- **Q: Quelle sera la ligne de commande git pour utiliser par défaut le proxy de l’université sur tous vos projets git ?**

- **R:** `` git config --global http.proxy ``

**Source:** <https://git-scm.com/docs/git-config#Documentation/git-config.txt-httpproxy>

### Accéder au port 3000

Redirection de port à faire sur l'interface graphique de VM.

## Installation de Gitea

- **Q: Qu'est-ce que Gitea ?**

- **R:** Gitea est un client git web avec la possibilité d'héberger sa propre instance de Gitea sur son propre serveur.

**Source:** <https://docs.gitea.io/fr-fr/>

---

- **Q: A quels logiciels bien connus dans ce domaine peut-on le comparer ?**

- **R:** Gitea est similaire à GitHub, GitLab

**Source:** <https://docs.gitea.io/fr-fr/>

### Installation du binaire

1. Récupérer le fichier binaire de la version 1.18.5
2. Ajout d'un utilisateur `git` et configurations des droits de l'utilisateur et de l'accès à ces fichiers
3. Déplace l'executable dans ``/usr/bin``

### Démarrage automatique du service

La commande ``sudo systemctl enable gitea`` fera en sorte que gitea soit lancé automatiquement à chaque démarrage de la machine.

### ParamÃ©trage de Gitea

::: box
On vérifie que le service est bien démarré:

```shell
systemctl status gitea.service
```

:::

En ce rendant sur l'url <http://localhost:3000>, on obtient bien le résultat voulu.

![ ](./img/gitea_launch.png)

::: warning
Ne pas oublier de changer les permissions des chemins `/etc/gitea` et `/etc/gitea/app.ini`.
:::

- **Q: Comment faire pour la mettre Ã  jour sans devoir tout reconfigurer ?**

- **R:** Un fichier shell `upgrade.sh` nous est mis à disposition par gitea qui permet d'automatiser la mise à jour disponible sur le github: <https://github.com/go-gitea/gitea/blob/main/contrib/upgrade.sh>. Il suffit d'entrer la commande correspondante comme ci-dessous:

**Source:** [Gitea](https://docs.gitea.io/en-us/upgrade-from-gitea/#upgrade-from-binary)

```shell
sudo ./upgrade.sh -v 1.19
```

## Utilisation basique

::: info
Clone du projet `dev-oo` d'un membre de l'équipe:

```shell
git clone https://gitlab.univ-lille.fr/atilla.tas.etu/dev-oo && cd dev-oo
git remote remove origin
git remote add origin http://localhost:3000/gitea/dev-oo
git branch -m main master
git push origin master
```

:::

::: info
Création d'un projet pour les rapports de la SAE:

```shell
git init
git remote add origin http://localhost:3000/gitea/sae2.03
git add .
git commit -m "Initial commit"
git push origin master
```

:::
