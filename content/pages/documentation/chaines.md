Title: Les chaînes de caractères
Menu: Doc
Slug: documentation/chaines-de-caracteres
Author: Alexandre Henriet

Classe String
-------------

Quelle que soit la manière utilisée pour l'instancier, une chaîne de caractères est toujours un objet de la classe **String**. Toutes les opérations communes sur les chaînes s'opèrent au moyen de méthodes fournies par cette classe. Les chaînes **Ruby** sont modifiables sur place (contrairement aux chaînes Python).

Les méthodes de la classe **String** peuvent être aisément listées grâce au **shell Ruby interactif PRY**.

`[9] pry(String):1> ls
String#methods: %  *  +  <<  <=>  ==  =~  []  []=  bytes  bytesize  capitalize  capitalize!  casecmp  center  chars
chomp  chomp!  chop  chop!  concat  count  crypt  delete  delete!  downcase  downcase!  dump  each  each_byte  
each_char  each_line  empty?  end_with?  eql?  gsub  gsub!  hash  hex  include?  index  insert  inspect  intern
length  lines  ljust  lstrip  lstrip!  match  next  next!  oct  partition  replace  reverse  reverse!  rindex
rjust  rpartition  rstrip  rstrip!  scan  shellescape  shellsplit  size  slice  slice!  split  squeeze  squeeze!
start_with?  strip  strip!  sub  sub!  succ  succ!  sum  swapcase  swapcase!  to_f  to_i  to_s  to_str  to_sym  tr
tr!  tr_s  tr_s!  unpack  upcase  upcase!  upto`

Les méthodes se terminant par un point d'exclamation sont les versions dites "dangereuses" de celles qui n'en ont pas. Bien souvent ces méthodes opèrent sur la variable elle même tandis que leur jumelle retourne une nouvelle valeur en laissant la variable originale intacte.

UTF-8
-----

Pour activer le support d'UTF-8 pour les chaînes de caractères, définissez la variable globale \$KCODE à 'u'.

`$KCODE = 'u'`

Création
--------

Vous pouvez instancier des objets **String** de nombreuses manières.

En appelant normalement le constructeur.

`name = String.new('John')`

En appelant la méthode **Kernel\#String** (disponible partout, le module Kernel étant chargé par Object, classe ancêtre de toutes les classes).

`name = String('John')`

En utilisant les guillemets simples ou doubles.

`name      = "Doe"    # Interpolation de variables via #{} et conversion des caractères spéciaux \n & co 
firstname = 'John'   # Pas d'interpolation de variables, ni de conversion des caractères spéciaux`

En utilisant d'autres délimiteurs.

`name      = %Q(Doe)    # Idem guillemets doubles, mais permet d'intégrer ces derniers dans la chaîne sans les échapper
firstname = %q(John)   # Idem guillemets simples, mais permet d'intégrer ces derniers dans la chaîne sans les échapper
sex       = %(male)    # Idem %Q()
woot = %Q!Ma chaîne avec "Guillemets" et (parenthèses)!   # On peut même remplacer les parenthèses par d'autres délimiteurs`

Multi-lignes
------------

Il est possible d'instancier aisément des chaînes de plusieurs lignes grâce à la syntaxe **heredoc**. La chaîne se termine par un délimiteur identique à celui d'ouverture.

`animal = 'lapin'
my_quote = <<mydelim
  ce matin un #{animal} a tué un chasseur
  c'était un #{animal} qui avait un fusil.
mydelim
print my_quote`

Entourer le délimiteur du début de guillemets simples désactive l'interpolation de variables et le remplacement des caractères spéciaux.

`my_quote = <<'mydelim'
  ..
mydelim`

La syntaxe **<<-** permet d'indenter le délimiteur de fin qui autrement doit se trouver tout en début de ligne.


`  my_text = <<-'eos'
       /\
      /  \   
     / /\ \  
    / ____ \ 
   /_/    \_\
  eos
  puts my_text`

