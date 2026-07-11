# 🌿 Langage Harmonique

> **Prototype exploratoire** — et si on décrivait des interfaces avec les proportions de la nature ?

Un mini-langage déclaratif (fichiers `.harm`) où le **nombre d'or (φ)**, la **suite de Fibonacci** et l'**angle d'or (137,5°)** sont des citoyens de première classe. On écrit des intentions harmoniques, un interpréteur JavaScript maison les traduit en animations DOM.

```harm
use φ = 1.618
use rhythm = fib(8)

on hover => scale φ rotate 61.8
on click => scale φ spiral rhythm
```

## 💡 L'idée

Le design "qui semble juste" repose souvent sur des proportions naturelles : le nombre d'or dans les layouts, l'angle d'or dans la disposition des feuilles (phyllotaxie), les rythmes de Fibonacci. Ces constantes sont pourtant absentes de nos langages front — on écrit `scale(1.618)` sans que le langage sache pourquoi.

Le Langage Harmonique inverse la démarche : **φ, `fib(n)` et `spiral` font partie de la grammaire.** L'harmonie n'est plus un nombre magique dans le CSS, c'est le vocabulaire de base.

## ⚙️ Comment ça marche

Aucune dépendance, aucun build. Trois fichiers :

| Fichier | Rôle |
|---|---|
| `main.harm` | Le code source harmonique — déclaré par `<script type="text/harm" src="main.harm">`, dont l'interpréteur lit le `src` |
| `index.html` | Contient `HarmoniqueInterpreter`, l'interpréteur écrit à la main |
| `index.css` | Mise en scène du démonstrateur (transition en `1.618s`, évidemment) |

L'interpréteur fait un vrai travail de langage, en miniature :

1. **Fetch & découpe** — le fichier `.harm` est chargé puis parsé ligne à ligne (les `#` sont des commentaires).
2. **Déclarations `use`** — `use φ = 1.618` alimente une table de variables (identifiants Unicode acceptés, φ compris) ; les expressions passent par un évaluateur qui substitue les variables déclarées et résout les appels comme `fib(8)`. φ reste prédéfini si on ne le redéclare pas.
3. **Événements `on`** — `on hover => …` et `on click => …` sont traduits en écouteurs DOM.
4. **Actions** — `scale`, `rotate` et `spiral` sont compilées en `transform` CSS. `spiral n` tourne de *n* pas d'angle d'or (n × 137,5°), comme les graines d'un tournesol.

## 🚀 Essayer

Le fichier `.harm` est chargé en `fetch`, il faut donc servir le dossier en HTTP :

```bash
python -m http.server 8000
# puis ouvrir http://localhost:8000
```

Survole le carré (scale φ + rotation 61,8°), clique dessus : spirale de Fibonacci — `fib(8) = 21` pas d'angle d'or, soit 2 887,7° de rotation. Re-clic pour libérer.

## 🧪 Grammaire actuelle

| Construction | Exemple | Effet |
|---|---|---|
| `use <var> = <expr>` | `use rhythm = fib(8)` | Déclare une variable, réutilisable dans les actions (φ et `fib(n)` sont prédéfinis) |
| `on hover => <actions>` | `on hover => scale φ` | Actions au survol, retour à l'état initial en sortie |
| `on click => <actions>` | `on click => spiral rhythm` | Applique/verrouille les actions, re-clic pour relâcher |
| `scale <expr>` | `scale φ` | Mise à l'échelle |
| `rotate <expr>` | `rotate 61.8` | Rotation en degrés |
| `spiral <expr>` | `spiral rhythm` | Rotation de *n* × 137,5° (angle d'or) |

## 🔭 Limites connues & pistes

C'est un prototype de recherche, pas une lib de prod — et c'est assumé :

- Parser à base de regex ligne à ligne → un vrai tokenizer serait la suite logique
- Une seule cible DOM (`.carre`) → sélecteurs dans la grammaire (`on hover .card => …`)
- Évaluation d'expressions naïve (substitution + `eval`, à réserver à ses propres fichiers `.harm`) → petit évaluateur arithmétique dédié
- À explorer : séquences rythmées sur Fibonacci (`animate rhythm`), spirales logarithmiques, grilles φ

---

*Expérimentation de [Oussama Halima-Filali](https://github.com/oussama-filali) — descendre sous les frameworks pour comprendre ce qu'est un langage.*
