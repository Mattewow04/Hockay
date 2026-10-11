# Puck Suisse

Jeu de hockey sur glace (National League, Swiss League, mode carrière et gestion de club), à jouer dans le navigateur ou installé comme une app sur le téléphone.

Tout le jeu tient dans `public/index.html`. Pas de compilation, pas de serveur spécial : ce sont de simples fichiers à héberger.

```
public/
  index.html            le jeu
  manifest.webmanifest  installation comme une app
  sw.js                 fonctionnement hors ligne
  icon-192.png, icon-512.png, apple-touch-icon.png
.gitlab-ci.yml          mise en ligne automatique sur GitLab Pages
```

---

## 1. Mettre le jeu sur GitLab (GitLab Pages)

### Sans ligne de commande (depuis le site gitlab.com)

1. Connecte-toi sur **gitlab.com**, puis **New project → Create blank project**.
   Donne-lui un nom, par exemple `puck-suisse`. Visibilité : **Public**, c'est le plus simple pour Pages.
2. Dans le projet : **+ (en haut) → Upload file**. Envoie `.gitlab-ci.yml` et `README.md` à la racine.
   > Astuce : `.gitlab-ci.yml` commence par un point, et certains téléphones le cachent. Si tu ne le vois pas, crée-le directement sur GitLab avec **+ → New file**, nomme-le `.gitlab-ci.yml` et colles-y son contenu.
3. Crée le dossier `public` : **+ → New directory**, nomme-le `public`, puis envoie dedans les 6 fichiers du dossier `public/`.
4. Chaque envoi sur la branche principale lance automatiquement la mise en ligne. Tu peux suivre ça dans **Build → Pipelines** : attends la coche verte, en général moins d'une minute.
5. L'adresse du jeu s'affiche dans **Deploy → Pages**. Elle ressemble à :
   `https://ton-pseudo.gitlab.io/puck-suisse/`
   Dans ce même menu, laisse cochée l'option qui force le HTTPS. Si GitLab propose « Use unique domain », tu peux le laisser : l'adresse sera juste différente.

### Avec git (sur ordinateur)

```bash
cd puck-suisse-gitlab
git init
git add .
git commit -m "Puck Suisse"
git branch -M main
git remote add origin https://gitlab.com/ton-pseudo/puck-suisse.git
git push -u origin main
```

### Mettre à jour le jeu

Remplace seulement `public/index.html` par la nouvelle version (upload ou `git push`). La mise en ligne se refait toute seule. Sur le téléphone, ferme et rouvre l'app (deux fois si besoin) pour charger la nouvelle version.

---

## 2. Mettre le jeu sur ton nom de domaine (à côté de ton autre jeu)

Les deux jeux peuvent cohabiter sans problème. Il suffit de mettre Puck Suisse dans **son propre dossier**.

1. Ouvre le gestionnaire de fichiers de ton hébergeur (Infomaniak, Hostpoint, OVH, cPanel…) ou un logiciel FTP comme FileZilla.
2. Va dans le dossier racine de ton site. Il s'appelle souvent `public_html`, `www`, `web` ou `httpdocs`, et c'est là que se trouvent les fichiers de ton autre jeu.
3. Crée un nouveau dossier, par exemple `hockey`.
4. Envoie dedans **le contenu du dossier `public/`**, c'est-à-dire les 6 fichiers (pas le dossier `public` lui-même).
5. Le jeu est alors à l'adresse : `https://tondomaine.ch/hockey/`

**Variante, un sous-domaine** comme `hockey.tondomaine.ch` : crée-le dans le panneau de ton hébergeur, fais-le pointer vers un nouveau dossier, et envoie les 6 fichiers dans ce dossier.

Points à vérifier :

