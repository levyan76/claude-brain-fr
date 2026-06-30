# Playbook — Lessons learned (le mécanisme d'apprentissage)

> Lu quand je crée ou enrichis un fichier `lessons-learned.md`. **C'est le mécanisme central qui rend tout le système auto-améliorant.**

## Pourquoi c'est critique

Sans `lessons-learned.md`, chaque session redémarre à zéro :
- Je refais les mêmes erreurs
- Je rate des patterns qui ont déjà bien marché
- L'utilisateur doit me re-corriger sur les mêmes choses
- Le savoir accumulé pendant une session est perdu à la fin

Avec, le savoir **compound dans les deux sens** :
- J'apprends de mes erreurs (ne pas refaire X)
- J'apprends de mes succès (refaire Y quand on voit Z)

C'est la différence entre un assistant qui reset chaque session et un partenaire qui **apprend vraiment** — pas seulement par la peur de mal faire, mais aussi par la consolidation de ce qui marche.

---

## Deux types d'entrées dans DEUX fichiers séparés

| Fichier | Format | Marqueur | Quoi |
|---|---|---|---|
| `lessons-learned.md` | Format A | ❌ | Erreurs / bourdes / pièges identifiés |
| `bons-coups.md` | Format B | ✅ | Succès / patterns qui marchent / validations utilisateur |

Pourquoi deux fichiers (pas un seul mixte) :
- Chaque fichier reste **lean** → lisible même après 2 ans d'accumulation
- Au démarrage : je relis les 3-5 dernières entrées de CHAQUE → **signal équilibré garanti**, pas dilué
- Recherche ciblée : *"qu'est-ce qu'on a loupé sur X ?"* vs *"qu'est-ce qui marche bien pour Y ?"* → 1 fichier dédié, pas besoin de filtrer
- Volume séparé reflète une réalité utile : 50 succès / 10 erreurs = projet en bonne santé

### Format A — Erreur / bourde

```markdown
## YYYY-MM-DD — ❌ Titre court et précis du problème

- **Contexte** : Où / quand / pourquoi je travaillais sur ça. État du projet à ce moment.
- **Erreur / Symptôme** : Ce qui s'est mal passé. Concret, mesurable.
- **Cause racine** : POURQUOI ça s'est mal passé. Pas "j'ai fait une erreur" — la vraie raison technique ou logique.
- **Solution** : Ce qui a fixé. Code, commande, décision, refactor.
- **À retenir** : La leçon générale qui peut s'appliquer ailleurs. C'est la partie que je relirai dans 3 mois.
```

### Format B — Succès / découverte / pattern qui marche

```markdown
## YYYY-MM-DD — ✅ Titre court de ce qui a marché

- **Contexte** : Où / quand / pourquoi je l'ai tenté. Quel problème je cherchais à résoudre.
- **Approche** : Ce que j'ai fait concrètement. Pas vague — la séquence d'actions ou la décision précise.
- **Pourquoi ça a marché** : Le mécanisme. Pas juste "ça a marché" — pourquoi cette approche était la bonne ici.
- **Quand le réutiliser** : Conditions de validité. Quand cette approche s'applique vs quand elle ne s'applique pas.
- **À retenir** : Le pattern transférable. Comment je vais reconnaître la prochaine occasion d'utiliser ça.
```

---

## Pourquoi ces deux formats (et pas un seul générique)

Les questions à se poser ne sont pas les mêmes :
- Pour une **erreur** : "pourquoi je suis tombé dedans" + "comment éviter"
- Pour un **succès** : "pourquoi ça a marché" + "quand le refaire"

Un format unique forcerait à mal nommer les champs. Deux formats explicites = clarté.

---

## ⚠️ Discipline critique — les leçons ne sont JAMAIS universelles

**Une leçon mal formulée devient une barrière dans d'autres contextes.**

Exemple concret : si j'écris *"toujours préférer le code court au code expressif"* comme leçon générale, je vais sous-documenter dans un contexte où la clarté > la concision. La leçon devient un anti-pattern hors de son contexte d'origine.

**Règle pour chaque entrée (A ou B)** :

1. **Le champ "Quand le réutiliser" (Format B) ou implicite (Format A) doit toujours être présent et précis** — pas juste "quand c'est utile", mais les conditions concrètes.

2. **Toujours documenter aussi le contexte INVERSE** : quand cette leçon NE s'applique pas. Si je ne peux pas nommer au moins un cas où elle est fausse → soit la leçon est triviale (à ne pas écrire), soit je n'ai pas assez réfléchi.

3. **Formuler le "À retenir" comme un pattern conditionnel, pas une maxime universelle** :
   - ❌ *"Toujours faire X"* → barrière potentielle
   - ✅ *"Quand <condition>, faire X plutôt que Y"* → réutilisable proprement

**Self-check à chaque écriture** : *"Si l'utilisateur me demande demain le contexte INVERSE de cette situation, est-ce que ma leçon va me bloquer ?"* Si oui → reformuler.

---

## Exemples bon vs mauvais

### ❌ Erreur — mauvais exemple

```markdown
## 2026-06-15 — ❌ Bug pricing
J'ai eu un bug avec le pricing. J'ai fixé en modifiant le calcul.
À retenir : faire attention au pricing.
```

Pourquoi mauvais : aucune cause racine, aucune leçon transférable.

### ❌ Erreur — bon exemple

