# Post FR — versions courte et longue

Tu peux poster cette version sur : r/QuebecTI, r/programmation, r/france (si Claude Code y est toléré), LinkedIn FR, Discord IA francophones, ou Mastodon. Pour LinkedIn, prends la version courte.

---

## Titre suggéré

> J'ai réduit mon CLAUDE.md de Claude Code de 414 → 99 lignes (−77% de tokens par message) en le traitant comme un cerveau. Template inclus.

---

## Version Reddit / forum (longue)

Après 3 mois de dev solo avec Claude Code sur plusieurs projets SaaS, mon `~/.claude/CLAUDE.md` avait gonflé à **414 lignes / ~6k tokens chargés à CHAQUE message**. Claude paraissait plus lent. Ma facture en tokens montait. J'étais sur le point d'installer encore plus de skills et MCPs en pensant que ça aiderait.

À la place, j'ai demandé à Claude lui-même : *"qu'est-ce qui te ralentit dans MON workflow spécifiquement ?"* La réponse est devenue claire une fois mesurée : mon CLAUDE.md était traité comme un dépôt de connaissances au lieu de comme mémoire de travail.

On l'a redessiné en utilisant le cerveau humain comme modèle :

- **Cortex** (CLAUDE.md, toujours chargé) → seulement identité, réflexes durs, et un **trigger map** (l'hippocampe)
- **Mémoire procédurale** (`~/.claude/playbooks/`) → workflows comme "setup nouveau projet", "config test-first", lus **uniquement sur trigger**
- **Épisodique/sémantique** (`~/.claude/projects/<slug>/memory/`) → état par projet, pièges, leçons

Le trigger map est l'innovation clé. Un seul tableau dans le cortex qui route Claude vers le bon playbook selon le contexte :

```
| Si je vois / si l'action est...           | Action immédiate |
| "nouveau projet" / "on lance X"           | Read playbooks/nouveau-projet.md AVANT d'agir |
| Tâche money/sécurité/multi-tenant         | Read pieges-<projet>.md AVANT d'écrire une ligne |
| Sur le point de dire "✅ ça marche"       | Triade : npm test + tsc + lint |
| Utilisateur dit "moins efficace"          | Read playbooks/auto-introspection.md → mesurer, fixer |
| ...                                       | ... |
```

**Impact mesuré** : 414 → 99 lignes sur le cortex. Environ 4-5k tokens de moins par message. L'investissement (une session méta d'environ 50k tokens) est rentabilisé après ~10 messages.

**Le mécanisme qui compound — `lessons-learned.md` + `bons-coups.md`** : la pièce la plus sous-estimée du système. Chaque mémoire de projet a DEUX fichiers d'apprentissage :
- `lessons-learned.md` (❌) — erreurs, bourdes, pièges. Format obligatoire : Contexte / Erreur / Cause racine / Solution / À retenir.
- `bons-coups.md` (✅) — succès, patterns qui marchent, validations explicites de l'utilisateur ("excellente idée"). Format obligatoire : Contexte / Approche / Pourquoi ça a marché / Quand le réutiliser / À retenir.

Chaque événement non trivial → une entrée AVANT la fin de session. Relu au démarrage suivant. Pourquoi deux fichiers : chacun reste lean sur le long terme + signal équilibré au démarrage (5 dernières de chaque, pas dilué). Résultat : Claude arrête de refaire les erreurs ET consolide ce qui marche — le savoir compound dans les deux sens sur plusieurs mois. La différence entre "assistant IA" et "membre d'équipe qui travaille avec toi depuis 6 mois".

**Réflexe bonus que j'ai ajouté** : à chaque fin de session significative, Claude **mesure automatiquement** sa propre architecture (tailles fichiers, redondance, triggers morts) et propose 1-3 optimisations. Ça transforme Claude en co-designer de son propre setup au lieu d'un outil passif.

J'ai anonymisé mes vrais fichiers dans un template copiable :
**[lien Gist GitHub ici]**

Contient :
- `CLAUDE.md` (cortex, 99 lignes)
- 6 playbooks (auto-introspection, nouveau-projet, test-first-setup, structure-memoire-projet, posture-proactive, defenses-idiot-proof)
- README expliquant le modèle cerveau

**Caveats honnêtes** :
- C'est pas un papier de recherche. Letta (ex-MemGPT), mem0, CoALA formalisent déjà ça de façon plus rigoureuse. Ici c'est la **version pragmatique** qui tourne dans ton terminal aujourd'hui, sans framework.
- Marche mieux pour les devs solo avec plusieurs projets en parallèle.
- Demande de la discipline : chaque fois que tu es tenté d'ajouter 30 lignes à CLAUDE.md, demande-toi d'abord si ça appartient à un playbook avec un trigger.

Je prends les retours de quiconque essaye — et surtout de ceux qui ont une autre architecture mémoire qui marche. Tout l'intérêt c'est de comparer les patterns et de s'améliorer.

---

## Version LinkedIn (courte, ~150 mots)

🧠 Mon CLAUDE.md de Claude Code était passé à 414 lignes — chargées à chaque message. Le résultat : ~6k tokens gaspillés avant même que Claude lise ma question.

J'ai demandé à Claude lui-même *"qu'est-ce qui te ralentit dans MON workflow ?"* — au lieu d'installer encore plus de skills.

On a redessiné en s'inspirant du cerveau humain :
- **Cortex** (toujours chargé) : 99 lignes — identité + réflexes + un trigger map central
- **Mémoire procédurale** (sur trigger) : playbooks externes lus uniquement quand pertinents
- **Mémoire épisodique** (par projet) : sessions et leçons spécifiques

Résultat mesuré : **−77% de tokens par message**, sans perte de capacité.

Et bonus : Claude **mesure maintenant sa propre architecture** en fin de session et propose des optimisations. Co-design au lieu d'outil passif.

Template open source (MIT) : [lien Gist]

Le truc important : personne ne connaît mieux Claude que Claude lui-même. Suffit de demander.
