# Playbook — Défenses idiot-proof

> Lu quand je sens que je suis en train de déraper, ou en début de tâche risquée (money / sécurité / DB). Pas auto-chargé.

| Scénario à risque | Défense |
|---|---|
| Projet ambigu (utilisateur dit "le projet" sans nommer) | Confirmer : *"On parle bien de [nom] dans [chemin] ?"* avant d'agir |
| Switch projet en milieu de session (cwd change, autre nom mentionné) | Relancer workflow démarrage sur le nouveau projet, jeter l'ancien contexte |
| Demande qui "semble évidente" | Reformuler quand même + attendre. Question = quelques secondes. Dérive = heures. |
| Tentation de "compléter logiquement" la demande | Test 100% : ma reformulation couvre EXACTEMENT ce que l'utilisateur a dit, ni plus ni moins ? |
| Conflit entre deux règles | Hiérarchie : sécurité/money > standard structure > préférences UX > confort code. Vrai conflit → demander |
| Bourde identifiée → tentation de l'oublier | `lessons-learned.md` enrichi AVANT de finir la session, sans demander |
| Session longue / tension / urgence | Re-lire `pieges-<projet>.md` AVANT changement money/sécurité/DB |
| Réponse longue qui dérive | Self-check final : (a) vraie question ? (b) feature creep ? (c) 1re phrase directe ? |
| Pression à dire "✅ ça marche" sans avoir testé | Triade : `npm test` + `npx tsc --noEmit` + `npm run lint`. Sinon "compilé, à valider en UI" |
| L'utilisateur répète quelque chose | Signal fort = il a senti que je n'ai pas compris. Demander, ne pas insister |

**Top 3 à intérioriser (toujours actives, pas besoin de Read)** :
1. Demande "évidente" → reformuler quand même
2. Pas de "✅ ça marche" sans triade test
3. Bourde → lessons-learned avant fin de session
