# defiiv
# Défi Interactions Vocales

![Université d'Avignon](Avignon.png)

Ce défi constitue une introduction au monde des interactions vocales humain-machine. Ce domaine complexe suppose d'enchaîner de nombreux traitements automatiques afin de reproduire (du moins en apparence) les capacités communicationelles des humains. Quelques heures sont donc une goutte d'eau pour appréhender ce vaste champ de recherche et de développement. Malgré tout le recours à un outil complet doit permettre de se donner un aperçu haut-niveau des implications de la mise en oeuvre d'une plateforme conversationnelle.


Nombreuses possibilités commerciales :

Quelques propositions libres (du moins open-source, l'usage peut être limitée dans certains cas, notamment commerciaux) :

Nous retiendrons la solution [RASA](https://rasa.com).

## Presentation de RASA

RASA c'est RASA qui en parle le mieux : 

[![Page d'accueil RASA](Screenshot_3.png)](https://rasa.com)

La documentation est assez complète, et très pédagogique. Elle sera à découvrir au fur et à mesure du déroulement du défi.

> Attention : à la gestion du temps, on peut vite passer beaucoup de temps à la lecture de la doc. Il faut bien cibler ses besoins et se limiter à ce qui est nécessaire à la réalisation du défi. l'application sera une occasion d'aller plus avant dans les concepts.

On distinguera notamment bien les nuances entre :
- RASA Open Source, la base de la distribution qui comprend tous les composants pour développer un chatbot
- RASA-X, plateforme web offrant des outils pour assurer la mise au point d'un chatbot RASA
	- Intègre sa propre version de RASA
	- Attention : la dernière version de RASA OS est 2.0.x alors que la version utilisée avec RASA-X est toujours 1.x
	
Avant l'installation locale il est possible de découvrir RASA sur le site au travers [une page playground](https://rasa.com/docs/rasa/playground) permettant de déveloper très rapidement un chatbot élémentaire. 

## Installation locale de RASA-X

RASA-X devrait être pré-installé sur les machines, afin de nous faire gagner du temps et éviter de surcharger les profils intinérants :
```bash
  mkdir monProjetRasax
  source /usr/local/stow/rasax/bin/activate
```
Toutefois une procédure d'[installation locale simple](https://rasa.com/docs/rasa-x/installation-and-setup/install/local-mode) est disponible sur le site de RASA. On notera ensuite dans la documentation que les moyens d'élargir facilement le fonctionnement de l'outil sur des serveurs dans le cloud sont prévus.
  
## Initialisation et lancement RASA-X

Pour initier un premier bot RASA simple, la commande suivante suffit :
```bash
rasa init
```
Ensuite un certain nombre de fichiers YAML sont créés permettant de configurer son bot :
```bash
data/nlu.md
data/stories.md
config.yml
domain.yml
endpoints.yml
actions.py
```
Le rôle de chacun de ces fichiers est clairement précisé dans la documentation et sera explicité plus tard.

Une fois un bot minimal créé il est possible de lancer RASA-X :
```bash
rasa x
```
Si tout va bien (lire les infos sur le terminal pour le vérifier), une page web devrait s'ouvrir pointant sur la page d'accueil :

![Page d'accueil RASA-X](Screenshot_1.png)

[//]: # (> Attention : votre page n'aura pas de conversations au démarrage. L'affichage précédent)

> Attention : le login se fait automatiquement au démarrage. En cas de besoin ultérieur, les identifiants générés automatiquement se retrouvent sour la commande rasa. Par exemple, 3ème ligne ci-dessous :
> ![Identifiants](Screenshot_2.png)
> Si besoin, voir la documentation pour définir votre propre mot de passe.

Lors de cette première ouverture les fichiers présents dans le répertoire sont utilisés pour initier le chatbot. La première étape va être d'entraîner un premier modèle, même sans faire de modifications ou d'ajouts. Le bouton "Train" permet ca.

> Attention : l'entraînement implique la création d'un modèle (en réalité il peut même être composé de plusieurs (sous-)modèles : pour les intents, les stories...). Ensuite il faudra indiquer comme "Actif" ce modèle pour qu'il soit utilisé par le chatbot lors des interactions de test.

> Attention (suite) : après chaque itératon d'entraînement un nouveau modèle est créé et doit être rendu "Actif", sinon on reste sur le précédent !

## Tâche : consultation d'un EdT

La tâche que nous allons traiter est une des parties prévues pour le bot complet prévu dans la partie application : **la consultation d'un emploi du temps**.

Pour information dans la partie application un robot d'accueil complet sera dévelopé. Parmi les fonctions possibles sont prévus :
- consultation d'emploi du temps étudiant
	- connexion à l'API Partage ou BD
	- requête par formation/groupe/enseignant/salle
	- retourne les cours suivant dans la 1/2 journée entamée avec affichage
- consultation occupation salles libre-service
	- connexion à l'API Partage ou BD
	- requête optionnelle par date/heure
	- retourne une salle sans cours affectée pour la date/heure demandée sinon maintenant
- orientation batiment
	- connexion à BD : liste des salles avec descriptif
	- requête par salle
	- retourne la route depuis le hall de la salle avec affichaghe du plan correspondant
- texte prof
	- connexion à BD : liste des enseignants avec contacts
	- requête par nom/cours
	- retourne confirmation envoi d'un sms avec le message dicté
- socialisation
	- état mental ("Comment allez-vous ?")
	- consultation météo
	- blagues

Il s'agira donc de compléter le chabot initié dans le défi puis de le connecter à un robot Pepper pour pouvoir ensuite le tester et enfin utliser des réseaux de neurones pour optimiser son comportement automatiquement. Mais on en reparle plus tard, pour l'instant il faut lancer la version de base du bot...

## Développement du bot 0.1

Dans la première version du bot, l'utilisateur pourra accéder à son emploi du temps en indiquant uniquement la formation et le groupe recherchés. 

Pour l'instant l'action de finalisation de la tâche se limitera à afficher les informations recoltées jusque là, puis à enchaîner sur une nouvelle reqête ou les salutations finales. L'interaction avec une BD sera étudiée plus tard dans le défi.

Pour cette première version du bot, les définitions seront mises directement dans les fichiers. Bien sur le recours a RASA-X directement est possible, mais cela permet de mieux comprendre les choses dans un premier temps.

Voici les fichiers à modifier :
- `data/nlu.md`
- `data/stories.md`
- `domain.yml`
- `endpoints.yml` 
- `actions.py`


Des exemples de chatbot sont donnés dans le répertoire [examples]. Ils sont testables facilement et rapidement avec ```rasa shell``` et leurs fichiers permettent de découvrir plusieurs type d'organisation de chatbot, fonction de la tâche.


## Développement du bot 0.2

On peut afficher une 'compilation' des stories en les sélectionnant (Ctrl) puis en cliquant sur `Compare multiple stories` (icône apparaissant sur la même line que le nom de la storie). Une fenêtre "Compare" apparaît donnant le graphe de toutes les options décrites dans les stories sélectionnées. le même résultat sur l'ensemble des stories existantes peut être obtenu par :
```bash
rasa visualize
```

A partir de la version initiale 0.1 il s'agit maintenant d'augmenter la qualité du bot. Pour cela on peut affiner l'écriture des éléments faits au 0.1 mais il est bien plus simple de procéder directement en utilisant le bot. Ainsi vous pouvez :
1 Vérifier le comportement attendu du bot: s'assurer que les intents et stories prévues fonctionnent comme attendu
2 Ajouter des variantes à l'aide l'interface : utiliser les fonctions d'annotations en ligne permettant de corriger les prédictions du modèle actuel pour lui permettre de distinguer de nouveaux cas
3 Ré-entraîner le modèle
4 Retour au 1

## Développement du bot 0.3

La version prédécente étant stabilisée le bot est augmenté pour pouvoir tenir compte de la date et l'heure dans la demande de l'utilisateur.

## Développement du bot 0.4

La version prédécente étant stabilisée on va pouvoir ajouter quelques interactions plus sociales en plus de la tâche de fond. 

Par exemple, après une invite à l'utilisateur sur son état d'esprit ("Comment ca va aujourd'hui ?"), on va prévoir que si ce dernier affiche une situation négative le bot va lui afficher une photo 'ho trop mignon' (un petit chat,...) avec le commentaire 'Désolé d'entendre ça, j'espére que cette image va vous remonter le moral").

Le bot pourrait aussi proposer une blague ou une info intéressante...

## Développement du bot 1.0

Une fois la mise au point 'interne' jugée suffisante (d'ailleurs selon quels critères ?), il est possible de faire tester son bot par d'autres utlisateurs. En allant dans la page "Conversations", il est possible de générer un lien à partir de l'icone "Share" qui ouvre le menu suivant :

![Invitation](Screenshot_4.png)

L'addresse communiquée permettra à vos collègues d'accéder à votre bot pour le tester et il sera possible de tracer les conversations faîtes par ces derniers pour les superviser ensuite.

> ATTENTION : le lien fournit peut-être basé sur l'addresse locale de votre machine (localhost). Il faut la remplacer par sa vraie IP (à récuperer dans la configuration réseau).

## Développement du bot 1.etc

Une fois le bot stabilisé dans un bon fonctionnement, deux options sont proposées pour la poursuite du défi :
- la connexion à une base de données
- l'ajout d'une interface pour entrées vocales

### Option :floppy_disk: : connexion base de données

Pour cette option il va s'agir de permettre au bot de rechercher les informations voulues par l'utilisateur dans une base de données. Pour cela n'importe quelle base de données disposant d'une librairie python fera l'affaire. Par exemple SQLite3, MangoDB...

Un chatbot illustrant cette capacité est donné dans le répertoire [connexion_db](connexion_db). 

Les opérations sont :
 1. Installation du gestionnaire de base de données souhaité en local (```pip3 install sqlite3```)
 2. Création d'une base dédiée au bot, avec identifiants (ou pas)
 3. Edition de `actions.py` pour adapter l'action de finalisation, en intégrant les opérations de la base de données (dans [db_sqlite.py](connexion_db/travel_agency_bot/db_sqlite.py) ici)
 
Une version plus intégrée à RASA de connexion BD existe depuis peu : les Knowledge Bases. Un tutorial est disponible sur [https://github.com/RasaHQ/tutorial-knowledge-base](https://github.com/RasaHQ/tutorial-knowledge-base). Cela permet en définitive les mêmes capcités que l'accès par les `actions` mais en simpifiant la manipulation des slots lors du dialogue. Toutefois l'approche est récente et proposée actuellement en test dans RASA, laisson-lui le temps de faire ses preuves !

### Option :speaker: : entrées vocales avec UI

Si les échanges de texte sont déjà une fonctionalité avantageuse pour les humains, ils ne sont toujours pas représentatifs du mode principal de communication entre humains : la parole. Aussi nous nous proposons à partir du bot actuel d'ajouter une interface permettant aux utilisateurs de lui parler. 

Bien sur les enjeux liés au développement d'une telle technologie, éminement complexe, sont encore très importants. Toutefois des solutions "clés en main" existent déjà permettant d'éviter toute la difficulté pratique à son déploiement. Ainsi Google offre des accès gratuits à sa solution STT (Speech-to-Text) sur le Cloud (moyennant une limitation mensuel des appels à l'API) ou directement au travers de son navigateur Chrome (sans limitation).

Install Chrome ici :point_right: [![Procédure d'installation de Chrome](google-chrome_00.png)](https://doc.ubuntu-fr.org/google_chrome)

Nous utiliserons cette dernière solution ici afin de développer conjointement l'interface vocale et son GUI dans le contexte bien maîtrisé d'HTML/JS. Un exemple de page utilisant la Web Speech API de Google est donné dans le répertoire [entrees_vocales](entrees_vocales), complétée par un affichage type "chatbot" `chatroom`. Pour l'utiliser, il faut seulement ajuster la ligne 15 qui indique l'url du serveur RASA visé.

Dans le cadre de l'application le même principe sera utlisé pour connecter un robot Pepper à votre bot en passant d'abord par l'API Google Cloud pour obtenir la transcription des entrées vocales de l'utlisateur. Il sera alors possible de converser avec le robot et de collecter les dialogues réalisés pour ensuite procéder à une analyse des données. L'objectif sera lors d'obtenir une stratégie de dialogue optimale l'aide d'un algorithme d'apprentissage par renforcement (par exemple un DQN, Deep Q-Network, basé sur des réseaux de neurones profonds), implémenté avec la librairie TensorFlow/Keras. Mais ca c'est une autre histoire...

![Pepper accueil](Screenshot_5.png)

> Last update: en cas de poursuite du distanciel durant le semestre, le problème sera l'accès aux robots et donc l'application pourra bien sur être développée seulement sous forme d'interface graphique (type UI Web).

&copy; Fabrice Lefèvre, 2020
