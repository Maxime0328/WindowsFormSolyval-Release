# 🏭 Solyval EV1200 Service

**Solyval EV1200 Service** est une application Windows de **maintenance, diagnostic et commande Modbus TCP** développée pour le site **SOLYVAL de Cambaie, La Réunion**.

L'application a été créée comme **solution palliative de maintenance** autour de l'équipement **EV BAT 1200 N°3**, afin de permettre des opérations utiles lorsque la supervision **InduScreen** existante ne peut pas être modifiée faute d'environnement / clé de développement disponible.

> **Version actuelle : v1.0.0**  
> **Site : SOLYVAL — Cambaie, La Réunion**  
> **Équipement : EV BAT 1200 N°3**  
> **Communication : Modbus TCP**  
> **Port standard : TCP 502**  
> **Framework : .NET Framework 4.8**  
> **Développeur : Maxime Gonthier (`Maxime0328`)**

---

## 📥 Télécharger l'application

### ✅ Version v1.0.0

La version publiée est disponible ici :

**[⬇️ Ouvrir la release v1.0.0](https://github.com/Maxime0328/WindowsFormSolyval-Release/releases/tag/v1.0.0)**

Une fois les fichiers ajoutés à la release, télécharger au minimum :

- `SolyvalEV1200.exe`
- `ModbusMG.dll`

Les deux fichiers doivent être placés **dans le même dossier**.

Puis lancer :

```text
SolyvalEV1200.exe
```

> ℹ️ Si Windows demande l'activation de **.NET Framework 4.8**, accepter l'installation de cette fonctionnalité Windows avant de relancer l'application.

### 🔗 Liens utiles

- **[📦 Toutes les releases](https://github.com/Maxime0328/WindowsFormSolyval-Release/releases)**
- **[🧩 Bibliothèque Modbus utilisée par l'application](https://github.com/Maxime0328/Bibliotheque-Modbus)**

---

## 🎯 À quoi sert l'application ?

Solyval EV1200 Service complète la supervision existante pour faciliter le diagnostic et certaines commandes de maintenance liées à **EV BAT 1200 N°3**.

L'application permet notamment de :

- se connecter à l'automate en **Modbus TCP** ;
- lire en temps réel les variables principales de l'EV BAT 1200 N°3 ;
- envoyer la commande manuelle de marche / arrêt prévue pour l'application ;
- envoyer un **réarmement impulsionnel** ;
- demander le passage du mode granulation en **AUTO** ou **MANUEL** ;
- visualiser les étapes actives du **Grafcet G7** ;
- afficher une vue dynamique de la logique Ladder de commande ;
- afficher la logique de défaut de retour marche ;
- lire et écrire des registres Modbus génériques ;
- lire et écrire un **bit précis d'un mot automate**, par exemple `195.7` ou `%MW195.7` ;
- consulter un journal des opérations et erreurs de communication.

---

## ⚠️ Important — utilisation en maintenance

Cette application est un **outil complémentaire de maintenance**. Elle ne remplace ni la supervision principale, ni les sécurités de l'installation, ni les procédures de consignation et d'intervention du site.

Après une commande, et en particulier après une **commande d'arrêt**, l'opérateur doit vérifier l'état réel de l'équipement à l'aide des informations disponibles dans l'automate, du retour de marche et, lorsque nécessaire, par un contrôle terrain.

Une perte réseau, une indisponibilité automate, une modification du programme automate, une modification du mapping Modbus ou une défaillance de l'équipement peut empêcher l'exécution ou la vérification d'une commande.

> **Ne jamais considérer l'affichage de l'application comme une fonction de sécurité.** Les dispositifs et procédures de sécurité de l'installation restent prioritaires.

---

## 🔌 1. Connexion à l'automate

Dans l'onglet **Supervision** :

1. saisir l'adresse IP de l'automate ;
2. vérifier le port Modbus TCP, généralement `502` ;
3. cliquer sur **Connecter** ;
4. vérifier que l'état de connexion passe au vert ;
5. laisser la **lecture automatique** activée pour disposer d'un diagnostic temps réel.

La période de lecture peut être adaptée suivant le réseau et les besoins de maintenance.

L'onglet affiche également des informations de diagnostic sur le poste Windows, l'application, la cible automate et la communication.

---

## ⚙️ 2. EV BAT 1200 N°3

L'onglet **EV BAT 1200 N°3** regroupe les informations et commandes dédiées à l'équipement.

### État de l'équipement

L'application surveille notamment :

- la commande manuelle utilisée par l'application ;
- la commande réellement calculée par le programme automate ;
- le retour de marche ;
- le défaut de retour marche ;
- les défauts / conditions de sécurité ;
- les forçages associés au réseau ;
- le temps de marche ;
- le nombre de démarrages ;
- les étapes actives du Grafcet G7.

Les valeurs affichées sont issues des lectures Modbus de l'automate.

---

## ▶️ 3. Commande de maintenance

Pour éviter une commande involontaire, les boutons sont désactivés tant que la case :

```text
Autoriser la commande Ma_ev_bat1200_3
```

n'est pas cochée.

Une fois la commande autorisée et l'automate connecté :

- **MARCHE** écrit la demande manuelle de marche ;
- **ARRÊT** remet cette demande manuelle à `0` ;
- **RÉARMEMENT** envoie une impulsion sur le bit de réarmement.

### Comportement important

La commande de marche n'est **pas forcée cycliquement** par l'application.

L'application écrit une fois la valeur demandée. Si une autre supervision ou le programme automate modifie ensuite cette variable, Solyval EV1200 Service ne la réécrit pas automatiquement.

Le réarmement fonctionne comme un **bouton poussoir** : le bit passe momentanément à `1`, puis revient automatiquement à `0`.

---

## 🔄 4. Mode granulation AUTO / MANUEL

Le bloc **Mode granulation** permet d'envoyer une demande :

- **AUTO** ;
- **MANUEL**.

Ces commandes sont envoyées sous forme d'**impulsions**, afin de reproduire le fonctionnement de boutons poussoirs dans le programme automate.

L'application ne maintient donc pas les bits de commande à `1` en permanence.

Avant toute commande de l'EV BAT 1200 N°3, vérifier que le mode de fonctionnement affiché correspond bien à la situation souhaitée.

---

## 🧠 5. Diagnostic Ladder

L'application contient deux vues Ladder dynamiques destinées au diagnostic.

### L22 — Commande EV BAT 1200 N°3

Cette vue permet de suivre les conditions participant à la commande de l'EV.

Le code couleur reprend le principe des logiciels d'automatisme :

- 🟢 **vert** : condition / signal passant ;
- 🔴 **rouge** : condition connue mais non passante ;
- ⚫ **noir** : liaison non alimentée ;
- ⚪ **gris** : état inconnu ou automate déconnecté.

Les contacts affichent directement leur état logique `0` ou `1`.

### L32 — Défaut retour marche

Cette vue présente la logique de surveillance du retour de marche :

```text
Commande EV active
        +
Absence de retour marche
        ↓
Temporisation TON_77 — 2 s
        ↓
Défaut D_ev_bat1200_3
```

Elle permet de comprendre rapidement pourquoi un défaut de retour marche peut apparaître.

---

## 🧩 6. Registres Modbus

L'onglet **Registres Modbus** peut être utilisé comme outil de lecture / écriture générique.

Il permet de :

- créer une variable ;
- lui donner un nom ;
- renseigner son adresse ;
- sélectionner la fonction Modbus ;
- lire la valeur ;
- écrire une valeur lorsque la fonction choisie l'autorise ;
- conserver une liste de variables utiles au diagnostic.

### Fonctions prises en charge

| Fonction | Utilisation |
|---|---|
| FC1 | Lecture de coils |
| FC2 | Lecture d'entrées discrètes |
| FC3 | Lecture de registres de maintien |
| FC4 | Lecture de registres d'entrée |
| FC5 | Écriture d'un coil |
| FC6 | Écriture d'un registre de maintien |
| Bit de mot | Lecture / écriture d'un bit précis dans un `%MW` |

### Bit de mot `%MWx.y`

Pour lire ou écrire un seul bit d'un mot, sélectionner :

```text
Bit de mot - %MWx.y
```

Formats acceptés :

```text
195.7
MW195.7
%MW195.7
```

Exemple :

```text
195.7
```

correspond à :

```text
%MW195.7
```

Lors d'une écriture, l'application effectue un **Read / Modify / Write** : seul le bit demandé est modifié, les autres bits du mot sont conservés.

---

## 📋 7. Journal

L'onglet **Journal** conserve les principales opérations de l'application :

- démarrage ;
- connexion / déconnexion ;
- lectures ;
- écritures ;
- commandes ;
- changements de mode ;
- réarmements ;
- erreurs de communication ;
- anomalies détectées lors des vérifications.

En cas de problème, consulter le journal avant de fermer l'application peut faciliter le diagnostic.

---

## ❓ 8. Aide intégrée

Le menu **Aide** de l'application contient une documentation consultable hors ligne avec :

- un **sommaire** ;
- une **recherche** ;
- un **index** ;
- les procédures de connexion ;
- les explications des commandes ;
- les informations sur les vues Ladder ;
- les fonctions Modbus ;
- les informations de maintenance.

Pour connaître la version du logiciel, utiliser également :

```text
Aide → À propos
```

---

## 🛠️ Dépannage rapide

### L'application ne se connecte pas

Vérifier :

1. que le PC est sur le bon réseau ;
2. l'adresse IP de l'automate ;
3. le port TCP `502` ;
4. que l'automate est accessible ;
5. qu'aucun équipement réseau ou pare-feu ne bloque la communication.

### Les valeurs ne changent plus

Vérifier :

- l'état de connexion ;
- que la lecture automatique est activée ;
- l'heure de la dernière lecture ;
- le journal de l'application.

### Une commande ne produit pas l'effet attendu

Ne pas répéter aveuglément la commande. Vérifier d'abord :

- le mode AUTO / MANUEL ;
- la commande automate ;
- le retour de marche ;
- les défauts ;
- le Grafcet ;
- la vue Ladder ;
- le journal ;
- l'état réel de l'équipement sur le terrain.

---

## 💾 Configuration

L'application mémorise ses principaux paramètres de communication et la liste des registres Modbus configurés afin de faciliter les interventions suivantes.

Les informations de configuration peuvent être modifiées depuis l'interface sans modifier le programme automate.

---

## 🧱 Technologies

| Élément | Technologie |
|---|---|
| Langage | C# |
| Interface | Windows Forms |
| Framework | .NET Framework 3.5 |
| Communication | Modbus TCP |
| Port standard | TCP 502 |
| Bibliothèque | `ModbusMG.dll` |

La communication Modbus repose sur la bibliothèque développée par **Maxime Gonthier** :

**[➡️ Bibliotheque-Modbus](https://github.com/Maxime0328/Bibliotheque-Modbus)**

---

## 🏭 Contexte de développement

Solyval EV1200 Service a été développé pour répondre à un **besoin de maintenance réel sur le site SOLYVAL de Cambaie**.

L'installation dispose d'une supervision **InduScreen** existante. L'application a été conçue comme outil complémentaire lorsqu'une modification directe de cette supervision n'était pas disponible dans le cadre de l'intervention.

Le logiciel a été utilisé dans un contexte d'intervention réalisé avec **Actemium**, tout en restant un outil logiciel développé par son auteur pour répondre à la problématique technique rencontrée.

---

## 📦 Version v1.0.0

Première version client de Solyval EV1200 Service.

Principales fonctions :

- connexion Modbus TCP ;
- supervision EV BAT 1200 N°3 ;
- commande de maintenance ;
- réarmement impulsionnel ;
- commande AUTO / MANUEL granulation ;
- diagnostic Grafcet ;
- vues Ladder L22 et L32 ;
- lecture / écriture de registres Modbus ;
- support des bits de mots `%MWx.y` ;
- journal ;
- aide intégrée ;
- diagnostic système.

Consulter également la page de release :

**[📦 Solyval EV1200 Service v1.0.0](https://github.com/Maxime0328/WindowsFormSolyval-Release/releases/tag/v1.0.0)**

---

## 👤 Auteur

**Maxime Gonthier**  
GitHub : **[`Maxime0328`](https://github.com/Maxime0328)**

---

> Ce dépôt est destiné à la **distribution des versions exécutables** de Solyval EV1200 Service. Il ne constitue pas le dépôt de développement de l'application.
