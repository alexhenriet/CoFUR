Title: Les bases 
Menu: Doc
Slug: documentation/les-bases
Author: Alexandre Henriet

Format de fichiers
------------------

Un programme **Ruby** est composé d'instructions qui se succèdent dans un fichier texte portant généralement l'extension **.rb**, sans toutefois que cela soit requis. Un tel fichier peut être créé avec n'importe quel éditeur de texte (gedit, vim, notepad++, etc.).

N'utilisez-pas de logiciel de traitement de textes car ceux-ci insèrent des caractères spéciaux de mise en forme qui n'apparaissent pas à l'écran et ne sont pas tolérés par l'interpréteur **Ruby**.

Lorsque vous vous lancerez sur un projet concret, optez pour un Environnement de Développement Intégré (IDE) qui vous offrira des fonctionnalités très utiles telles que la complétion de code, la coloration syntaxique, la documentation à la volée, le debugging et j'en passe.

La première ligne d'un script **Ruby** est appelée Shebang et permet au système de déterminer quel interpréteur utiliser pour exécuter les instructions du fichier. Cette ligne doit être la toute première du fichier et prend la forme :

`#!/usr/bin/env ruby`

Les instructions **Ruby** sont séparées par les retours à la ligne. Bien qu'il soit toléré par l'interpréteur, le point-virgule à la fin d'une instruction n'est pas requis et n'est jamais utilisé sauf pour séparer deux instructions différentes sur une même ligne.

L'indention du code n'est pas significative en **Ruby**.

Tout ce qui se trouve entre un caractère \# (dièse) et un retour à la ligne est un commentaire en **Ruby**. Il est également possible de créer des commentaires multi-lignes via la syntaxe

`=begin
mon
commentaire
multi-lignes
=end`

Exécuter un script Ruby
-----------------------

Pour exécuter un script, passez son nom en argument à l'interpréteur **Ruby** comme suit

`$ ruby monscript.rb`

ou appelez-le directement après l'avoir rendu exécutable

`chmod +x monscript.rb
./monscript.rb`

Interactive Ruby (IRB)
----------------------

**irb** est un shell interactif **Ruby** très pratique pour l'expérimentation. Lorsque vous l'appelez depuis un terminal, il vous présente son prompt qui ressemble à ceci

`$ irb
irb(main):001:0>`

Vous pouvez alors taper des instructions **Ruby** et **irb** les exécutera de manière interactive, affichant à l'écran les messages à destination de la sortie standard et les valeurs de retour de chaque expression. **irb** offre la possibilité de définir des variables, un historique des commandes tapées, etc.

`irb(main):002:0> var = "5"
=> "5"                           # Valeur de retour de l'expression
irb(main):003:0> puts var
5                                # Message à destination de la sortie standard
=> nil                           # Valeur de retour de l'expression`

