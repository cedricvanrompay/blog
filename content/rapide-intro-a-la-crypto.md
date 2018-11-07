---
date published: 2018-11-07
---

# Une (très) Rapide Introduction à la Cryptographie

## Qu'est-ce que la Cryptographie ?

La cryptographie, c'est l'étude de *l'information en présence d'un adversaire*.

Regardons cette définition de plus près. Il faut que ce qu'on veuille protéger soit de l'information: si vous cherchez à protéger votre vélo par exemple, il vous faut un antivol, pas un protocole cryptographique. C'est parce que votre vélo est un objet physique, pas de l'information. Si votre antivol fonctionne avec un code secret, ce code secret, lui, est de l'information: si vous voulez l'envoyer de façon sécurisée à quelqu'un, vous allez peut-être devoir utiliser la cryptographie.

Ensuite dans un problème de cryptographie il faut qu'il y ait un adversaire. C'est plutôt évident ; mais une question intéressante est : comment modélise-t-on cet adversaire ? La cryptographie, comme toute science, demande à modéliser les phénomènes physiques qu'on étudie. Souvent on va modéliser l'adversaire par deux choses: ses capacités (ce qu'il est capable de faire, ce à quoi il a accès) et son objectif (ce qu'il essaie de faire). Dans les exemples pratiques qu'on verra plus tard (chiffrement d'un message avant de l'envoyer à un destinataire), l'adversaire sera capable de lire les messages pendant qu'ils sont en transit entre l'ordinateur de celui qui envoie le message et l'ordinateur de celui qui le reçoit. Par contre l'adversaire n'aura pas accès à quoi que ce soit qui se passe à l'intérieur de ces ordinateurs (la plupart du temps, si un hacker prend le contrôle de votre ordinateur, vous ne pouvez plus vraiment espérer envoyer des messages de façon sécurisée). Du côté de l'objectif, dans le cas du chiffrement de messages ce que l'adversaire cherche à faire est d'apprendre le contenu du message. Le chiffrement est le cas d'application le plus connu de la cryptographie, mais une application au moins aussi répandue est la *signature*, où l'objectif de l'adversaire cette fois-ci n'est pas d'apprendre le contenu du message mais de le modifier sans que le destinataire s'en rende compte.

En pratique, quand on détermine « l'objectif » de l'adversaire, on se demande ce que nous voulons rendre impossible, plutôt que de se poser la question de « ce que veut faire » un hypothétique adversaire. On met en place du chiffrement quand on tient à ce que personne ne découvre le contenu du message, et on déploie de la signature quand on veut que le destinataire ait l'assurance de l'authenticité du message qu'il reçoit.

Enfin, pour avoir un problème de cryptographie il faut que l'information que l'on veut protéger soit *en présence de l'adversaire*. Car en fait le plus simple pour protéger de l'information est d'empêcher l'adversaire d'y avoir accès. Par exemple, on met le message dans une mallette (ou une clé USB) que l'on donne à un messager en qui on a confiance et qui saura protéger la mallette. Comme dans les films *le transporteur* !
Un autre exemple : dans un ordinateur, les programmes n'ont pas directement accès aux fichiers, ils dépendent du *système d'exploitation* (Windows, Mac OS, GNU/Linux …) pour cela. Pour empêcher un programme ou un utilisateur d'avoir accès à un fichier, on peut donc simplement demander au système d'exploitation de n'accorder l'accès à ce fichier qu'à certains programmes ou utilisateurs. Pas besoin de cryptographie. En revanche si une personne a accès physiquement à l'ordinateur (elle peut en ouvrir le boîtier), elle peut démonter le disque dur et le brancher sur un autre ordinateur qui lui laissera lire le fichier. Dans ce cas-là, on va avoir besoin de cryptographie : on va devoir chiffrer le disque dur pour que l'utilisateur malveillant ne puisse pas lire le fichier même s'il réussi à avoir accès au disque !
La cryptographie devient nécessaire quand on ne peut pas empêcher l'adversaire d'avoir accès à l'information que l'on veut protéger. C'est pour cette raison que le développement de la cryptographie à grande échelle a commencé avec les communications par radio: quand on envoie un message par radio, n'importe qui avec une antenne peut le recevoir. On a de l'information, et un adversaire qui y a accès: on a besoin de la cryptographie.

