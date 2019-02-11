---
publication date: 2019-02-11
lang: fr
---

# Un TP Niveau Lycée sur la Signature RSA

Je suis très fier d'annoncer l'inauguration d'un TP que j'ai conçu sur la signature numérique RSA. Le TP a été fait le 1er Février 2019 par les élèves de terminale S option informatique du Lycée Français de Barcelone.

Le TP est hébergé à l'adresse suivante:

<https://cedricvanrompay.gitlab.io/tp-rsa>

L'idée de ce TP a été dans ma tête pendant un certain moment. Pendant mes études et mon doctorat à EURECOM, il y avait tous les ans un TP qui présentait RSA, et chaque année en le supervisant je ne pouvais m'empêcher de penser à ce que j'aurais voulu changer dans ce TP. Rapidement, voici les principales différences qu'introduit ce TP:

- Le TP commence par les opérations centrales de RSA (chiffrement/ déchiffrement) en utilisant des clés générés pour les élèves, et dans un second temps seulement la génération de clés est étudiée.
- L'étude de l'algorithme d'Euclide étendu, nécessaire pour la génération de clé, est passée pour gagner du temps (une implémentation de l'algorithme est donnée prête à être utilisée)
- Le TP encourage une bonne méthodologie de développement informatique: l'élève doit créer des fonctions qu'il doit utiliser et composer ensuite, et il apprend à utiliser une bibliothèque logicielle (et à la lire).
- Les tâches de programmation sont de la forme « code à trou ».
- C'est la signature RSA qui est étudiée et non le chiffrement RSA parce que les exemples concrets semblent plus faciles à trouver avec de la signature.
- Le TP amène rapidement à des exemples issus du monde réel. Ici à la fin du TP l'élève télécharge les certificats de véritables sites Internet et vérifie leur authenticité en validant la signature RSA qu'ils contiennent. Difficile de faire plus concret.

Je tenais à ce que les élèves commencent par utiliser des clés RSA avant de voir comment les générer pour plusieurs raisons: tout d'abord il me semble que les élèves comprennent mieux la génération de clé s'ils ont déjà vu comment allaient être utilisées ces clés qu'ils doivent générer. En fait il me semble que c'est aussi comme cela que les systèmes sont conçus: on commence par se rendre compte de ce qu'on pourrait faire avec des nombres ayant telle ou telle propriétés, et ensuite seulement on cherche à trouver comment obtenir de tels nombres. Aussi à commencer par des préliminaires dont le sens n'aparaîtera que plus tard dans le TP, on prend le risque que l'intérêt des élèves ce soit évaporé d'ici là.

Le fait de ne pas aborder l'algorithme d'Euclide étendu suit la même logique: il est vrai que si notre but était de construire RSA « à partir de rien », on serait obligé d'implémenter cet algorithme. Mais on ne cherche pas à construire RSA à partir de rien, on cherche à comprendre comment il fonctionne. L'algorithme d'Euclide étendu est intéressant en lui-même, mais il ne nous dit pas grand chose sur RSA. Il ne fait que nous fournir l'inverse modulaire d'un nombre, parce qu'on en a besoin. Ne pas l'étudier permet de passer plus de temps sur des points qui parraissent plus essentiels.

Les tâches de type « code à trou » sont fantastiques pour des cours d'initiation à la programmation, parce qu'elles permettent de « forcer » les étudiants à suivre une bonne méthodologie de développement et un bon style d'écriture du code.

Enfin, je suis très content de voir validée mon intuition qu'on peut dès le premier TP sur RSA finir par la manipulation de « vrais » objets RSA. Ce n'est pas sur tous les sujets en cryptographie qu'on a l'opportunité d'aller aussi loin dans l'application pratique, et ce serait dommage de s'en priver.

Le TP va sans doute évoluer, notamment avec l'ajout de sections supplémentaires qui peuvent être données en « bonus » ou être juste là pour les élèves curieux, mais de façon générale il est considéré comme une version finale.

Pour des commentaires, questions et suggestions, j'invite à utiliser le mécanisme d'« issues » de GitLab:

<https://gitlab.com/cedricvanrompay/tp-rsa/issues>

