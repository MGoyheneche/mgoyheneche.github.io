---
layout: post
title: "Ajouter des faders, des encodeurs et des boutons au ETC EOS Nomad sur MacOS"
date: 2023-11-22 09:49:00
category: "Lumière"
---

# Ajouter des faders, des encodeurs et des boutons au ETC EOS sur MacOS

### Dans ce post nous allons voir comment utiliser les faders, encodeurs et boutons d’un Korg nanoKONTROL 2 sur notre ETC EOS Nomad via OSCulator. C’est parti !

## Ce dont nous aurons besoin

Pour ajouter ces faders il nous faut :

- un Korg nanoKONTROL 2 (ou tout autre interface USB-MIDI)
- OSCulator ([https://osculator.net/download/](https://osculator.net/download/))
- ETC EOS

<div class="highlight" markdown="1">
💡 Bon à savoir
- Il n’y a pas besoin que le dongle Nomad soit connecté à l’ordinateur pour que l’OSC
- OSCulator est testable gratuitement, vous verrez simplement un message apparaitre toutes les 20 mninutes.

**Vous pouvez donc essayer d’ajouter vos faders gratuitement.**

**Ceci dit**, si vous utilisiez OSCulator “gratuit” dans votre spectacle, soyez conscient que lorsque la fenêtre proposant d’acheter le logiciel apparait, le routing est en pause ; le logiciel ne transfèrera donc aucune donnée.
</div>


## les différentes étapes que nous allons suivre

- #1 Autoriser les entrées OSC sur EOS
- #2 Lancer OSCulator et le rendre heureux
- #3 Faire découvrir à OSCulator les affectations de notre interface MIDI
- #4 Ajouter des routes OSC à OSCulator
- #5 Envoyer un message OSC via un bouton
- #6 Envoyer un message OSC via un fader ou un encodeur
- #7 Télécharger mon fichier OSCulator pour NanoKontrol

## #1 Autoriser les entrées OSC sur EOS

Lancez ETC EOS Online + viz ou Offline + Viz, peu importe. Allez dans System, puis sélectionnez Show Control. Cliquez maintenant sur l’onglet OSC. Afin d’autoriser les entrées OSC, bascullez l’interrupteur en position allumée. Le port part défaut pour recevoir l’OSC via UDP sur ETC EOS est le port 8000. Nous utiliserons celui-là.

**PHOTO #1**

## #2 Lancer OSCulator et le rendre heureux

Démarrez OSCulator. Un bandeau rouge au bas de la fenêtre indique “Routing Stopped (OSC UDP Server error: Port 8000 already in use)”. C’est tout à fait normal : EOS utilise déjà ce port ! 

**PHOTO #2**

Pour faire disparaitre cette erreur, changez simplement le port. Vous pouvez par exemple taper le port 8002 et appuyer sur la touche Entrée. Vous devriex maintenant voir une à la place du bandeau rouge un point vert avec écrit “Running…”. 

**PHOTO #3**

Merveilleux !

## #3 Faire découvrir à OSCulator les affectations de notre interface MIDI

Toujours dans OSCulator, allez dans “Parameters”, pour ce faire cliquer sur la roue crantée dans le coin supérieur droit de l’application. Dans le menu “Device” selectionnez MIDI. Dans cette vue cochez votre interface (dans notre cas Korg nanoKONTROL 2) dans la section “Midi input ports”. Vous pouvez maintenant fermer la fenmêtre “Paramaters” et retourner sur la fenêtre principale.

Lorsque vous tournez un encodeur, bougez un fader ou appuyez sur un bouton, OSCulator ajoute des entrées

**PHOTO #4**

Bien joué !

## #4 Ajouter des routes OSC à OSCulator

Avant d’indiquer à OSCulator quoi dire à EOS nous avons  besoin d’apprendre à OSCulator ou est situé EOS. Dans OSCulator allez à nouveau dans la fenêtre “Parameters” en cliquant sur la roue crantée. Sélectionnez l’onglet “OSC Routes”. Dans la colonne “OSC URL or Nearby OSC Service” cliquez sur la première ligne est ajouter le texte suivant : osc.udp://127.0.0.1:8000

**PHOTO #5**

## #5 Envoyer un message OSC via un bouton

En guise d’exemple, créons un bouton qui augment l’intensité du channel 1 lorsque l’on appui sur la flèche droite du Nanokontrol.

Appuyer plusieurs fois sur la flèche droite afin de créer et de voir quelle ligne correspond à notre bouton. Le carré à gauche de la ligne correspondant au bouton change de couleur. Lorsque vous avez localisé la ligne, dans la colonne “Event type” sélectionnez “OSC Message” puis “New”. L’onglet “OSC Routes” apparait. Une nouvelle route est créée à la fin de la liste (si votre “Routes list” est fournie il se peut que vous ayez besoin de scroller pour voir la nouvelle ligne). Assurez vous dans la section “Targets” que l’adresse OSC que nous avons entré tout à l’heure est sélectionnée. Cliquez sur “same address” pour modifier la route.

Dans la nouvelle fenêtre, dans le champ “Rewrite Address” écrivez : /eos/chan/1/+%

Ensuite cliquez sur “All args” et appuyer sur la touche supprimer, nous n’avons pas besoin d’argmutent pour cette action. Dans la section “send when” selectionnez “when value increase in the list”.

Vous pouvexz maintenant fermer la fenêtre “OSC Template Editor window”.

Il est temps de jouer avec notre nouveau bouton OSC. En appuyant sur la flèche de droite l’intensité du channel 1 augmente !

Yiha !

Vous pouvez maintenant utiliser le même principe pour ajouter d’autres boutons. Merci de vous référer à la section OSC du manuel pour voir la liste des commandes disponibles.

## #6 Envoyer un message OSC via un fader ou un encodeur

💡 Avant d’utiliser un fader via OSC, il nous faut dire à EOS combien de fader nous allons utiliser et dans quelle banque ils sont.
Pour se faire, vous pouvez créer un bouton, comme nous l’avons vu dans la section précédente. Dans le chant “Rewrite Address” indiquez : /eos/fader/1/config/20, supprimer ce qu’il y a dans “argument field” et choisissez “send when the value increase”.
Cette commande demande à EOS de créer 20 faders dans la bank 1. N’hésitez pas à vous référer au manuel pour plus de détail.
Maintenant, appuyer sur ce bouton, même si rien ne semble avoir changé, EOS sait maintenant comment gérer les banques de faders en OSC.
Souvenez vous bien que avant chaque session il vous faudra appuyer sur ce boutton de manière à ce que vous puissiez utilisez vos faders en OSC.

La “fader bank” étant créée, nous allons pouvoir pouvoir créer les faders.

En guise d’exemple, nous allons mapper le premier fader (celui de gauche) du Nanokontrol au premier fader de la première page de EOS.

Bougez le premier fader afin de voir à quelle ligne il correspond, le carré correspondant au fader change de couleur. Lorsque vous avez repéré la ligne correspondant au fader, dans la colonne “Event Type” selectionnez “OSC Message” dans la colonne “value” selectionnez “new”. L’onglet “OSC Routes” apparait. Une nouvelle route est créée à la fin de la liste (si votre “Routes list” est fournie il se peut que vous ayez besoin de scroller pour voir la nouvelle ligne). Assurez vous dans la section “Targets” que l’adresse OSC que nous avons entré tout à l’heure est sélectionnée. Cliquez sur “same address” pour modifier la route.

Dans la nouvelle fenêtre, dans le champ “Rewrite Address” écrivez  : /eos/fader/1/1.

Ensuite cliquez sur All args et supprimez le. Maintenant glissez déposez arg[0] dans “send when section”.

laisser la section “send when” sur “Any value is processed”.

Vous pouvez maintenant fermer la fenetre “OSC Template Editor”.

Il est maintenant temps de mettre quelque chose dans le fader 1 de EOS. 

Votre fader 1 est maintenant fonctionnel. Amusez-vous bien avec !

## #7 Télécharger mon fichier OSCulator pour NanoKontrol
