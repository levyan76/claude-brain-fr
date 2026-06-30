# Playbook — Setup test-first

> Trigger : nouveau projet OU projet existant sans tests. Read avant de proposer le setup.

## Pourquoi (retour d'expérience sur un vrai SaaS)

- Détecte les bugs immédiatement (double marge pricing, routing rôle-based qui casse, etc.)
- Refactor en confiance (un test casse = signal direct)
- Évite les heures de tests manuels en fin de projet
- Documentation vivante du comportement attendu

## Install

```bash
npm install -D vitest @vitest/ui jsdom @testing-library/react @testing-library/jest-dom
npm install -D @playwright/test @axe-core/playwright
npx playwright install chromium
```

## Configs

- `vitest.config.ts` — alias `@/`, environnement jsdom, outputFile dans `test-results/`
- `playwright.config.ts` — baseURL prod + projet mobile-chrome
- `test-setup.ts` — import `@testing-library/jest-dom`
- `tests/unit/` + `tests/e2e/` structurés
- `package.json` scripts : `test`, `test:e2e`, `test:all`
- `.gitignore` : `test-results/`, `playwright-report/`, `.playwright-cache/`

## 5 premiers tests obligatoires

1. ✅ Smoke E2E — root, login, 404 ne crashent pas
2. ✅ Test unitaire — fonctions de calcul critiques (pricing, geometry, etc.)
3. ✅ Cohérence i18n FR/EN si bilingue
4. ✅ A11y axe-core sur pages publiques
5. ✅ Sécurité DB — RLS + advisors Supabase si applicable
