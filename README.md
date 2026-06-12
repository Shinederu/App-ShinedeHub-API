# Main Site API

Backend dedie au site principal `shinederu.ch`.

## Etat de reprise

Le site principal est mis en pause stable depuis le 2026-06-12. La documentation
complete de reprise est dans le repo frontend: `../App-ShinedeHub/REPRISE.md`.

Ce module ne porte aujourd'hui que les annonces du site principal. Le reste du
handoff API du site est partage avec `Module-Auth-API` (authentification,
utilisateurs, avatars, blocage de comptes) et `Module-ShinedeCore-PHP`
(permissions centralisees).

## Endpoint

- URL cible (Nginx): `https://api.shinederu.ch/main-site/`
- Entry point source: `index.php`
- Runtime prod: `P:\PROD\API\main-site\index.php`

## Base de donnees

- Meme schema partage que `Module-Auth-API` et `App-MelodyQuest-API` (`ShinedeCore`)
- Utilise les variables `DB_*` chargees depuis `Module-Auth-API/.env` en source ou `P:\PROD\API\auth\.env` en prod

## Actions exposees

Public:
- `GET action=listPublicAnnouncements`

Admin (session requise + droit central `main.announcements.manage` ou super-admin global):
- `GET action=listAnnouncements`
- `POST action=createAnnouncement`
- `PUT action=updateAnnouncement`
- `DELETE action=deleteAnnouncement`

## Migration SQL

Table annonces:
- `sql/001_main_site_announcements.sql`
- `sql/002_rename_main_announcements.sql`

Etat cible:
- table `main_announcements`
