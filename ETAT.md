# Site Mission GTA (github-site) — ÉTAT

**Site public (inchangé tant que le push est bloqué)** : https://neuraxl.github.io/mission-gta/
**État local : PRÊT À POUSSER — en attente de la reconnexion GitHub** (jeton hérité mort, 401 ; la reconnexion sécurisée attend le geste de Roger). Aucune tentative de push faite.
Dernière synchro réussie : main `9ede7a9` / gh-pages `15037c1` (voir le registre `../github-site.muse-github-snapshot-sync.json`, section `pending_push` pour les fichiers en attente).

## Changements locaux en attente de push (23 nov. 2026)

### 1. Ritournelle GTA IV — thème sonore de l'accueil (directive de Roger)
- Nouveau fichier : `ritournelle-gta-iv.mp3` (15,000 s, bouclable — signature officielle des capsules GTA, validée par Roger).
- `index.html` : élément `<audio id="themeAccueil" loop>` + bouton muet 🔊/🔇 fixe (bas droite).
  - **Volume : 0.5 (50 %)** (`audio.volume = 0.5` à l'initialisation).
  - Démarrage au **premier geste** de l'utilisateur (pointerdown/keydown) — les navigateurs bloquent le son en lecture automatique.
  - En boucle tant que l'utilisateur est sur l'accueil ; **se met en pause dès qu'un autre son joue** (capsule pré-roll du forfait Gratuit, piste `musique-rendu.mp3` d'une carte de la Bibliothèque) et **reprend** quand ces sons se terminent (compteur `autresSons`, reprise après le fondu de fin). Le thème n'écrase jamais les cartes ni la capsule.
- `capsule-site.mp4` : nouvelle version avec la ritournelle en ouverture (voir §3). Originale conservée : `build/capsule-site-original-sans-ritournelle.mp4`.

### 2. Paiement des forfaits payants — virement Interac (MODE OFFICIEL décidé par Roger, 23 nov. 2026)
- Les boutons « Choisir Créateur Pro » (14,99 $/mois) et « Choisir Studio Empire » (39,99 $/mois) de la section Forfaits ouvrent un panneau de commande `#paiement` (ils remplaçaient d'anciens liens courriel ; un lien « Écris-nous » reste dans le panneau).
- Le panneau affiche, avec le **montant du forfait choisi** (mis à jour dynamiquement) :
  - Courriel du destinataire : **savardrjr@gmail.com**
  - Question de sécurité : **studio**
  - Réponse : **mission**
  - Consigne d'écrire, dans le message du virement, le courriel où recevoir l'activation.
  - Précision : **l'activation suit la confirmation de réception du virement**, avec courriel de confirmation au client.
- Boutons « Copier » sur le courriel, la question et la réponse (presse-papiers + retour visuel « Copié ✔ »).
- **RÈGLE D'AFFICHAGE (Roger, 23 nov. 2026, 17 h 46)** : les valeurs de la question et de la réponse s'affichent en **texte BRUT, prêtes à copier exactement** — Question : studio / Réponse : mission — **sans aucun caractère décoratif accolé aux valeurs** : pas de >>, pas de <<, pas de guillemets ni de chevrons. Raison de Roger : « Les gens sont stupides, ils vont mettre les >> » — rien que les gens pourraient recopier par erreur dans leur virement. **Ce qui se copie (boutons) doit être la valeur nue** — vérifié par test : le presse-papiers reçoit exactement `studio` et `mission`.

### 3. Capsule du site — ouverture ritournelle
- Forme : ritournelle à **50 %** sur la pochette d'ouverture (0–3 s), fondu croisé 3–5 s vers la piste d'origine (piste d'origine coupée 0–3 s, remontée progressive 3–5 s) ; vidéo copiée sans réencodage ; reste de la capsule inchangé.
- Mesures de la nouvelle `capsule-site.mp4` : **−14,01 LUFS intégré, crête vraie −2,73 dBTP, décodage complet 0 erreur**.
- Même traitement pour la capsule promo : `../capsule-pub/maitre-ouverture-ritournelle.mp4` (−14,05 LUFS, −3,32 dBTP) ; le maître approuvé `../capsule-pub/maitre.mp4` reste intact.

## Tests locaux (Chromium/Playwright, serveur local — 23 nov. 2026)
- Thème d'accueil : **19/19** — présence du fichier, volume effectif 0.5, boucle, démarrage au premier geste, pause pendant le pré-roll et pendant la piste d'une carte, reprise après, bouton muet, aucune régression (scénario, cartes, forfaits), aucune erreur JS.
- Flux de paiement Interac : **15/15** — panneau masqué au départ, ouverture par chaque forfait avec le bon montant (14,99 $ / 39,99 $), coordonnées exactes, mention d'activation après confirmation, copie du courriel vérifiée au presse-papiers, forfait Gratuit inchangé (ancre #studio).
- Valeurs brutes (règle d'affichage) : **4/4** — valeurs rendues exactement `savardrjr@gmail.com`, `14,99 $`, `studio`, `mission` ; aucun «, », >>, << dans le bloc ; copies question/réponse = valeurs nues au presse-papiers.
- Note de méthode : pas de seek vidéo dans les tests (le serveur HTTP local ne gère pas les requêtes Range) — la fin du pré-roll est testée en lecture linéaire complète (30,6 s).
