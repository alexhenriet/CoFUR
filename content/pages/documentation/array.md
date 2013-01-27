Title: Les tableaux
Menu: Doc
Slug: documentation/tableaux
Author: Alexandre Henriet

Class Array
-----------

Les tableaux **Ruby** sont des **collections ordonnées d'objets**. Ils reposent sur la classe **Array** et ont notamment les caractéristiques suivantes :

-   La taille d'un tableau n'est pas figée, il est automatiquement redimensionné au besoin.
-   Les éléments d'un tableau sont indexés au moyen d'entiers, le premier ayant l'indice 0.
-   Les tableaux peuvent contenir n'importe quel type d'objet (Fixnum, String, Symbol, Hash, Array, etc.)
-   Un même tableau peut contenir plusieurs types d'objets différents.

La classe **Array** propose de nombreuses méthodes et implémente en plus le mixin **Enumerable**.

`[3] pry(Array):1> ls\
Array.methods: []\
Array#methods: &  *  +  -  <<  <=>  ==  []  []=  assoc  at  choice  clear  collect  collect!  combination  compact  \
compact! concat   count  cycle  delete  delete_at  delete_if  drop  drop_while  each  each_index  empty?  eql?  \
fetch  fill  find_index  first  flatten   flatten!  frozen?  hash  include?  index  indexes  indices  insert  inspect  \
join  last  length  map  map!  nitems  pack  permutation  place  pop  pretty_print  pretty_print_cycle  product  push  \
rassoc  reject  reject!  replace  reverse  reverse!  reverse_each  rindex  select  shelljoin  shift  shuffle  shuffle!\
size  slice  slice!  sort  sort!  take  take_while  to_a  to_ary  to_s  transpose  uniq  uniq!  unshift  values_at  \
zip  |`

Création
--------

**Ruby** propose plusieurs moyens de créer un tableau.

via le constructeur

Tous deux optionnels, ses paramètres permettent simplement d'initialiser le tableau. Il ne s'agit nullement de fixer la taille du tableau qui s'adaptera toujours dynamiquement au besoin.

`[1] pry(main)> my_array = Array.new(5, 0)
=> [0, 0, 0, 0, 0]
[2] pry(main)> my_array = Array.new(4)
=> [nil, nil, nil, nil]
[3] pry(main)> my_array = Array.new()
=> []`

Le constructeur offre également la possibilité d'initialiser un tableau au moyen d'un bloc qui reçoit en paramètre l'indice de l'élément et dont la valeur de retour est utilisée comme valeur pour l'élément.

`[2] pry(main)> Array.new(5) { |i| i * i + 1 }
=> [1, 2, 5, 10, 17]`

via la méthode **Array** du module Kernel

`[3] pry(main)> my_array = Array[1, "bla", Array.new]
=> [1, "bla", []]`

via la syntaxe raccourcie

`my_array = [7, 1, 3, 7, 0, 5]
=> [7, 1, 3, 7, 0, 5]`

Enfin, il est également possible de construire un tableau de chaînes pour peu qu'elles ne contiennent pas d'espaces via la syntaxe suivante. Notez l'absence de guillemets.

`my_array = %w(jack john bob)
=> ["jack", "john", "bob"]`

Taille
------

Pour tester qu'un tableau est vide on utilise

`[7] pry(main)> my_array = Array.new
=> []
[8] pry(main)> my_array.empty?
=> true`

Pour obtenir le nombre d'éléments qu'il contient

`[9] pry(main)> my_array = %w(aa bb cc)
=> ["aa", "bb", "cc"]
[10] pry(main)> my_array.size
=> 3`

La méthode **count** est un alias de **size**.

Accès aux éléments
------------------

Pour accéder aux éléments d'un tableau, on peut utiliser la méthode **slice** et surtout sa syntaxe raccourcie

`[11] pry(main)> my_array = %w(aa bb cc)
=> ["aa", "bb", "cc"]
[12] pry(main)> my_array.slice(1)
=> "bb"
[14] pry(main)> my_array[1]
=> "bb"`

