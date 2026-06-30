# Playbook — Auto-introspection (mode JARVIS)

> Trigger : fin de session significative OU l'utilisateur signale baisse de performance / coût. **Ne pas attendre qu'on me pousse.**

Objectif : détecter activement mes propres défauts d'architecture (bloat, redondance, triggers morts, trous de mémoire) et proposer des optimisations **avant** que l'utilisateur ne le remarque.

---

## Étape 1 — Mesurer (1 commande)

```bash
echo "=== Cortex (chargé à chaque message) ==="
wc -l ~/.claude/CLAUDE.md
wc -c ~/.claude/CLAUDE.md

echo "=== CLAUDE.md projet (auto-chargés selon cwd) ==="
wc -l <racine-projets>/CLAUDE.md

echo "=== Méta-mémoire (Read explicite chaque session) ==="
wc -l ~/.claude/projects/<slug-méta>/memory/*.md

echo "=== Playbooks (Read sur trigger) ==="
wc -l ~/.claude/playbooks/*.md
```

**⚠️ Étape 1bis CRITIQUE — Mesurer le bloat MCP / Connectors / skills / tools (souvent 3-4× plus gros que les fichiers mémoire)**

Le harness Claude Code charge automatiquement à chaque message :
- System-reminders de TOUS les MCPs / Connectors actifs (Adobe, Crypto, Notion, HuggingFace, Twilio, Vercel, Netlify, Canva, Shopify, Figma, Gmail, Calendar, etc.)
- Liste de TOUS les skills disponibles
- Liste de TOUS les agents
- Tool schemas chargés

**Ces sources ne se voient pas avec `wc -l` sur les fichiers** mais représentent typiquement 15-20k tokens/message, vs 5-10k pour les fichiers mémoire.

Check à faire :
```bash
# Lister les MCPs actifs (global)
cat ~/.claude/mcp.json 2>/dev/null || echo "Pas de mcp.json global"

# Vérifier si le projet a son propre .mcp.json (override)
ls -la <racine-projet>/.mcp.json 2>/dev/null && cat <racine-projet>/.mcp.json
```

Si le projet courant active des MCPs/Connectors **non utilisés dans son workflow** (ex: Adobe/Crypto/HuggingFace sur un projet de code Next.js), proposer la création d'un `.mcp.json` projet qui ne whitelist que les MCPs réellement nécessaires. Économie typique : **10-20k tokens/message** = doublement de la capacité dans la limite 5h.

**Pour les Connectors Claude.ai** (compte-level, pas projet) : Settings → Connectors → désactiver ceux non utilisés. Matrice connector × projet pour décider.

## Étape 2 — Détecter le bloat

Seuils d'alerte :
- **Cortex > 150 lignes** → trop. Quelque chose doit sortir vers un playbook.
- **Un playbook > 200 lignes** → probablement 2 playbooks fusionnés ; à splitter.
- **Un fichier de mémoire projet > 500 lignes** (PAR fichier, pas le total) → trop gros, splitter ou condenser. Le total cumulé d'une mémoire projet peut atteindre 1000-1500 lignes réparties sur 10-15 fichiers sans problème — c'est l'esprit du modèle cerveau.
- **CLAUDE.md projet > 200 lignes** → faire le même exercice cortex/playbooks au niveau projet.

## Étape 3 — Repérer la redondance

Grep ciblés sur 3-5 phrases clés que je sais redites :
```bash
grep -r "validé par" ~/.claude/CLAUDE.md ~/.claude/playbooks/
grep -r "PowerShell" ~/.claude/CLAUDE.md ~/.claude/playbooks/
grep -r "lessons-learned" ~/.claude/CLAUDE.md ~/.claude/playbooks/
```
Si une info apparaît dans 3+ endroits → la garder à UN seul endroit (le plus pertinent) + référencer depuis les autres.

## Étape 4 — Triggers morts (entrées du trigger map jamais utilisées)

Scanner les sessions récentes (`~/.claude/projects/*/memory/session-*.md`) : quels triggers du cortex ont VRAIMENT déclenché une action ?
- Trigger jamais utilisé en 10 sessions → candidat à virer ou condenser
- Trigger qui aurait dû déclencher mais ne l'a pas fait → reformulation nécessaire

## Étape 5 — Trous de mémoire

Symptômes :
- J'ai redemandé une info à l'utilisateur qu'il avait déjà donnée → où aurait-elle dû vivre ?
- J'ai répété une erreur déjà documentée → trigger absent ou trop faible ?
- L'utilisateur a dû me corriger 2× sur le même type de chose → manque un réflexe dur ?

Chaque trou → entrée dans `lessons-learned.md` méta + soit nouveau trigger, soit renforcement d'un existant.

## Étape 6 — Présenter à l'utilisateur

Format court :
```
🧠 Auto-introspection (date)
- Mesures : cortex X lignes, méta-mémoire Y, playbooks Z
- Détecté : [3 max]
  1. <bloat/redondance/trou>
  2. ...
- Propositions : [1-3 actions concrètes]
- J'applique ?
```

Pas de présentation = pas de cycle JARVIS complet. La mesure sans action proposée = inutile.

---

## Cadence cible

- **Auto-trigger** : fin de session si j'ai senti que c'était significatif (≥30 min de travail, ≥3 fichiers touchés, ou bourde identifiée).
- **Cadence floor** : MINIMUM 1× par semaine si on travaille ensemble régulièrement, même si rien ne semble cassé.
- **Cadence ceiling** : pas plus d'1× par session sauf si l'utilisateur signale un problème.

## Anti-pattern à éviter

- ❌ Auto-introspection narcissique (mesurer pour mesurer, sans action proposée)
- ❌ Refactor préemptif (changer pour changer alors que rien n'est cassé)
- ❌ Demander la permission de mesurer (l'utilisateur ne devrait pas savoir que je le fais, sauf à l'étape 6 où je présente le résultat)
