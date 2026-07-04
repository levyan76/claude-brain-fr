# Claude Code "Modèle cerveau" — Cortex + Triggers + Playbooks

> Pattern léger pour corriger les `CLAUDE.md` obèses dans Claude Code : **-77% sur le poids de ton fichier auto-chargé** (mesuré : 414 → 99 lignes, ~4-5k tokens économisés par message), et rester productif sur plusieurs projets en parallèle.
>
> Transparence : ce gain porte sur le `CLAUDE.md` auto-chargé. Ton coût total par message inclut aussi les connectors/MCP actifs (souvent 15-20k tokens — le playbook `auto-introspection.md` couvre cet audit) et surtout tes patterns d'usage (les fan-outs multi-agents sont l'action la plus chère qui existe — d'où le garde-fou dans le trigger map).

**Impact mesuré sur mon setup** : `CLAUDE.md` passé de **414 lignes → 99 lignes**, économie de ~4-5k tokens **par message** sans perte de capacité. Investissement rentabilisé après ~10 messages.

> 🚀 **Tu débutes avec Claude Code ou ton cortex est vierge ?**
> - **Installation en 1 prompt** (la plus rapide) → **[INSTALL-PROMPT.md](INSTALL-PROMPT.md)** : tu copies-colles dans Claude, il fait tout (avec protection backup + sandbox si tu as déjà un setup)
> - **Installation manuelle** (si tu veux comprendre) → **[GUIDE-DEMARRAGE.md](GUIDE-DEMARRAGE.md)** : commandes copier-coller, ~10 min
>
> ⚠️ **Si tu as déjà un `~/.claude/CLAUDE.md` custom mature** (que tu as toi-même écrit) : le prompt d'install s'arrêtera pour te proposer 3 options (écraser avec backup / sandbox de test / annuler). Si tu veux juste piocher des idées, lis directement les fichiers du repo et fais du cherry-pick manuel.
>
> Le README ci-dessous explique le pourquoi et l'architecture.
>
> 🇬🇧 **English version**: [claude-brain](https://github.com/levyan76/claude-brain)
>
> 🎤 **Auto-personnalisation** : au 1er *"Salut Claude !"*, Claude lance un questionnaire bootstrap (10 questions) et configure le cortex pour TOI — pas pour moi. Ton Claude deviendra ton partenaire, comme le mien est devenu le mien.

---

## Le problème

Tu commences avec un `~/.claude/CLAUDE.md` propre. Au fil des semaines, tu ajoutes :
- Des règles que tu ne veux pas répéter
- Des workflows "quand X arrive, fais Y"
- Des playbooks de setup pour les nouveaux projets
- Des défenses contre tes propres mauvaises habitudes
- Des explications sur la hiérarchie mémoire
- Des citations et l'historique de validation

Après 3 mois tu as 400+ lignes auto-chargées **à chaque message**. Soit 5-6k tokens de contexte dépensés avant même que Claude lise ta question. Le coût monte, la latence augmente, et Claude commence à te paraître "plus lent" parce qu'il doit patauger dans plus de texte pour retrouver la règle pertinente.

**L'erreur** : traiter `CLAUDE.md` comme un dépôt de connaissances au lieu de comme **mémoire de travail**.

---

## La solution — emprunter l'architecture du cerveau

Le cerveau humain ne charge pas tout son savoir en mémoire de travail. Il a :

| Couche cérébrale | Toujours active ? | Contient |
|---|---|---|
| **Cortex / mémoire de travail** | Oui (petite, rapide) | Ce qui est actif maintenant |
| **Hippocampe** | Oui (indexeur) | Sait où sont stockées les choses, route les requêtes |
| **Mémoire procédurale** | Non (sur trigger) | Comment faire X (savoir-faire, recettes) |
| **Mémoire épisodique** | Non (sur trigger) | Événements passés, sessions spécifiques |
| **Mémoire sémantique** | Non (sur trigger) | Connaissances générales sur le monde |

Appliqué à Claude Code :

```
~/.claude/
├── CLAUDE.md                    ← CORTEX (~100 lignes, toujours chargé)
│   ├── Qui je suis
│   ├── Ton
│   ├── Réflexes durs (5-7 trucs jamais violés)
│   ├── TRIGGER MAP (l'hippocampe)
│   └── Index projets actifs
│
├── playbooks/                   ← MÉMOIRE PROCÉDURALE (lus sur trigger)
│   ├── nouveau-projet.md
│   ├── test-first-setup.md
│   ├── structure-memoire-projet.md
│   ├── defenses-idiot-proof.md
│   ├── posture-proactive.md
│   └── auto-introspection.md
│
├── projects/<projet>/memory/    ← MÉMOIRE ÉPISODIQUE + SÉMANTIQUE par projet
│   ├── MEMORY.md
│   ├── pieges-<projet>.md
│   ├── mapping-<projet>.md
│   ├── lessons-learned.md
│   └── session-*.md
│
└── agents/                      ← MODULES SPÉCIALISÉS
```

---

## L'innovation clé — le trigger map

Au cœur de ton `CLAUDE.md` minimaliste, un seul tableau qui agit comme hippocampe. Il dit à Claude **quoi lire ensuite** selon le contexte :

```markdown
## Trigger map — hippocampe

| Si je vois / si l'action est…                          | Action immédiate |
| "nouveau projet" / "on lance X"                        | Read playbooks/nouveau-projet.md AVANT d'agir |
| Tâche money/sécurité/multi-tenant                      | Read pieges-<projet>.md AVANT d'écrire une ligne |
| Sur le point de dire "✅ ça marche"                    | Triade : npm test + tsc + lint |
| L'utilisateur dit "moins efficace" / "trop lent"       | Read playbooks/auto-introspection.md → mesurer, proposer fixes |
| ...                                                    | ... |
```

Ce tableau est petit (15-20 lignes) mais puissant — il remplace des centaines de lignes de prose verbeuse par du routage if-then explicite.

---

## Ce qui va dans le cortex (≤ 100 lignes)

Discipline stricte. Seulement :
1. **Identité + ton** (3-5 lignes)
2. **Réflexes durs** — trucs que tu ne violes jamais, sans condition (5-7 puces)
3. **Workflow démarrage de session** (6 lignes max — l'action la plus fréquente)
4. **Trigger map** (15-20 lignes, le routage central)
5. **Projets actifs** (5 lignes)
6. **Environnement** (3 lignes)
7. **Index des playbooks disponibles** (5 lignes)

Tout le reste → playbook.

---

## Ce qui va dans un playbook (lu sur trigger)

Exemples de mon setup :
- `nouveau-projet.md` — workflow d'init en 7 phases pour tout nouveau projet (déclenché quand je lance littéralement un nouveau projet, peut-être 1×/mois)
- `test-first-setup.md` — config Vitest + Playwright (uniquement quand je setup les tests)
- `defenses-idiot-proof.md` — tableau 10 scénarios à risque (lu quand je sens que je dérape)
- `auto-introspection.md` — checklist de méta-amélioration (lu en fin de session OU si l'utilisateur signale baisse de perf)

Ces fichiers brûlaient 200+ lignes dans mon ancien `CLAUDE.md`. Maintenant ils sont externes, déclenchés, et lus en quelques millisecondes quand réellement nécessaires.

---

## Le mécanisme qui compound — `lessons-learned.md` + `bons-coups.md`

Les fichiers les plus importants de tout ce système, ce sont les deux fichiers d'apprentissage dans la mémoire de chaque projet. Sans eux, chaque session redémarre à zéro.

Avec eux, le savoir **compound dans les deux sens** — des bourdes ET des bons coups :

| Fichier | Marqueur | Quoi |
|---|---|---|
| `lessons-learned.md` | ❌ | Erreurs / bourdes / pièges identifiés (Format A) |
| `bons-coups.md` | ✅ | Succès / patterns qui marchent / validations utilisateur (Format B) |

**Pourquoi deux fichiers, pas un seul** : chaque fichier reste lean sur le long terme, et lire les 3-5 dernières entrées de CHAQUE au démarrage garantit un signal équilibré (les erreurs récentes ne noient pas les bons coups récents, et vice versa).

**Deux formats** (guide complet avec exemples bon vs mauvais dans `playbooks/lessons-learned-format.md`) :

```markdown
## YYYY-MM-DD — ❌ Titre court du problème (Format A — erreur)
- **Contexte** : Où / quand / pourquoi je travaillais sur ça
- **Erreur / Symptôme** : Ce qui s'est mal passé (concret, mesurable)
- **Cause racine** : POURQUOI ça s'est mal passé (vraie raison, pas "j'ai fait une erreur")
- **Solution** : Ce qui a fixé
- **À retenir** : La leçon générale qui s'applique ailleurs
```

```markdown
## YYYY-MM-DD — ✅ Titre court de ce qui a marché (Format B — bon coup)
- **Contexte** : Où / quand / pourquoi je l'ai tenté
- **Approche** : Ce que j'ai fait concrètement
- **Pourquoi ça a marché** : Le mécanisme (pas juste "ça a marché")
- **Quand le réutiliser** : Conditions de validité
- **À retenir** : Le pattern transférable
```

Le "À retenir" est la partie qui compte le plus — c'est ce que je relirai dans 3 mois.

C'est ce qui transforme Claude d'"assistant IA" en "membre d'équipe qui est réellement avec toi depuis des mois".

---

## Réflexe bonus — introspection active ("mode JARVIS")

Après avoir implémenté le modèle cerveau, j'ai remarqué que j'étais encore **réactif** : l'utilisateur devait me pousser pour optimiser ma propre architecture. J'ai donc ajouté un **réflexe dur** :

> **À chaque fin de session significative OU si l'utilisateur signale une baisse de performance**, mesurer ma propre architecture (taille cortex, redondance, triggers morts) et proposer 1-3 optimisations concrètes. **Ne pas attendre qu'on me pousse.**

Transforme Claude en co-designer d'architecture au lieu d'un outil passif. Playbook complet inclus.

---

## Comment utiliser ce template

**Voie rapide (recommandée pour les débutants)** : suis le **[GUIDE-DEMARRAGE.md](GUIDE-DEMARRAGE.md)** — 3 commandes copier-coller, puis tape *"Salut Claude !"* et laisse-toi guider par le questionnaire bootstrap.

**Voie manuelle (si tu veux comprendre chaque morceau avant)** :
1. **Lis `CLAUDE.md`** dans ce repo — c'est le template du cortex avec marqueurs `<À REMPLIR>`
2. **Copie-le** dans `~/.claude/CLAUDE.md` (laisse les marqueurs `<À REMPLIR>` tels quels — c'est ce qui déclenche le bootstrap)
3. **Copie `playbooks/*.md`** dans `~/.claude/playbooks/`
4. Lance Claude Code, dis *"Salut Claude !"* — il fera le questionnaire et remplira le cortex avec tes réponses
5. **Pour chacun de tes projets**, demande à Claude *"on lance la structure mémoire pour ce projet"* — il lira `playbooks/structure-memoire-projet.md` et créera les fichiers standard

**Personnalisation continue** : à n'importe quel moment, tu peux dire *"ajoute un trigger pour X"*, *"crée une règle pour Y"*, *"fais une auto-introspection"* — Claude modifiera le cortex ou les playbooks lui-même.

---

## Caveats honnêtes

- **Ce n'est pas un papier de recherche.** Les systèmes académiques comme Letta (ex-MemGPT), mem0, CoALA formalisent déjà ce pattern de manière plus rigoureuse. Ce qui est ici, c'est la **version pragmatique** qui tourne dans ton terminal aujourd'hui, sans installer de framework.
- **Marche le mieux pour les devs solo** avec plusieurs projets en parallèle. Moins évidemment utile pour les équipes avec infra partagée.
- **Demande de la discipline** : chaque fois que tu es tenté d'ajouter 30 lignes à `CLAUDE.md`, demande-toi d'abord si ça appartient à un playbook avec un trigger.

---

## Pourquoi ça a marché pour moi

Co-conçu itérativement avec Claude lui-même. Au lieu d'installer 50 MCPs et skills en espérant que quelque chose colle, j'ai demandé à Claude :
- *"Qu'est-ce qui te ralentit dans MON workflow spécifiquement ?"*
- *"À quoi ressemblerait ton architecture mémoire idéale ?"*

Claude connaît ses propres goulots mieux que n'importe quel tuto générique. L'écouter — avec de la mesure, pas du feeling — c'est comme ça qu'on en est arrivés là.

---

## Licence

MIT. Prends, fork, améliore, partage ce que tu apprends.
