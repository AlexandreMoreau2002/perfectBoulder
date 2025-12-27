# Contrat agent (tests)

## Objectif
Des tests **stables** qui couvrent le comportement, sans dépendance réseau, avec un niveau de coverage cohérent.

## Règles générales
- AAA (Arrange / Act / Assert) ; assertions explicites.
- Mock uniquement les dépendances externes (réseau, DB, mailer, temps).
- Nettoyer l’état entre tests (`clearMocks`, reset si nécessaire).
- Éviter les tests fragiles (timers non maîtrisés, snapshots inutiles).

## Backend
- Runner stable en CI (parallélisme maîtrisé si nécessaire).
- Coverage threshold global (ex. 80%) ; ignorer seulement ce qui est justifié (migrations/seeders/public).
- Pour une couche data (ORM/DB) : mocker les appels externes et piloter les branches via Promises/retours contrôlés.

## Frontend
- Setup unique pour mocks/polyfills/warnings (ex. filtres de warnings, mock client HTTP, mock icônes).
- Tester le rendu et les interactions utilisateur, pas les détails d’implémentation.
