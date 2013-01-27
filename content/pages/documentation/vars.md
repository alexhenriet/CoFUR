Title: Variables prédéfinies
Menu: Doc
Slug: documentation/variables-predefinies
Author: Alexandre Henriet

Introduction
------------

Il existe en **Ruby** comme dans la plupart des langages des variables et des constantes globales prédéfinies qui contiennent des informations diverses telles que la version du langage, le charset courant, etc. Elles sont reprises de manière plus ou moins exhaustive sur :

- *(en)* <http://web.njit.edu/all_topics/Prog_Lang_Docs/html/ruby/variable.html>
- *(en)* <http://ruby.runpaint.org/globals>

$LOAD\_PATH ou $:
-----------------

Ce tableau contient la liste des répertoires dans lesquels **Ruby** doit chercher un fichier lorsqu'on tente de l'inclure dans un script avec **require**. L'ordre des entrées a de l'importance dans la mesure où le premier fichier portant le nom recherché sera chargé.

`irb(main):001:0> p $LOAD_PATH
["/usr/local/lib/site_ruby/1.8", "/usr/local/lib/site_ruby/1.8/i686-linux", "/usr/local/lib/site_ruby/1.8/i486-linux", 
"/usr/local/lib/site_ruby", "/usr/lib/ruby/vendor_ruby/1.8", "/usr/lib/ruby/vendor_ruby/1.8/i686-linux", 
"/usr/lib/ruby/vendor_ruby", "/usr/lib/ruby/1.8", "/usr/lib/ruby/1.8/i686-linux", "/usr/lib/ruby/1.8/i486-linux", "."]`

Vous pouvez bien entendu le modifier. L'exemple ci-dessous ajoute le chemin complet vers le dossier du script courant au début du tableau s'il n'est pas déjà inclus de manière relative ou absolue.

`$:.unshift(File.expand_path(File.dirname(__FILE__))) unless
  $:.include?(File.dirname(__FILE__)) || $:.include?(File.expand_path(File.dirname(__FILE__)))`

$KCODE ou $-K
-------------

Cette variable permet de spécifier le charset utilisé. Pour que vos accents soient supportés lorsque vous travaillez en **UTF-8**, utilisez simplement

`$KCODE = 'u'`

Pour obtenir plus d'information sur le sujet, visitez *(en)* <http://blog.grayproductions.net/categories/character_encodings>.
