# AGENTS - App-ShinedeHub-API

Projet courant: **ShinedeHub API**, runtime `main-site`.

Avant toute modification:

1. Lire `P:\AGENTS.md`, `P:\ECOSYSTEM.md`, puis ce README.
2. Verifier aussi `P:\DEV\GitHub\App-ShinedeHub\REPRISE.md` si la tache touche
   les annonces, le dashboard ou le deploiement.
3. Travailler dans `P:\DEV\GitHub\App-ShinedeHub-API` et pousser sur `main`.

Regles locales:

- Endpoint public stable: `https://api.shinederu.ch/main-site/`.
- Runtime stable: `P:\PROD\API\main-site`.
- Permission metier: `main.announcements.manage`.
- Ne pas deployer `.git`, `.env.example`, README, `sql/`, tests, caches,
  brouillons ou secrets vers `PROD`.
- Les secrets DB viennent de `Module-Auth-API/.env` en DEV ou
  `P:\PROD\API\auth\.env` en PROD.
