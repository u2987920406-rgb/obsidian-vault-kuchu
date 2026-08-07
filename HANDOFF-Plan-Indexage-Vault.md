# HANDOFF — Plan d'indexage Vault Obsidian (kuchu)

Document autonome. À redonner à une session Hermès pour exécution.
Contexte validé avec kuchu le 2026-08-07. Usage perso. Rien n'est à exécuter
tant que l'instance Hermès parallèle (même PC, profil partagé) n'est pas fermée.

================================================================
1. BUT
================================================================
- Alléger MEMORY.md (injecté à chaque tour, plafond 2200 chars) en déplaçant
  la connaissance projet vers le Vault Obsidian.
- Créer un INDEX unique (MOC / finding aid) que l'IA lit à la demande.
- Faire un Vault UNIQUE, tool-agnostic, où chaque outil IA est un locataire
  niché (pas à la racine, pas de "trois homes à la racine").
- Vault = repo GitHub PRIVÉ : suit la vie de kuchu, backup hors-site, historique.

================================================================
2. GARDE-FOUS (ne pas briser le système de fichiers Windows)
================================================================
- TOUS les chemins restent sous :
    C:\Users\kuchu\Documents\Obsidian Vault\
    C:\Users\kuchu\AppData\Local\hermes\   (MEMORY.md seulement)
- JAMAIS écrire en racine C:\ ou ailleurs hors de ces deux zones.
- Ne pas toucher aux dossiers d'autres outils (Claude : C:\Users\kuchu\Claude,
  .claude, Dev_app_Claude) sauf pour créer leur locataire 20.02.
- Le dossier "Hermes Home" EXISTANT dans le Vault est déplacé/renommé, pas
  dupliqué. Aucune copie parallèle (loi wire-don't-rebuild).

================================================================
3. ARCHITECTURE VALIDÉE (Johnny Decimal + PARA + MOC)
================================================================
C:\Users\kuchu\Documents\Obsidian Vault\
  00 Index.md              <- MOC unique, SEUL fichier cité par MEMORY.md
  10 Projets\              <- PROJETS de kuchu (pas ceux des IA)
      10.01 Ulysse\
      10.02 (projet parallèle, seedé plus tard, ne pas créer maintenant)
  20 Outils\               <- namespace par outil, jamais à la racine
      20.01 Hermès\   <- l'ancien "Hermes Home" va ici (renommé)
      20.02 Claude\
      (20.03+ ouverts si d'autres outils ajoutés)
  30 Journal\              <- VOIR section 5 (horodatage)

Règle d'or : 1 Vault unique. Racine = index partagé. Outils = locataires 20.xx.
Ajouter un outil = ajouter un 20.0x, jamais recréer un Vault.

================================================================
4. RÔLE DE CHAQUE PIÈCE
================================================================
- 00 Index.md = le finding aid (carte maîtresse). Liste chaque zone avec son
  adresse JD + lien vers la note détaillée. L'IA le lit quand un sujet arrive,
  puis suit le lien. Évite d'injecter 3000 chars à chaque tour.
- MEMORY.md (allégé, injecté chaque tour) GARDE :
    * Règles de travail (JAMAIS/DETESTE/DOIT)
    * Profil user
    * UNE ligne-pointeur :
      "Coffre : C:\Users\kuchu\Documents\Obsidian Vault\00 Index.md
       — lire si sujet Hermès/Ulysse/Freebuff"
- Contenu DÉPLACÉ hors MEMORY vers le Vault :
    * Archi détaillée Ulysse (ports 8645/8644/9119, auth Portal, 6 rôles, perms)
    * Note Freebuff2API (fallback perso si provider free Nous saturé)
- Le VAULT est un repo Git PRIVÉ. .gitignore exclut .obsidian/.

================================================================
5. NOTION D'HORODATAGE (date / heure / entrée / sortie)
================================================================
AVIS HERMÈS : pertinent, MAIS à part, pas dans l'index.
- L'index (00 Index.md) doit rester stable et lisible. Pas de timestamps dedans.
- En revanche, un JOURNAL de session (entrée = début de travail, sortie = fin)
  est utile : il donne une trace humaine lisible qui COMPLÈTE l'historique Git
  (Git horodate les commits, le journal horodate les séances de travail).
- Implémentation proposée : dossier 30 Journal\ avec une note par jour
  (ex. 30 Journal\2026-08-07.md), chaque entrée au format :
      ## [14:32 → 15:10] Ulysse — indexage Vault
      - entrée : discussion architecture JD/PARA/MOC
      - sortie : plan validé, handoff écrit
- Plugin recommandé : "Daily Notes" (ou Periodic Notes) pour générer la note
  du jour automatiquement ; "Dataview" pour requêter le journal si besoin.
- C'est une PHASE 2 optionnelle. Ne bloque pas l'exécution de la section 6.

================================================================
6. PLAN D'EXÉCUTION (à lancer quand instance parallèle FERMÉE)
================================================================
ORDRE STRICT :
  1. Réécrire MEMORY.md en version lean VIA l'action memory() (pour que
     l'outil redevienne propriétaire du fichier et enterre la "dérive").
     Contenu = règles + profil + ligne-pointeur Vault. ~< 2000 chars.
  2. Créer 00 Index.md dans le Vault : liste 10/20/30 + liens.
  3. Déplacer le dossier "Hermes Home" existant -> 20.01 Hermès\
     (mv, pas copie). Mettre à jour le chemin HOME de Hermès/Ulysse.
  4. Vider l'archi Ulysse + note Freebuff dans 10.01 / 11 Ressources
     (créer 11 Ressources\ si on veut séparer ressources de projets).
  5. Init repo Git PRIVÉ sur le Vault + .gitignore (.obsidian/).
     Via plugin Obsidian Git (zéro ligne de commande pour kuchu) de préférence.
  6. (Phase 2) Activer Daily Notes -> 30 Journal\, seeder une premiere note.

================================================================
7. DÉCLENCHEUR
================================================================
kuchu redonnera le signal "instance fermée" -> exécuter section 6.
Avant ce signal : NE RIEN ÉCRIRE dans MEMORY.md ni restructurer le Vault
(en dehors de CE fichier handoff, posé dans le Vault volontairement).

================================================================
8. RÉCAP POUR L'IA RECEVEUSE
================================================================
- Lis 00 Index.md avant tout sujet Hermès/Ulysse/Freebuff.
- MEMORY.md est lean : il pointe ici, il ne contient pas l'archi.
- Respecte les garde-fous section 2 (chemins Windows, pas de duplication).
- Le Vault est unique et privé (GitHub). Étends par 20.0x, jamais par
  nouveau Vault.