## La Substitution de Mots: Pas Pratique

La première chose à laquelle on pense quand on veut rendre un message incompréhensible pour tout le monde sauf un petit groupe de personnes choisies, c'est d'utiliser des mots dont seul les membres du groupe connaissent la signification. Ça a longtemps été le but de *l'argot* en France, utilisé par les bandits pour ne pas être compris par la police, comme on peut le voir dans [l'*Essai sur l'argot*][balzac-argot] de Balzac. Quelques exemples donnés par Balzac: Une femme est une « largue », dormir se dit « pioncer », qui est encore utilisé aujourd'hui, etc …

Ici le « secret » qui permet de chiffrer les messages et de comprendre les messages chiffrés consisterait donc en une liste de mots, et pour chaque mot le « véritable » mot auquel il correspond. C'est véritablement ce qu'on appelle un dictionnaire.

Alors, est-ce que l'argot est une bonne façon de chiffrer ses communications ? Le principal problème ici est la *taille* du « secret » : il faut garder en mémoire un substitut pour chaque mot du dictionnaire ! C'est un code secret qui demande beaucoup de temps et d'efforts à apprendre. Ce qui le rend aussi difficile à changer: si une des personnes hors du groupe arrive à connaître votre code (dans notre cas: à comprendre l'argot que vous utilisez), il vous faut créer un nouveau code, et re-faire tout ce long travail d'apprentissage.

## La Substitution de Lettres

C'est donc avant tout un problème de facilité d'utilisation qui nous pousse à chercher un autre moyen de chiffrer les communications. Et l'étape suivante la plus logique est sans doute ceci: au lieu de substituer des mots par d'autres mots, on substitue … des lettres par d'autres lettres !

L'avantage est que on a maintenant beaucoup moins de substitutions à se remémorer: 26 lettres si on ne compte pas les accents. Le « secret » peut se représenter simplement par l'alphabet après substitution. Par exemple:

> rwoafensmvubtcypjdxkilhgzq

Indique que la lettre « a » sera remplacée par la lettre « r » lors du chiffrement, la lettre « b » par « w », etc. Beaucoup plus court qu'un dictionnaire d'argot !

Plusieurs problèmes, tout de même: tout d'abord, que fait-on des espaces et de la ponctuation ? Si on les laisse sans les remplacer, cela peut donner beaucoup d'indications sur les mots chiffrés et le sens des phrases. Le mieux est de les supprimer, même si cela peut rendre le texte plus difficile à comprendre. cestpaspratiquemaiscamarche. En pratique, on n'a pas ce problème car on travaille avec des *octets* et non avec des lettres de l'alphabet. Un autre problème est que le texte encodé de cette manière devient impossible à prononcer. Donc ça ne va fonctionner que pour des communication écrites. Si on est obligé de communiquer à l'oral, on va toujours avoir besoin de codes comme l'argot.

Mais à part ça, on a un chiffrement, qu'on pourrait appeler « substitution de lettres », qui est beaucoup plus pratique. Mais est-ce que ça protège vraiment nos messages ? Faites-vous une idée, essayez de déchiffrer ce message chiffré :

> jifbjifxebfidxxidbrkfddffkkyikfxbfxfkymbfxarcxbfomfb

Pas évident, non ? À première vue, ce message ressemble à des lettres prises au hasard. Alors, est-ce que cela veut dire que notre méthode de chiffrement est sécurisée ?

## Casser une Substitution Fixe

La réponse est *non*, et il y a une raison précise à cela: une lettre sera toujours substituée par la même lettre dans tout le message. Si on utilise le « secret » proposé plus haut, la lettre « a » sera *toujours* remplacée par un « r ». Sachant cela, en regardant un message chiffré on peut faire quelques déductions. Par exemple en regardant les lettres apparaissant deux fois de suite : dans notre message chiffré, on voit un double « x », un double « d », un double « f » et un double « k ». Soit ces lettres correspondent à une lettre qui peut être doublée en français comme « l » ou « m », soit cela est dû au collage de mots puisqu'on a supprimé les espaces.

