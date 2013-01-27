Title: Structures de boucles
Menu: Doc
Slug: documentation/structures-de-boucles
Author: Alexandre Henriet

Embarras du choix
-----------------

**Ruby** propose un large éventail de possibilités en matière de structures de boucles.

While
-----

La boucle **while**, probablement la plus connue, fonctionne comme escompté. Elle exécute ses instructions **tant que** la condition s'évalue à **true**.

`i = 1
while i <= 10 do
  puts i
  i+=1
end # Affiche 1 2 3 4 5 6 7 8 9 10 sur des lignes différentes`

Begin, end while
----------------

L'équivalent du **Do .. while** d'autres langages, impliquant au moins une itération.

`begin
  print "bash:~$ "
  command = $stdin.gets.chomp
  puts "#{command} : commande introuvable"
end while not command.eql? "exit"
puts "Aurevoir :p" # Affiche "commande_saisie : commande introuvable" jusqu'à ce que "exit" soit saisie`

Until
-----

A l'instar de **unless**, il s'agit là d'un opérateur bien connu des Mongueurs, qui s'exécute tant que la condtion s'évalue à **false**.

`magic_word = nil
until magic_word.eql? "svp"
  puts "Tu ne peux pas quitter la table, tu n'as pas dit le mot magique !"
  magic_word = $stdin.gets.chomp
end
puts "Ok, file faire tes devoirs !" # Affiche "Tu ne peux pas ..." jusqu'à ce que le mot magique soit saisi`

Begin, end until
----------------

Fonctionne comme **begin, end while** mais tant que la condition s'évalue à **false**.

Modificateur d'instruction
--------------------------

Aussi bien **while** que **until** peuvent être utilisés comme modificateurs d'instructions.

`countdown = 4
puts "Décollage dans #{countdown-=1}" while countdown > 1
puts "Mise à feu !" # Affiche le décompte "Décollage dans 3", 2, 1 puis "Mise à feu !"`

Loop
----

Boucle infinie dont on sort grâce à l'instruction **break**.

`loop do
  print "Prénom : "
  firstname = $stdin.gets.chomp
  break if firstname.eql? "toto"
  puts "Bonjour #{firstname}"
end # Affiche "Prénom : " jusqu'à ce que "toto" soit saisi`

Times
-----

La méthode **times** fournie par la classe Integer ou **Integer\#times** en version courte, se base sur le valeur de l'entier pour déterminer le nombre de fois que le bloc de code passé doit être exécuté (voir section Blocs de codes).

`(5 + 5).times { |i|
  print "%d " % i
}
print "\n" # Affiche 0 1 2 3 4 5 6 7 8 9`

Upto, downto
------------

Les méthodes **upto** et **downto**, définies par Integer et Date (ainsi que String pour upto), permettent d'itérer d'une valeur "jusqu'à" une autre ..

`#!/usr/bin/env ruby
1.upto(3) { |i|
  print "%02d " % i
}
print "\n" # Affiche 01 02 03`

For, in
-------

La boucle **for .. in** permet d'itérer sur chacune des valeurs d'une collection.

`kings = %w(Gaspard Melchior Balthazar)
for king in kings do
  puts king
end # Affiche les 3 prénoms sur une ligne différente`

Github décourage l'utilisation de **for .. in** dans son *(en)* [**guide de codage Ruby**](https://github.com/styleguide/ruby/), ce au profit de **each** qui suit.

Each
----

La méthode **each** et ses petites soeurs **each\_\*** sont implémentées par moult classes. Elles permettent à l'instar de la boucle **for** d'exécuter un bloc de code sur une collection d'éléments.

`['Jupiler', 'Stella', 'Rodenbach', 'Leffe'].each { |beer|
  puts "Aubergiiiste, une %s siouplait !" % beer
} # Commande à boire`
