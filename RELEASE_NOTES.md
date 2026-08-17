# Solyval EV1200 Service — v1.0.0

Première version client de **Solyval EV1200 Service**, développée pour le site **SOLYVAL de Cambaie, La Réunion**, autour de l'équipement **EV BAT 1200 N°3**.

## Fonctions principales

- Connexion Modbus TCP à l'automate.
- Lecture automatique des états utiles de l'EV BAT 1200 N°3.
- Commande manuelle de marche / arrêt dédiée à l'application.
- Réarmement impulsionnel sur `%MW195.7`.
- Demandes AUTO / MANUEL de la granulation par impulsion.
- Affichage des étapes actives du Grafcet G7.
- Diagnostic Ladder dynamique de la commande L22.
- Diagnostic Ladder du défaut retour marche L32 avec temporisation TON_77.
- Lecture / écriture générique de registres Modbus.
- Lecture / écriture de bits de mots avec les formats `195.7`, `MW195.7` et `%MW195.7`.
- Journal des opérations et erreurs.
- Aide intégrée avec sommaire, recherche et index.
- Informations système et diagnostic.

## Installation

Télécharger les fichiers de la release puis placer dans le même dossier :

- `SolyvalEV1200.exe`
- `ModbusMG.dll`

Lancer ensuite `SolyvalEV1200.exe`.

L'application utilise **.NET Framework 3.5**. Si Windows le demande, activer cette fonctionnalité avant de relancer le logiciel.

## Important

Cette application est une **solution palliative de maintenance**. Elle ne remplace pas la supervision principale, les dispositifs de sécurité de l'installation ni les procédures d'intervention du site.

Après toute commande, en particulier une commande d'arrêt, vérifier l'état réel de l'équipement à l'aide du retour automate et, si nécessaire, par un contrôle terrain.

## Fichiers de la release

Les exécutables seront ajoutés manuellement à cette release.
