# MIGRATION WEGOFUNK

Wegofunk.com est un magazine hébergé par Wmaker, or ce prestataire maintient ce service, mais pour combien de temps encore ?

## Wegofunk, c'est quoi ?

Lire la [présentation détaillée](ABOUT.md) et [l'historique est là](https://www.wegofunk.com/Historique-et-presentation-de-l-association-Wegofunk_a953.html)

## Migrer Wegofunk, quelle stratégie adopter ?

Wegofunk utilise [WMaker](https://www.wmaker.net/) pour créer et gérer le contenu du site, et je souhaite récupérer l'ensemble du contenu et le publier via une solution dont j'ai la maitrise totale.
Wmaker est une solution en ligne pour créer son site web. Au moment du passage de wegofunk de site statique en site avec backoffice, je n'avais pas les compétences nécessaires, j'ai bien essayé d'apprendre php ou wordpress mais wmaker m'offrait la possibilité de ne plus gérer la technique pour me consacrer sur la gestion du magazine.
Sauf que, le contenu est désormais captif et je paye encore l'hébergement.
Et puis, le service est vieillissant, c'est une LTS qui est fournie, la version mobile ne fonctionne plus très bien, il y a beaucoup de liens cassés.
C'est risqué de rester plus longtemps sur cette plateforme.

Je souhaite récupérer les données et les métadonnées pour republier sur une nouvelle plateforme.
Je ne sais pas encore si je souhaite créer un nouveau CMS, migrer vers une nouvelle plateforme ou donner accès au contenu via des fichiers statiques.
J'ai arrêté Wegofunk par manque de temps et le manque d'un modèle économique. 
Je ne veux pas partir sur une solution qui m'obligerait à maintenir de suite.

Voici les options que j’ai identifiées :

1. Scraper l’intégralité du site afin de générer une version statique
2. Utiliser l’API de WMaker (https://api.wmaker.net/)
   - données en json
   - données incomplètes
   - nécessite plusieurs appels pour reconstituer entièrement le contenu d’un article
3. Exploiter les fichiers XML de sauvegarde fournis par WMaker, qui semblent être la seule solution d’export complète proposée.
   - intéressant car contient tous les types de publication sur un laps de temps donné
   - il y a des méta données différentes ou complémentaires de celles récupérées par l'api wmaker

Une version "scrappée" était disponible, ainsi que les versions successives, sur [https://funk.free.fr/](https://funk.free.fr/). C'était un instanné pris en 2012. Hélas, free a supprimé récemment cet accès.
Je suis donc partie sur les pistes 2 et 3. À savoir, récupérer un maximum de données.