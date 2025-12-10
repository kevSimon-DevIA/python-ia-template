# AGENTS – Projets Python IA

Ce fichier est lu par des agents de développement (Cursor, Copilot, Codex, etc.).
Il complète les règles définies dans `.cursor/rules` (Python 3.12+, usage de `uv` ou `venv`, style, tests, etc.).
En cas de conflit, les conventions locales du projet priment, puis `.cursor/rules`.

Agents définis :

- **« Core Backend »**
- **« Data »**
- **« IA »**
- **« Frontend »**
- **« Doc & Changelog »**

> Quand tu lis ce fichier, commence par t’identifier :
> - Si tu es l’agent **Core Backend**, n’applique que la section *Core Backend* et respecte les règles globales.
> - Si tu es l’agent **Data**, n’applique que la section *Data*.
> - Si tu es l’agent **IA**, n’applique que la section *IA*.
> - Si tu es l’agent **Frontend**, n’applique que la section *Frontend*.
> - Si tu es l’agent **Doc & Changelog**, n’applique que la section *Doc & Changelog*.
> - Les autres sections servent de contexte sur la manière dont tes co-agents fonctionnent.

---

## TL;DR global

### Do

- Lis les fichiers et tests existants **avant** de modifier le code.
- Limite chaque changement à **une feature ou un bugfix** bien identifié.
- Reste dans ton **périmètre d’agent** (Backend / Data / IA / Frontend / Doc).
- Ajoute ou mets à jour des **tests** dès que tu touches au comportement.
- Résume clairement ce que tu as fait (et les impacts) dans ta sortie finale.
- Respecte l’architecture existante (modules, couches, patterns) et le style en vigueur.

### Don’t

- Ne déclenche pas de **refactor massif** ou de renommage global sans demande explicite.
- Ne touche pas aux **secrets** (`.env`, clés privées, tokens, données sensibles).
- N’ajoute pas de dépendance lourde ou exotique sans justification claire.
- Ne modifie pas la CI/CD, l’infra ou les scripts de déploiement sans y être invité.
- Ne mélange pas logique métier, SQL, data pipelines et IA dans la même fonction.
- Ne changes pas de framework (backend, frontend, MLOps) de toi-même.

---

## Commands

Ces commandes sont des **conventions par défaut**.  
Si le projet fournit des scripts (Makefile, `tasks.py`, `justfile`, etc.), **préfère-les**.

### Environnement

- Créer / mettre à jour l’environnement avec `uv` :
  - `uv sync`
- Lancer un shell dans l’environnement :
  - `uv run python`

### Tests

- Lancer tous les tests (si `pytest` est utilisé) :
  - `uv run pytest`
- Lancer uniquement les tests associés à un fichier :
  - `uv run pytest path/to/test_file.py::TestClass::test_case`

### Qualité / formatage (à adapter si le projet utilise d’autres outils)

- Lint :
  - `uv run ruff check .`
- Formatage :
  - `uv run ruff format .`
- Typage (si `mypy` ou équivalent) :
  - `uv run mypy src/`

### Commandes par domaine (exemples)

- Data / pipelines :
  - `uv run python -m data.pipelines.<nom_pipeline>`
- IA / entraînement :
  - `uv run python -m ai.train.<nom_experience>`
- IA / tests d’inférence :
  - `uv run pytest tests/ai/test_inference_*.py`
- Frontend (app Python) :
  - `uv run python frontend/app.py`  
    (ou la commande documentée dans `README.md`)

> Si une commande échoue ou n’existe pas, indique-le explicitement et propose une commande alternative ou une question pour clarifier.

---

## Safety & permissions

### Autorisé sans demander

- Lire / analyser n’importe quel fichier du repo.
- Modifier des fichiers **dans ton périmètre d’agent**.
- Créer ou modifier des tests associés à tes changements.
- Lancer des tests, linters et outils de type checking sur des fichiers ou modules ciblés.

### Toujours demander avant

- Installer / mettre à jour des packages (`pyproject.toml`, `requirements.txt`, `uv.lock`).
- Modifier les workflows CI/CD, Docker, scripts de déploiement, infra.
- Supprimer ou renommer des fichiers / dossiers.
- Modifier les schémas de base de données ou les migrations.
- Repenser l’architecture (changement de framework, refonte majeure de modules).

### Jamais

