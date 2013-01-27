Title: Procs et blocs
Menu: Doc
Slug: documentation/procs-et-blocs
Author: Alexandre Henriet

Introduction
------------

En **Ruby**, il est possible de créer des **objets** à partir de **blocs
de codes**.

Dans la mesure où ils encapsulent une série d'instructions, acceptent
des paramètres et ont une valeur de retour, ces objets de classe
**Proc** ressemblent un peu à des méthodes. Il s'agit de la version
**Ruby** des fonctions anonymes.

Instanciation
-------------

Pour créer un objet **Proc**, on utilise le **constructeur** de la
classe Proc ou la méthode **Kernel\#lambda** en leur passant un bloc de
code délimité par **do .. end** ou par des **accolades**. Bien qu'en
apparence similaires, ces deux méthodes de création ne sont pas égales
comme illustré un peu plus bas dans la page.

**le constructeur de la classe**

`my_proc = Proc.new() do
  puts "Je suis une instruction !"
end
puts my_proc   # <Proc:0xb73cad90@./procs.rb:3>`

*Les parenthèses sont présentes pour illustrer qu'il s'agit nullement
d'un paramètre mais bien d'un bloc d'instructions passé à la méthode un
peu comme quand on définit une nouvelle méthode avec def.*

**la méthode lamba**

`my_proc = lambda {
  puts "Moi aussi je suis une instruction !"
}
puts my_proc   # <Proc:0xb73f8dbc@./procs.rb:3>`

Exécution
---------

Pour exécuter le code contenu dans un objet **Proc**, on appelle sa
méthode **call**.

`my_proc = lambda {
  puts "Gomu gomu nooo ... Bazooka!"
}
my_proc.call   # Gomu gomu nooo ... Bazooka!`

Arguments
---------

Les objets **Proc** acceptent des arguments. Il suffit pour cela de les
positionner au début du bloc de code, entre **|** et séparés par des
virgules.

`my_proc = lambda { |who,x|
  puts "<#{who}> Kaa#{'meehaa'*x}aaa!"
}
my_proc.call('Gohan', 3)   # <Gohan> Kaameehaameehaameehaaaaa!`

Valeur de retour
----------------

Comme les méthodes, les objets **Proc** retournent la dernière
expression évaluée.

`my_proc = lambda {
  1337
}
puts my_proc.call   # 1337`

Closures
--------

Les objets **Proc** conservent accès aux variables locales du scope dans
lequel elles sont définies, il s'agit donc de **[closures](http://fr.wikipedia.org/wiki/Fermeture_(informatique))**.

`lover = 'Natsu'
juvia = Proc.new {
  lover = 'Grey'
}
juvia.call
puts lover   # Grey`

Blocs et méthodes
-----------------

On peut obtenir un objet **Proc** en passant un **bloc de code** à une
méthode. L'objet **Proc** sera assigné à la variable dont le nom est
préfixé par une esperluette.

`def cook(&tasks)
  tasks.call
end
cook do
  puts "1. Faire fondre du beurre"
  puts "2. Faire fricasser"
  puts "3. Assaisonner"
end # 1. ... 2. ... 3. ...`

Lorsqu'il n'est pas nécessaire d'avoir accès à l'objet **Proc** et qu'on
souhaite simplement exécuter le bloc de code passé à la méthode, on peut
omettre l'argument préfixé d'une esperluette et utiliser l'instruction
**yield** dans le corps de la méthode.

`def cook()
  yield
end
cook do
  puts "1. Faire fondre du beurre"
  puts "2. Faire fricasser"
  puts "3. Assaisonner"
end # 1. ... 2. ... 3. ...`

Il est également possible de passer un objet **Proc** en paramètre à une
méthode, comme n'importe quel autre objet.

`def cook(tasks)
  tasks.call
end
list = Proc.new do
  puts "1. Faire fondre du beurre"
  puts "2. Faire fricasser"
  puts "3. Assaisonner"
end
cook(list)   # 1. ... 2. ... 3. ...`

Proc vs Lambdas
---------------

On distingue en réalité deux types d'objets **Proc**, les **procs**
(créés avec Proc.new) et les **lambdas** (créés avec lambda). Bien qu'en
apparence identiques, ils ne le sont pas tout à fait.

La première différence réside au niveau du passage d'arguments.
Lorsqu'on utilise **Proc.new** avec un bloc de code disposant
d'arguments, ceux-ci seront instanciés à **nil** si aucune valeur ne
leur est passée à l'exécution, tandis qu'un objet **Proc** créé avec
**lambda** génèrera une erreur.

`my_proc = Proc.new { |a,b,c| puts "#{a.class} - #{b.class} - #{c.class}" }
my_proc.call('Spartacus', 'Gannicus')   # String - String - NilClass
my_lamb = lambda { |a,b,c| puts "#{a.class} - #{b.class} - #{c.class}" }
my_lamb.call('Spartacus', 'Gannicus')   # wrong number of arguments (2 for 3) (ArgumentError)`

La seconde différence réside au niveau de la valeur de retour. Un
**return** rencontré dans un objet **Proc** créé avec **Proc.new** met
fin à l'exécution de la méthode dans laquelle l'objet **Proc** est
exécuté, retournant la valeur de retour du **Proc**.

`def method_a
  Proc.new { return "Proc.new" }.call
  "method_a"
end
puts method_a   # Proc.new`

Un **return** explicite rencontré dans un objet **Proc** créé avec
**lambda** retourne la valeur de retour du **Proc** à la méthode qui
poursuit simplement son exécution.

`def method_b
  lambda { return "lambda" }.call
  "method_b"
end
puts method_b   # method_b`
