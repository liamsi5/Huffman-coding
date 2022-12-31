# Huffman-coding
authors : Ismail Chaf-i

# coding Huffman
<br/>
Le '''codage de Huffman''' est un [[Algorithmique|algorithme]] de [[compression de données sans perte]]. Le codage de Huffman utilise un [[code à longueur variable]] pour représenter un symbole de la source (par exemple un [[Caractère (informatique)|caractère]] dans un fichier). Le code est déterminé à partir d'une estimation des probabilités d'apparition des symboles de source, un code court étant associé aux symboles de source les plus fréquents.

Un code de Huffman est optimal au sens de la plus courte longueur pour un codage par symbole, et une distribution de probabilité connue. Des méthodes plus complexes réalisant une modélisation probabiliste de la source permettent d'obtenir de meilleurs ratios de compression.

Il a été inventé par [[David Albert Huffman]], et publié en 1952.

== Principe ==

Le principe du codage de Huffman repose sur la création d'une structure d'[[Arbre enraciné|arbre]] composée de nœuds.

Supposons que la phrase à coder est « ''{{Langue|en|this is an example of a huffman tree}}'' ». On recherche tout d'abord le nombre d'[[wikt:occurrence|occurrences]] de chaque caractère. Dans l'exemple précédent, la phrase contient 2 fois le caractère ''h'' et 7 espaces. Chaque caractère constitue une des feuilles de l'arbre à laquelle on associe un poids égal à son nombre d'occurrences.

L'arbre est créé de la manière suivante, on associe chaque fois les deux nœuds de plus faibles poids, pour donner un nouveau nœud dont le poids équivaut à la somme des poids de ses fils. On réitère ce processus jusqu'à n'en avoir plus qu'un seul nœud : la racine. On associe ensuite par exemple le code 0 à chaque embranchement partant vers la gauche et le code 1 vers la droite.