- **HTTPS obligatoire** pour l'installation en app et le mode hors ligne. La plupart des hébergeurs l'activent en un clic (certificat Let's Encrypt gratuit).
- **N'écrase rien de ton autre jeu** : mets tout dans `hockey/`, rien à la racine.
- **Si ton autre jeu installe lui aussi un mode hors ligne** (un fichier `sw.js` ou `service-worker.js` à la racine du site), il peut parfois intercepter les pages de `/hockey/`. Si Puck Suisse ne s'affiche pas ou affiche ton autre jeu, mets plutôt Puck Suisse sur un sous-domaine.
- **Les sauvegardes ne se mélangent pas** : Puck Suisse enregistre ses données sous des noms à lui (`puck.career`, `puck-suisse`).

---

## 3. Installer le jeu comme une app sur le téléphone

- **iPhone (Safari)** : ouvre l'adresse du jeu, puis **Partager → Sur l'écran d'accueil**.
- **Android (Chrome)** : ouvre l'adresse, puis **menu ⋮ → Installer l'application** (ou « Ajouter à l'écran d'accueil »).

Le jeu s'ouvre alors en plein écran. Une fois chargé, il fonctionne même sans connexion, sauf l'IA.

---

## 4. Activer l'IA Claude avec ta clé d'API (facultatif)

Sans clé, tout fonctionne avec les réponses automatiques intégrées au jeu. Avec une clé, c'est Claude qui écrit les répliques des **négociations par chat** (joueurs, ventes, sponsors, loges), des **interviews** et des réactions à tes **messages libres en conférence de presse**.

### Obtenir la clé

1. Va sur **platform.claude.com** et crée un compte (c'est séparé de ton abonnement Claude).
2. **Billing** : ajoute une carte et un peu de crédit, par exemple 5 $. Les nouveaux comptes reçoivent parfois un petit crédit gratuit pour tester.
3. **Limits** : fixe une **limite de dépenses mensuelle** basse, par exemple 5 $, pour être tranquille.
4. **API Keys → Create Key** : donne-lui un nom (« Puck Suisse ») et **copie la clé tout de suite**. Elle commence par `sk-ant-…` et ne s'affiche qu'une seule fois.

### La mettre dans le jeu

1. Ouvre le jeu. Sur le menu principal, touche le bouton **🤖 IA Claude** en bas à droite.
2. Colle la clé, puis touche **Enregistrer**. Le jeu fait un test automatique : « Ça marche ! » veut dire que c'est bon.
3. Le bouton affiche alors **IA Claude : activée**.

Pour la retirer : même bouton, puis **Effacer la clé**.

### Combien ça coûte

Le jeu utilise le modèle le moins cher (Claude Haiku 5.5). Une réplique coûte environ **0,0002 $**, donc 1000 répliques font environ 0,15 $. Une saison bien remplie revient à quelques centimes. Les tarifs à jour sont sur : https://platform.claude.com/docs/en/about-claude/pricing

### Sécurité

- La clé est enregistrée **uniquement dans le navigateur de ton appareil**. Elle n'est jamais dans le code du site et n'est envoyée qu'à Anthropic.
- **Ne la partage jamais** et ne la colle pas sur l'appareil de quelqu'un d'autre : elle permet de dépenser ton crédit.
- Si tu penses qu'elle a fuité, supprime-la dans **API Keys** sur la console et crée-en une nouvelle.
- Ce système est fait pour **toi**. Si tu veux un jour offrir l'IA à tous les joueurs de ton site, il faudra un petit serveur relais qui garde la clé secrète.

### Messages d'erreur du test

| Message | Solution |
|---|---|
| Clé refusée | Recopie la clé en entier, ou crée-en une nouvelle |
| Plus de crédit | Recharge ton crédit dans **Billing** |
| Trop de demandes | Attends une minute |
| Échec / pas de connexion | Vérifie ta connexion internet |

Si l'IA échoue pendant une partie, le jeu repasse tout seul sur les réponses automatiques.

---

## 5. Bon à savoir

- **Sauvegardes** : elles restent dans le navigateur de chaque appareil. Effacer les données du site efface la carrière. La carrière jouée dans Claude ne se transfère pas automatiquement.
- **Musiques** (but, entrée des joueurs, homme du match, célébration) : choisis tes fichiers dans le jeu. Ils sont gardés sur ton appareil, pas sur le serveur.