Une méthode encore plus efficace consiste à étudier la fréquence des lettres. En français la lettre « e » est de loin la plus fréquente (à peu près 14%, quand les autres lettres ne dépassent pas 10%). Dans notre message chiffré les lettres les plus fréquente sont la lettre « f » qui apparaît 12 fois, puis les lettres « b » et « x » qui apparaissent 7 fois chacune. Est-ce que la lettre « f » pourrait représenter la lettre « e », alors ? Dans le chiffré, la lettre « f » est très souvent à côté de « x » ou de « b ». En français la lettre « e » est souvent à côté de la lettre « l » ou « s » (on appelle ça un *bigramme* ou *digramme*, et en anglais *bigram*). Le bigramme le plus fréquent est « es », puis « le » ([source][bigram-fr]). Faisons donc le pari que « b » représente un « l », et le « x » un « s ». Dans ce cas le début du message chiffré, « jifbjifx » avec ce motif « jif » qui se répète et se « x » (donc sans doute un « s ») à la fin, peut facilement être reconnu: c'est le mot « quelques ». Ça nous donne des nouvelles lettres, et notamment le « u », ce qui fait que la suite du message chiffré (« ebfidx ») correspond à « ?leu?s » (je mets un point d'interrogation pour les lettres encore inconnues), où la deuxième lettre inconnue (lettre « d » dans le chiffré) est sans doute une lettre qui peut se doubler en français (on a un « dd » dans le chiffré). Donc ? « fleurs » ou « pleurs ». Le groupe « kfddf » dans le chiffré est donc « ?erre », sans doute « terre » ? D'autant plus que la fréquence de « k » correspondrait bien à celle de « t » en français. On en est à:

> quelques?leurssurl?terree???utesleset??les???sle??el

