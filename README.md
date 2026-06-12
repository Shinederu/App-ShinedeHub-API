# ShinedeHub API

Backend PHP du projet **ShinedeHub** pour les annonces et contenus dynamiques du
site principal `https://shinederu.ch/`.

## Role

Ce repo porte uniquement l'API `main-site`, proprietaire de la table
`main_announcements`. L'authentification, les comptes, les avatars et les
permissions sont portes par les modules partages:

- `Module-Auth-API`;
- `Module-ShinedeCore-PHP`.

## Repo et deploiement

- Source: `P:\DEV\GitHub\App-ShinedeHub-API`
- Remote: `https://github.com/Shinederu/App-ShinedeHub-API.git`
- Runtime production: `P:\PROD\API\main-site`
- Endpoint public: `https://api.shinederu.ch/main-site/`
- Entry point: `index.php`

La production ne doit contenir que le runtime necessaire: `index.php`,
`config/`, `controllers/`, `middlewares/`, `services/`, `utils/`.
Ne pas deployer `.git`, `.env.example`, `README.md`, `sql/`, tests, caches,
brouillons ou secrets.

## Endpoints

Public:

- `GET ?action=listPublicAnnouncements`

Admin, session requise et permission `main.announcements.manage` ou
`core.super_admin`:

- `GET ?action=listAnnouncements`
- `POST action=createAnnouncement`
- `POST action=updateAnnouncement`
- `PUT action=updateAnnouncement`
- `POST action=deleteAnnouncement`
- `DELETE action=deleteAnnouncement`

Les APIs PHP historiques utilisent `action` en `camelCase`. Les reponses suivent
le contrat JSON `success`, `data`, `error`.

## Authentification et permissions

- Session: cookie `sid` gere par `Module-Auth-API`.
- Validation session: `middlewares/AuthMiddleware.php`.
- Permission stable: `main.announcements.manage`.
- Service de droits: `Module-ShinedeCore-PHP/services/ProjectAccessService.php`,
  deploye en prod sous `P:\PROD\API\core\services\ProjectAccessService.php`.

Le middleware admin ne doit pas dependre d'un libelle de role.

## Base de donnees

Schema partage: `ShinedeCore`.

Table proprietaire:

- `main_announcements`

Migrations versionnees:

- `sql/001_main_site_announcements.sql`
- `sql/002_rename_main_announcements.sql`

Les migrations restent dans le repo source et ne sont pas necessaires au runtime
prod quotidien.

## Dossiers runtime et fichiers partages

Ce module ne possede aucun stockage persistant propre. Il lit les variables
`DB_*` depuis l'environnement charge par `Module-Auth-API`:

- source locale: `P:\DEV\GitHub\Module-Auth-API\.env`;
- production: `P:\PROD\API\auth\.env`.

Ne pas creer de fichier secret dans ce repo.

## Temps reel et evenements

Aucun evenement Mercure n'est publie par `main-site` actuellement. Les clients
relisent les annonces via HTTP.

## Dependances inter-projets

- `Module-Auth-API` pour session, utilisateur et `.env` DB;
- `Module-ShinedeCore-PHP` pour `ProjectAccessService`;
- `App-ShinedeHub` pour l'interface frontend `/announcements` et l'affichage
  public des annonces.

## Configuration

Variables attendues par nom, sans valeur dans Git:

- `DB_TYPE`
- `DB_HOST`
- `DB_PORT`
- `DB_NAME`
- `DB_USER`
- `DB_PASS`

## Verifications

```powershell
Get-ChildItem P:\DEV\GitHub\App-ShinedeHub-API -Recurse -Filter *.php |
  % { php -l $_.FullName }
```

Smoke public:

```powershell
Invoke-WebRequest -Uri 'https://api.shinederu.ch/main-site/?action=listPublicAnnouncements' -UseBasicParsing -TimeoutSec 20
```

## Deploiement

Copier uniquement les fichiers runtime:

```powershell
$src = 'P:\DEV\GitHub\App-ShinedeHub-API'
$dst = 'P:\PROD\API\main-site'
Copy-Item -LiteralPath "$src\index.php" -Destination "$dst\index.php" -Force
foreach ($dir in 'config','controllers','middlewares','services','utils') {
  Copy-Item -LiteralPath "$src\$dir" -Destination $dst -Recurse -Force
}
```

Preserver les eventuels fichiers runtime existants. Ne pas copier `sql/`,
`.env.example` ni la documentation vers `PROD`.

## Notes de reprise

Le handoff global du projet est dans:

- `P:\DEV\GitHub\App-ShinedeHub\REPRISE.md`