Concaténation
-------------

La classe **String** fournit 3 méthodes pour la concaténation.

`my_string = "Bonjour " << "le monde"                   # Bonjour le monde
my_string = "Bonjour " + "le monde"                    # Bonjour le monde
my_string = "Bonjour ".concat('le ').concat('monde')   # Bonjour le monde`

On peut également concaténer des chaînes simplement en les plaçant les unes à la suite des autres.

`my_string = "Bonjour " "le " "monde"
puts my_string                                         # Bonjour le monde`

Github préconise l'utilisation de **<<** lorsqu'il s'agit de concaténer de longues chaînes, pour des raisons de performances.

Multiplication
--------------

Il est possible de multiplier une chaîne grâce à la méthode \*.

`puts "Allo " * 2   # Allo Allo`

Interpolation de variables
--------------------------

On utilise **\#{nom\_variable}** dans une chaîne entre guillemets doubles pour que le contenu de la variable soit substitué lors de l'évaluation de l'expression.

`name = "Bond"
puts "My name is #{name}, James #{name}"`

Formatage
---------

Pour la mise en forme des chaînes, plusieurs solutions également, la méthode **%** de String et les méthodes **Kernel\#printf** et
**\#sprintf**.

La manière la plus simple, une chaîne de formatage suivie de **%** et des valeurs.

`number = 55
puts "Nombre formaté %010d" % number   # Nombre formaté 0000000055
number = 33.45456456
puts "Nombre formaté %.2f" % number   # Nombre formaté 33.45
string = "lapin"
puts "ce matin un %10s" % string   # ce matin un      lapin
array = %w(lapin chasseur)
puts "ce matin un %s a tué un %s" % array   # ce matin un lapin a tué un chasseur`

Alternative, **printf** et **sprintf**, la dernière retournant la valeur au lieu de l'afficher.

`printf("ce matin un %s a tué un %s\n", 'lapin', 'chasseur')   # ce matin un lapin a tué un chasseur`

Conversion de type
------------------

Contrairement à PHP, **Ruby** n'effectue pas de conversion implicite des types. Dès lors si vous exécutez un code tel que

`a = "5"
b = 5
puts a + b`

Vous obtiendrez une erreur :

`can't convert Fixnum into String (TypeError)`

Il est nécessaire de convertir les chaînes explicitement au moyen des méthodes **String\#to\_\***.

`a = "5".to_i   # a est maintenant un entier
b = 5
puts a + b     # 10`

Tableau vers chaîne
-------------------

Il est possible de former une chaîne à partir des différents éléments d'un tableau grâce à **Array\#join**.

`puts [ "Al", "ex", "andre" ].join   # "Alexandre"
puts [ "S", "O", "S" ].join("-")    # "S-O-S" avec un caractère glue spécifique`

Taille
------

La taille d'une chaîne s'obtient grâce aux méthodes **String\#length** et son alias **String\#size**.

`puts "Mangez des pommes".size   # 17`

Pour tester si un chaîne est vide, utilisez la méthode **String\#empty?** qui retourne un booléen.

`puts "Suis-je une chaîne vide".empty?   # false`

Comparaison
-----------

Pour vérifier si deux chaînes sont identiques, utilisez la méthode **String\#eql?** ou la méthode **==**.

`a = "Mens sana in corpore sano"
b = "Mens sana in corpore sano"
puts a.eql?(b)   # true
puts a == b      # true
puts a != b      # false`

Pour déterminer quelle chaîne précède l'autre dans l'ordre alphabétique, utilisez l'opérateur **spaceship**.

`puts "a" <=> "b"   # -1 car "a" est inférieur à "b"
puts "a" <=> "a"   # 0 car "a" est égale à "a"
puts "b" <=> "a"   # 1 car "b" est supérieur à "a"
puts "B" <=> "a"   # -1 car B (ASCII 66) est inférieur à "a" (ASCII 97)`