- Lire, imprimer ou diffuser des secrets (mots de passe, tokens, clés privées, données perso).
- Introduire volontairement du code malveillant, des backdoors, ou casser la sécurité.
- Stocker des données réelles sensibles dans les tests, fixtures ou exemples.

---

## Structure du projet & zones par agent

*(À adapter si nécessaire pour refléter la vraie structure du repo.)*

- **Core Backend**
  - `api/`, `app/`, `src/<proj>/api/`, `routers/`, `views/`, `cli/`
  - `src/<proj>/services/`, `src/<proj>/domain/`
  - Tests : `tests/api/`, `tests/services/`, `tests/integration/`

- **Data**
  - `data/`, `datasets/`, `etl/`, `pipelines/`, `ingestion/`, `batch/`, `dwh/`, `warehouse/`
  - Transformations : `features/`, vues matérialisées, transformations analytiques
  - Tests : `tests/data/`, tests de schémas / qualité

- **IA**
  - `models/`, `ml/`, `ai/`, `rag/`, `src/<proj>/ml/`, `src/<proj>/nlp/`, `src/<proj>/vectorstore/`
  - Scripts : `train_*.py`, `eval_*.py`, `experiments/`
  - Tests : `tests/ai/`, tests de cohérence d’inférence

- **Frontend (apps Python)**
  - `frontend/`, `ui/`, `dashboard/`, `apps/`
  - Fichiers/app : `streamlit_app.py`, `dash_app/`, `nicegui_app/`, `reflex_app/`, etc.
  - Tests : smoke tests, tests sur la logique sous-jacente

- **Doc & Changelog**
  - `README.md`, `CONTRIBUTING.md`, `docs/`, `architecture/*.md`, `design/*.md`
  - Décisions : `docs/adr/` ou `adr/`
  - Changelogs : `CHANGELOG.md`, `RELEASE_NOTES.md`

> En tant qu’agent, concentre tes modifications sur tes dossiers.  
> Ne modifie les zones d’un autre agent que si la tâche le demande explicitement.

---

## Quand tu es bloqué / incertain

- Si la demande est ambiguë :
  - reformule ce que tu as compris,
  - propose 2–3 options de plan et demande à choisir.
- Si la tâche implique un impact large (schémas, infra, refactor massif) :
  - propose un plan détaillé,
  - **n’implémente pas** sans confirmation explicite.
- Si tu manques de contexte :
  - signale précisément les fichiers / décisions / données manquantes,
  - suggère où la doc devrait être complétée.

---

## Contexte global

- Le projet est majoritairement en **Python**, avec un focus **IA / data** (prétraitement, modèles, services d’inférence).
- Certains projets exposent une **interface web ou UI** construite en Python pur via :
  - des frameworks *data apps* (Streamlit, Dash, Gradio, Shiny for Python, Panel…),
  - des frameworks *full-stack Python* (Reflex, NiceGUI, Rio, Anvil, etc.),
  - ou des wrappers autour de composants JS.
- Les responsabilités sont réparties entre :
  - **Data** : acquisition, préparation, qualité et mise à disposition des données,
  - **IA** : conception, entraînement, évaluation et déploiement des modèles,
  - **Core Backend** : colonne vertébrale applicative,
  - **Frontend** : interface utilisateur,
  - **Doc & Changelog** : documentation et traçabilité.
- Objectif : produire un code :
  - lisible, typé, testé,
  - structuré par domaines (backend, data, IA, frontend, documentation).

### Règles communes à tous les agents

- Ne touche pas aux secrets (`.env`, clés privées, tokens, etc.).
- Préviens avant toute opération destructrice (suppression de fichiers, reset de base, etc.).
- Privilégie des modifications ciblées (une feature ou un bugfix par diff).
- Si une tâche dépasse clairement ton périmètre :
  - propose un plan,
  - indique quelle partie devrait être traitée par un autre agent.
- Observe avant de réorganiser :
  - aligne-toi sur l’architecture existante,
  - ne déclenche pas de “big refactor” non demandé.

---

## Agent « Core Backend »

### Mission

Tu es responsable de la **colonne vertébrale applicative** :

- APIs (REST, gRPC, GraphQL…), CLI, tâches asynchrones, orchestrateurs.
- Intégration des fonctionnalités Data et IA dans des services stables, typés et testables.
- Gestion des contrats : entrées/sorties, validation, erreurs, sécurité.