Une rapide recherche sur Google, Qwant ou autre moteur de recherche de « "quelques fleurs sur la terre" » (dans la plupart des moteurs de recherche, les guillemets indiquent qu'on cherche la phrase exacte) amène rapidement à la citation issue des *Misérables* de Victor Hugo:

> Quelques fleurs sur la terre et toutes les étoiles dans le ciel.

Qu'on avait en fait chiffré avec l'alphabet de substitution donné plus haut en exemple.

## Le Chiffre de César: Encore plus Pratique, et Encore plus Faible

Avoir une substitution fixe que l'on ré-utilise pour chaque lettre sans la changer est donc une mauvaise méthode de chiffrement, même si a priori les messages chiffrés obtenus semblent incompréhensibles. L'attaque qu'on vient de voir, en utilisant la fréquence des lettres, est un grand classique de toutes les introductions à la cryptographie, mais souvent elle est présentée contre une méthode de chiffrement appelée le *chiffre de César*, connue pour avoir été utilisée par l'empereur romain Jules César. Il s'agit d'un cas particulier de la substitution que l'on vient de voir, où le nouvel alphabet consiste en une « rotation » de l'alphabet original. C'est à dire qu'on obtient notre nouvel alphabet en « décalant » l'alphabet d'un certain nombre de lettres. Si on le décale de 4 lettres par exemple, la lettre « a » deviendra la lettre « e », « b » devient « f », etc. On obtient l'alphabet suivant:

> efghijklmnopqrstuvwxyzabcd

Où on est revenu au début de l'alphabet après être arrivé à la lettre « z ».

L'avantage de cette méthode par rapport à la précédente, c'est qu'il y a encore moins d'information nécessaire pour pouvoir chiffrer et déchiffrer des messages: au lieu d'avoir à se souvenir de 26 lettres dans l'ordre, il suffit de se souvenir de combien on a décalé l'alphabet (ici: de 4), ou juste de se souvenir de la lettre par laquelle on a remplacé « a » (ici: « e »). C'est suffisamment peu d'information pour pouvoir le retenir sans avoir à le noter, ce qui est extrêmement pratique.

Par contre, c'est encore plus facile à attaquer que la méthode précédente: Avec le message précédent (la citation des *Misérables*), on aurait juste eu à trouver la lettre la plus fréquente dans le message chiffré, faire l'hypothèse qu'elle représente la lettre « e », en déduire l'alphabet de substitution en entier: pas besoin de tous les raisonnements alambiqués qu'on a du faire précédemment !

En fait avec le chiffre de César, le nombre de « clés » possibles est tellement petit (il n'y a que 26 possibilités) qu'on a un moyen encore plus efficace de casser le code: essayer toutes les clés possibles ! Dès qu'une des clés donne un message qui a du sens, on sait qu'on a trouvé la bonne.

## Le Chiffre de Vernam: Incassable mais pas Pratique

On est toujours à la recherche d'une méthode de chiffrement qui soit incassable.

On remarque quelque chose: notre attaque précédente, basée sur la fréquence des lettres, ne marche pas à tous les coups. Ce ne sont que des statistiques, et on peut tomber sur un message où la lettre « e » n'est pas la plus fréquente. Surtout, ces statistiques seront surtout vérifiées sur des messages longs (c'est ce qu'on appelle la *loi des grands nombres* en probabilités, voir [Wikipedia: loi des grands nombres][]). On a eu beaucoup de chance que notre courte citation des *Misérables* respecte aussi bien les fréquences moyennes des lettres en français.

Du coup, on a une première solution: essayer de n'envoyer que de petits messages. On espère que les attaques « par analyse fréquentielle » ne fonctionnent pas bien dans cette situation. Si on doit envoyer un nouveau message, on utilise une nouvelle clé, c'est à dire une nouvelle substitution, qu'il faut envoyer à notre correspondant. Si on pousse la logique jusqu'au bout, on en vient à générer une nouvelle substitution **pour chaque lettre du message**. C'est un peu extrême, et très peu pratique. Mais il se trouve que … c'est incassable.

En fait, c'est même incassable si on utilise le chiffre ce César, du moment qu'on change la « lettre-clé » à chaque lettre du message.

Reprenons notre message précédent:

> quelquesfleurssurlaterreettouteslesetoilesdansleciel

Il y a 52 lettres. On a donc besoin de 52 « clés »: 52 fois de suite, on va tirer au hasard un lettre de l'alphabet. Voici ce que ce ça peut donner:

> ozesedjmfdzauhvlmeuuecelxaokwmkqdbspnzpfewydhexhfsdm

La première lettre est un « o », cela correspond à une rotation de l'alphabet de 14 (la lettre « a », première lettre de l'alphabet, est remplacée par « o » 15ème lettre de l'alphabet). Donc la première lettre du message, « q » (17ème lettre de l'alphabet), sera remplacée par la lettre « e » (17 + 14 = 31, or après la 26ème lettre on revient au début de l'alphabet, donc on arrive à la 31 - 26 = 5ème lettre, c'est à dire « e »).

On continue avec la lettre « u » pour laquelle on va utiliser la clé « z », et ainsi de suite, ce qui donne le message chiffré suivant:

> etiduxnekodulznfdpunitvpbthyqfoiofktgnxqiobduwilhahx

Pourquoi ce code est-il incassable ? C'est un peu technique à montrer, mais on va quand même essayer d'en donner une vision intuitive. Intéressons-nous à la première lettre du message chiffré. L'adversaire voit que c'est un « e ». Quelle peut être la première lettre du message ? Ça pourrait être un « a » avec une clé « e »; ou bien un « b » avec une clé « d »; ou encore un « c » avec une clé « c »; etc … en fait, ça pourrait être n'importe quelle lettre: pour chaque lettre, il y a une clé qui donnerait un « e » après chiffrement. Comme on a tiré la lettre-clé au hasard, l'adversaire ne gagne aucune information en voyant le message chiffré.

Même chose pour la deuxième lettre du message chiffré: elle ne dépend que de la deuxième lettre du message et de la deuxième lettre-clé. Donc à nouveau, l'adversaire n'obtient aucune information. En particulier, cela ne lui sert à rien de connaître la première lettre du message chiffré car il n'y a aucun lien entre la première et la deuxième. Ça n'était pas le cas pour une substitution fixe: la substitution utilisée restait la même d'une lettre à l'autre.

Avec un peu plus de mathématique, on peut montrer la chose suivante: l'adversaire, en recevant le message chiffré, n'a pas plus d'information sur le message qu'avant.

On a enfin un chiffrement incassable.

Incassable, mais très peu pratique ! Pour envoyer un message de 100 lettres, il faut tout d'abord envoyer (de façon sécurisée !) une clé de 100 lettres. Notez que ça n'est pas totalement impossible: on peut rencontrer son correspondant lors d'un rendez-vous organisé et lui donner un très gros livre (ou un disque dur) contenant une clé gigantesque, disons un milliard de lettres qu'on aura tiré au hasard avant. Puis, plus tard, à n'importe quel moment, on pourra envoyer des messages par radio à notre correspondant, lettre-par-lettre. Le correspondant rayera les lettres de la clé au fur et à mesure qu'il les utilise pour déchiffrer les lettres qu'on lui enverra plus tard par radio.

Cette méthode de chiffrement s'appelle en français « Chiffre de Vernam » ou « Chiffrement par la méthode du masque jetable », et en anglais « *one-time pad* » (ou « *Vernam cipher* »). Elle a été utilisée en pratique, notamment (probablement) pour chiffrer les communications de la ligne diplomatique dite « téléphone rouge » qui reliait les États-Unis et l’Union soviétique pendant la guerre froide. Régulièrement, un diplomate convoyait de manière hautement sécurisée d'un pays à l'autre une mallette contenant de nouveaux caractères aléatoires. Puis, à n'importe quel moment, un des pays pouvait envoyer un message à l'autre sans se soucier que quelqu'un écoute le message transmis: tant que la clé restait secrète, intercepter le message ne donnait aucune information. Quand tous les caractères de la dernière clé avaient été utilisés (un caractère de clé utilisé pour un caractère de message envoyé), il fallait générer de nouveaux caractères aléatoires et renvoyer une valise diplomatique (ce qui pouvait être fait en avance).

## Algorithmes de Chiffrement par Flot

Vous l'aurez sans doute deviné, on a aujourd'hui des méthodes de chiffrement qui sont à la fois sécurisées (par forcément invincibles, mais on ne pense pas que quiconque puisse les casser ni maintenant ni dans un futur proche) et qui sont bien plus pratiques que le chiffre de Vernam.

L'une d'entre elles est le chiffrement dit AES (pour *Advanced Encryption Standard*), successeur de DES qui avait un problème de clés trop courtes (DES était donc exposé à l'attaque dite *par force brute* où on essaie simplement toutes les clés possibles, qu'on avait déjà mentionné contre le chiffre de César).

Mais AES et DES font partie de la famille des algorithmes de *chiffrement par bloc*, dont le fonctionnement est un peu différent de ce dont on a parlé jusque là. À la place, je préférerais parler de l'autre famille, celle des *chiffrement par flot*. Un exemple moderne d'algorithme de chiffrement par flot utilisé massivement est Chacha20, une variante d'un autre algorithme appelé Salsa20.

Conceptuellement, un algorithme de chiffrement par flot fait quelque chose de très simple: il prend en entrée une clé secrète relativement courte, et génère un long flux de caractères qui ressemble très fortement à un flux  complètement aléatoire. Cela permet de générer la très longue clé dont on a besoin dans le chiffre de Vernam, en n'ayant en fait qu'une petite clé à communiquer à son correspondant.

Plus précisément, l'algorithme qui transforme la petite clé en un long flux ayant l'air aléatoire s'appelle une *fonction pseudo-aléatoire*.  Dans le cas de Salsa20 par exemple, cette fonction peut générer un flux pseudo-aléatoire d'à peu près mille milliards de Gigaoctets à partir d'une clé de seulement 32 octets.

Comment fonctionne cette fonction pseudo-aléatoire ? Là aussi on ne va pas pouvoir aborder tous les détails, mais voici l'idée: on rempli un tableau (plus exactement une « matrice ») avec la clé, un « compteur » et certaines valeurs pré-définies. Puis on applique une transformation sur ce tableau dont le but est véritablement de mettre un maximum de « désordre » dans le tableau, de tout mélanger. On applique plusieurs fois cette transformation pour maximiser le désordre, puis l'état final du tableau sert de source de caractères pseudo-aléatoires. Pour générer plus de caractères pseudo-aléatoires avec la même clé, on relance la même procédure en augmentant le « compteur » de 1.

Pour quelqu'un qui n'a pas la clé, il est impossible de re-construire ce flux pseudo-aléatoire, et la sécurité est similaire à quand on utilise le chiffre de Vernam.

Chacha20 est utilisé par la quasi-totalité des téléphones et tablettes pour le chiffrement des données sur internet (quand vous allez sur une adresse web commençant par `https://`). Pour voir quels algorithmes votre ordinateur/tablette/téléphone utilise pour se connecter en HTTPS, allez sur le site https://www.howsmyssl.com/ et allez à la section « Given Cipher Suites ». Vous voyez la liste des algorithmes que votre navigateur a suggéré au serveur, par ordre de préférence. Pour moi, avec mon ordinateur portable, les deux premières lignes sont:

    TLS_AES_128_GCM_SHA256
    TLS_CHACHA20_POLY1305_SHA256

Ce qui veut dire que mon ordinateur propose au serveur d'utiliser AES pour le chiffrement, ou en deuxième option d'utiliser Chacha20. Pour la plupart des téléphones et tablettes, Chacha20 est donné en premier choix.

## Conclusion

Évidemment, on n'a fait qu'effleurer le sujet de la cryptographie. Mais faisons tout de même un rapide bilan du chemin parcouru: la cryptographie étudie les situations où on veut protéger de l'information contre un adversaire qui peut y avoir accès. On peut vouloir empêcher l'adversaire de lire cette information, mais pas forcément: on peut aussi vouloir l'empêcher de la modifier par exemple. On peut même viser les deux objectifs à la fois.
Quand on déploie une méthode pour empêcher l'adversaire de lire l'information qu'on protège, on parle de *chiffrement*. Trouver un « code » fixe qu'on appliquera à tout le message est la solution la plus intuitive mais est en fait facile à casser parce qu'on peut voir quand une partie du message se répète. Étrangement, on arrive à un système incassable avec une seule modification qui consiste à changer de « code » à chaque caractère du message. Cette méthode n'est pas pratique du tout, mais on arrive aujourd'hui à créer des fonctions dites « pseudo-aléatoires » qui avec une clé relativement courte arrivent à générer un long flux de caractères qui *ont l'air aléatoires* pour n'importe quel adversaire n'ayant pas cette clé. On peut utiliser ce long flux pour chiffrer un message, et cela donne une famille d'algorithmes de chiffrement dits « par flux ».

Mais cela ne suffit pas à communiquer de façon sécurisée, et avant de pouvoir comprendre comment on arrive à protéger l'information aujourd'hui il faudrait se pencher sur les questions suivantes: comment envoyer à son correspondant la clé qu'on va utiliser pour chiffrer les communications ? Et comment s'assurer que les messages que l'on reçoit viennent bien de la personne avec qui on pense communiquer ?

Tout cela serait l'occasion d'écrire d'autres billets de blog …



[balzac-argot]: https://fr.wikisource.org/wiki/Essai_philosophique,_linguistique_et_litt%C3%A9raire_sur_l%E2%80%99argot,_les_filles_et_les_voleurs

[bigram-fr]: http://practicalcryptography.com/cryptanalysis/letter-frequencies-various-languages/french-letter-frequencies/

[Wikipedia: loi des grands nombres]: https://fr.m.wikipedia.org/wiki/Loi_des_grands_nombres
