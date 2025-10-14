+++
title = 'Serveur Minitel'
date = 2025-10-14T21:39:48+02:00
draft = true
+++

Pendant les vacances d'été 2025, je me suis plongé dans le monde merveilleux de la téléphonie analogique. Entre les modems, les connexions 56k et les BBS, j'ai eu l'idée quelque peu saugrenue d'acheter un Minitel. Vous savez, cet écran glorifié qui pèse presque 5 kilos et qui a plus de 50 ans... A une époque, c'était la pointe de la technologie française (oui oui française), mais France Télécom a débranché les derniers services en 2012, faute d'utilisateurs.

> Alors pourquoi acheter un Minitel en 2025 si plus rien ne fonctionne depuis plus de 10 ans ?

Tout simplement parce que des passionné.es ont refusé de laisser disparaître ce qui était jadis le fleuron de la technologie française.

L'exemple le plus connu est certainement *Minipavi* (https://www.minipavi.fr), joignable soit par Internet si on ne possède pas de Minitel, soit par téléphone.

J'avais initialement décidé d'en acheter un pour accéder à ces services restaurés, ce qui est sympathique pendant 10 minutes mais on en fait malheureusement vite le tour :(.

Cependant, comme je possèdais déjà un modem 56k, je me suis lancé le défi de réaliser un serveur Minitel de presque zéro. Je dis presque zéro car les pages sont créées via *Miedit* (https://minitel.cquest.org), mais le reste est fait à la main avec mes petites mains :).

## Conception

Avant de me lancer dans ce (gros) projet, il était primordial d'établir un cahier des charges des fonctionnalités que je souhaitais avoir sur ce serveur: j'assumais le rôle du client ainsi que du fournisseur en même temps. 

J'ai réalisé un (magnifique) schéma réseau de l'infrastructure a implémenter : 

![Schéma infrastructure](../../photos/infra_schema.png)
*Figure 2* : Schéma de l'infrastructure à réaliser


