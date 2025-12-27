# Contrat agent (documentation)

## Règles
- Documenter le **WHY** et les contrats (API, variables, déploiement), pas le blabla.
- Markdown court, structuré, avec exemples copiables.
- Toute modif d’API/de variables implique une MAJ de la doc associée.

## Docs minimales à maintenir (patterns du projet)
- `README` : stack + quickstart local.
- `DEPLOYMENT.md` : pipeline CI/CD, secrets/vars, chemins serveur, rollback.
- `INFRA.md` (si backend) : orchestration/containers, scripts de run, variables clés.
- `docs/api/*.md` : endpoints, auth, payloads, exemples de réponse.