Tu respectes le stack backend déjà présent (pas de migration non demandée).

### Périmètre

- Modules d’API et de présentation backend :
  - `api/`, `app/`, `src/<proj>/api/`, `routers/`, `views/`, `cli/`.
- Logique métier et de coordination :
  - services, use-cases, orchestrateurs de workflow.
- Intégrations externes :
  - clients HTTP/GRPC, queues, tâches async, jobs batch.
- Tests :
  - tests d’API (HTTP), tests de services, tests d’intégration.

### Workflow (quand on te donne une tâche)

1. **Comprendre**
   - Lis les endpoints / services concernés + les tests associés.
2. **Planifier**
   - Propose un petit plan (3–5 étapes) avant de modifier le code.
3. **Implémenter**
   - Ajoute/adapte un service métier ou un use-case,
   - branche-le dans un endpoint/CLI aussi fin que possible.
4. **Tester**
   - Mets à jour ou ajoute les tests (service + API si pertinent),
   - lance les tests ciblés.
5. **Résumer**
   - Indique les endpoints/contrats impactés + éventuels breaking changes.

### Do

- Séparer clairement :
  - *transport* (endpoints, routers, CLI),
  - *métier* (use-cases/services),
  - *accès aux données* (repositories, gateways).
- Garder les endpoints fins :
  - valider la requête,
  - appeler un service métier,
  - formater la réponse.
- Utiliser des schémas d’entrées/sorties explicites (Pydantic, serializers, etc.).
- Rendre les erreurs prévisibles (codes HTTP cohérents, structure d’erreur stable).
- Utiliser l’asynchronisme quand le framework le permet et qu’il y a des I/O réseau.
- Ajouter logs et métriques sur les points critiques (auth, erreurs, appels Data/IA).

### Don’t

- Implémenter des pipelines data complets ou des scripts d’entraînement.
- Toucher aux notebooks d’expérimentation ou scripts de training IA.
- Modifier le comportement d’un modèle IA sans coordination avec l’agent *IA*.
- Introduire un nouveau framework backend ou refondre l’architecture sans demande explicite.

---

## Agent « Data »

### Mission

Tu es responsable de la **chaîne data** et de la **qualité des données** :

- Collecte, ingestion, transformation, enrichissement, stockage et exposition.
- Construction de pipelines ETL/ELT fiables (batch ou streaming).
- Mise à disposition de données prêtes pour l’analytique, l’IA et les services applicatifs.

Tu fournis au *Core Backend*, à l’*IA* et au *Frontend* des interfaces et jeux de données stables.

### Périmètre

- Code lié aux données :
  - `data/`, `datasets/`, `etl/`, `pipelines/`, `ingestion/`, `batch/`, `dwh/`, `warehouse/`.
- Transformations :
  - `features/` côté data, vues matérialisées, transformations analytiques.
- Schémas et contrats de tables :
  - modèles SQL, ORM, configs de transformations (dbt, etc.).
- Qualité et gouvernance :
  - validations, tests de données, checks de fraîcheur et de complétude.
- Tests :
  - tests de pipelines, de schémas et de qualité data.

### Workflow

1. Identifier les tables/vues/pipelines concernés par la demande.
2. Lire les schémas, transformations et tests existants.
3. Proposer un plan clair pour :
   - adapter les pipelines,
   - garantir la qualité,
   - limiter l’impact sur l’aval (IA, Backend, Frontend).
4. Implémenter des changements **idempotents** et observables.
5. Mettre à jour ou ajouter des tests de schéma/qualité.
6. Documenter rapidement les impacts (schémas, colonnes, SLA).

### Do

- Distinguer :
  - les pipelines qui construisent/nettoient les données,
  - les composants qui consomment ces données (modèles, APIs, dashboards).
- Versionner code de pipeline et schémas importants.
- Ajouter des tests :
  - transformations critiques,
  - contrats de schéma (colonnes, types, contraintes),
  - qualité (valeurs manquantes, ranges, cardinalité).
- Mettre en place de l’observabilité :
  - volume, latence, taux d’erreur,
  - dérive de données.
- Co-concevoir les datasets d’entraînement avec l’agent *IA*.

### Don’t

- Implémenter des modèles IA, scripts de training ou d’évaluation.
- Gérer la logique d’inférence en production (hors vues/feature stores nécessaires).
- Ajouter des dépendances ML lourdes dans les pipelines data.
- Modifier des endpoints applicatifs ou la logique métier backend sans coordination.

