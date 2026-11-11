[English](README.md) · **Français**

**Anthony Hunt** · [Fiche du projet sur ingevision.cloud](https://ingevision.cloud/agents-ia/?p=agent-gardien-du-parc) · [LinkedIn](https://www.linkedin.com/in/anthony-hunt-3948a9196) · [Profil GitHub](https://github.com/anthohunt)

> Dépôt vitrine : le code et les données de ce projet sont privés ; ce README le décrit.

# Agent - Gardien du parc

Un agent permanent qui fait tourner une salle de six postes de calcul partagés, sans rien décider seul de qui les obtient : il les attribue à la demande, les reprend quand le travail est fini et répare ce qui casse.

## Le problème

Six postes identiques (réalité virtuelle, Unity, modèles locaux) sont partagés entre des personnes et des agents de code. Sans gardien, deux agents démarrent sur la même machine, un travail oublié garde un poste des jours, une session se verrouille et plus personne ne joint la machine, et on le découvre quand une démo échoue.

## Comment ça marche

- **Demander, utiliser, rendre** : `parc demander -n 2 --besoin "à quoi ça sert"`, `parc rendre <poste>`, `parc etat`. Chaque poste a un seul titulaire et un usage déclaré.
- **Une porte, pas seulement une règle** : sans attribution, l'accès SSH à un poste est refusé. Les collègues n'atteignent la salle que par une petite passerelle à trois rôles (administrateur, opérateur, utilisateur).
- **Aucune priorité automatique** : s'il n'y a pas assez de postes, l'agent dit qui tient quoi et le propriétaire décide. Il ne retire jamais un poste à un travail en cours sans son ordre ; seule exception, un poste dont le titulaire a disparu et où rien ne tourne.
- **Rondes** : toutes les 10 minutes, titulaires disparus et travaux non déclarés ; toutes les 4 heures, contrôle de santé (disque, carte graphique, réseau, Unity) ; un contrôle approfondi chaque dimanche dans la nuit.
- **L'agent dort** jusqu'à ce qu'un signal arrive (panne, travail non déclaré, poste à rendre), le traite, puis attend de nouveau. Une panne qu'il ne sait pas réparer devient une ligne au propriétaire : le poste, la cause simple, ce qu'il faut faire.

## Ce qui est construit

En service depuis le 4 novembre 2026 sur 6 postes : attribution et porte SSH, passerelle des collègues, rondes et contrôles de santé, scripts de réparation (session verrouillée, lien réseau, garde contre les arrêts intempestifs), et une page qui montre l'état de chaque poste.

## Ce qui est public

Cette page seulement. Le code et les journaux restent privés.

## Technologies

- Agent Claude Code (session permanente)
- Python (attribution, rondes, contrôles de santé)
- Go (passerelle des collègues)
- PowerShell (scripts déployés sur les postes Windows)
- Tailscale, OpenSSH, minuteurs systemd
