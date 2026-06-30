# Guide de démarrage — 10 minutes chrono

> Pour quelqu'un qui débute avec Claude Code. Si tu es déjà à l'aise, file directement au `README.md`.

---

## C'est quoi exactement ?

Un **template de configuration** pour Claude Code qui :
- **Économise tes tokens** (~50-80% de moins par message)
- **Garde la mémoire** entre tes sessions (Claude se souvient de toi, de tes projets, de tes erreurs/succès)
- **Se personnalise tout seul** : au 1er lancement, Claude te pose 10 questions et adapte le cortex pour toi

Au lieu de répéter qui tu es à chaque session, Claude a un "cerveau" structuré : un cortex toujours actif + des mémoires lues seulement quand utiles.

---

## Prérequis (5 min)

- **Claude Code installé** : https://docs.claude.com/en/docs/claude-code/quickstart
- Un compte Anthropic (Pro ou API)
- Un terminal (Terminal sur macOS, PowerShell sur Windows, n'importe quel shell sur Linux)

Test que Claude Code marche :
```bash
claude --version
```

Si tu vois un numéro de version, t'es bon. Sinon, retour à la doc Claude Code.

---

## Installation — voie #1 (la plus rapide, recommandée)

**Une seule étape** : ouvre Claude Code et copie-colle le prompt fourni dans **[INSTALL-PROMPT.md](INSTALL-PROMPT.md)**.

Claude détecte ton OS, clone le repo, copie les fichiers aux bons endroits, vérifie, puis **lance directement le questionnaire bootstrap**. Tout dans la même session, ~30 secondes avant la 1re question.

Si tu choisis cette voie : saute la section "Installation — voie #2" et "Premier Salut Claude" ci-dessous (le bootstrap démarre tout seul). Reviens directement à **"Comment vérifier que ça marche vraiment"**.

---

## Installation — voie #2 (manuelle, si tu veux comprendre)

### 1. Copier le template dans ton dossier `~/.claude/`

**macOS / Linux** :
```bash
# Va dans le dossier où tu as téléchargé/cloné le partage
cd <chemin-vers-ce-dossier>/share-claude-brain-fr

# Copie le cortex à la racine de ~/.claude
cp CLAUDE.md ~/.claude/CLAUDE.md

# Copie les playbooks
mkdir -p ~/.claude/playbooks
cp playbooks/*.md ~/.claude/playbooks/
```

**Windows (PowerShell)** :
```powershell
cd <chemin-vers-ce-dossier>\share-claude-brain-fr

# Copie le cortex
Copy-Item CLAUDE.md "$env:USERPROFILE\.claude\CLAUDE.md" -Force

# Copie les playbooks
New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\playbooks" | Out-Null
Copy-Item playbooks\*.md "$env:USERPROFILE\.claude\playbooks\" -Force
```

### 2. Vérifie que les fichiers sont bien là

```bash
# macOS / Linux
ls ~/.claude/CLAUDE.md ~/.claude/playbooks/

# Windows PowerShell
ls "$env:USERPROFILE\.claude\CLAUDE.md"; ls "$env:USERPROFILE\.claude\playbooks\"
```

Tu dois voir `CLAUDE.md` et 7 playbooks (`auto-introspection.md`, `defenses-idiot-proof.md`, `lessons-learned-format.md`, `nouveau-projet.md`, `posture-proactive.md`, `structure-memoire-projet.md`, `test-first-setup.md`).

---

## Premier "Salut Claude !" (2 min)

Lance Claude Code dans n'importe quel dossier :
```bash
claude
```

Puis tape exactement :
```
Salut Claude !
```

**Ce qui devrait se passer** :
1. Claude détecte que ton cortex contient encore `<À REMPLIR>`
2. Il lance le **questionnaire bootstrap** : 10 questions courtes (nom, métier, OS, stack, style préféré, projets actifs, etc.)
3. Réponds une par une, naturellement (pas besoin de réponses parfaites — tu pourras éditer après)
4. À la fin, Claude **modifie `~/.claude/CLAUDE.md` tout seul** avec tes réponses
5. Il te confirme : *"Voilà ton cortex configuré"*

**Bravo, ton Claude est désormais TON Claude.**

---

## Comment vérifier que ça marche vraiment

### Test 1 — la mémoire persiste
Quitte Claude Code (`Ctrl+C` ou `exit`). Relance-le. Tape :
```
Salut Claude ! Tu te souviens de moi ?
```
Il devrait répondre en utilisant ton prénom et ton contexte (pas redemander qui tu es).

### Test 2 — le trigger map fonctionne
Tape :
```
Je veux lancer un nouveau projet.
```
Claude doit **lire `playbooks/nouveau-projet.md`** avant de te répondre (au lieu d'improviser).

### Test 3 — les économies de tokens
Avant d'avoir le template, regarde le footer de Claude Code (`/cost` ou `/status` selon ta version). Note le coût/tokens par message. Après installation, refais le même genre de question — la différence devrait être visible.

---

## Erreurs courantes

| Symptôme | Cause probable | Fix |
|---|---|---|
| Claude ne fait pas le questionnaire au 1er "Salut Claude !" | Le fichier `~/.claude/CLAUDE.md` n'a pas été chargé | Vérifier qu'il existe : `cat ~/.claude/CLAUDE.md` (doit contenir `<À REMPLIR>`) |
| Claude n'ouvre pas les playbooks sur trigger | Mauvais chemin dans le cortex | Le cortex pointe vers `~/.claude/playbooks/...` — vérifier que les fichiers sont bien là |
| Erreurs Windows avec `&&` | PowerShell 5.1 ne le supporte pas | Au bootstrap, réponds que ton OS est "Windows / PowerShell 5.1" — Claude adaptera |
| Le cortex me semble trop long après bootstrap | Normal au début | Après quelques sessions, demande : *"fais une auto-introspection"* — Claude mesurera et proposera des coupes |

---

## Et après ?

### Crée la mémoire de ton 1er projet
Une fois dans un projet, dis à Claude :
```
On lance la structure mémoire pour ce projet.
```
Il va lire `playbooks/structure-memoire-projet.md` et créer les fichiers standard (`MEMORY.md`, `dev-rules.md`, `pieges-<projet>.md`, `lessons-learned.md`, `bons-coups.md`).

### Les 2 fichiers qui changent tout : `lessons-learned.md` + `bons-coups.md`
- Chaque fois que tu **résous un bug non trivial** → Claude doit ajouter une entrée dans `lessons-learned.md`
- Chaque fois que tu valides explicitement une approche ("excellente idée", "parfait") → entrée dans `bons-coups.md`
- Au prochain "Salut Claude !", il relit les 80 dernières lignes de chaque → il ne refait pas les mêmes erreurs ET capitalise sur ce qui marche

C'est ce qui transforme Claude d'un "assistant IA" en un **partenaire qui te connaît**.

### Ajuste à ton goût
Le cortex est À TOI. Si une règle te casse les pieds, supprime-la. Si tu veux un nouveau trigger, ajoute-le. Demande à Claude :
```
J'aimerais que tu fasses X chaque fois que Y. Ajoute le trigger.
```

### Quand tu sens que Claude "rame" ou consomme trop
```
Fais une auto-introspection.
```
Il mesurera la taille de ton cortex, des MCPs actifs, des mémoires projet — et te proposera 1-3 optimisations concrètes.

---

## Besoin d'aide ?

Tout est dans le `README.md` (explication du modèle cerveau) et les playbooks (qui s'auto-déclenchent quand pertinents).

Bonne route. 🚀
