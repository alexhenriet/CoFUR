Title: Les expressions régulières
Menu: Doc
Slug: documentation/expressions-regulieres
Author: Alexandre Henriet

Présentation
------------

Le concept d'expressions régulières, aussi appelées expressions rationnelles (en anglais regular expressions, RegExp) n'est pas propre à **Ruby**, celles-ci existent dans la plupart des langages de programmation.

Basiquement, les expressions régulières sont définies par un masque (pattern) et ont vocation à être comparées à des chaînes de caractères afin de déterminer si celles-ci leur correspondent. Elles peuvent également servir à capturer et/ou à remplacer des portions de chaînes.

Le masque en question est une séquence plus ou moins complexe de caractères qui prennent une signification particulière comme vous le verrez dans cette page.

Les expressions régulières ont tendance à rebuter les développeurs en raison de leur apparente complexité. Il s'agit pourtant d'un outil puissant et fort pratique dès qu'il est question de manipuler du texte.

Classe Regexp
-------------

Comme tout en **Ruby**, une expression régulière est un objet, instance de la classe **Regexp**. On peut créer ce genre d'objets de trois manières :

via le constructeur

`mask = Regexp.new('test')`

via la syntaxe raccourcie

`mask = /test/`

via la syntaxe alternative

`mask = %r{test}`

Le résultat est identique

`puts mask.class     # Regexp
puts mask.inspect   # /test/`

Pour comparer notre expression régulière à une chaîne, on peut utiliser la méthode **Regexp\#match** en lui passant en paramètre la chaîne en question. La valeur de retour sera une instance de la classe **MatchData** en cas de correspondance et **nil** dans le cas contraire.

`[6] pry(Regexp):1> mask = /test/
=> /test/
[7] pry(Regexp):1> mask.match('test')
=> #<MatchData "test">
[8] pry(Regexp):1> mask.match('tes')
=> nil`

L'objet **MatchData** fournit quelques informations sur la correspondance.

`my_string = 'Mens sana in corpore sano'
mask = /in/
res = mask.match(my_string)
p res.to_S        # "in"
p res.pre_match   # "Mens sana "
p res.post_match  # " corpore sano"
p res.string      # "Mens sana in corpore sano"`

L'opérateur **Regexp\#=~** peut également être utilisé pour comparer une regexp à une chaîne. Il retourne un Fixnum indiquant la position de la correspondance dans la chaîne lorsque correspondance il y a et nil dans le cas contraire.

`my_string = 'Mens sana in corpore sano'
mask = /in/
p mask =~ my_string   # 10`


Masques (Patterns)
------------------

La complexité des expressions régulières réside dans la richesse de la syntaxe de ces masques. En effet on n'utilise rarement les expressions régulières pour chercher des sous-chaînes simples dans d'autres chaînes, la classe **String** dispose d'autres méthodes pour ça.

### Alternatives

Dans un masque, le caractère **|** (pipe) permet de spécifier une alternative.

`/chat|chien|lapin/    # match "Oh le joli chat" "Oh le joli chien" et "Oh le joli lapin"`

### Regroupements

Les parenthèses permettent de créer des regroupements.

`/al(e|i)x/   # match "alex" "alix"`

### Ancres

Par défaut **Regexp\#match** recherche votre masque n'importe où dans la chaîne. Ajouter un **^** en début de masque signifie que le masque doit correspondre au début de la ligne.

`[1] pry(main)> mask = /^ce/
=> /^ce/
[2] pry(main)> mask.match('as-tu veux cet ours ?')
=> nil
[3] pry(main)> mask.match('cette girafe a un long cou')
=> #<MatchData "ce">`

A l'inverse, ajouter un **$** en fin de masque signifie qu'il doit correspondre à la fin de la ligne.

`[4] pry(main)> mask = /sou$/
=> /sou$/
[7] pry(main)> mask.match("C'est le plus grand boss de toute la ville, Picsou Picsou")
=> #<MatchData "sou">
[8] pry(main)> mask.match("Je n'ai plus un sou en poche")
=> nil`

**\\A** et **\\Z** peuvent être utilisés pour faire référence au **début et à la fin de la chaîne** dans le cas d'une chaîne qui contient  **plusieurs lignes**.