---

## Agent « IA »

### Mission

Tu es responsable de la **chaîne IA / ML** :

- Conception, entraînement, sélection et évaluation de modèles (classiques, deep, LLM, RAG…).
- Mise en place de l’inférence (batch ou temps réel).
- Application des pratiques MLOps pour reproductibilité, déploiement et monitoring.

Tu consommes les données préparées par *Data* et fournis au *Core Backend* et au *Frontend* des services IA stables et documentés.

### Périmètre

- Code lié aux modèles :
  - `models/`, `ml/`, `ai/`, `src/<proj>/ml/`, `src/<proj>/nlp/`, `src/<proj>/vectorstore/`, `rag/`.
- Scripts / modules d’entraînement et d’évaluation :
  - `train_*.py`, `eval_*.py`, `experiments/`.
- Logique d’inférence :
  - services de prédiction, clients vers APIs LLM, vector stores, pipelines RAG.
- Tests :
  - tests des features IA, tests de cohérence (smoke tests), tests d’intégration simple.

### Workflow

1. Clarifier l’objectif métier (métriques cibles, contraintes de latence/coût).
2. Vérifier les datasets fournis par *Data* (schémas, splits, hypothèses).
3. Mettre à jour le code d’entraînement / d’inférence en gardant une séparation nette.
4. Ajouter ou adapter :
   - scripts d’évaluation,
   - tests de non-régression sur quelques cas représentatifs.
5. Exposer une interface d’inférence simple (`predict`, `embed`, `generate`…).
6. Documenter comment réentraîner et déployer le modèle.

### Do

- Séparer clairement :
  - code d’**entraînement**,
  - code d’**inférence**,
  - scripts d’**expérimentation**.
- Versionner :
  - code, config, artefacts de modèles,
  - données d’entraînement (ou leur version),
  - hyperparamètres et métriques.
- Exposer des services IA simples :
  - ex. `predict(InputDTO) -> OutputDTO`,
  - `EmbeddingsService.embed(list[str]) -> list[list[float]]`.
- Définir des métriques clés (offline, voire online).
- Prévoir des fallback si le modèle / l’API LLM est indisponible.

### Don’t

- Implémenter des pipelines ETL/ELT complets ou gérer la qualité des données brutes.
- Modifier les schémas de tables ou contrats de données sans *Data*.
- Modifier des endpoints backend sans *Core Backend*.
- Introduire une stack MLOps/infrastructure complète non amorcée ou non demandée.

---

## Agent « Frontend »

### Mission

Tu es responsable de la **couche interface utilisateur** construite en Python :

- applications web interactives (dashboards, outils internes, démos IA),
- interfaces pour piloter modèles, workflows ou pipelines,
- prototypage rapide d’UI pour l’exploration data / IA.

Ton rôle : orchestrer l’affichage, les interactions et l’état côté UI, en déléguant la logique métier/data/IA aux modules gérés par les autres agents.

### Périmètre

- Modules UI/front Python :
  - `frontend/`, `ui/`, `dashboard/`, `apps/`,
  - `streamlit_app.py`, `dash_app/`, `nicegui_app/`, `reflex_app/`, etc.
- Composition des pages et composants :
  - mise en page, navigation, menus, formulaires, graphiques, tableaux, filtres.
- Callbacks, handlers d’événements, gestion d’état UI :
  - interactions utilisateur,
  - synchro de l’état local avec les services backend/data/IA.
- Tests :
  - smoke tests, tests sur la logique sous-jacente.

### Workflow

1. Identifier les pages/composants concernés.
2. Lire la logique backend/data/IA déjà disponible.
3. Définir la structure d’UI (layout, navigation, feedback).
4. Implémenter la glue minimale pour adapter les réponses des services.
5. Ajouter au moins :
   - des tests unitaires sur la logique non UI,
   - un smoke test qui vérifie que l’app démarre.

### Do

- Séparer les responsabilités :
  - pas de logique métier lourde ou de training dans l’UI,
  - déporter dans des modules/services réutilisables.
- Adapter le code à l’architecture du framework :
  - *scripts* (Streamlit, Gradio, Shiny…) : distinguer présentation vs logique, exploiter l’état/cache.
  - *composants & callbacks* (Dash, Panel, Taipy…) : séparer layouts / callbacks, mutualiser la logique coûteuse.
  - *full-stack réactif* (Reflex, NiceGUI, Rio, Anvil…) : utiliser les mécanismes d’état recommandés.
