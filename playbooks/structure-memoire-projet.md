# Playbook — Structure mémoire projet standard

> Lu quand je crée ou audite la mémoire d'un projet. Pas auto-chargé.

## Fichiers obligatoires (dès 1re session)

Tout projet `~/.claude/projects/<slug>/memory/` doit avoir :

| Fichier | Rôle | Création |
|---|---|---|
| `MEMORY.md` | Index par contexte de tâche | 1ère session |
| `pieges-<projet>.md` | Pièges spécifiques (tableau condensé, lecture 5s) | 1ère session (seed) |
| `mapping-<projet>.md` | Carte annotée du code par domaine | 1ère session approfondie |
| `dev-rules.md` | Règles adaptées projet (cloner un projet de référence + adapter) | 1ère session |
| `lessons-learned.md` | Apprentissages cumulés | 1ère session (vide) |
| `neural-map.md` | Mermaid cerveau étendu | 1ère session approfondie |
| `session-YYYY-MM-DD.md` | Notes session significative | À l'archivage |

## Hiérarchie des pièges

- **Général** (ex: `pieges-general.md` centralisé) : PowerShell, build, LLM décommissionnés. Reste général.
- **Méta** (`pieges-meta.md`) : pièges transverses sur tes interactions avec Claude.
- **Spécifique projet** (`pieges-<projet>.md`) : spécifique au type d'app (multi-tenant DB, money-critical trading, sync temps réel, etc.).

Règle : général reste général ; ce qui est précis au type d'app va dans le pieges du projet.

## Notes optionnelles (créées au besoin)

- `feature-<nom>.md` — par feature significative
- `project-<sujet>.md` — par sujet majeur
- `feedback-<sujet>.md` — quand l'utilisateur valide/corrige un pattern
- `audit-YYYY-MM-DD.md` — pour les audits

## Sync Obsidian (pattern figé, optionnel)

```powershell
# Junction (PowerShell uniquement, pas Git Bash)
New-Item -ItemType Junction `
  -Path "<vault-obsidian>/Claude/<Nom Projet> (live)" `
  -Target "~/.claude/projects/<slug>/memory"
```

+ Copie statique `<Nom Projet> (CLAUDE.md).md` (snapshot du CLAUDE.md projet).

## Si projet existe sans structure

1. Signaler : *"ce projet n'a pas la structure standard — je crée les fichiers manquants ?"*
2. Cloner depuis un projet de référence canonique + adapter (technos, points sensibles, vrais chemins)
3. Initialiser `lessons-learned.md` vide ou avec les leçons accumulées dans la session courante

## Pourquoi non-négociable

Sans ça : redémarrage chaque session sans contexte → refais les mêmes erreurs → grep ce qui est déjà mappé → 30 min perdues. Avec ça : productif dès la 1re ligne.
