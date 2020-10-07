# defiiv
# Défi Interactions Vocales

Ce défi constitue une introduction au monde des interactions vocales humain-machine. Ce domaine complexe suppose d'enchaîner de nombreux traitements automatiques afin de reproduire (du moins en apparence) les capacités communicationelles des humains. Quelques heures sont donc une goutte d'eau pour appréhender ce vaste champ de recherche et de développement. Malgré tout le recours à un outil complet doit permettre de se donner un aperçu haut-niveau des implications de la mise en oeuvre d'une plateforme conversationnelle.

Nombreuses possibilités commerciales :

Quelques propositions libres (du moins open-source, l'usage peut être limitée dans certains cas, notamment commerciaux) :

Nous retiendrons la solution [RASA](rasa.com).

## Presentation de RASA

RASA c'est RASA qui en parle le mieux : 

## Installation locale de RASA-X

RASA-X devrait être pré-installé sur les machines, afin de nous faire gagner du temps et éviter de surcharger les profils intinérants. Toutefois une procédure d'installation simple est disponible <url>, à laquelle nous ajoutons quelques éléments :
'''
  code
  '''
  
## Initialisation et lancement

Pour initier un premier bot RASA simple, il suffit :
```bash
rasa init
```
Ensuite un certain nombre de fichiers YAML sont créés permettant de configurer son bot :
```bash
config.yml
domain.yml
```
