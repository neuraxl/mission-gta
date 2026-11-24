# Site Mission GTA (github-site) — ÉTAT

**Site public** : https://neuraxl.github.io/mission-gta/
**État : EN LIGNE ET À JOUR — push réussi le 23 nov. 2026** (reconnexion GitHub faite par Roger : nouveau jeton classique, scope repo ; l'ancien jeton hérité était mort, 401). Site public vérifié en ligne après le push (page + les 3 fichiers médias servis).
Dernière synchro réussie : main `c53ea7f` / gh-pages `ccb6608` (voir le registre `../github-site.muse-github-snapshot-sync.json`).
Vérification côté visiteur faite le 23 nov. (navigateur, 5 points : accueil, ritournelle et bouton muet, pré-roll Gratuit, Bibliothèque, panneau Interac des deux forfaits — tout confirmé ; seule limite : l'environnement de test ne pouvait pas entendre le son). **Confirmation finale de Roger le 23 nov. : « C'est parfait pour moi, téléphone et portable »** — la ritournelle entendue à son oreille, sur son téléphone et son portable.
**Bibliothèque — DÉCISION DE ROGER (23 nov., après diagnostic)** : la Bibliothèque ne conserve rien (aucun stockage local ; au rechargement, tout s'efface) — un rendu n'y apparaît que pendant les 15 minutes qui suivent sa création, puis disparaît. Roger a choisi explicitement de **LAISSER COMME C'EST** : cette disparition après 15 minutes est le design voulu, pas un bogue. Une Bibliothèque vide = aucune mission en cours, c'est normal. Ne pas « corriger » sans un nouvel ordre de Roger.
**MISE À JOUR (24 nov. 2026, directive de Roger)** : la durée d'affichage passe de 15 à **10 minutes** (« Pas besoin de conserver plus de 10 minutes après le rendu ») — le design de disparition, lui, reste exactement comme décidé le 23 nov.

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

## Corrections du 24 nov. 2026 — le site devient 100 % opérationnel (directives de Roger)

**Problème signalé par Roger** : un client qui décrivait sa mission (ex. entrer dans un dépanneur, acheter un paquet de cigarettes, ressortir) ne recevait que le gabarit — les 4 panneaux de la pochette avec des textes par-dessus — jamais la vidéo de CE QU'IL AVAIT DÉCRIT. Et le site ne demandait même pas son courriel : aucune livraison n'était possible.

**Correctifs appliqués à `index.html`** (21 remplacements, sauvegarde préalable : `build/index-avant-correction-2026-11-24.html`) :
1. **Le rendu est étiqueté APERÇU partout** : chip APERÇU sur le lecteur, badge de l'accueil, étape 03 réécrite, bibliothèque renommée « les aperçus en cours » + phrase explicite « L'aperçu est une simulation instantanée », bouton « Lancer l'aperçu », compte à rebours « Aperçu disponible encore », carte Gratuit, règle de la maison nº 2, ligne d'archive, note du pré-roll.
2. **Durée d'affichage de l'aperçu : 10 minutes** (au lieu de 15) — compte à rebours `remain=600`, textes harmonisés.
3. **Deux nouveaux champs au formulaire** : nº 9 « Ton courriel — pour recevoir ta vidéo » (`#clientEmail`) et nº 10 « Décris ta mission avec tes mots » (`#missionStory`, la description libre qui manquait — ex. de Roger : le salon de coiffure). La description libre entre aussi dans le scénario .txt téléchargé.
4. **Bouton « 📩 Envoyer ma demande au studio »** : ouvre le courriel du client pré-rempli vers savardrjr@gmail.com — sujet « Mission GTA — Demande de capsule : <commerce> », corps complet : courriel du client, héros, commerce, produit, ville, véhicule, ton, forfait, SA description libre et le scénario d'aperçu. Sans courriel valide : avertissement sur place, aucun envoi. Note affichée après clic : joindre sa photo de héros au courriel.
5. **Promesse clarifiée** : l'accueil, l'étape 03, la bibliothèque, la carte Gratuit et la règle nº 2 disent maintenant que la VRAIE capsule, produite par le studio à partir du scénario du client, est **envoyée par courriel**.

**Boucle de production (mise en place le 24 nov.)** : recette `../PRODUCTION-DEMANDES.md` (capsule gratuite 10 s verticale : pochette 4 panneaux **animés indépendamment**, plans générés à partir de la description du client — son lieu, son action, ses textes —, HUD GTA, ritournelle 50 %, final MISSION ACCOMPLIE + logo Sav'art, −14 LUFS, double vérification, livraison par courriel au client, étiquette Gmail Label_9, registre `../demandes/registre.json`). Tâche planifiée **aux 2 heures** : `mission-gta-demandes-production` (mandat Empire des émotions — aucun OK de Roger requis).

**Tests locaux (Playwright, 24 nov.)** : 22/23 — formulaire, avertissement sans courriel, mailto complet vérifié champ par champ sur le cas « Salon Nathalie Coiffure » (description « coiffer une cliente » incluse), carte d'aperçu + chip APERÇU + compte à rebours parti de 10:00, panneau Interac et thème d'accueil sans régression. Le seul échec : le chargement des polices Google Fonts bloqué par le tunnel de la VM de test (pré-existant, identique sur la version précédente ; la page se rabat sur ses polices de secours).
