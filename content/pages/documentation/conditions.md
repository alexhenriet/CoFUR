Title: Structures conditionnelles 
Menu: Doc
Slug: documentation/structures-conditionnelles
Author: Alexandre Henriet

Important
---------

En **Ruby**, tout ce qui n'est pas **nil** ou **false** s'évalue à **true**. C'est par exemple le cas de l'entier 0, d'une chaîne vide et d'un tableau vide, ce qui peut être déconcertant au début.

If, elsif, else
---------------

Ceux-ci se passent de commentaire. Notez peut-être toutefois le **elsif** (et pas elseif)

`nb_visitors = 1
if nb_visitors.zero? then
  puts "aucun visiteur"             # Sera exécuté si nb_visitors vaut 0
elsif nb_visitors == 1 then
  puts "un visiteur"                # Sera exécuté si nb_visitors vaut 1
else
  puts "#{nb_visitors} visiteurs"   # Sera exécuté dans les autre cas
end`

La négation d'une expression peut être obtenue grâce aux opérateurs **!** et **not**.

Les opérateurs de comparaison **==, !=, \<=, \>=** et les opérateurs logiques **&&, ||** peuvent évidemment être utilisés pour créer des expressions booléennes.

Unless
------

Bien connu des Mongueurs, **unless** est l'exact opposé de if. Le code s'exécute quand la condition s'évalue à false.

`arms = false
unless arms then
  puts "pas de bras, pas de chocolat !"   # Sera exécuté car arms vaut false
end`

[...](http://fr.wikipedia.org/wiki/Pas_de_bras,_pas_de_chocolat)

Modificateurs d'instruction
---------------------------

**Ruby** vous permet d'écrire du code tel que :

`free_beer = false
puts ":-)" if free_beer
puts ":-(" unless free_beer`

Opérateur ternaire
------------------

Syntaxe compacte a utiliser avec modération :

`uid  = 0
role = (uid.zero?) ? "root" : "user"   # condition ? valeur si true : valeur si false`

Case, when
----------

Le **case** de **Ruby** s'apparente au **switch** des autres langages.

`age = 15
case age
  when 0      then puts "Maison"
  when 1..3   then puts "Garderie"
  when 3..6   then puts "Maternelle"
  when 6..12  then puts "Primaire"
  when 12..18 then puts "Secondaire"  # Sera affiché car age est compris entre 12 et 18
  else puts "Adulte"
end`

`lang = 'nl'
greeting = case lang
  when 'nl' then 'Goedemorgen'
  when 'fr' then 'Bonjour'
  when 'en' then 'Good morning'
end`