Soit la chaîne : "Tic et Tac\\nrangers du risque\\n"

`/^rangers/    # match car il s'agit d'un début de ligne
/\Arangers/   # pas de match car il ne s'agit pas du début de la chaîne
/^tic/        # match car il s'agit d'un début de ligne
/\Atic/       # match car il s'agit du début de la chaîne 
/Tac$/        # match car il s'agit d'une fin de ligne
/Tac\Z/       # pas de match car il ne s'agit pas de la fin de la chaîne`

### Liste de caractères

En utilisant les crochets **[]**, il est possible de spécifier une **liste de caractères autorisés**.

`[32] pry(main)> mask = /b[aeiou]/
=> /b[aeiou]/
[33] pry(main)> mask.match("ba")
=> #<MatchData "ba">
[34] pry(main)> mask.match("be")
=> #<MatchData "be">
[35] pry(main)> mask.match("bi")
=> #<MatchData "bi">
[36] pry(main)> mask.match("by")
=> nil`

Un **^** au début d'une liste de caractères en fait une **liste de caractères non-autorisés**.

`[37] pry(main)> mask = /b[^aeiou]/
=> /b[^aeiou]/
[38] pry(main)> mask.match("ba")
=> nil
[39] pry(main)> mask.match("be")
=> nil
[40] pry(main)> mask.match("bi")
=> nil
[41] pry(main)> mask.match("by")
=> #<MatchData "by">`

Les crochets permettent également de spécifier des **intervalles de caractères**.

`/[a-z]/    # match un caractère entre a et z
/[A-z_]/   # match un caractère entre A et Z et a et z et _
/[0-9]/    # match un chiffre entre 0 et 9`

### Quantificateurs

Des **quantificateurs** permettent de spécifier le nombre de fois que sont censés apparaître les motifs qui les précèdent.

Le motif précédent est censé apparaitre :

`?     # 0 ou 1 fois
+     # 1 ou plusieurs fois
*     # 0 ou plusieurs fois
{m,n} # au minimum m fois et au maximum n fois
{m}   # précisément m fois`

Par exemple

`/^a?c$/        # match "c" et "ac"
/^[abc]{2}d$/  # match aad abd acd bad bbd bcd cad cbd ccd`

### Le caractère point

Le caractère **.** dans un masque correspond à **n'importe quel caractère**.

`/.*/   # masque universel correspondant à toutes les chaînes`

### Séquences spéciales

Les séquences suivantes lorsqu'elle apparaissent dans un masque correspondent à des séries particulières de caractères.

`\d          # match un chiffre entre 0 et 9, comme [0-9]
\D          # match tout sauf un chiffre, comme [^0-9]
\w          # match [A-z0-9_]
\W          # match [^A-z0-9_]
\s          # match un caractère de type espace [\n\r\f\s\t]
\S          # match tout sauf un espace [^\n\r\f\s\t]`

Il en va de même pour les classes POSIX ci-dessous.

`[:alnum:]   # match [A-Za-z0-9]
[:alpha:]   # match [A-Za-z]
[:blank:]   # match caractère blanc et tabulation
[:space:]   # match caractère de type espace
[:digit:]   # match chiffres
[:lower:]   # match lettres minuscules
[:upper:]   # match lettres majuscules
[:print:]   # match tout caractère imprimable, espace y compris
[:graph:]   # match tout caractère imprimable, espace exclus
[:punct:]   # match tout caractère imprimable, sauf alphanumériques et espaces
[:cntrl]    # match caractères de contrôle (de 0x00 à 0x1F et 0x7F)
[:xdigit:]  # match caractères de tyep hexadécimaux [0-9a-fA-F]`

### Echappement

