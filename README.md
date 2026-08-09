# Pricing d'Options par Méthodes Numériques (MATLAB)

Ce projet propose une implémentation complète en MATLAB pour la valorisation (pricing) d'options financières via la méthode des différences finies (FDM). Il couvre les options de type Européen, Américain et Asiatique, en explorant différents schémas de discrétisation et algorithmes d'optimisation pour traiter le problème de l'exercice anticipé.

---

## 🧮 Méthodologie Mathématique

### 1. L'Équation de Black-Scholes
Le modèle repose sur l'équation aux dérivées partielles (EDP) de Black-Scholes, qui régit le prix $V(S,t)$ d'un produit dérivé sans opportunité d'arbitrage :

$$ \frac{\partial V}{\partial t} + \frac{1}{2}\sigma^2 S^2 \frac{\partial^2 V}{\partial S^2} + rS \frac{\partial V}{\partial S} - rV = 0 $$

Où :
*   $S$ représente le prix du sous-jacent.
*   $t$ est le temps.
*   $\sigma$ est la volatilité du sous-jacent.
*   $r$ est le taux d'intérêt sans risque.

### 2. Discrétisation par Différences Finies (FDM)
Pour résoudre cette EDP numériquement, l'espace (prix du sous-jacent) et le temps sont discrétisés en une grille[cite: 1, 2, 4]. Le projet implémente deux approches principales :
*   **Schéma Explicite :** Calcule l'état futur du système directement à partir de l'état actuel[cite: 1, 7]. Il est simple à implémenter mais soumis à des conditions de stabilité strictes (condition CFL).
*   **Schéma Implicite :** Nécessite la résolution d'un système d'équations linéaires à chaque pas de temps (utilisation de la décomposition LU), mais garantit une stabilité inconditionnelle[cite: 2, 3].

### 3. Traitement de l'Exercice Anticipé (Options Américaines)
Contrairement aux options européennes, les options américaines peuvent être exercées à tout moment jusqu'à la maturité. Mathématiquement, cela se traduit par une inéquation de type Problème de Complémentarité Linéaire (LCP) :

$$ V(S,t) \geq \max(S - K, 0) $$

Pour résoudre cette contrainte d'obstacle à chaque pas de temps, ce projet implémente et compare trois algorithmes distincts :
*   **Algorithme de Brennan-Schwartz :** Approche directe adaptée aux matrices tridiagonales[cite: 3].
*   **Algorithme d'Howard (Policy Iteration) :** Méthode itérative recherchant la frontière d'exercice optimale[cite: 3, 5].
*   **Méthode de Newton :** Algorithme de recherche de racines appliqué au système non linéaire induit par l'obstacle[cite: 3, 6].

---

## 📂 Structure du Projet et Fichiers

Le dépôt est organisé autour de plusieurs scripts MATLAB autonomes. Le tableau ci-dessous détaille le rôle de chaque fichier :

| Nom du Fichier | Description de l'implémentation |
| :--- | :--- |
| `call_europeen.m` / `call_europeen_direct.m` | Valorisation classique d'un Call Européen (sans exercice anticipé). |
| `call_american.m` | Valorisation d'un Call Américain avec schéma standard et contrainte d'obstacle simple[cite: 4]. |
| `call_american_howars.m` | Call Américain résolu spécifiquement via l'algorithme d'Howard[cite: 5]. |
| `call_american_newton.m` | Call Américain résolu spécifiquement via l'algorithme de Newton[cite: 6]. |
| `call_americano_asiatique.m` | Pricing d'une option hybride (Asiatique avec exercice à l'américaine) basée sur la moyenne des prix[cite: 7]. |
| `call_am_as_explicit_direct.m` | Schéma explicite pour la résolution de l'option américano-asiatique[cite: 1]. |
| `call_am_as_implicit_dex.m` | Schéma implicite avec initialisation spécifique des paramètres pour l'option américano-asiatique[cite: 2]. |
| `call_am_as_implicit_direct.m` | Schéma implicite centralisant le choix du solveur (Brennan-Schwartz, Howard ou Newton) via une variable `style`[cite: 3]. |

---

## 🛠️ Prérequis et Exécution

*   **Environnement :** MATLAB.
*   **Dépendances :** La *Financial Toolbox* de MATLAB est recommandée, car certains scripts utilisent la fonction `blsprice` pour comparer la solution numérique à la solution analytique exacte (formule de Black-Scholes)[cite: 4, 6].

**Comment lancer un test :**
1. Clonez ce dépôt.
2. Ouvrez l'un des scripts, par exemple `call_am_as_implicit_direct.m`[cite: 3].
3. Modifiez si besoin les paramètres initiaux situés en haut du fichier (par exemple, modifiez la variable `style="newton"` ou `style="howard"` pour changer d'algorithme)[cite: 3].
4. Exécutez le script pour obtenir le prix de l'option (`CALL`) et visualiser la surface de prix si la fonction `surf` est activée[cite: 3, 4, 5].