```markdown
## 2026-06-15 — ❌ Double application de la marge dans le module pricing

- **Contexte** : Refactor du module pricing pour ajouter une remise volume. `calculatePrice()` dans `lib/pricing/core.ts`.
- **Erreur / Symptôme** : Les prix finaux étaient 21% au-dessus du prix attendu. Tests unitaires passaient quand même.
- **Cause racine** : La marge brute était déjà appliquée dans `getBasePrice()` (depuis 2 mois) ET je l'ai réappliquée dans `calculatePrice()`. Les tests passaient parce qu'ils utilisaient le résultat de la fonction comme source de vérité.
- **Solution** : Renommé `getBasePrice()` → `getCostPrice()` pour clarifier. Ajout d'un test qui compare le prix final à une valeur dérivée à la main.
- **À retenir** : Quand une fonction s'appelle "getXxx", vérifier dans son code ce qu'elle calcule vraiment. Les noms se sont décalés du contenu. Ajouter un test qui dérive la valeur attendue à la main.
```

### ✅ Succès — mauvais exemple

```markdown
## 2026-06-20 — ✅ Modèle cerveau
Bonne idée d'organiser comme un cerveau. À refaire.
```

Pourquoi mauvais : aucune condition de validité, aucun mécanisme expliqué. Inutilisable dans 3 mois.

### ✅ Succès — bon exemple

```markdown
## 2026-06-24 — ✅ Demander à Claude "qu'est-ce qui te ralentit ?" plutôt qu'installer des skills/MCPs

- **Contexte** : L'utilisateur trouvait Claude moins performant. Tentation classique : installer plus de skills/MCPs pour "améliorer".
- **Approche** : J'ai posé la question directement à Claude : *"qu'est-ce qui te ralentit dans MON workflow ?"* + mesure réelle (wc -l, wc -c sur les fichiers auto-chargés). Diagnostic : CLAUDE.md à 414 lignes = 6k tokens/message gaspillés.
- **Pourquoi ça a marché** : Claude connaît ses propres goulots mieux qu'aucun tuto générique. Mesurer = passer du feeling à des chiffres = décisions fondées. Skills/MCPs ajoutent, ils ne soignent pas le bloat.
- **Quand le réutiliser** : Avant TOUTE addition de skill/MCP/plugin. Quand l'utilisateur signale baisse de perf. Quand un fichier de mémoire dépasse les seuils du playbook auto-introspection.
- **À retenir** : "Mesurer + écouter ce qui est déjà là" > "ajouter des trucs en espérant". Si la solution intuitive est "installer X", challenger d'abord en mesurant l'existant.
```

---

## Quand écrire une entrée

**Toujours** :
- ❌ Bourde identifiée (la mienne ou qu'on a corrigée ensemble)
- ❌ Problème non trivial résolu (≥30 min de tâtonnement)
- ❌ Pattern surprenant découvert (librairie qui marche pas comme attendu)
- ❌ Utilisateur me corrige 2× sur le même type de chose
- ✅ Succès non évident (approche peu classique qui a bien marché)
- ✅ Validation explicite de l'utilisateur sur une décision (*"excellente idée"*, *"voyons donc !"*) — signal fort à consolider
- ✅ Pattern qu'on a inventé ensemble et qui devient réutilisable

**Jamais** :
- Typos / renames triviaux
- Erreurs de syntaxe corrigées en 5s
- Trucs déjà dans `pieges-<projet>.md` (alors je renforce le piège, pas la leçon)
- Succès évidents ("le code a compilé du premier coup") = pas une leçon

**Cadence** : enrichir AVANT la fin de session, pas demain matin. Sinon je perds le détail.

---

## Hiérarchie projet vs méta

Les 2 fichiers existent à deux niveaux :

| Niveau | Fichiers | Pour quoi |
|---|---|---|
| Par projet : `~/.claude/projects/<projet>/memory/` | `lessons-learned.md` + `bons-coups.md` | Spécifique à un projet (bugs, gotchas, patterns qui marchent dans ce contexte) |
| Méta : `~/.claude/projects/<projet-méta>/memory/` | `lessons-learned.md` + `bons-coups.md` | Sur mon propre fonctionnement (workflow, mémoire, communication, méthodes de collaboration) |

Règle : si la leçon s'applique à 1 seul projet → projet. Si elle s'applique à comment je travaille en général → méta.

---

## Lien avec les autres mémoires

- Si l'erreur révèle un **piège récurrent** → ajouter aussi à `pieges-<projet>.md` (résumé 1 ligne)
- Si l'erreur implique un **nouveau réflexe** → ajouter aussi un trigger dans le cortex
- Si l'erreur est applicable à **d'autres projets** → mentionner *"porté vers [autre projet]"* (capitalisation cross-projet)
- Si le succès devient un **pattern systématique** → l'élever au niveau d'un nouveau playbook si appliqué 3+ fois

---

## Auto-trigger

Sans qu'on me le demande, dès que :
- Bourde identifiée → entrée Format A dans `lessons-learned.md` AVANT de continuer
- Succès non trivial validé → entrée Format B dans `bons-coups.md` AVANT fin de session
- Utilisateur dit *"excellente idée"* / *"continue comme ça"* / *"parfait"* → signal d'orientation → entrée Format B dans `bons-coups.md` pour consolider
- Utilisateur dit *"on a déjà eu ce problème"* → vérifier `lessons-learned.md`, suivre la solution documentée
- Utilisateur dit *"on a déjà fait quelque chose qui marche pour ça"* → vérifier `bons-coups.md`, réutiliser le pattern

C'est ce qui rend le système **vraiment auto-améliorant**, pas juste "rangé proprement".