Cette méthode permet d'accéder aux éléments en partant de la fin en utilisant un indice négatif.

`[15] pry(main)> my_array[-1]
=> "cc"`

On peut également utiliser la méthode **at** qui serait plus performante.

`[16] pry(main)> my_array.at(0)
=> "aa"`

La méthode **fetch** est intéressante car elle permet de retourner une valeur par défaut lorsque l'indice est hors-limites.

`[21] pry(main)> my_array = [1, 3]
=> [1, 3]
[22] pry(main)> my_array.fetch(1, 'default')
=> 3
[23] pry(main)> my_array.fetch(2, 'default')
=> "default"`

La méthode **values\_at** permet d'indiquer plusieurs indices en une fois.

`[24] pry(main)> my_array = Array[2, 4, 6, 1, 3, 5]
=> [2, 4, 6, 1, 3, 5]
[25] pry(main)> my_array.values_at(0, 2, 3)
=> [2, 6, 1]`

Les méthodes **first** et **last** offrent une alternative pour accéder respectivement au premier et au dernier éléments.

`[2] pry(main)> my_array = [1, 2, 3, 4, 5]
=> [1, 2, 3, 4, 5]
[3] pry(main)> my_array.first
=> 1
[4] pry(main)> my_array.last
=> 5`

Recherche
---------

On peut s'assurer qu'un tableau contient une certaine valeur via

`[26] pry(main)> my_array = %w(alex eloise moya snoopy)
=> ["alex", "eloise", "moya", "snoopy"]
[27] pry(main)> my_array.include?("kervette")
=> false`

On peut déterminer l'indice d'une valeur via

`[30] pry(main)> my_array = %w(aria spencer emily)
=> ["arya", "spencer", "emily"]
[31] pry(main)> my_array.index('spencer')
=> 1`

Ajout et suppression d'éléments
-------------------------------

Pour ajouter un élément à un tableau existant

`[5] pry(main)> my_array = %w(a b c)
=> ["a", "b", "c"]
[6] pry(main)> my_array << "d"
=> ["a", "b", "c", "d"]`

Pour supprimer un élément

`[17] pry(main)> my_array = %w(a b c)
=> ["a", "b", "c"]
[18] pry(main)> my_array.delete('a')
=> "a"
[19] pry(main)> my_array
=> ["b", "c"]
[20] pry(main)> my_array.delete('a')
=> nil`

Pour supprimer un élément grâce à son indice

`[21] pry(main)> my_array = %w(a b c)
=> ["a", "b", "c"]
[22] pry(main)> my_array.delete_at(1)
=> "b"
[23] pry(main)> my_array
=> ["a", "c"]`

Il est possible d'insérer un élément à un indice donné, décalant ce faisant les indices des éléments suivant.

`[27] pry(main)> my_array = %w(a b c)
=> ["a", "b", "c"]
[28] pry(main)> my_array.insert(1, "z")
=> ["a", "z", "b", "c"]`

Les méthodes **push, pop, shift, unshift** vous permettent d'implémenter **piles, queues, tas** (LIFO, FIFO).

Extraction
----------

La méthode **slice** déjà évoquée plus haut permet également d'extraire des portions de tableau.

`[29] pry(main)> my_array = [1, 2, 3, 4, 5]
=> [1, 2, 3, 4, 5]
[30] pry(main)> my_array[0,2]
=> [1, 2]   # 2 éléments à partir de l'indice 0
[31] pry(main)> my_array[0..2]
=> [1, 2, 3]   # éléments dont l'indice est compris dans le range 0 à 2
[32] pry(main)> my_array[0...2]
=> [1, 2]   # éléments dont l'indice est compris dans le range 0 à 2 à l'exception de 2 et d'effectuer des remplacements.`

`[33] pry(main)> my_array = [1, 2, 3, 4, 5]
=> [1, 2, 3, 4, 5]
[34] pry(main)> my_array[0..2] = "x"
=> "x"
[35] pry(main)> my_array\
=> ["x", 4, 5]`

Opérations ensemblistes
-----------------------

