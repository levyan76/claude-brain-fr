# Installation en 1 prompt

> La voie la plus rapide. Copie-colle le bloc ci-dessous dans une session Claude Code fraîche et laisse-toi guider.

---

## 📋 Prompt à copier-coller dans Claude Code

```
Installe pour moi le template "claude-brain-fr" depuis ce repo GitHub :
https://github.com/levyan76/claude-brain-fr

Procédure exacte à suivre (ne dévie pas) :

1. Détecte mon OS (Windows / macOS / Linux) et adapte les commandes shell.

2. Clone le repo dans un dossier temporaire :
   - macOS / Linux : /tmp/claude-brain-fr
   - Windows : $env:TEMP\claude-brain-fr (PowerShell) ou %TEMP%\claude-brain-fr

3. Crée le dossier ~/.claude/playbooks/ s'il n'existe pas.

4. Si un fichier ~/.claude/CLAUDE.md existe déjà :
   - SAUVEGARDE-le en ~/.claude/CLAUDE.md.backup-AAAA-MM-JJ avant tout
   - Préviens-moi avant d'écraser
   - Attends ma confirmation explicite

5. Copie depuis le clone :
   - CLAUDE.md → ~/.claude/CLAUDE.md
   - playbooks/*.md → ~/.claude/playbooks/

6. Vérifie que ~/.claude/CLAUDE.md contient bien les marqueurs <À REMPLIR>
   (sinon le bootstrap ne se déclenchera pas — c'est un signal d'erreur).

7. Confirme-moi : "✅ Template installé. Cortex en place avec marqueurs <À REMPLIR>."

8. Enchaîne IMMÉDIATEMENT avec le questionnaire bootstrap des 10 questions
   décrit dans le cortex (section "🎤 Questionnaire bootstrap").
   Pose-les une à la fois, attends ma réponse entre chaque, explique brièvement
   pourquoi tu poses chaque question.

9. À la fin du bootstrap, remplis le cortex avec mes réponses (Edit/Write) et
   confirme-moi que je suis prêt à utiliser le système.
```

---

## Pourquoi cette voie est meilleure pour un débutant

- **Pas de commande shell à comprendre** : Claude détecte ton OS et utilise les bons chemins
- **Backup automatique** : si tu avais déjà un `CLAUDE.md`, il est sauvegardé avant
- **Vérification intégrée** : Claude confirme l'installation avant de continuer
- **Bootstrap enchaîné** : le questionnaire 10 questions se lance tout de suite après — pas besoin de redémarrer ou de taper "Salut Claude !"
- **Tout dans une seule session** : du clone à la 1re question en ~30 secondes

---

## Si tu préfères installer toi-même

Va dans [`GUIDE-DEMARRAGE.md`](GUIDE-DEMARRAGE.md) — méthode manuelle avec les commandes copier-coller (macOS/Linux et Windows PowerShell séparés).

---

## Si quelque chose foire

- Claude n'a pas trouvé `~/.claude/` → vérifie que **Claude Code est bien installé** (pas juste l'app web)
- Le bootstrap ne se déclenche pas → vérifie que `~/.claude/CLAUDE.md` contient encore `<À REMPLIR>` (sinon il croit que tu es déjà configuré)
- Tu veux recommencer le bootstrap → édite `~/.claude/CLAUDE.md` et remets `<À REMPLIR>` dans la section "Mon utilisateur", puis dis *"Salut Claude !"*
