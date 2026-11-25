# VKT ReadPos

[English](../README.md) | [中文](README_zh.md) | [日本語](README_ja.md) | [Deutsch](README_de.md) | [Español](README_es.md) | Français

Position de lecture et scènes de page pour Chrome, Edge et Firefox. Chaque page longue retient où vous vous êtes arrêté ; les pages qui portent un état — un tableau de bord, un formulaire à moitié rempli, un ticket commencé — peuvent être enregistrées comme *scène* et restaurées en entier.

Installation : https://annmax1983.com/extensions/readpos

## Fonctionnalités

### Positions de lecture (gratuit)

- **Par page, automatiquement** — la position est enregistrée 600 ms après l'arrêt du défilement, puis de nouveau quand l'onglet est masqué ou fermé.
- **La pilule de reprise** — en rouvrant une page, *Reprendre où vous en étiez · 62 %* apparaît en bas à droite. Un clic fait défiler en douceur jusqu'à cet endroit. Elle ne défile jamais seule.
- **Discrète** — la pilule s'efface au bout de quelques secondes ; amener le pointeur dans le coin inférieur droit la fait revenir. Rien d'autre n'est dessiné sur la page.
- **Les pages courtes restent muettes** — une position sous le seuil (600 px par défaut) n'est pas proposée.
- **Mettre un site en pause, effacer une page** — un clic chacun, dans le panneau.
- **20 entrées d'historique** — le panneau les liste avec le pourcentage atteint ; un clic sur une ligne rouvre la page.
- **Routes SPA séparées** — `/app/inbox` et `/app/settings` sont deux pages. Les routes à ancre (`#/route`) aussi.
- **Pages à chargement différé** — si la position enregistrée dépasse la hauteur actuelle du document, la restauration réessaie deux fois puis renonce en silence.
- **Portable** — export et import JSON, gratuits.
- **Confirmation avant restauration** — avant de rendre une scène, ce qui sera écrasé est listé. Gratuit et actif par défaut.

### Pro

- **Scènes de page** — enregistrez la position et chaque champ du formulaire sous un nom que vous choisissez, puis remettez tout en place plus tard.
- **500 entrées d'historique** au lieu de 20.
- **Réglages de la pilule** — délai, effacement et seuil réglables.
- **Liste d'exclusion** — domaines et expressions régulières.
- **Plusieurs scènes nommées par page** — de 1 à 20.
- **Champs en lecture seule** inclus en option dans une scène.
- **Raccourci clavier** pour enregistrer une scène.
- **Scènes dans l'export** — le fichier JSON contient positions et scènes.

| | Gratuit | Pro |
|---|---|---|
| Positions de lecture | ✅ | ✅ |
| Entrées d'historique | 20 | 500 |
| Pilule de reprise | minutage fixe | délai / effacement / seuil réglables |
| Liste d'exclusion | domaines | domaines **et** expressions régulières |
| Scènes de page (enregistrer + restaurer) | — | ✅ |
| Champs texte, zones de texte, listes, cases à cocher, boutons radio, panneaux ouverts | — | ✅ |
| Plusieurs scènes nommées par page | — | ✅ |
| Limite de scènes par page | — | réglable (1–20) |
| Inclure les champs en lecture seule | — | en option |
| Enregistrer par raccourci | — | en option (désactivé par défaut) |
| Export / import JSON | positions | positions + scènes |

Une licence s'active sur deux navigateurs. La retirer libère la place.

## Ce qu'une scène contient

| Élément | Restauré |
|---|---|
| `input` de type texte (`text`, `email`, `url`, `tel`, `number`, `search`, `date`, …) | la valeur |
| `textarea` | la valeur |
| `select` | l'option choisie (à défaut, correspondance sur le libellé) |
| `checkbox`, `radio` | l'état coché |
| `details` | ouvert / fermé |
| position de défilement | oui, même logique de nouvelle tentative que la pilule |

Les valeurs repassent par le setter natif, suivies d'événements `input` et `change`, donc React et Vue le voient. Les champs sont localisés d'abord par chemin structurel, puis par `id`, puis par `name` : une page qui bouge légèrement se restaure quand même.

### Ce qui n'est pas enregistré

- **Mots de passe, fichiers, champs masqués, codes à usage unique, numéros de carte, CVV, IBAN, SSN** — exclus à la capture. Aucun réglage ne les réactive.
- **Champs en lecture seule et désactivés** — sauf si vous activez l'inclusion du lecture seule (Pro).
- **Panneaux `role="tab"` et éditeurs `contenteditable`** — aucun moyen sûr de remettre l'état sans casser le framework ; laissés de côté plutôt qu'à moitié faits.
- **Champs dans les shadow roots et les iframes d'une autre origine** — aucun chemin stable ne peut être construit au-delà de ces frontières.

## Utilisation

### Lire

1. Faites défiler n'importe quelle page. À l'arrêt, la position est retenue.
2. Revenez sur cette page. Si vous êtes encore près du haut, la pilule apparaît au bout d'un instant en bas à droite.
3. Cliquez dessus pour revenir en douceur à votre endroit, ou ignorez-la et elle s'efface.
4. Le reste est dans le panneau : **Reprendre la lecture**, **Mettre ce site en pause**, **Effacer cette page** et la liste d'historique.

### Enregistrer une scène (Pro)

1. Mettez la page dans l'état que vous voulez conserver.
2. Ouvrez le panneau, cliquez **Enregistrer la scène**, donnez-lui un nom et validez. Ou utilisez le raccourci si vous l'avez activé.
3. Plus tard, sur la même page, le panneau liste chaque scène enregistrée là.
4. Cliquez **Restaurer**. Un panneau indique ce qui sera écrasé — confirmez, et tous les champs plus la position reviennent.

La scène reste modifiable : renommez-la ou supprimez-la depuis sa ligne à tout moment.

### Gérer les données

- **Ligne d'historique** — un clic rouvre la page ; l'icône corbeille supprime seulement cet enregistrement.
- **Section données** — indique combien de positions et de scènes sont stockées et l'espace qu'elles occupent.
- **Effacer toutes les positions** / **Supprimer toutes les scènes** — les deux demandent confirmation.
- **Exporter** écrit les positions (et les scènes avec Pro) dans un fichier JSON. **Importer** fusionne au lieu de remplacer et conserve l'enregistrement le plus récent quand les deux côtés connaissent la même page.

### Réglages

- **Gratuit** — mettre un site en pause, effacer une page, exporter et importer.
- **Pro** — délai, effacement et seuil de la pilule ; domaines et expressions régulières exclus ; scènes par page ; champs en lecture seule ; raccourci clavier.

Sous Firefox, le raccourci optionnel se définit dans le Gestionnaire de modules : **about:addons → roue dentée → Gérer les raccourcis de l'extension**.

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
