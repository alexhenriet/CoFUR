Title: Programmation orientée objet
Menu: Doc
Slug: documentation/oriente-objet
Author: Alexandre Henriet

Introduction
------------

**Ruby** est un langage de **Programmation Orientée Objet** (POO). Ce
paradigme, qui s'appuie sur la programmation procédurale, repose sur la
notion d'objets, structures intégrant à la fois données (**attributs**)
et traitements pouvant leur être appliqués (**méthodes**). Cette page
n'a pas vocation à présenter la théorie de la POO mais uniquement les
outils que **Ruby** met à votre disposition dans ce contexte.

Classes
-------

Pour définir une classe, on utilise le mot clé **class** suivi d'un nom.
Les noms de classes en **Ruby** sont des constantes et commencent donc
par une majuscule.

`class MyClass
end
puts MyClass.class   # class`

Instanciation
-------------

Pour créer un objet, on parle aussi d'instanciation de classe, on
appelle simplement sa méthode **new** en lui passant éventuellement les
paramètres acceptés par son constructeur (voir plus bas).

`class Cat
end
my_cat = Cat.new
p my_cat   # <Cat:0xb7398c8c>`

Un objet de type Cat a été créé et assigné à la variable **my\_cat**.

Méthodes
--------

Pour définir une méthode on utilise le mot clé **def** suivi du nom de
la méthode et de ses paramètres, qui sont TOUJOURS passés par référence
et non par valeur.

Pour appeler une méthode sur un objet **Ruby**, on utilise la syntaxe
**objet.méthode()**. Les parenthèses sont optionnelles tant à la
définition que lors de l'appel et on les laisse généralement tomber
lorsque la méthode n'accepte aucun paramètre.

`class Cat
  def talk
    puts "Maow"
  end
end
Cat.new.talk   # Maow`

Bien que le mot clé **return** existe pour retourner une valeur, il est
rarement employé en **Ruby**. Par défaut la valeur de la dernière
expression évaluée est renvoyée.

`class Cat
  def likes?(what)
    case what
      when 'chiens'  : false
      when 'souris' : true
    end
  end
end
my_cat = Cat.new
puts my_cat.likes?('souris')   # true car il s'agit de la dernière expression évaluée lors de l'appel de la méthode`

