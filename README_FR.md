### [English version here!](README.md)

# Gladys-Xiaomi : un module pour [Gladys](https://github.com/GladysProject)

Ce module vous permet de piloter les appareils Xiaomi Zigbee compatibles depuis Gladys.  
Ce module est retro-compatible avec le module officiel : les périphériques déjà créés ne seront pas recréés.

>A noter que pour "Xiaomi Door & Window", ce dernier pouvant prendre 3 états (ouvert, fermé, inconnu), un nouveau deviceType sera créé.  
>Je pense qu'il est important de savoir si l'information qui est remontée est fiable ou pas.  
Vous pouvez donc supprimez après installation l'ancien, de type "binary".

- [Gladys-Xiaomi : un module pour Gladys](#gladys-xiaomi--un-module-pour-gladys)
  - [Prérequis](#prérequis)
  - [Limitations](#limitations)
  - [Installation](#installation)
    - [Ajouter ce module à Gladys](#ajouter-ce-module-à-gladys)
    - [Redémarrer Gladys](#redémarrer-gladys)
    - [Renseigner le ou les mot(s) de passe de votre ou vos passerelle(s) Xiaomi](#renseigner-le-ou-les-mots-de-passe-de-votre-ou-vos-passerelles-xiaomi)
    - [Logique de fonctionnement de ce module](#logique-de-fonctionnement-de-ce-module)
  - [Caractéristiques des appareils compatibles](#caractéristiques-des-appareils-compatibles)
    - [La gateway](#la-gateway)
    - [Le cube Magique](#le-cube-magique)
    - [Le détecteur de fuite d'eau](#le-détecteur-de-fuite-deau)
    - [Le détecteur d'ouverture de porte ou fenêtre](#le-détecteur-douverture-de-porte-ou-fenêtre)
    - [Les détecteurs de mouvement](#les-détecteurs-de-mouvement)
    - [La prise de courant](#la-prise-de-courant)
    - [Le détecteur de fumée](#le-détecteur-de-fumée)
    - [Le bouton wifi à 3 état](#le-bouton-wifi-à-3-état)
    - [La sonde de température et d'humidité](#la-sonde-de-température-et-dhumidité)
    - [La sonde de température, d'humidité et de pression](#la-sonde-de-température-dhumidité-et-de-pression)
    - [Le capteur de vibration](#le-capteur-de-vibration)
    - [Les interrupteurs muraux filaires](#les-interrupteurs-muraux-filaires)
    - [Les boutons poussoirs muraux sans fils](#les-boutons-poussoirs-muraux-sans-fils)
  - [Annexe 1 : mode "débogage"](#annexe-1--mode-débogage)
    - [Xiaomi_debug](#xiaomi_debug)
    - [Xiaomi_debug_device](#xiaomi_debug_device)
  - [Annexe 2 : Demander l'ajout d'un périphérique](#annexe-2--demander-lajout-dun-périphérique)
    - [Plongée dans les logs de Gladys](#plongée-dans-les-logs-de-gladys)
  - [Annexe 3 : Liste des équipements compatibles](#annexe-3--liste-des-équipements-compatibles)

## Prérequis

Pour fonctionner, ce module nécessite :

- Gladys version 3.10.0 au minimum,
- Une passerelle Xiaomi non-compatible "HomeKit", avec mode "développeur" activé,

## Limitations

Nécessite de connaître le mot de passe "développeur" et l'identifiant de chaque passerelle pour pouvoir la commander depuis Gladys, ainsi que les actionneurs, comme la prise ou l'interrupteur.

Voir [Activation du mode développeur](doc/developper_FR.md)

## Installation

### Ajouter ce module à Gladys

- Installez ce module à partir de l'onglet "avancé" du menu "module", en remplissant les différents champs de la façon suivante :

  - Nom : `gladys-xiaomi`
  - Version : `0.1.2`
  - URL : `https://github.com/piznel/gladys-xiaomi.git`
  - Slug : `xiaomi-home`

### Redémarrer Gladys

Depuis le premier onglet du menu "Paramètres", en cliquant sur le bouton "Redémarrer".

### Renseigner le ou les mot(s) de passe de votre ou vos passerelle(s) Xiaomi

Retourner dans la liste des modules, et cliquez sur le bouton 'configuration'. La page de configuration contient 3 tableaux :

- tableau de gauche :

il contient la liste des passerelles découvertes, leur adresse IP, ainsi que leur mot de passe, si vous l'avez déjà renseigné. Sinon, renseignez le.

- tableau central :

Il contient la liste des périphériques non-gérés actuellement par ce module. Vous pouvez participez au développement de ce module, en suivant les consignes en [annexe](#annexe--demander-lajout-dun-périphérique).

- tableau de droite :

il contient la liste des matériels qui n'ont pas pu être intégré dans Gladys, car ils ont été déclaré par plusieurs passerelle. Voir le paragraphe suivant pour résoudre cette difficulté.

### Logique de fonctionnement de ce module

A chaque démarrage de Gladys, un message est envoyé sur votre réseau, auquel seules les passerelles répondent, en nous donnant leur identifiant (= sid chez Xiaomi), ainsi que leur adresse IP. En retour, nous leur demandons alors la liste des identifiants des périphériques qu'elles ont d'enregistré.

Nous interrogeons alors chaque périphérique, qui nous répond en retour en nous donnant son nom et quelques une de ses caractéristiques.

Puis la liste des périphériques qui ont répondu est envoyés à Gladys pour être créé ou mise à jour.

**Dans le cas où vous avez plusieurs passerelles**, il arrive (rarement) qu'un périphérique apparaisse dans plusieurs listes ; il ne sera pas alors créé dans Gladys. En effet, si ce périphérique est un actionneur, il ne peut être commandé que par une seule passerelle. Et à ce stade, nous ne pouvons savoir laquelle.

Pour lever le doute, nous avons besoin de vous (ou pas !)

*Comment connaître les périphériques coupables ?*

A la fin de l'étape de démarrage, et après la création des périphériques dans Gladys, si un tel périphérique est détecté, dans le tableau adéquate de la page de configuration du module.  
Il vous suffit alors, à l'aide de l'application (ou du bouton d'activation du périphérique) d'actionner ce dernier ; il sera alors créé correctement dans Gladys.

>Vous pouvez aussi ne rien faire :)

En effet, à interval régulier, les périphériques signalent leur présence à la passerelle, donc à Gladys. Il sera créé à ce moment là.  
Cet interval est en général d'une heure pour les périphériques à pile, et de 10 mn pour ceux branché électriquement.

## Caractéristiques des appareils compatibles

*Vous trouverez leur référence dans ce document.*

Ce module ne gère que des appareils fonctionnant en zigbee avec la passerelle correspondante.

>*Tous les ***noms*** en gras et en italiques sont les fonctionnalités que vous retrouverez dans Gladys*

Pour tous les appareils à pile (CR2032 ), Xiaomi préconise une tension comprise entre 2.800 et 3.300 mV. La valeur de la batterie (***battery***) qui apparaît dans Gladys est donc un pourcentage par rapport à ces consignes :

    0% = 2.800 mV
    100% = 3.300 mV

### La gateway

>La gateway ne doit pas être celle compatible "HomeKit"

La gateway possède :

- un capteur de luminosité
- un capteur de mouvement
- un haut-parleur
- un bandeau de led

Depuis Gladys, vous pouvez commander l'allumage (***gateway***), la couleur (***color***) est l'intensité (***intensity***) des led.  
La luminosité (**illumination**) est également enregistrée dans Gladys. Perso, j'ai pas trouvé que la valeur était très fiable.

Le capteur de mouvement n'est pas à ce jour accessible par l'api de Xiaomi. Dommage.  
La diffusion de vos mp3 sur le haut-parleur n'a pas été codé, bien que pouvant l'être. Lors d'une prochaine release ?

> A l'issu de la première installation, si la gateway est éteinte pendant ce processus, la couleur et la luminosité ne seront pas initialisées.  
> Il faut donc, pour pouvoir l'allumer, définir avant une couleur et une luminosité.

### Le cube Magique

Le cube présente de nombreuse ***status***, codifiés de la façons suivantes dans Gladys :

    0 = repos
    1 = Alerte (= vibration)
    2 = double tap (sur une surface dure !)
    3 = bascule sur une face latérale
    4 = bascule sur la face opposée
    5 = rotation à l'horizontale
    6 = secoué en l'air
    7 = poussé ou tiré en le glissant sur le support
    8 = chute libre

Pour la rotation horizontale, l'angle de rotation est également renvoyé dans Gladys (***rotate***), en % d'un tour complet ( 25% = 90°, 50% = 180°, ...), positif ou négatif selon le sens de rotation positif dans le sens des aiguilles d'une montre, négatif dans l'autre sens.
Quant à ***speed***, c'est la vitesse avec laquelle vous avez tourné le cube. De là à faire des concours...

>Après chaque changement d'état, le cube repasse à "repos" après 1 seconde automatiquement.

### Le détecteur de fuite d'eau

Fonctionnement basique, avec 2 ***status*** :

    0 = pas de fuite
    1 = fuite !

### Le détecteur d'ouverture de porte ou fenêtre

Les différents ***status*** sont codés dans Gladys de la façon suivante :

    0 = fermé
    1 = ouvert
    -1 = inconnu. Sera mis-à-jour automatiquement.

### Les détecteurs de mouvement

2 modèles sont compatibles :

- Le modèle Xiaomi
- le modèle Aqara

Pour ces 2 modèles, les différents états (***binary***) sont codés dans Gladys de la façon suivante :

    1 = mouvement
    0 = pas de mouvement

Le modèle "Aqara" renvoie également la valeur de la luminosité (***illumination***), comprise entre 0 et 1200 lux, ainsi que le temps écoulé en minute depuis le dernier mouvement (***minutes***).  
Cette info est renvoyée au bout de 2 minutes, puis 3, 5, 10, 20 et enfin 30 minutes.

### La prise de courant

Les différents états (***binary***) sont codés dans Gladys de la façon suivante :

    1 = allumé
    0 = Eteins

Cette prise permet également de faire un suivi de la consommation :

- La puissance instantannée (***instantaneous_power***), en watt, plafonnée à 2,2 kW,
- la puissance totale (***total_power***), en Wh.

Elle possède également la fonctionnalité ***inuse***, indiquant, dans le cas où elle est sur "on", si du courant est consommé ou non. Utile pour couper automatiquement l'alimentation après la charge de vos téléphones !

### Le détecteur de fumée

Ce capteur de détection de fumée peut avoir plusieurs ***status*** :

    0 = Pas de fumée
    1 = Densité de fumée supérieure à la valeur-limite.
        Déclenchement de l'alarme.
    2 = Test manuel de l'alarme
    8 = Alarme de défaut de batterie
    64 = Alarme de défaut de capteur
    32768 = Alarme de défaut de communication.

Il informe également de la densité de fumée mesurée (***density***).

### Le bouton wifi à 3 état

Les différents ***button*** sont codés dans Gladys de la façon suivante :

    0 = Pas de clic
    1 = clic simple
    2 = clic double
    3 = clic long (bouton appuyé)
    4 = clic long (bouton relâché)

>Après chaque changement d'état, le bouton repasse à "pas de clic" après 1 seconde automatiquement.

### La sonde de température et d'humidité

Capteur classique, permettant d'obtenir la température (***temperature***) en °C et l'humidité (***humidity***) en % du lieu où il se trouve.

### La sonde de température, d'humidité et de pression

En plus des fonctionnalités du capteur précédent, vous obtiendrez également la pression atmosphérique (***pressure***), en Pa.  
>"En quoi ???? c'est quoi Pa ???"  
>Pa = Pascal, unité de mesure de la pression atmosphérique. Comme dans les bulletins météo !

### Le capteur de vibration

Capteur assez complexe :

il peut prendre différents ***status***, codifiés de la façon suivante :

    0 = repos
    1 = vibration
    2 = tilt (changement d'orientation)
    3 = chute libre

Après chaque "tilt", la valeur de la rotation (***angle***) en degré est renvoyée dans Gladys. Elle est toujours positive.
**exemple :** en passant le capteur de la position horizontale à la position verticale, "***angle***" prendra une valeur aux alentours de 90°.

Ce capteur, grâce à son accéléromètre, retourne également les valeurs de l'accélération de la pesanteur selon les axes x, y et z. L'unité est en millième de g, accélération de la pesanteur (le même g que celui des pilotes, qui subissent des accélérations de plusieurs g).

Que faire de ces 3 valeurs d'accélération ? Elles peuvent servir, par exemple, à déterminer qu'elle est la face supérieure visible du capteur. Je vous ai fait le calcul : c'est la valeur "***side***" dans Gladys.  
Les faces sont numérotées de `0` à `5`. Je vous laisse découvrir quel nombre correspond à quelle face :)

>"Super ton truc, mais je peux en faire quoi ???"  
Ca peut servir, par exemple, à déterminer la position d'un objet pouvant être mis en rotation comme :
>
>- une porte de garage à basculement (pour savoir si elle est ouverte ou fermée)
>- un cube dans lequel vous collez le capteur : vous obtenez un bouton-poussoir à 6 positions. Bref, comme le cube, mais en moins bien ! ;)

Dernière caractéristique : l'activité !

Appelé par Xiaomi de façon étrange ***bed_activity***.

C'est prévu pour passer la nuit avec vous, et ainsi, déterminer la qualité de votre sommeil, selon si vous avez beaucoup bougé ou pas ...Pas testé pour ma part !

### Les interrupteurs muraux filaires

Qu'ils soient à un ou deux boutons, avec raccordement du neutre ou sans, les 4 modèles sont gérés.
Fonctionnement très simple : **on** ou **off**.

Dans Gladys, ce sont donc des boutons de type *binary*, identifié par ***Channel_0*** et ***Channel_1***, selon le nombre de boutons de l'interrupteur.

### Les boutons poussoirs muraux sans fils

Il existe 2 types : avec 1 bouton ou avec 2 boutons.

Ils fonctionnent différemment des précédents : ils ne connaissent que le *simple click* et le *double click*. une particularité pour le modèle à 2 butons : il reconnait l'appui simultané sur les 2.

Ils sont identifiés dans Gladys par ***Channel_0***, ***Channel_1*** et ***dual_channel***, et peuvent prendre les valeurs suivantes :

    0 = repos
    1 = simple click
    2 = double click
    3 = les 2 boutons simultanément

>Ils reviennent automatiquement à l'état de repos après 1 seconde.

## Annexe 1 : mode "débogage"

2 paramètres supplémentaires peuvent être configurés dans Gladys, afin d'obtenir des logs adaptés à la situation.

### Xiaomi_debug

Selon sa valeur, nous obtenons dans les logs les éléments suivants :

| valeur | Résultat                                                                          |
|:------:|-----------------------------------------------------------------------------------|
| 0      | Pas de log (comme "vide" ou paramètre absent).                                    |
| 1      | tous les messages envoyés par la gateway.                                         |
| 2      | 1 +  la liste détaillée des périphériques, version "Xiaomi" ( = model)            |
| 3      | 2 + la liste des périphériques avec le statut de leur réponse à "qui êtes-vous ?" |
| 4      | les périphériques qui seront créés/mis à jour dans Gladys.                        |
| 5      | Les états des périphériques du 4 qui seront créés dans Gladys.                    |
| 6      | Le message renvoyé aux périphériques qui tardent à répondre.                      |
| 7      | La commande envoyée par la passerelle vers un actionneur.                         |
| 10     | L'intégralité des messages Log. L'Aspirine n'est pas fourni !                     |

### Xiaomi_debug_device

Ce paramètre n'est pas créé lors de l'installation du module.  
Il permet de n'obtenir dans les logs uniquement les messages provenant des périphériques dont le modèle est la valeur de ce paramètre.
Le modèle est au sens 'Xiaomi', par exemple, pour le détecteur d'ouverture de porte 'Aqara', son modèle est `sensor_magnet.aq2`.  
Vous trouverez en [Annexe 3](#annexe-3--liste-des-équipements-compatibles) les différents modèles gérés par le module.

**Attention il sera nécessaire de redémarrer Gladys pour que le mode débogage souhaité soit pris en compte !**

## Annexe 2 : Demander l'ajout d'un périphérique

Excellente idée ! :)

Xiaomi fait évoluer régulièrement ces matériels, tant au niveau logiciel que physique.

Vous avez un périphérique, connecté à une passerelle mais qui n'est pas dans Gladys ?
S'il fonctionne en Zigbee, il y a de forte chance de pouvoir l'ajouter.

Au démarrage de Gladys, en cas de détection d'un tel équipement, il sera visible dans le tableau adéquate de la page de configuration du module.  
Vous aurez également un message dans les logs :

    Xiaomi module : impossible to create device  + 'détail du périphérique'

### Plongée dans les logs de Gladys

Ne vous inquiettez pas, c'est très simple : dans le tableau précédent, cocher le périphérique qui vous intéresse, puis cliquez sur le bouton "Affichez les logs".  
Une fenêtre va s'ourvir, il ne vous reste plus qu'à attendre l'affichage des messages du périphérique coché, ou bien de les générer en le manipulant.

Nous obtenons alors des messages de type `report`, `Heartbeat` ou `read_ack`, dont voici des exemples  :

    Xiaomi _onMessage.msg : { cmd: 'report',
    model: 'weather.v1',
    sid: '158d0002490638',
    short_id: 50313,
    data: '{"temperature":"2105"}' }


    Xiaomi _onMessage.msg : { cmd: 'report',
    model: 'weather.v1',
    sid: '158d0002490638',
    short_id: 50313,
    data: '{"humidity":"4394"}' }


    Xiaomi _onMessage.msg : { cmd: 'report',
    model: 'weather.v1',
    sid: '158d0002490638',
    short_id: 50313,
    data: '{"pressure":"101660"}' }

    Xiaomi _onMessage.msg : { cmd: 'read_ack',
    model: 'weather.v1',
    sid: '158d0002322824',
    short_id: 31008,
    data: '{"voltage":2975,"temperature":"1793","humidity":"5450","pressure":"100950"}' }

*Remarque :* Si c'est une prise, un bouton poussoir, etc..., bref, quelque chose qui n'est pas passif, manipulez le, pour déclencher un maximum de log et être sûr d'avoir toutes les fonctionnalités.  
Les messages `Heartbeat`sont plus rare : 1 toutes les 10 minutes pour les appareils branchés sur le secteur, 1 par heure pour ceux à pile.

Par exemple, un modèle de bouton poussoir sans fil peut déclencher de 1 à 4 click.

Il ne vous reste plus qu'à créer une issue sur le github du module
ou de faire la demande sur le forum, en fournissant ces infos (un copier-coller suffira).

## Annexe 3 : Liste des équipements compatibles

Cette liste est consultable [ici](https://docs.google.com/spreadsheets/d/1dXCox4rBz478192GKNM8puhFgsQNb1209wt9j3YuIuQ/edit?usp=sharing).  
Vous pouvez contribuer à renseigner ce tableau, en mettant en commentaire vos remarques/ajouts/modifications.

[![haut de page](img/fleche-haut.png)](#gladys-xiaomi--un-module-pour-gladys)