[[Image:Huffman tree 2.svg|thumb|350px|Un exemple d'arbre de Huffman, généré avec la phrase « ''{{Langue|en|this is an example of a huffman tree}}'' ».]]

Pour obtenir le [[Système binaire|code binaire]] de chaque caractère, on remonte l'arbre à partir de la racine jusqu'aux feuilles en rajoutant à chaque fois au code un 0 ou un 1 selon la branche suivie. La phrase « ''this is an example of a huffman tree'' » se code alors sur 135 bits au lieu de 288 bits (si le codage initial des caractères tient sur 8 bits). Il est nécessaire de partir de la racine pour obtenir les codes binaires car sinon lors de la décompression, partir des feuilles peut entraîner une confusion lors du décodage.

Pour coder « ''Wikipedia'' », nous obtenons donc en binaire : 101 11 011 11 100 010 001 11 000, soit 24 bits au lieu de 63 (9 caractères x 7 bits par caractère) en utilisant les codes [[ASCII]] (7 bits).

== Différentes méthodes de construction de l'arbre ==
Il existe trois variantes de l'algorithme de Huffman, chacune d'elles définissant une méthode pour la création de l'arbre :

# statique : chaque octet a un code prédéfini par le logiciel. L'arbre n'a pas besoin d'être transmis, mais la compression ne peut s'effectuer que sur un seul type de fichier (ex. : un texte en français, où les fréquences d'apparition du 'e' sont énormes ; celui-ci aura donc un code très court, rappelant l'[[alphabet morse]]) ;
# semi-adaptatif : le fichier est d'abord lu, de manière à calculer les occurrences de chaque octet, puis l'arbre est construit à partir des poids de chaque octet. Cet arbre restera le même jusqu'à la fin de la compression. Cette compression occasionnera un gain de bits supérieur ou égal au codage de Huffman statique mais il sera nécessaire, pour la décompression, de transmettre l'arbre, ce qui annulera généralement le gain obtenu<ref name=":0">{{Ouvrage|langue=fr|langue originale=Américain|auteur1=Mark Nelson|traducteur=Soulard Hervé|titre=La Compression des données|sous-titre=Texte, Images, Sons|lieu=France|éditeur=[[Éditions Dunod|Dunod]]|année=1993|pages totales=420|passage=page 65|isbn=2-10-001681-4}}.</ref> ;
# adaptatif : c'est la méthode qui offre a priori les meilleurs taux de compression car il utilise un arbre connu (et ainsi non transmis) qui sera ensuite modifié de manière dynamique au fur et à mesure de la compression du [[flux]] selon les symboles précédemment rencontrés<ref name=":0" />. Cette méthode représente cependant le gros désavantage de devoir modifier souvent l'arbre, ce qui implique un temps d'exécution plus long. Par contre, la compression est toujours optimale et il n'est pas nécessaire que le fichier soit connu ''avant'' de compresser. En particulier, l'algorithme est capable de travailler sur des flux de données (''{{Langue|en|streaming}}''), car il n'est pas nécessaire de connaître les symboles à venir.

== Propriétés ==
Un code de Huffman est un [[code de source]]. Pour une source <math>S</math>, représentée par une [[variable aléatoire]] <math>X</math>, de [[distribution de probabilité]] <math>p</math>, l'espérance de la longueur d'un code <math>C</math> est donnée par 
{{retrait|1= <center><math>L(C)=\sum_{x \in \Omega}p(x) \cdot l(x)</math></center> }}

Où <math>l(x)</math> est la longueur du mot de code, <math>C(x)</math> le code associé au symbole de source <math>x</math>, et <math>\Omega</math> est l'ensemble des symboles de source.

Un code de Huffman est un [[code préfixe]] à [[code à longueur variable|longueur variable]]. Il est optimal, au sens de la plus courte longueur, pour un codage par '''symbole'''<ref name=Cover>[[#CoTh06|Cover, Thomas (2006)]], {{p.}}123-127.</ref>. C'est-à-dire que pour un code de Huffman <math>C^*</math>, et pour tout code <math>C</math> [[code uniquement décodable|uniquement décodable]], alors :
{{retrait|1= <center><math>L(C^*) \le L(C)</math></center> }}

== Limitations du codage de Huffman ==
On peut montrer que pour une source <math>X</math>, d'[[entropie de Shannon]] <math>H(X)</math>  la longueur moyenne <math>L </math> d'un mot de code obtenu par codage de Huffman vérifie:
{{retrait|1= <center><math>H(X) \le L < H(X)+1</math></center> }}

Cette relation montre que le codage de Huffman s'approche de l'entropie de la source et c'est-à-dire du code optimum mais cela peut s'avérer en fait assez peu intéressant dans le cas où l'entropie de la source est forte, et où un surcoût de 1 bit devient important. De plus le codage de Huffman impose d'utiliser un nombre entier de bit pour un symbole source, ce qui peut s'avérer peu efficace.

Une solution à ce problème est de travailler sur des blocs de <math>n</math> symboles. On montre alors qu'on peut s'approcher de façon plus fine de l'entropie :
{{retrait|1= <center><math>H(X) \le L < H(X)+\frac{1}{n}</math></center> }}
mais le processus d'estimation des probabilités devient plus complexe et coûteux.

De plus, le codage de Huffman n'est pas adapté dans le cas d'une source dont les propriétés statistiques évoluent au cours du temps, puisque les probabilités des symboles se modifient et le codage devient inadapté. La solution consistant à ré-estimer à chaque itération les probabilités symboles est impraticable du fait de sa complexité en temps.
La technique devient alors le '''codage Huffman adaptatif''' : à chaque nouveau symbole la table des fréquences est remise à jour et l'arbre de codage modifié si nécessaire. Le décompresseur faisant de même pour les mêmes causes… il reste synchronisé sur ce qu'avait fait le compresseur.

En pratique, lorsque l'on veut s'approcher de l'entropie, on préfèrera un [[codage arithmétique]] qui est optimal au niveau du bit.

Des méthodes plus complexes réalisant une modélisation probabiliste de la source et tirant profit de cette [[Redondance (théorie de l'information)|redondance]] supplémentaire permettent d'améliorer les performances de compression de cet algorithme (voir [[LZ77]], [[prédiction par reconnaissance partielle]], [[pondération de contextes]]).

== Code canonique ==

Pour un même ensemble de symboles à coder, plusieurs codes de Huffman différents peuvent être obtenus.

Il est possible de transformer un code de Huffman en un code de Huffman canonique qui est unique pour un ensemble de symboles d'entrée donné. Le principe est d'ordonner au départ les symboles dans l'ordre lexical.

Remarque: entre deux symboles S1 et S2 qui, dans un code de Huffman spécifique, sont codés de la même longueur sont toujours codés de la même longueur dans le code Huffman canonique.
Dans le cas où deux symboles ont la même probabilité et deux longueurs de code différentes, il est possible que le passage d'un code de Huffman à un code de Huffman canonique modifie la longueur de ces codes, afin de garantir l'attribution du code le plus court au premier symbole dans l'ordre lexicographique.

{{...}}<!-- exemples ! -->

== Utilisations ==

Le codage de Huffman ne se base que sur la fréquence relative des symboles d'entrée (suites de bits) sans distinction pour leur provenance (images, vidéos, sons{{etc.}}). C'est pourquoi il est en général utilisé au second étage de compression, une fois la redondance propre au média mise en évidence par d'autres algorithmes. On pense en particulier à la compression [[JPEG]] pour les [[Compression d'images|images]], [[MPEG]] pour les [[Compression vidéo|vidéos]] et [[MPEG-1/2 Audio Layer 3|MP3]] pour le [[Compression de débit audio|son]], qui peuvent retirer les éléments superflus imperceptibles pour les humains. On parle alors de compression non conservative (avec pertes).

D'autres algorithmes de compression, dits conservatifs (sans pertes), tels que ceux utilisés pour la compression de fichiers, utilisent également Huffman pour comprimer le dictionnaire résultant.
Par exemple, [[LZH]] ([[Lha]]) et [[deflate]] ([[ZIP (format de fichier)|ZIP]], [[gzip]], [[Portable Network Graphics|PNG]]) combinent un algorithme de [[compression par dictionnaire]] ([[LZ77]]) et un [[codage entropique]] de Huffman.

== Histoire ==
Le codage a été inventé par [[David Albert Huffman]], lors de sa [[thèse de doctorat]] au [[Massachusetts Institute of Technology|MIT]]. L'algorithme a été publié en 1952 dans l'article ''{{Langue|en|A Method for the Construction of Minimum-Redundancy Codes}}'', dans les ''{{Langue|en|Proceedings of the Institute of Radio Engineers}}''<ref>D.A. Huffman, ''A method for the construction of minimum-redundancy codes'', Proceedings of the I.R.E., septembre 1952, pp. 1098-1102.</ref>.

Les premiers [[Macintosh]] de la société [[Apple Inc.|Apple]] utilisaient un code inspiré de Huffman pour la représentation des textes : les 15 caractères les plus fréquents d'une langue étaient codés sur 4 bits, et la {{16e|configuration}} servait de préfixe au codage des autres sur un octet (ce qui faisait donc tantôt 4 bits, tantôt 12 bits par caractère, voir [[UTF-8]]). Cette méthode simple permettait d'économiser 30 % d'espace sur un texte moyen, à une époque où la [[mémoire vive]] restait encore un composant coûteux.

== Voir aussi ==

=== Articles connexes ===
* [[Codage entropique]]
* [[Codage de Shannon-Fano]], similaire au codage de Huffman mais où la méthode de calcul des codes diffère et peut aboutir à un arbre de codage différent.
* [[Compression de données]]

=== Liens externes ===
* [http://huffman.ooz.ie Generateur visuel de l'arbre d'Huffman]

== Bibliographie ==

* {{en}} D.A. Huffman, "[http://compression.ru/download/articles/huff/huffman_1952_minimum-redundancy-codes.pdf A method for the construction of minimum-redundancy codes]" (PDF), Proceedings of the I.R.E., {{date-|septembre 1952}}, pp 1098-1102
* {{en}} {{CoverThomas06}}

== Notes et références ==
{{ Traduction/Référence|en|Huffman coding|324603933}}
{{Références}}

{{Palette|Techniques de compression de données}}
{{Portail|informatique théorique}}

[[Catégorie:Codage entropique|Huffman]]
[[Catégorie:Code préfixe|Huffman]]
