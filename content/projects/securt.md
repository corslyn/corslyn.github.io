+++
title = 'SecuRT 2026'
date = 2025-10-12T19:01:38+02:00
draft = false
+++

## Remise en contexte

Dans le cadre de la SAÉ 601 de notre troisième année en BUT Réseaux & Télécoms - parcours Cybersécurité - nous avons pour mission d'organiser la troisième édition du CTF SecuRT à destination des étudiants de première et deuxième année intéressés par la cybersécurité.

## Qu'est-ce qu'un CTF ?

Un CTF (Capture The Flag, où dans la *lingua franca* la Capture de Drapeau) est une compétition de cybersécurité. Des joueurs s'affrontent sur des *challenges* dont l'objectif est de récupérer des *flags* (preuve de la résolution du challenge), rapportant plus ou moins de points, et l'objectif est évidemment d'obtenir le plus de points possible.

## Que suis-je en train de faire ?  

Je travaille sur la mise en place de toute l’infrastructure nécessaire au bon déroulement de ce CTF.

Et comme l'a dit un grand philosophe (je crois) : *"Pas d'infra, pas de CTF. Pas de CTF... pas de CTF."*  

Et quand on parle d’infrastructure, on parle de **TOUTE** l’infrastructure :

- Le serveur principal hébergeant la plateforme de présentation et de validation des challenges
- Les serveurs annexes (supervision, hébergement des challenges...)
- L’accès à distance (administration + accès des participants)
- La sécurisation de l’infrastructure (HTTPS, pare-feu, VPN)
- Les besoins spécifiques pour certains challenges

Ce CTF aura lieu le 3 février à partir de 18h jusqu'à environ 22h, et la participation sera **obligatoire** pour les premières années souhaitant s'inscrire au parcours Cyber en deuxième année afin de prouver leur intérêt dans le parcours.

## L'infrastructure en elle-même

Je travaille sur cette infrastructure depuis le début de l'année universitaire, et cette infrastructure n'a cessé d'évoluer en fonctionnalités, et donc en complexité. C'est pourquoi j'ai réalisé dès les premiers jours un schéma d'architecture réseau, dont voici la dernière version en date :

![Schéma infrastructure](../../photos/InfraSECURT.png)
*Figure 1* : Schéma d'architecture de l'infrastructure SecuRT

En tant qu'étudiant dans le parcours Cyber, il était évident que je devais sécuriser cette infrastructure de façon cohérente. C'est pour cette raison que le serveur le plus "sensible" étant le serveur hébergeant *CTFd* (la plateforme permettant la présentation des challenges disponibles, et la validation des flags) doit être le plus sécurisé possible.

Par exemple, pour décourager la détection de vulnérabilités sur la plateforme, j'ai mis en place un *reverse-proxy*, permettant de :  
- sécuriser l'accès au site en HTTPS  

![HTTPS](../../photos/https.png)
*Figure 2* : HTTPS activé sur les sites

- limiter le nombre de connexions réalisées sur le site pour éviter la surcharge sur le serveur.

![Rate limit](../../photos/rate_limit.png)
*Figure 3* : Rejet des requêtes trop rapides

Ensuite, j'ai mis en place *Anubis* (https://anubis.techaro.lol/), un programme s'interposant entre le reverse-proxy (où les personnes se connectent) et le site demandé (CTFd). Lorsqu'un utilisateur se connecte, Anubis impose aux clients un défi de *Proof of Work* [^1] exécuté côté navigateur. Ce mécanisme introduit une latence de quelques secondes pour un utilisateur légitime, mais augmente fortement le coût temporel et énergétique d’attaques d’énumération, chaque tentative nécessitant la résolution préalable d’un challenge cryptographique avant que le trafic ne soit relayé vers le service cible.

![Anubis](../../photos/anubis.png)
*Figure 4* : Anubis s'interposant à la connexion

Avec une infrastructure aussi complexe, si un élément serait amené à ne plus fonctionner, par exemple un challenge ou un serveur, nous devons être capables de le détecter dans les plus brefs délais, afin de limiter au maximum la durée d'indisponibilité.

Pour cela, j'ai mis en place un serveur Grafana, que j'ai découvert à mon travail. Nous nous en servons afin de suivre la cadence de production de notre synchrone P54 (Peugeot 408). Dans mon infrastructure, je l'ai utilisé pour suivre la disponibilités des challeges :

![Grafana](../../photos/grafana.png)
*Figure 5* : Tableau de bord des challenges (le chat porte bonheur)

Quant aux challenges en eux-mêmes, chaque troisième année est chargé de réaliser *au moins* un challenge. Si le challenge est un simple fichier, il est rendu disponible sur l'interface CTFd :

![Upload CTFd](../../photos/file_ctfd.png)
*Figure 6* : Vue d'un challenge (flouté pour éviter le *divulgâchage*), le bouton bleu permet de télécharger le fichier associé

Si le challenge nécessite une connexion à un service (serveur web, SSH ou autre), j'ai donné pour consigne d'utiliser *Docker*, permettant de créer des conteneurs bien plus légers que des machines virtuelles, et plus simples à mettre en place. Le mode de connexion est ensuite donné dans l'énoncé :

![Connexion](../../photos/connect.png)
*Figure 7* : Commande de connexion (ici SSH) utilisée pour accéder au challenge

Pour les personnes voulant participer au CTF, mais ne pouvant pas être présents sur place, j'ai mis en place un accès à distance par VPN. Ce VPN utilise *Wireguard* (https://www.wireguard.com/), et permet des vitesses de connexions semblables à celles sans VPN.

Cette infrastructure est actuellement en cours de finalisation, il ne me reste plus qu'à réaliser la configuration de la partie physique, ainsi que des tests complets de l'infrastructure. Je vais également solliciter des camarades de troisième année ne connaissant pas l'infrastructure pour réaliser un audit de sécurité, afin de m'assurer de la bonne sécurisation des services.

La réalisation de cette infrastructure m'a permis une drastique montée en compétences, notamment en administration système et réseaux, déjà mises à l'épreuve lors de notre dernière SAÉ en date : la *SAÉ 5.01 - Concevoir, réaliser et présenter une solution technique*. De plus, j'ai permis l'application de l'apprentissage critique *Mettre en œuvre des outils avancés de sécurisation d’une infrastructure du réseau* en utilisant des solutions de reverse-proxy ou encore Anubis. L'apprentissage critique *Surveiller l’activité du système d’information* est également appliqué, grâce aux tableaux de bord Grafana.

[^1]: La *Proof of Work* (preuve de travail) est un test de calcul. Le serveur n'a qu'un calcul cryptographique à réaliser, tandis que le client doit réaliser des milliers voire des millions de calculs pour trouver la réponse au test.
