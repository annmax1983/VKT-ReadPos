# VKT ReadPos

[English](../README.md) | [中文](README_zh.md) | [日本語](README_ja.md) | [Deutsch](README_de.md) | [Español](README_es.md) | Français

Position de lecture et points de restauration de page pour Chrome, Edge et Firefox. Chaque page longue retient où vous vous êtes arrêté ; les pages qui portent un état — un tableau de bord, un formulaire à moitié rempli, un ticket commencé — peuvent être enregistrées comme *point de restauration* et restaurées en entier.

Installation : https://annmax1983.com/extensions/readpos

## Fonctionnalités

### Positions de lecture (gratuit)

- **Par page, automatiquement** — la position est enregistrée 600 ms après l'arrêt du défilement, puis de nouveau quand l'onglet est masqué ou fermé.
- **Reprise, automatique** — la même page s'ouvre directement à votre position. Toute autre page affiche la pilule *Reprendre où vous en étiez · 62 %*, à un clic. Le saut automatique se désactive dans les réglages.
- **Discrète** — la pilule s'efface au bout de quelques secondes ; amener le pointeur dans le coin inférieur droit la fait revenir. Rien d'autre n'est dessiné sur la page.
- **Les pages courtes restent muettes** — une position sous le seuil (600 px par défaut) n'est pas proposée.
- **Mettre un site en pause, effacer une page** — un clic chacun, dans le panneau.
- **N’enregistrer que les sites choisis** — ajoutez des domaines à la main ou d’un clic sur « Ajouter le site actuel » ; les autres sont ignorés. Aucun réglage pour tout enregistrer : une liste vide n'enregistre rien. Gratuit.
- **Exclure une page, ou un motif** — « Exclure cette page » dans le panneau, un clic : cette adresse n’est plus jamais enregistrée, le reste du site continue. Les règles de motif (expressions régulières) écartent des sections entières sur tous les sites sans les nommer une par une. Les deux listes sont gratuites et se trouvent dans l’onglet **Exclues** de la liste complète.
- **20 entrées d'historique** — le panneau liste les pages du site où vous êtes, avec le pourcentage atteint ; un clic sur une ligne rouvre la page.
- **Routes SPA séparées** — `/app/inbox` et `/app/settings` sont deux pages. Les routes à ancre (`#/route`) aussi.
- **Pages à chargement différé** — si la position enregistrée dépasse la hauteur actuelle du document, la restauration réessaie deux fois puis renonce en silence.
- **Portable** — export et import JSON, gratuits.
- **Confirmation avant restauration** — avant de rendre un point de restauration, ce qui sera écrasé est listé. Gratuit et actif par défaut.

### Pro

- **Scènes de page** — enregistrez la position et chaque champ du formulaire sous un nom que vous choisissez, puis remettez tout en place plus tard.
- **500 entrées d'historique** au lieu de 20.
- **Réglages de la pilule** — délai, effacement et seuil réglables.
- **Plusieurs points de restauration nommées par page** — de 1 à 20.
- **Champs en lecture seule** inclus en option dans un point de restauration.
- **Raccourci clavier** pour enregistrer un point de restauration.
- **Les points de restauration voyagent avec l'export** — le fichier JSON contient positions et points de restauration ; réimporter les points de restauration demande Pro.

| | Gratuit | Pro |
|---|---|---|
| Positions de lecture | ✅ | ✅ |
| Entrées d'historique | 20 | 500 |
| Pilule de reprise | minutage fixe | délai / effacement / seuil réglables |
| Scènes de page (enregistrer + restaurer) | — | ✅ |
| Champs texte, zones de texte, listes, cases à cocher, boutons radio, panneaux ouverts | — | ✅ |
| Plusieurs points de restauration nommées par page | — | ✅ |
| Points de restauration par page | — | réglable (1–20) |
| Inclure les champs en lecture seule | — | en option |
| Enregistrer par raccourci | — | en option (désactivé par défaut) |
| Export / import JSON | positions | positions + points de restauration |

Une licence s'active sur deux navigateurs. La retirer libère la place.

## Ce qu'un point de restauration contient

| Élément | Restauré |
|---|---|
| `input` de type texte (`text`, `email`, `url`, `tel`, `number`, `search`, `date`, …) | la valeur |
| `textarea` | la valeur |
| `select` | l'option choisie (à défaut, correspondance sur le libellé) |
| `select multiple` | toutes les options choisies |
| listes déroulantes de composant (Element Plus, Ant Design, Arco, Naive, Semi, iView) | le libellé choisi |
| éditeurs `contenteditable` | le texte |
| champs dans un shadow root ouvert (Web Components) | valeur |
| `checkbox`, `radio` | l'état coché |
| `details` | ouvert / fermé |
| position de défilement | oui, même logique de nouvelle tentative que la pilule |

