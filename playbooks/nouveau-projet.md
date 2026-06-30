# Playbook — Nouveau projet from scratch

> Lu uniquement quand l'utilisateur dit "nouveau projet", "on lance X", "from scratch". Pas auto-chargé.

## Phase 1 — Cadrage (3-5 questions AVANT tout fichier)

1. Nature (web app / desktop / API / mobile / lib / CLI / site marketing) ?
2. Stack (Next.js / Vite+React / Node Express / Supabase / autre) ?
3. Audience (B2B SaaS / app perso / outil interne / MVP rapide vs prod sérieuse) ?
4. Code existant à analyser **ou** part de zéro ?
5. Nom + chemin (souvent `<racine-projets>/<nom>/`) ?

## Phase 2 — Structure mémoire (auto, j'informe en passant)

Dans `~/.claude/projects/<slug>/memory/`, créer dans cet ordre :

| Fichier | Contenu initial |
|---|---|
| `MEMORY.md` | Index + tableau "Quoi lire selon tâche" |
| `pieges-<projet>.md` | Seed pièges génériques de la stack (RLS Supabase, idempotence trading, hydration Next.js…) |
| `dev-rules.md` | Clone d'un projet de référence + adapté (money, multi-tenant, etc.) |
| `lessons-learned.md` | Template vide avec format Contexte/Erreur/Cause/Solution/À retenir |
| `neural-map.md` | Mermaid nœuds génériques (utilisateur, Claude, mémoire, code, MCP) |
| `mapping-<projet>.md` | Différé après 1re exploration code |

## Phase 3 — Sync Obsidian optionnelle (PowerShell sur Windows)

```powershell
New-Item -ItemType Junction `
  -Path "<vault-obsidian>/Claude/<Nom Projet> (live)" `
  -Target "~/.claude/projects/<slug>/memory"
```
+ copie statique `<Nom Projet> (CLAUDE.md).md` dans le vault.

## Phase 4 — CLAUDE.md projet à la racine

Créer `<racine-projets>/<nom>/CLAUDE.md` avec :
- Bannière "🧠 mémoire projet OBLIGATOIRE" en tête
- Identité (nom, type, GitHub, priorité)
- Stack **RÉELLE** (pas inventée)
- Carte de structure
- Commandes dev/test/build
- Points sensibles spécifiques au domaine
- État au YYYY-MM-DD

## Phase 5 — Setup test-first (avant 1er feature commit)

```bash
npm install -D vitest @vitest/ui jsdom @testing-library/react @testing-library/jest-dom
npm install -D @playwright/test @axe-core/playwright
npx playwright install chromium
```

Configs : `vitest.config.ts` (alias @/, jsdom), `playwright.config.ts` (baseURL + mobile-chrome), `test-setup.ts` avec `@testing-library/jest-dom`. Scripts package.json : `test`, `test:e2e`, `test:all`.

**3 premiers tests obligatoires** :
1. Smoke E2E (root / login / 404 ne crashent pas)
2. 1 test unitaire sur 1 fonction critique métier
3. Test a11y axe-core si front public

## Phase 6 — Ajouter au profil global

Section "Projets actifs (ordre de priorité)" du CLAUDE.md global. Proposer placement, l'utilisateur valide.

## Phase 7 — Récap final court

```
✅ Projet <nom> initialisé.
- Mémoire projet : 5 fichiers seed, sync Obsidian active
- Tests : Vitest + Playwright + smoke E2E
- CLAUDE.md projet créé + ajouté au profil global

Qu'est-ce qu'on construit en premier ?
```

## Quand SAUTER ce playbook

- "POC en 30 min" / "script jetable" → setup minimal (juste CLAUDE.md projet de base)
- Lib externe / outil tiers qu'on ne maintient pas

## Cas particulier — clone d'un repo existant à analyser

1. Phase 1 cadrage rapide (continue / refait / archive ?)
2. Phase 2 + 3 (mémoire vide + Obsidian)
3. **Skip Phase 5** (tests peut-être déjà présents — auditer d'abord)
4. Audit léger : structure / stack / points sensibles / dette tech → résumé 5 puces
5. Demander à l'utilisateur : continue / refait / archive ?
