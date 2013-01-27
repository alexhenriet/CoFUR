Title: Installation de Ruby
Menu: Doc
Slug: documentation/installation
Author: Alexandre Henriet

Ruby MRI
--------

Comme mentionné sur la page **[Ruby](../ruby.html "Ruby")**, il existe plusieurs implémentations de l'interpréteur **Ruby**. Cette page fournit des procédures d'installation pour la version dite MRI (Matz's Ruby Interpreter) qui est l'implémentation de référence en C. Le nombre de possibilités qui s'offrent à vous pour l'installer varie en fonction du système d'exploitation cible, des privilèges dont vous disposez et de la version du MRI visée.

Installation sous Linux
-----------------------

Sous Linux, **Ruby** peut moyennant de disposer des privilèges root, être installé via le gestionnaire de paquets de la distribution (voir ci-dessous), cependant la version disponible depuis les dépôts officiels est rarement à jour. Il existe pour certaines distributions des dépôts tiers plus à jour mais la qualité des paquets que vous y trouverez n'est pas garantie.

Deux alternatives de choix pour Linux sont **RVM** et le couple **rbenv** + **ruby-build** (voir Multi-OS plus bas).

### Debian/Ubuntu

`$ apt-get install ruby rubygems ruby-dev ri   # Il existe un paquet ruby-full mais il inclut beaucoup de dépendances inutiles ..`

Pour information :

`$ cat /etc/issue
Debian GNU/Linux 6.0
$ ruby -v
ruby 1.8.7 (2010-08-16 patchlevel 302) [x86_64-linux]`

`$ cat /etc/issue
Ubuntu 12.04 LTS \n \l
$ ruby -v
ruby 1.8.7 (2011-06-30 patchlevel 352) [i686-linux]`

### Centos/Red Hat

`yum install ruby rubygems ruby-devel ruby-ri ruby-irb`

Pour information :

`$ cat /etc/issue
Red Hat Enterprise Linux Server release 5.7 (Tikanga)
$ ruby -v
ruby 1.8.5 (2006-08-25) [x86_64-linux]`

`$ cat /etc/issue
CentOS release 6.2 (Final)
$ ruby -v
ruby 1.8.7 (2011-06-30 patchlevel 352) [x86_64-linux]`

Unix
----

### Freebsd

Sous Freebsd, **Ruby** peut, moyennant de disposer des privilèges root, être installé via les packages ou via l'arborescence des ports. Si cette dernière est à jour, il est possible d'installer les dernières versions de **Ruby**.

#### Ports

Pour installer **Ruby** depuis les ports, il suffit d'installer **devel/ruby-gems** et toutes les dépendances sont installées automatiquement.

`cd /usr/ports/devel/ruby-gems && make && make install && make clean`

Pour information :

`$ uname -a
FreeBSD freebsd 9.0-RELEASE ...
$ ruby -v
ruby 1.8.7 (2012-06-29 patchlevel 370) [i386-freebsd9]`

#### Packages

Pour installer **Ruby** via le système de paquets, un seul suffit également et les dépendances suivent.

`pkg_add -r ruby18-gems`

### Solaris

Le package **Ruby** est disponible sur *(en)* <http://www.opencsw.org/get-it/packages/> et peut être installé comme suit :

`$ pkgadd -d http://get.opencsw.org/now
$ /opt/csw/bin/pkgutil -U   # Mettre à jour la liste des paquets disponibles
$ /opt/csw/bin/pkgutil -y -i ruby18
$ /opt/csw/bin/pkgutil -y -i rubygems`

Pour information :

`$ uname -a
SunOS new-host-4 5.10 Generic_147441-01 i86pc i386 i86pc
$ /opt/csw/bin/ruby18 -v
ruby 1.8.7 (2011-02-18 patchlevel 334) [i386-solaris2.9]`

MAC OS X
--------

**Ruby** est installé par défaut sous Mac OS X. Il est toutefois possible d'installer une version plus récente via **RVM** et **Rbenv** (voir Multi-OS plus bas).

Pour information :

`$ uname -a
Darwin portae 10.8.0 Darwin Kernel Version 10.8.0 ...
$ ruby -v
ruby 1.8.7 (2010-01-10 patchlevel 249) [universal-darwin10.0]`

Multi-OS
--------

**RVM** et **Rbenv** (+ruby-build) sont deux outils en ligne de commande qui permettent d'installer, gérer et travailler aisément avec plusieurs environnements **Ruby** différents. Il est ainsi possible de disposer conjointement des versions 1.8.7 et 1.9.3 de **Ruby** et de switcher facilement de l'un à l'autre. Tous deux peuvent être utilisés en tant que simple utilisateur, auquel cas l'environnement **Ruby** est installé intégralement dans le home. Les procédures ci-dessous ont été testées sous Linux mais il est possible d'utiliser ces outils sous d'autres OS. Vérifiez sur leurs sites respectifs si le vôtre est supporté. **Les deux outils ne sont pas compatibles entre eux**.

### Rbenv (+ruby-build)

Contrairement à **RVM** qui est une solution tout en un, **Rbenv** tout seul ne permet que d'alterner entre différents interpréteurs qu'il vous
appartient d'installer. Il peut toutefois gérer ces installations lorsqu'il est utilisé conjointement avec son plugin **ruby-build**. **Rbenv** se veut beaucoup moins intrusif que **RVM**.

La documentation officielle de l'outil est disponible sur **(en)** **<https://github.com/sstephenson/rbenv/>**

#### Installer Rbenv

Pour installer **Rbenv**, récupérez son code sur github :

`$ git clone git://github.com/sstephenson/rbenv.git .rbenv`

Puis ajoutez ces 2 commandes au bon endroit pour qu'elles soient exécutée à chaque session (.bashrc ou .bash\_profile) :

`$ export PATH=~/.rbenv/bin:$PATH
$ eval "$(rbenv init -)"`

Enfin relancez votre shell via :

`$ exec $SHELL`

Appelez **Rbenv** sans argument pour afficher son menu d'aide :

`$ rbenv
rbenv 0.3.0
usage: rbenv command [args]
Some useful rbenv commands are:
   commands      List all rbenv commands
   rehash        Rehash rbenv shims (run this after installing binaries)
   global        Set or show the global Ruby version
   local         Set or show the local directory-specific Ruby version
   shell         Set or show the shell-specific Ruby version
   version       Show the current Ruby version
   versions      List all Ruby versions known by rbenv
   which         Show the full path for the given Ruby command
   whence        List all Ruby versions with the given command
See rbenv help command for information on a specific command.
For full documentation, see: https://github.com/sstephenson/rbenv#readme`

#### Installer ruby-build

Pour installer le plugin **ruby-build** :

`$ mkdir -p ~/.rbenv/plugins && cd ~/.rbenv/plugins
$ git clone git://github.com/sstephenson/ruby-build.git`

#### Utilisation

Pour lister les **Ruby** disponibles pour installation :

`$ rbenv install`

Pour installer une version spécifique :

`$ rbenv install 1.9.3-p194
$ rbenv rehash   # A réexécuter à chaque installation d'une version de Ruby ou d'une gem apportant un binaire, telle que 'pry'`

Pour définir la version par défaut :

`$ rbenv global 1.9.3-p194   # Définie dans ~/.rbenv/version`

Pour définir la version pour un folder spécifique :

`$ cd /tmp/project/
$ rbenv local 1.8.7-p370`

Pour voir uniquement la version utilisée dans le folder où vous vous
trouvez ($PWD) :

`$ rbenv version
1.8.7-p370 (set by /tmp/project/.rbenv-version)`

Pour lister l'ensemble des **Ruby** connus par **rbenv** :

`$ rbenv versions
  1.8.7-p370
* 1.9.3-p194 (set by ~/.rbenv/version)  # Version utilisée dans $PWD`

### RVM

La documentation officielle de l'outil est disponible sur **(en)** **<http://rvm.io/>**

#### Installer RVM

Pour installer **RVM**, récupérez le script d'installation et exécutez-le via :

`$ curl -L get.rvm.io | bash -s stable`

Chargez le dans votre shell courant via :

`$ source ~/.rvm/scripts/rvm   # Ajoutez la commande au bon endroit pour qu'elle soit exécutée à chaque session`

La commande suivante devrait alors être disponible :

`$ rvm -v
rvm 1.14.5 (stable) by Wayne E. Seguin, Michal Papis`

#### Utilisation

Pour afficher la liste de tous les interpréteurs pouvant être installés via RVM :

`$ rvm list known`

Pour installer Ruby 1.9.3 :

`$ rvm install 1.9.3
... # Téléchargement et compilation, ça demande un peu de temps`

Pour afficher les interpréteurs installés :

`$ rvm list`

Pour en définir un comme interpréteur **Ruby** par défaut :

`$ rvm use ruby-1.9.3-p194
$ ruby -v
ruby 1.9.3p194 (2012-04-20 revision 35410) [x86_64-linux]`

Windows
-------

### RubyInstaller

**[RubyInstaller](http://rubyinstaller.org/downloads/)** vous permet d'installer les versions majeures du MRI compilées pour Windows avec MinGW. C'est à priori la meilleure méthode d'installation de **Ruby** sur ce système d'exploitation. La marche à suivre est la suivante :

1.  Lancez **rubyinstaller-1.8.7-p370.exe**
2.  Choisissez la **langue**, acceptez la **licence**
3.  Cochez l' **ajout des exécutables au PATH** et l' **association des .rb** à Ruby
4.  Cliquez sur **next** pour lancer l'installation.

Pour information :

`C:\>ruby -v
ruby 1.8.7 (2012-06-29 patchlevel 370) [i386-mingw32]`

### Cygwin

Composé d'une .dll Windows et d'un tas d'autres outils, **[Cygwin](http://www.cygwin.com/)** vous offre un environnement **Linux-like** pour Windows. Il vous permet d'installer **Ruby** en quelques clics via son installer. La marche à suivre :

1.  Lancez le **setup.exe**, cliquez jusqu'à parvenir à la liste des paquets.
2.  Allez dans **All** / **Ruby**, cliquez jusqu'à afficher **1.8.7-p370-1**.
3.  Cochez la colonne **Bin?** et cliquez sur **next** pour lancer téléchargement et installation.

**RubyGems** n'est pas disponible via Cygwin, il est nécessaire de l'installer à la main.

1.  **[Téléchargez l'installeur](http://rubyforge.org/frs/?group_id=126)** sur RubyForge.
2.  Décompressez l'archive via **unzip rubygems-1.8.24.zip**.
3.  Lancez le script d'installation via **cd rubygems-1.8.24 && ruby setup.rb**.

Pour information :

`$ uname -a
CYGWIN_NT-6.1-WOW64 zabimaru 1.7.16(0.262/5/3) 2012-07-20 22:55 i686 Cygwin
$ ruby -v
ruby 1.8.7 (2012-06-29 patchlevel 370) [i386-cygwin]`