Vous avez peut-être constaté le **?** à la fin du nom de la méthode. Non
seulement il est autorisé, mais en plus il s'agit d'une convention en
**Ruby**. Le nom des méthodes qui retournent un booléen se termine par
**?**, tandis que celui des méthodes dangereuses (*(en)* [bang methods](http://dablog.rubypal.com/2007/8/15/bang-methods-or-danger-will-rubyist)) se termine par un **!**.

Les arguments des méthodes peuvent recevoir une valeur par défaut via la
syntaxe **argument=valeur**. Lorsque la méthode est appelée sans
argument, la valeur par défaut est affectée à la variable.

`class Cat
  def talk(count=1)
    puts "Maow!\n" * count
  end
end
my_cat = Cat.new
my_cat.talk      # Maow!
my_cat.talk(2)   # Maow! Maow!`

Les méthodes peuvent également recevoir un **nombre variable
d'arguments** via la syntaxe **méthode(\*args)**. Le **\*** transforme **args** en tableau qui recevra tous les arguments.

`class ShoppingRobot
  def buy(*list)
    puts "Checklist des courses (#{list.size} produits) :"
    list.each { |product|
      puts "[ ] #{product}"
    }
  end
end
robot = ShoppingRobot.new
robot.buy('pain', 'lait', 'dentifrice')`

Une **méthode de classe** peut être définie via la syntaxe **def
NomClasse.méthode()** ou **self.méthode()**. Les méthodes de classes
peuvent être appelées directement sur la classe sans qu'il soit
nécessaire d'instancier un objet. Elles n'ont logiquement accès qu'aux
variables de classe préfixées par **@@** et pas aux variables
d'instances **@**.

`class Player
  @@count = 5
  def self.football
    puts "plus que #{11-@@count} pour faire une équipe"
  end
end
Player.football   # plus que 6 pour faire une équipe`

Une méthode peut-être définie en dehors d'une définition de classe et
être appelée simplement par son nom à l'instar d'une fonction dans un
autre langage. En réalité, **Ruby** définit ces **méthodes globales**
comme méthodes privées de la classe Object.

Les méthodes peuvent enfin être regroupées dans des **modules**.

Constructeur
------------

Lors de la création d'un objet via **NomClass.new**, la méthode **initialize** de la classe est appelée par la méthode **new** définie
dans la classe *(en)* **[Class](http://www.ruby-doc.org/core-1.9.3/Class.html)**. Elle peut donc être utilisée pour initialiser les variables d'instance.

`class Cat
  def initialize(type, name)
    @type = type
    @name = name
  end
end
my_cat = Cat.new('persan', 'Moya')
p my_cat   # <Cat:0xb7483a48 @name="Moya", @type="persan">`

Attributs
---------

Les variables préfixées d'un seul arobase telles que **@name** sont des
variables d'instance. Elles sont propres à chaque objet.

`class Cat
  def initialize(type, name)
    @type = type
    @name = name
  end
  def talk
    puts "Bonjour je suis #{@name}, un chat #{@type}"
  end
end
Cat.new('persan', 'Moya').talk   # Bonjour je suis Moya, un chat persan
Cat.new('de gouttière', 'Sherman').talk   # Bonjour je suis Sherman, un chat de gouttière`

Les variables préfixées de 2 arobases telles que **@@count** sont des
variables de classe. Elles sont uniques pour les différents objets
instanciés à partir de la classe.

`class Cat
  @@count = 0
  def initialize(type, name)
    @@count += 1
    @type = type
    @name = name
  end
  def count
    @@count
  end
end
puts Cat.new('persan', 'Moya').count   # 1
puts Cat.new('de gouttière', 'Sherman').count   # 2`

Getters et setters
------------------

**Ruby** promeut l' **encapsulation des données** dans la mesure où, par
défaut, il n'est pas possible d'accéder aux attributs d'un objet en
dehors de celui-ci. Il est nécessaire de définir des méthodes pour les
manipuler, que l'on nomme **accesseurs** (getters) et **mutateurs**
(setters).

`class Human
 @name
 def name
   @name
 end
 def name=( name )
   @name = name
 end
end
bob = Human.new
bob.name = "Morane"
puts bob.name   # Morane`

Comme illustré, la possibilité d'appeler une méthode **Ruby** en
omettant les parenthèses donne l'illusion de manipuler les attributs.

La métaprogrammation (voir section dédiée) offre une alternative à la
définition manuelle des getters et setters. Il est possible de
transformer la classe précédente comme suit pour obtenir le même
comportement.

`class Human
 attr_accessor :name   # Définit getter et setter
end
bob = Human.new
bob.name = "Morane"
puts bob.name   # Morane`

**attr\_reader** et **attr\_writer** existent également pour ne définir
respectivement que le **getter** et que le **setter**.

self
----

A l'intérieur d'une classe, il est parfois nécessaire de pouvoir
référencer l'objet courant. On y parvient à l'aide du mot clé **self**.

`class Pokemon
 attr_accessor :type
 def me
   self
 end
end
bulbi = Pokemon.new
bulbi.type = 'Pokémon bulbe'
p bulbi.me   # <Pokemon:0xb744b9cc @type="Pokémon bulbe">`

Héritage
--------

Le langage **Ruby** propose un modèle d'héritage simple, contrairement à
Python ou C++. Une même classe peut hériter directement d'une seule
autre classe. Pour assouvir vos besoins d'héritage multiple, **Ruby**
dispose des **mixins** (voir section dédiée).

Pour déclarer qu'une classe hérite d'une autre on utilise la syntaxe
**class Fille < Mère**.

`class Animal
  def me
    "Je suis un animal"
  end
end
class Dog < Animal
end
brutus = Dog.new
puts brutus.me   # Je suis un animal`

Le constructeur d'une classe fille n'appelle pas automatiquement celui
de sa classe mère, il est nécessaire de l'appeler explicitement via le
mot clé **super**.

`class Animal
  def initialize(blood)
    @blood = blood
  end
end
class Dog < Animal
  def initialize(hair)
    super('chaud')
    @hair = hair
  end
end
brutus = Dog.new('brun')
p brutus   #   <Dog:0xb739d818 @blood="chaud", @hair="brun">`

Il n'existe qu'un jeu de variables d'instance à travers tout l'arbre
d'héritage.

Modules (Mixins)
----------------

A l'instar des classes, les **modules** peuvent contenir des méthodes,
toutefois ils ne peuvent être instanciés.

Un module peut être inclus dans une classe, rendant ses méthodes
accessibles à celle-ci. Les méthodes d'un module inclus peuvent
manipuler les variables d'instance de la classe. Ces modules partagés
entre classes portent le nom de **mixins**.

`module Gender
  def male?
    @gender.eql?( 'male' )
  end
  def female?
    @gender.eql?( 'female' )
  end
end
class Human
  include Gender
  @gender
  def initialize( gender )
    @gender = gender
  end
end
bob = Human.new('male')
p bob.male?
p bob.female?`


Classes ouvertes (open classes)
-------------------------------

En **Ruby**, les classes ne sont jamais fermées. Vous pouvez à tout
moment ré-ouvrir une classe pour redéfinir une méthode ou en ajouter une
nouvelle. C'est vrai pour toutes les classes, y compris celles fournies
par **Ruby**.

`my_string = "ça ne se passera pas comme ça !"
class String
  def haddockize
    self << " Mille milliards de mille sabords !!"
  end
end
puts my_string.haddockize   # ça ne se passera pas comme ça ! Mille milliards de mille sabords !!`

Classes imbriquées (nested classes)
-----------------------------------

Il est possible d'imbriquer la définition d'une classe à l'intérieur de
celle d'une autre classe. La classe imbriquée est stockée sous la forme
d'une constante et on y accède via la syntaxe
**ClasseExterne::ClassImbriquée**.

`class Ocean
  class Fish
    def talk
      puts "Bloub bloub, je suis un poisson qui vit dans l'océan."
    end
  end
end
my_fish = Ocean::Fish.new
my_fish.talk   #   Bloub bloub, je suis un poisson qui vit dans l'océan.`