Les opérations ensemblistes sont disponibles pour la classe **Array** mais la classe **Set** est à priori un meilleur choix.

`[36] pry(main)> my_array_a = [1, 2, 3]
=> [1, 2, 3]
[37] pry(main)> my_array_b = [1, 2, 4]
=> [1, 2, 4]
[38] pry(main)> my_array_a & my_array_b
=> [1, 2]
[39] pry(main)> my_array_a | my_array_b
=> [1, 2, 3, 4]`

Opérateurs arithmétiques
------------------------

L'opérateur **+** permet de réaliser une concaténation de deux tableaux.

`[42] pry(main)> my_array_a = [1, 2, 3]
=> [1, 2, 3]
[43] pry(main)> my_array_b = [1, 2, 4]
=> [1, 2, 4]
[44] pry(main)> my_array_a + my_array_b
=> [1, 2, 3, 1, 2, 4]`

L'opérateur **-** permet de soustraires les éléments du second tableau du premier.

`[45] pry(main)> my_array_a = [1, 2, 3]
=> [1, 2, 3]
[46] pry(main)> my_array_b = [1, 2, 4]
=> [1, 2, 4]
[47] pry(main)> my_array_a - my_array_b
=> [3]`

L'opérateur **\*** multiplie le nombre d'éléments du tableau, réutilisant les mêmes valeurs.

`[51] pry(main)> my_array_a = [1, 2, 3]
=> [1, 2, 3]
[52] pry(main)> my_array_a * 3
=> [1, 2, 3, 1, 2, 3, 1, 2, 3]`

Comparaison
-----------

L'opérateur **==** retourne **vrai** si deux tableaux ont le même nombre d'éléments et si ceux-ci sont égaux entre eux ..

`[1] pry(main)> ["a", "c", "b"] == ["a", "b", "c"]
=> false
[2] pry(main)> ["a", "c", "b"] == ["a", "c", "b"]
=> true
[3] pry(main)> ["a", "c", "b"] == ["a", "c", "b", "d"]
=> false`

La méthode **eql?** se comporte à peu près de la même manière.

`[8] pry(main)> ["a", "c", "b"].eql? ["a", "b", "c"]
=> false
[9] pry(main)> ["a", "c", "b"].eql? ["a", "c", "b"]
=> true
[10] pry(main)> ["a", "c", "b"].eql? ["a", "c", "b", "d"]
=> false`

Mais pas tout à fait (Merci Mon\_Ouïe)

`[11] pry(main)> [1] == [1.0]
=> true
[12] pry(main)> [1].eql? [1.0]
=> false`

Lorsqu'on compare deux tableaux avec l'une ou l'autre méthode, leurs éléments sont comparés deux à deux avec la même méthode.

`[13] pry(main)> 1 == 1.0
=> true
[14] pry(main)> 1.eql? 1.0
=> false`

Tri
---

Il est possible de trier un tableau grâce à **sort**. Les éléments sont comparés entre eux avec l'opérateur spaceship **<=\>**.

`[15] pry(main)> ["z", "a", "b"].sort
=> ["a", "b", "z"]
[16] pry(main)> [19, 5, 12].sort
=> [5, 12, 19]`

Filtrer
-------

Pour filtrer les éléments d'un tableau, on peut utiliser **select** ou **reject**.

La méthode **select** accepte un bloc qui reçoit les éléments du tableau en paramètre. Si la valeur de retour du bloc est **true** les éléments sont conservés.

`[17] pry(main)> [-1, -5, 0, 3, 6].select { |e| e >= 0 }
=> [0, 3, 6]`

La méthode **reject** est son exact opposé.

`[18] pry(main)> [-1, -5, 0, 3, 6].reject { |e| e >= 0 }
=> [-1, -5]`

Itération
---------

En **Ruby** on préfère utiliser les méthodes **each** ou **each\_index** pour parcourir un tableau plutôt que **for**.

`[24] pry(main)> %w(pierre pol jacques).each { |e| e.capitalize! }
=> ["Pierre", "Pol", "Jacques"]`