La gem **(en)** **[pry](http://rubygems.org/gems/pry)** fournit un shell **Ruby** alternatif beaucoup plus puissant que **IRB** (voir la section sur RubyGems).

Variables, constantes et symboles
---------------------------------

**Ruby** est dynamiquement typé, il n'est pas nécessaire de déclarer les variables et leur type avant de les utiliser. Il ne pratique toutefois pas la conversion implicite de type. Il n'est pas possible de comparer une chaîne à un entier sans conversion explicite dans un type compatible au préalable.

Les noms de variables sont sensibles à la casse. Ils sont composés de lettres, de chiffres et d'underscores (\_) et ne peuvent commencer ni
pas un chiffre, ni par une majuscule.

`my_var # valide
myVar  # valide
MyVar  # invalide car commençant par une majuscule
5var   # invalide car commençant par un chiffre`

Les noms de constantes répondent aux mêmes exigences mais doivent commencer par une majuscule.

Petite curiosité du langage, les constantes **Ruby** sont mutables. Réassigner valeur à une constante déjà initialisée aura pour effet l'affichage d'un avertissement :

`warning: already initialized constant MyConstant`

En fonction de leur contexte (scope), les variables peuvent être préfixées de caractères spéciaux.

`var     # variable locale
@var    # variable d'instance => voir section sur la POO
@@var   # variable de classe  => voir section sur la POO
$var    # variable globale`

L'assignation d'une valeur aux variables et aux constantes se fait au moyen de l'opérateur **=** comme dans la plupart des langages.

`var = expression_retournant_une_valeur`

**Ruby** propose enfin les symboles, que l'on peut voir comme des constantes uniques dont on ne définit pas soi-même la valeur et qui la conservent durant toute l'exécution d'un programme. Ils constituent des identifiants idéaux. Ils sont créés simplement en les nommant, précédés par **:**.

Au lieu de coder quelque chose comme

`North = 1  # Constante North valant 1
South = 2`

vous pouvez simplement écrire

`:north     # Identifiant unique
:south`

Saisies clavier et affichage écran
----------------------------------

Pendant votre phase d'expérimentation, vous aimerez surement pouvoir demander une saisie de texte au clavier et afficher simplement du texte à l'écran. Donc bien que les modules, les objets et les méthodes n'aient pas encore été abordés, sachez que vous pouvez utiliser pour déclencher une saisie clavier :

`var = $stdin.gets   # Assignation de la saisie clavier à la variable locale var`

et pour écrire à destination de l'écran :

`puts var            # Affiche le contenu de var à l'écran`

En Ruby, TOUT est objet
-----------------------

En Ruby, toutes les données que vous manipulez sont des objets (des instances de classes) dont les classes descendent toutes plus ou moins directement et sans exception d'un ancêtre commun, la classe **(en)** **[Object](http://ruby-doc.org/core-1.9.3/Object.html)**.

Quelle que soit la donnée manipulée, vous pouvez appeler sur elle les méthodes définies par cette dernière, comme par exemple la méthode .class() qui retourne la classe de objet.

`5.class                                        # Fixnum, la classe qui sert à représenter les nombres décimaux entiers
'Rock, paper, scissors, lizard, Spock'.class   # String, la classe qui sert à représenter les chaines de caractères`

Dans cet exemple, 5 est un objet de la classe Fixnum et .class représente un appel de sa méthode class. Pour appeler une méthode sur un objet, il suffit de lui adjoindre le nom de la méthode en question séparé par un point.

Vous pouvez également constater que lors d'un tel appel, les parenthèses sont facultatives en Ruby. D'ailleurs, quand vous effectuez une opération arithmétique telle que

`sum = 5 + 5`

vous appelez en réalité [la méthode +()](http://www.ruby-doc.org/core-1.9.3/Fixnum.html#2B-method) de la classe Fixnum, ce qui pourrait également s'écrire :

`sum = 5.+(5)`

La classe Fixnum a une méthode pour chacun des opérateurs arithmétiques.

Les modules
-----------

**Ruby** intègre également la notion de modules, semblables à des classes mais ne pouvant être instanciés. Ceux-ci servent à regrouper des constantes et des méthodes par thèmes et à les rendre utilisables dans plusieurs classes (mixin, alternative à l'héritage multiple qui n'existe pas en Ruby).

La librairie standard propose de nombreux modules tels que [Math](http://www.ruby-doc.org/core-1.9.3/Math.html).

La classe Object, ancêtre de toute les classes intègre le module [Kernel](http://www.ruby-doc.org/core-1.9.3/Kernel.html) et donc toutes les méthodes de Kernel sont toujours disponibles. C'est de la que vient la méthode **puts** utilisée plus haut sur cette page.

Les types de données
--------------------

### Les types simples

- NilClass
- String
- Numeric
    - Float
    - Integer
        - BigNum
        - FixNum
- TrueClass
- FalseClass

### Les types composites

- Array
- Hash
