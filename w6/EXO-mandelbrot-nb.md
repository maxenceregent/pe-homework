---
jupytext:
  cell_metadata_json: true
  encoding: '# -*- coding: utf-8 -*-'
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.19.5
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
---

# l'ensemble de Mandelbrot

il s'agit de calculer l'image de la convergence de mandelbrot:

```{image} media/mandelbrot.svg
:width: 400px
:align: right
```

```{code-cell} ipython3
import numpy as np
import matplotlib.pyplot as plt
%matplotlib ipympl
```

## comment ça marche ?

+++ {"cell_style": "center"}

* dans l'espace complexe, on définit pour chaque $c\in\mathbb{C}$ la suite
   * $z_0 = c$
   * $z_{n+1} = z_n^2 + c$
* on démontre que 
  * lorsque $|z_n|>2$, la suite diverge

+++ {"cell_style": "center"}

il s'agit pour nous de 

* partir d'un pavé rectangulaire; par exemple sur la figure, on a pris l'habituel  
  $re \in [-2, 0.8]$ et  $im \in [-1.4, 1.4]$

* découper ce pavé en un maillage de $h \times w$ points  (sur la figure, 1000 x 1000)
* on se fixe un nombre maximal `max` d'itérations (disons 20)  
  et pour chaque point du maillage, on va calculer si la suite diverge avant `max` itérations

* c'est-à-dire plus spécifiquement on calcule un tableau `diverge` de la taille du maillage
  * pour chaque point `z`, on calcule les `max` premiers termes de la suite
  * et à la première itération `n` où la suite diverge (son module est supérieur à 2)  
    alors on affecte `diverge[z] = n`

* on n'a plus qu'à afficher ensuite l'image obtenue `diverge` avec `plt.imshow`

+++

````{admonition} indices

pour fabriquer la grille des points de départ, on pourra regarder `np.linspace` et `np.meshgrid`
````

```{code-cell} ipython3
:tags: [level_basic]

# à vous de jouer

def mandelbrot(h, w):
    maxiter = 20
    
    re = np.linspace(-2, 0.8, w)
    im = np.linspace(-1.4, 1.4, h)
    
    X, Y = np.meshgrid(re, im)
    C = X + 1j * Y
    
    Z = C.copy()
    diverge = np.zeros((h, w))
    
    for n in range(1, maxiter + 1):
        Z = Z**2 + C
        mask = (np.abs(Z) > 2) & (diverge == 0)
        diverge[mask] = n
    plt.imshow(diverge)
    
    pass
```

```{code-cell} ipython3
:tags: [level_intermediate]

# et pour la tester, pour produire la mème figure que ci-dessus

mandelbrot(1000, 1000)
#ca renvoie une erreur untimeWarning: overflow encountered in square Z = Z**2 + C
#mais ca renvoie aussi l'image donc on pourrait arreter plus tot la boucle quand ca diverge mais vu que l'image fonctionne c'est pas grave
```

## v2

* on peut passer en paramètre à la fonction
  * le domaine en x et en y
  * le nombre maximum d'itérations
* on veut pouvoir produire une image (pour l'insérer dans l'énoncé par exemple)
  * quels formats sont disponibles ?
  * sauvez votre image dans un format vectoriel
  * affichez cette depuis votre notebook

```{code-cell} ipython3
:tags: [level_basic]

# à vous de jouer
# je vous laisse définir la signature de votre fonction


def mandelbrot2(h, w):
    x = np.linspace(-2, 0.8, w)
    y = np.linspace(-1.4, 1.4, h)
    X, Y = np.meshgrid(x, y)
    C = X + 1j*Y
    Z = C.copy()
    D = np.zeros((h, w))

    for n in range(1, 21):
        Z = Z**2 + C
        m = (np.abs(Z) > 2) & (D == 0)
        D[m] = n

    plt.imshow(D)
    plt.show()

mandelbrot2(1000, 1000)
mandelbrot2(500, 500)

#idem
#ca renvoie une erreur untimeWarning: overflow encountered in square Z = Z**2 + C
#mais ca renvoie aussi l'image donc on pourrait arreter plus tot la boucle quand ca diverge mais vu que l'image fonctionne c'est pas grave
```

et je vous demande de mettre ici quelques tests de votre deuxième fonction

```{code-cell} ipython3
:tags: [level_basic]

# test #1
```

```{code-cell} ipython3
:tags: [level_basic]

# test #2
```

```{code-cell} ipython3
:tags: [level_basic]

# test #3
```

----