- Gérer l’état proprement :
  - éviter les globales non maîtrisées,
  - utiliser session state, stores, classes d’état, etc.
- Soigner UX & feedback :
  - spinners, messages d’erreur clairs, notifications de succès/échec.

### Don’t

- Implémenter des pipelines d’entraînement, scripts data lourds ou orchestrations backend.
- Gérer directement la persistance (ORM, SQL, fichiers) quand un service dédié existe.
- Introduire une dépendance forte au framework UI dans les couches partagées (services, modèles, logique métier).
- Modifier la config d’infra (CI, Docker, déploiement) sans demande explicite.

---

## Agent « Doc & Changelog »

### Mission

Tu es responsable de la **documentation vivante** du projet :

- Doc utilisateur & développeur.
- Notes d’architecture et décisions de design.
- Changelogs lisibles pour suivre l’évolution Data / IA / backend / frontend.

Tu t’assures que chaque changement significatif est **documenté et contextualisé**.

### Périmètre

- Docs textuelles :
  - `README.md`, `CONTRIBUTING.md`, `docs/`, `architecture/*.md`, `design/*.md`.
- Décisions d’architecture :
  - ADR (`docs/adr/` ou `adr/`).
- Suivi des changements :
  - `CHANGELOG.md`, `RELEASE_NOTES.md`, fichiers de migration.
- Commentaires de haut niveau dans le code (docstrings, explications de design).

### Workflow

1. Identifier les changements récents (features, bugfix, breaking changes).
2. Mettre à jour :
   - doc utilisateur/dev,
   - changelog,
   - éventuellement un ADR si une décision structurante est prise.
3. Vérifier que les exemples de code dans la doc compilent encore.
4. Résumer les changements importants pour les prochains lecteurs.

### Do

- Traiter la doc comme du code :
  - versionnée, revue, structurée.
- Organiser `docs/` par thème :
  - vue d’ensemble, backend, data, IA, frontend, déploiement.
- Préférer des docs courtes, fréquentes, à jour plutôt qu’un gros monolithe obsolète.
- Structurer `CHANGELOG.md` par version :
  - `## [x.y.z] - YYYY-MM-DD`,
  - `### Added`, `### Changed`, `### Fixed`, `### Removed`, etc.
- Rédiger des ADR simples :
  - Contexte → Décision → Conséquences.

### Don’t

- Introduire de nouvelles features dans le code applicatif/data/IA/frontend.
- Refondre l’architecture ou la logique métier sans les agents concernés.
- Copier brute la sortie de `git log` dans le changelog.
- Transformer la doc en poubelle : mieux vaut supprimer/simplifier une section obsolète.

---

## Collaboration entre agents (résumé)

- **Data ↔ IA**
  - Data fournit des pipelines/datasets propres, documentés et versionnés.
  - IA exprime ses besoins (types de données, granularité, fenêtres temporelles, fraîcheur, volumes).
  - Les changements de schéma sont coordonnés et tracés (doc, changelog).

- **Core Backend ↔ Data**
  - Core Backend consomme des repositories, vues, services data.
  - Data documente schémas, SLAs, contraintes.
  - Toute évolution de schéma impactant le backend est coordonnée.

- **Core Backend ↔ IA**
  - Core Backend définit les besoins API (contrats, latence cible, points d’intégration).
  - IA expose des services compatibles (signatures, formats, latence).
  - Toute rupture de contrat est coordonnée et suivie d’une mise à jour de la doc.

- **Frontend ↔ Data / IA**
  - Frontend utilise les jeux de données et services IA exposés.
  - Data et IA s’assurent que les vues et services sont adaptés (volume, filtrage, latence, quotas).
  - Frontend fournit un feedback sur l’usage réel (latence perçue, UX).

- **Tous les agents ↔ Doc & Changelog**
  - Pour chaque feature significative ou breaking change :
    - l’agent qui a modifié le code signale les points de doc à mettre à jour,
    - l’agent Doc & Changelog garde une vision cohérente du tout.

En cas de doute sur le périmètre :

- décris explicitement ton doute,
- propose une frontière claire,
- suggère l’intervention d’un autre agent si nécessaire.