Les valeurs repassent par le setter natif, suivies d'événements `input` et `change`, donc React et Vue le voient ; si un framework l'ignore, le texte est retapé avec `insertText`. Les champs sont localisés d'abord par un sélecteur CSS court (il entre dans les shadow roots ouverts), puis par `id`, puis par `name` et la valeur courante, et en dernier recours par chemin structurel. Avant d'écrire, le libellé est comparé à celui enregistré : un formulaire réordonné ne reçoit pas de valeurs croisées. Si un champ résiste encore, le texte est saisi via le débogueur du navigateur : connexion, une insertion, déconnexion — ces composants n'acceptent que la saisie réelle.

### Ce qui n'est pas enregistré

- **Mots de passe, fichiers, champs masqués, codes à usage unique, numéros de carte, CVV, IBAN, SSN** — exclus à la capture. Aucun réglage ne les réactive.
- **Champs en lecture seule et désactivés** — sauf si vous activez l'inclusion du lecture seule (Pro).
- **Panneaux `role="tab"`** — changer d'onglet déclenche le chargement du framework ; rien ne peut y être réécrit sans risque.
- **Champs dans les shadow roots fermés et les iframes d'une autre origine** — les nœuds sont inaccessibles, aucun chemin n'est possible.

## Utilisation

### Lire

1. Faites défiler n'importe quelle page. À l'arrêt, la position est retenue.
2. Revenez sur cette page. La même page revient d'elle-même à votre position ; sinon, la pilule apparaît en bas à droite.
3. Avec la pilule, cliquez pour revenir en douceur, ou ignorez-la et elle s'efface. Pour garder la pilule à chaque fois, désactivez le saut automatique dans les réglages.
4. Le reste est dans le panneau : **Reprendre la lecture**, **Mettre ce site en pause**, **Exclure cette page**, **Effacer cette page** et la liste d'historique.

### Enregistrer un point de restauration (Pro)

1. Mettez la page dans l'état que vous voulez conserver.
2. Ouvrez le panneau, cliquez **Enregistrer le point de restauration**, donnez-lui un nom et validez. Ou utilisez le raccourci si vous l'avez activé.
3. Plus tard, sur la même page, le panneau liste chaque point de restauration enregistré là.
4. Cliquez **Restaurer**. Un panneau indique ce qui sera écrasé — confirmez, et tous les champs plus la position reviennent.

Le point de restauration reste modifiable : renommez-le ou supprimez-le depuis sa ligne à tout moment.

### Gérer les données

- **Ligne d'historique** — un clic rouvre la page ; l'icône corbeille supprime seulement cet enregistrement.
- **Onglet « Exclues »** — dans la liste complète : collez une adresse ou ajoutez une règle de motif, et chaque entrée se retire depuis sa ligne. Une règle indique combien de pages enregistrées elle attrape actuellement.
- **Section données** — indique combien de positions et de points de restauration sont stockées et l'espace qu'elles occupent.
- **Effacer toutes les positions** / **Supprimer toutes les points de restauration** — les deux demandent confirmation.
- **Exporter** écrit les positions (et les points de restauration avec Pro) dans un fichier JSON. **Importer** fusionne au lieu de remplacer et conserve l'enregistrement le plus récent quand les deux côtés connaissent la même page.

### Réglages

- **Gratuit** — mettre un site en pause, exclure des pages et des motifs, effacer une page, exporter et importer.
- **Pro** — délai, effacement et seuil de la pilule ; points de restauration par page ; champs en lecture seule ; raccourci clavier.

Sous Firefox, le raccourci optionnel se définit dans le Gestionnaire de modules : **about:addons → roue dentée → Gérer les raccourcis de l'extension**.

## Autorisations

- **`storage`** — positions, points de restauration, réglages et état de licence. En local uniquement.
- **Accès aux pages http et https** — le script de contenu doit être là avant votre défilement, sinon il n'y a rien à retenir.
- **`debugger`** — pour une seule chose : saisir du texte dans un champ qui rejette toute écriture par script. Connexion, une insertion, déconnexion immédiate. Il ne lit jamais la page et ne reste jamais connecté.

## Navigateurs

| Navigateur | Version minimale |
|---|---|
| Chrome | 116 |
| Edge | 116 |
| Firefox | 140 |

La version Firefox se trouve dans `codeup/VKT-ReadPos`. Un export JSON de l'une s'importe tel quel dans l'autre.

## Liens

- Site web : https://annmax1983.com/extensions/readpos
- Politique de confidentialité : [privacy-policy.html](../privacy-policy.html)
