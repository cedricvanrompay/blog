---
publication date: 2019-04-27
lang: fr
slug: mesure-de-la-qualite-de-l-air-un-projet-plateforme-pour-le-lycee
---

# Mesure de la qualité de l'air : un projet-plateforme pour le lycée

Il y a quelques mois j'ai eu l'opportunité d'échanger et de travailler avec des professeurs du Lycée Français de Barcelone. En particulier je me suis penché avec eux sur les nouveaux programmes de « Sciences numériques et technologie » et sur la réalisation de projets par les élèves.

Tout cela m'a donné une idée que j'appelle un « projet-plateforme » et qui consiste principalement en un ensemble de projets liés entre eux par un thème commun (ici la mesure de la qualité de l'air par des stations). Ces projets pourraient être intégrés entre eux mais seraient aussi pensés pour pouvoir être réalisés de manière indépendante, sans avoir besoin d'avoir déjà réalisé d'autres projets. Par exemple il n'y aurait pas besoin de construire une station pour commencer à faire de l'analyse de données : on pourrait utiliser les données produites par d'autres stations déjà construites.

Le nom de « projet-plateforme » reflète l'idée d'un projet de grande échelle qui sert de « base » à un ensemble de sous-projets plus ou moins indépendants.

Voici les sous-projets qu'on peut envisager pour ce projet-plateforme :

- la création de la station de mesure en elle-même (typiquement, une raspberry pi ou une arduino sur laquelle on branche des détecteurs)
- la programmation de la routine d'acquisition (programmer la raspberry pi / arduino pour démarrer, effectuer des mesures, enregistrer et/ou transmettre les résultats ...)
- exploitation des données : calculs statistiques, corrélation avec d'autres phénomènes, validation d'hypothèses, visualisation ...
- publication des données, typiquement sous forme d'un site web
- agrégation de résultats venant de plusieurs stations

Ces projets couvrent une grande partie des thèmes présents dans [le nouveau programme « Sciences numériques et technologie » de la classe de seconde][prog-seconde]. Plus précisément, ce projet-plateforme couvre naturellement les thématiques suivantes :

- Internet
- Le Web
- Les données structurées et leur traitement
- Informatique embarquée et objets connectés

L'agrégation de données venant de plusieurs stations peut permettre de couvrir également la thématique « localisation, cartographie et mobilité ». On pourrait réfléchir à des projets se rattachant à ce projet-plateforme qui couvriraient les deux thématiques restantes, « les réseaux sociaux » et « la photographie numérique ».

[prog-seconde]: https://cache.media.education.gouv.fr/file/SP1-MEN-22-1-2019/08/5/spe641_annexe_1063085.pdf

Le thème de la mesure de la qualité de l'air a d'autres avantages comme projet-plateforme. Tout d'abord il touche à un sujet important, la pollution et l'environnement. Ce thème est aussi en lien étroit avec des matières comme la physique et la chimie et permet d'étendre le « projet-plateforme » encore plus dans la scolarité des élèves. On pourrait envisager que les élèves effectuent en cours de mathématique des calculs relatifs aux phénomènes étudiés en cours de physique/chimie sur des données mesurées par la station qu'ils ont construite en cours de technologie. Un bel exemple de pluridisciplinarité et de synergie, et sans doute un moyen efficace d'intéresser d'avantage les élèves en rattachant des cours qui peuvent paraître « abstraits » et « impersonnels » à quelque chose de définitivement concret et qu'ils puissent s'approprier.

Pour faciliter l'intégration des différents sous-projets entre eux et donner la possibilité aux élèves d'en réaliser un sans avoir nécessairement à réaliser les autres, il faudrait avoir une implémentation de référence qui serve de norme pour les implémentations des élèves et de substituts aux projets dont ils dépendent mais qu'ils n'ont pas réalisé.

Avoir plusieurs classes voire établissements qui réalisent des projets similaires et/ou interopérables serait une chose fantastique. Cela ajouterait des dimensions de compétition et surtout de collaboration qui pourraient décupler l'intérêt des élèves et révéler des vocations.

Aussi, un projet-plateforme bénéficie d'un « effet de réseau » : une fois qu'un des projets est réalisé dans le cadre d'un cours, cela devient encore plus intéressant pour les autres cours ou les autres classes de trouver des moyens de s'y rattacher. Et au fur et à mesure que des établissements rejoignent un même projet-plateforme, il deviendra de plus en plus intéressant pour les autres établissements de le rejoindre également.

Je me dis donc que si je trouve le temps et la motivation, je devrais réaliser moi-même une première série d'implémentations de projets qui s'appuie sur cette plateforme, pour pouvoir fournir des projets presque « clé en main » aux enseignants qui voudraient rejoindre le programme.

Je ne peux pas m'empêcher de réfléchir à d'autres thèmes que la mesure de la qualité de l'air, mais j'ai du mal à en trouver qui se prêtent aussi bien à ce concept de « projet-plateforme » scolaire.
