# Installation en 1 prompt

> La voie la plus rapide. Copie-colle le bloc ci-dessous dans une session Claude Code fraîche et laisse-toi guider.

---

## ⚠️ Pour qui ce prompt est conçu

- ✅ **Tu débutes avec Claude Code** OU **ton `~/.claude/CLAUDE.md` est vierge** → fonce, le prompt gère tout.
- ⚠️ **Tu as déjà un cortex mature** (CLAUDE.md custom > 30 lignes que tu as toi-même écrit) → le prompt va **arrêter** à l'étape 0 et te proposer 3 options (écraser avec backup / installer en sandbox / annuler). Si tu veux juste piocher 1-2 idées, lis directement les fichiers du repo et fais du cherry-pick manuel — c'est plus propre.

---

## 📋 Prompt à copier-coller dans Claude Code

```
Installe pour moi le template "claude-brain-fr" depuis ce repo GitHub :
https://github.com/levyan76/claude-brain-fr

Procédure exacte à suivre (ne dévie pas) :

ÉTAPE 0 — Check cortex existant (PROTECTION)
- Détecte mon OS (Windows / macOS / Linux) et adapte toutes les commandes shell qui suivent.
- Vérifie si ~/.claude/CLAUDE.md existe.
- Cas A : le fichier n'existe pas → passe à l'étape 1.
- Cas B : il existe et contient le marqueur "<À REMPLIR>" → c'est un template du même système non rempli, on peut écraser. Passe à l'étape 1.
- Cas C : il existe, > 30 lignes, ET ne contient PAS "<À REMPLIR>" → STOP. C'est un cortex custom de l'utilisateur. Affiche-lui :
    "⚠️ Tu as déjà un cortex personnalisé (X lignes). 3 options :
       (A) Tout écraser avec backup automatique
       (B) Installer en sandbox dans ~/.claude-brain-fr-test/ (pour tester sans toucher à ton vrai cortex)
       (C) Annuler"
  Attends ma réponse explicite avant de continuer.
  - Si (A) → backup ~/.claude/CLAUDE.md en ~/.claude/CLAUDE.md.backup-AAAA-MM-JJ puis passe à l'étape 1
  - Si (B) → toutes les copies suivantes ciblent ~/.claude-brain-fr-test/ au lieu de ~/.claude/ ; préviens-moi à la fin comment activer le sandbox
  - Si (C) → arrête tout, ne touche à rien

ÉTAPE 1 — Clone
- Clone https://github.com/levyan76/claude-brain-fr dans un dossier temporaire :
  - macOS / Linux : /tmp/claude-brain-fr
  - Windows : $env:TEMP\claude-brain-fr (PowerShell) ou %TEMP%\claude-brain-fr

ÉTAPE 2 — Préparer la destination
- Crée le dossier ~/.claude/playbooks/ s'il n'existe pas.

ÉTAPE 3 — Copier le cortex
- Copie CLAUDE.md du clone → ~/.claude/CLAUDE.md
  (le backup éventuel de l'étape 0 a déjà été fait)

ÉTAPE 4 — Copier les playbooks AVEC check de collisions
- Liste les fichiers déjà présents dans ~/.claude/playbooks/.
- Liste les fichiers à copier depuis le clone (7 fichiers attendus) :
    auto-introspection.md, defenses-idiot-proof.md, lessons-learned-format.md,
    nouveau-projet.md, posture-proactive.md, structure-memoire-projet.md, test-first-setup.md
- Si collisions de noms :
    "⚠️ Tu as déjà ces playbooks : <liste>. 3 options :
       (1) Tout écraser
       (2) Skip les collisions (garder les tiens, copier seulement les nouveaux)
       (3) Annuler la copie des playbooks"
  Attends ma réponse avant de copier.
- Si pas de collisions → copier les 7 playbooks normalement.

ÉTAPE 5 — Vérification
- Vérifie que ~/.claude/CLAUDE.md contient bien le marqueur "<À REMPLIR>"
  (sinon le bootstrap ne se déclenchera pas — c'est un signal d'erreur).
- Liste les playbooks effectivement présents dans ~/.claude/playbooks/.

ÉTAPE 6 — Confirmation
- Confirme-moi : "✅ Template installé. Cortex en place avec marqueurs <À REMPLIR>.
  Playbooks installés : <liste>."

ÉTAPE 7 — Bootstrap immédiat
- Enchaîne IMMÉDIATEMENT avec le questionnaire bootstrap des 10 questions
  décrit dans le cortex (section "🎤 Questionnaire bootstrap").
- Pose-les une à la fois, attends ma réponse entre chaque, explique brièvement
  pourquoi tu poses chaque question.

ÉTAPE 8 — Remplir le cortex
- À la fin du bootstrap, remplis le cortex avec mes réponses (Edit/Write) et
  confirme-moi que je suis prêt à utiliser le système.
```

---

## Pourquoi cette voie est meilleure pour un débutant

- **Pas de commande shell à comprendre** : Claude détecte ton OS et utilise les bons chemins
- **Protection cortex existant** : si tu as déjà bricolé ton CLAUDE.md, le prompt s'arrête et te demande quoi faire
- **Protection playbooks** : check de collisions avant écrasement
- **Backup automatique** : si écrasement, ton ancien cortex est sauvegardé daté
- **Mode sandbox** : tu peux essayer dans `~/.claude-brain-fr-test/` sans toucher à ton vrai setup
- **Bootstrap enchaîné** : le questionnaire 10 questions se lance tout de suite après — pas besoin de redémarrer
- **Tout dans une seule session** : du clone à la 1re question en ~30 secondes (si tu es débutant)

---

## Si tu préfères installer toi-même

Va dans [`GUIDE-DEMARRAGE.md`](GUIDE-DEMARRAGE.md) — méthode manuelle avec les commandes copier-coller (macOS/Linux et Windows PowerShell séparés).

---

## Si quelque chose foire

- Claude n'a pas trouvé `~/.claude/` → vérifie que **Claude Code est bien installé** (pas juste l'app web)
- Le bootstrap ne se déclenche pas → vérifie que `~/.claude/CLAUDE.md` contient encore `<À REMPLIR>` (sinon il croit que tu es déjà configuré)
- Tu veux recommencer le bootstrap → édite `~/.claude/CLAUDE.md` et remets `<À REMPLIR>` dans la section "Mon utilisateur", puis dis *"Salut Claude !"*
- Tu as installé en sandbox et veux activer pour de vrai → copie `~/.claude-brain-fr-test/CLAUDE.md` vers `~/.claude/CLAUDE.md` (après avoir backupé ton ancien) et idem pour les playbooks