Lorsque votre masque doit réellement contenir un **.**, un **?**, une **(** ou n'importe lequel des caractères spéciaux évoqués plus haut sur cette page, il est nécessaire de l'échapper avec **\\**.

`/Vous allez bien \?/   # match "Vous allez bien ?"`

### Options

Les expressions régulières acceptent certaines options dont voici les fonctions.

`/i   # insensible à la casse
/x   # espaces ignorés
/m   # multi-ligne, les caractères retour à la ligne devenant des caractères normaux
/o   # substitution #{...} unique`

Ces options se placent après le **/** fermant du pattern.

`/mon/i   # match mon MON mOn MoN ...`

### Captures

Les parenthèses **()** permettent également de capturer des portions de chaînes. Les éléments capturés sont accessibles soit via l'objet **MatchData**, soit dans les variables **$1-\>$9**.

Exemples

`mask = /^Mens (\w+) in (\w+) sano$/
match = mask.match('Mens sana in corpore sano')
puts "#{match[1]} et #{match[2]}" if match   # "sana et corpore"
if mask =~ 'Mens sana in corpore sano' then
  puts "#{$1} et #{$2}"   # "sana et corpore"
end`

En **Ruby 1.9**, il est également possible d'assigner les captures à des variables aux noms plus évocateurs.

`mask = /^Mens (?<first>\w+) in (?<second>\w+) sano$/
if mask =~ 'Mens sana in corpore sano' then
  puts "#{first} et #{second}"   # "sana et corpore"
end`

### Non-glouton (Non greedy)

Par défaut, les quantificateurs **+** et **\*** sont dits **gloutons**. C'est à dire qu'ils cherchent à absorber la plus longue chaîne possible (les gourmands !)

Un exemple

`my_string = "Ce matin un lapin a tué un chasseur, c'était un lapin qui avait un fusil"
mask = /Ce(.*)un/   # capture " matin un lapin a tué un chasseur, c'était un lapin qui avait "`

Une fois le "Ce" découvert en début de chaîne, la recherche du "un" s'effectue en partant de la fin de la chaîne, avec pour résultat la plus grande correspondance possible. Lorsqu'on accole un **?** à ces deux quantificateurs, ce comportement s'inverse, on passe en mode non-glouton (non greedy).

Le même exemple

`my_string = "Ce matin un lapin a tué un chasseur, c'était un lapin qui avait un fusil"
mask = /Ce(.*?)un/   # capture " matin "`


Les RegExp sont partout
-----------------------

Des méthodes de différentes classes acceptent des expressions régulières comme paramètres, en voici quelques exemples.

### String\#slice

`p "Ce matin un lapin"[/Ce.*un lapin/]       # match "Ce matin un lapin"
p "Ce matin un lapin"[/Ce(.*)un lapin/,1]   # capture " matin "
p "Ce matin un lapin"[/Ce.*une fouine/]     # pas de match`

### String\#index

`my_string = <<TEXT
Sur le pont, d'Avignon,
On y danse, on y danse,
Sur le pont, d'Avignon,
On y danse tous en rond.
TEXT
p my_string.index(/p.nt/)      # 7
p my_string.index(/p.nt/, 8)   # 55`

### String\#scan

`my_string = "ba be bi bo bu" 
p my_string.scan(/b./)   # ["ba", "be", "bi", "bo", "bu"]`

### String\#sub

Il est possible de combiner capture et remplacement. Les éléments
capturés sont accessibles via **\\\#**.

`$KCODE = 'u'
p "J'ai vu le voleur s'enfuir par là".sub(/le (\w+)/, "les \\1s")   # "J'ai vu les voleurs s'enfuir par là"`

### String\#gsub

Il est possible de passer un bloc à **gsub**. Celui-ci recevra chacune des correspondances en argument et sa valeur de retour sera utilisée comme valeur de remplacement.

`p "aLeX XaVieR OlIviEr".gsub(/\w+/) { |name|
  name.capitalize
}   # "Alex Xavier Olivier"`

### Enumerable\#grep

`my_string = <<TEXT
C'est un beau roman,
C'est une belle histoire,
C'est une romance d'aujourd'hui
TEXT
p my_string.grep(/une/)   # ["C'est une belle histoire,\n", "C'est une romance d'aujourd'hui\n"]`

**Attention : En Ruby 1.9, les objets de la classe String ne sont plus des Enumerable.**

Testeur en ligne
----------------

Il existe en ligne des outils tel que <http://rubular.com/> pour tester les expressions régulières.