Utilisez la méthode **String\#casecmp** pour déterminer l'ordre sans vous préoccuper de la casse.

`puts 'B'.casecmp('a') <=> "a"   # 1 car la casse n'a plus d'importance`

Code ASCII
----------

Pour convertir un caractère en son code ASCII et réciproquement, on utilise **Fixnum\#ord** et **Fixnum\#chr**.

`[17] pry(main)> 'a'[0].ord
=> 97
[18] pry(main)> 65.chr
=> "A"`

Elagage
-------

Lors d'une saisie au clavier, il peut être nécessaire de supprimer le caractère fin de ligne. A cette fin, on emploie **String\#chomp** qui par défaut supprime **\\n**, **\\r** et **\\r\\n**.

`puts "Eloïse\n".chomp   # Eloïse`

On peut également spécifier la fin de chaîne à supprimer en paramètre.

`puts 'ce bon vincent'.chomp('cent')   # ce bon vin`

La méthode **String\#chop** existe également et supprime quant à elle le dernier caractère quel qu'il soit.

`puts 'Jupilere'.chop   # Jupiler`

Pour supprimer les espaces vides en début et fin de chaîne enfin, on peut utiliser les méthodes **String\#strip**, **lstrip** et **rstrip**.

`puts "  bla  ".strip    # "bla"
puts "  bla  ".rstrip   # "  bla"
puts "  bla  ".lstrip   # "bla  "`

Extraction
----------

Pour extraire des portions de chaînes, on utilise la méthode **String\#slice**. Comme il s'agit là d'un besoin récurrents, **Ruby** propose la syntaxe raccourcie **[]**.

`my_string = 'Laisse moi kiffer la vibe avec mon mec'
p my_string[0].chr        # L => caractère à la position 0
p my_string[0,6]          # Laisse => début pos. 0, taille de 6
p my_string[0..5]         # Laisse => début pos. 0, fin pos. 5
p my_string[0...5]        # Laiss  => début pos. 0, fin pos. 5 exclue
p my_string['moi']        # moi    => la chaine cherchée ou nil si pas présent
p my_string.slice(0..5)   # équivalent à [0..5]`

Position sous-chaîne
--------------------

Il est possible de déterminer la position d'une sous-chaîne dans une chaîne à l'aide de **String\#index** et **rindex**.

`my_string = "ch'suis pas d'humeur à c'qu'on m'prenne la tête"
p my_string.index('pas')   # 8
p my_string.index('h')     # 1
p my_string.index('h',2)   # 14
p my_string.rindex('h')    # 14`

Remplacement
------------

Pour remplacer une portion de chaîne, on utilise **String\#sub** et **String\#gsub**.

`my_string = "Born to be wiiild .."
p my_string.sub('wiiild', 'alive')   # "Born to be alive .."  => mot entier
p my_string.sub('i', '*')            # "Born to be w*iild .." => simple caractère
p my_string.gsub('i', '*')           # "Born to be w***ld .." => toutes les occurences du caractère`

Transformation
--------------

Les diverses transformations de chaînes habituelles sont évidemment possibles au moyen d'autres méthodes.

`puts "ruby".upcase           # RUBY     => majuscules
puts "RAILS".downcase        # rails    => minuscules
puts "loligrub".capitalize   # Loligrub => initiales en majuscules
puts "cOfur".swapcase        # CoFUR    => inversion de la casse
puts "ellieriM".reverse      # Mireille => inversion de l'ordre des caractères de la chaîne`

Itération
---------

La classe **String** offre plusieurs possibilités d'itérations.

`my_string = "abc"
my_string.each_byte do |val|
  print "#{val} "    # 97 98 99
end`

`my_string.each_char do |char|
  print "#{char} "   # a b c
end`

`my_text = <<MULTILINE
Ligne 1
Ligne 2
MULTILINE
my_text.each_line do |line|
  print "#{line.chomp} "   # Line 1 Line 2 
end`
