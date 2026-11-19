# VKT ReadPos

[English](../README.md) | [中文](README_zh.md) | [日本語](README_ja.md) | [Deutsch](README_de.md) | [Español](README_es.md) | Français

Position de lecture et scènes de page pour Chrome, Edge et Firefox. Chaque page longue retient où vous vous êtes arrêté ; les pages courtes qui portent un état — un tableau de bord, un formulaire à moitié rempli, un ticket commencé — peuvent être enregistrées comme *scène* et restaurées en entier.

Tout est calculé et stocké sur votre appareil. Pas de compte, pas de service de synchronisation, pas de télémétrie. La seule requête réseau est la validation de licence.

## Gratuit

- **Position par page** — écrite 600 ms après l'arrêt du défilement, puis à nouveau quand l'onglet est masqué ou fermé. Jamais d'écriture au pixel.
- **Le bouton de reprise** — à la réouverture, un petit contrôle en bas à droite : *Reprendre où vous en étiez · 62 %*. Un clic fait défiler en douceur jusqu'à cet endroit. **Il ne défile jamais de lui-même.**
- **Discret par construction** — le bouton s'efface au bout de quelques secondes ; amener le pointeur dans le coin inférieur droit le fait revenir. Rien d'autre n'est dessiné sur la page.
- **Les pages courtes restent muettes** — une position sous le seuil (600 px par défaut) n'est pas proposée.
- **Pause pour ce site / Effacer cette page** — à un clic depuis le popup.
- **20 pages d'historique** — avec le pourcentage atteint ; un clic sur une ligne la rouvre.
- **Routes SPA séparées** — `/app/inbox` et `/app/settings` sont deux pages distinctes ; les routeurs à hash (`#/route`) aussi.
- **Chargement différé pris en compte** — si la position enregistrée dépasse la hauteur actuelle du document, la restauration réessaie deux fois puis renonce en silence.
- **Vos données sont portables** — export et import JSON, gratuits.
- **Demander avant de restaurer** — ce qui sera écrasé est toujours affiché d'abord. Activé par défaut et gratuit : une confirmation n'est pas un argument de vente.

## Pro (9,99 USD, paiement unique)

| | Gratuit | Pro |
|---|---|---|
| Positions de lecture | ✅ | ✅ |
| Pages d'historique | 20 | 500 |
| Bouton de reprise | minutage fixe | délai / effacement / seuil réglables |
| Liste d'exclusion | domaines | domaines **et** expressions régulières |
| Scènes (enregistrer + restaurer) | — | ✅ |
| Champs texte, zones de texte, listes, cases, boutons radio, panneaux ouverts | — | ✅ |
| Plusieurs scènes nommées par page | — | ✅ |
| Scènes par page | — | réglable (1–20) |
| Inclure les champs en lecture seule | — | en option |
| Raccourci clavier d'enregistrement | — | en option (désactivé par défaut) |
| Export / import JSON | ✅ positions | ✅ positions + scènes |

Une licence s'active sur deux navigateurs. La retirer libère la place. La validation est limitée à une fois toutes les deux heures, fonctionne hors ligne depuis la licence en cache, et un échec ne vous bloque jamais.

## Ce qu'une scène capture

Valeurs des `input` (text, email, url, tel, number, search, date…), des `textarea`, l'option choisie d'un `select`, l'état des `checkbox`/`radio`, les `details` ouverts et la position de défilement. L'écriture passe par le setter natif puis déclenche `input` et `change`, donc React et Vue le remarquent. Les champs sont localisés par chemin structurel d'abord, puis par `id`, puis par `name`.

### Ce qu'elle ne touche délibérément pas

- **Mots de passe, fichiers, champs cachés, codes à usage unique, numéros de carte, CVV, IBAN, SSN** — exclus à la capture, pas filtrés à la restauration. Aucun réglage ne permet de les activer.
- **Champs en lecture seule et désactivés** — ignorés par défaut (Pro : lecture seule en option).
- **Panneaux `role="tab"`** — figuraient dans le plan initial et ont été retirés : l'état des onglets est piloté par le framework, et réécrire `aria-selected` ne fait rien ou déclenche une navigation. Une fonction qui semble marcher est pire que pas de fonction.
- **Éditeurs `contenteditable`** — aucune façon fiable et compatible framework de réinjecter le texte ; laissé de côté plutôt qu'à moitié fait.
- **Champs dans les shadow roots et les iframes d'une autre origine** — aucun chemin stable ne peut être construit au-delà de ces frontières ; ignorés au lieu d'être devinés.

## Confidentialité

Positions, scènes et réglages vivent uniquement dans le stockage local de l’extension (`chrome.storage.local`, `browser.storage.local` sous Firefox). Pas de statistiques, pas de télémétrie, pas de comptes. Les valeurs capturées ne quittent pas le navigateur ; la seule connexion est la validation de licence auprès de `api.annmax1983.com`.

## Autorisations

| Autorisation | Raison |
|---|---|
| `storage` | Positions, scènes, réglages et état de licence — tout en local |
| `http://*/*`, `https://*/*` | Le script de contenu doit exister sur chaque page **avant** votre défilement, sinon il n'y a rien à retenir. Contrairement à un outil de favoris, il ne peut pas attendre qu'on le demande site par site : d'où l'accès large, indiqué clairement dans la fiche du store. |

Pas de `tabs`, `scripting`, `debugger` ni `activeTab`. Rien ne s'exécute sur les pages `chrome://` (Chrome), `edge://` (Edge), `about:` (Firefox), `file://` ou les pages d'autres extensions.

## Navigateurs pris en charge

| Navigateur | Où se trouve la version | Minimum |
|---|---|---|
| Chrome | ce dépôt | Chrome 116 — Manifest V3, `background.service_worker` |
| Edge | ce dépôt, même paquet que Chrome | Edge 116 |
| Firefox | `codeup/VKT-ReadPos` | Firefox 140 — Manifest V3, `background.scripts` |

Les deux versions sont la même extension : `common.js`, `content/content.js`, `popup/`, `_locales/`, les icônes et ce document sont partagés, et un export JSON de l’une s’importe tel quel dans l’autre. Trois choses seulement diffèrent : le manifeste (une page d’événements chargée depuis une liste de scripts au lieu d’un service worker, plus un identifiant gecko et la déclaration de données exigée par AMO), l’espace de noms de l’API (Firefox utilise `browser.*` avec des promesses, pas `chrome.*` avec des callbacks) et l’empaquetage (la version Firefox produit en plus un zip de sources non minifiées pour la relecture).

Une différence visible : le raccourci clavier facultatif se règle dans le Gestionnaire de modules de Firefox (about:addons → roue dentée → Gérer les raccourcis de l’extension), pas dans `chrome://extensions/shortcuts`.

## Développement

```bash
npm install
npm test            # node --test (99 assertions)
npm run build       # minifier + zip → publish/readpos-v{version}.zip
npm run locales     # régénérer les six langues
npm run icons       # régénérer les icônes
npm run shoot       # captures pour le store (nécessite Chrome)
```
