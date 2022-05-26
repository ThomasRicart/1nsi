# Algorithmes gloutons 



## Pourquoi Glouton ?

Imaginons un voyageur de commerce qui doit passer dans 20 villes afin. Comment rendre son trajet le plus court possible ?

Et bien il n'y a qu'une seule façon de le savoir : lister tous les trajets possibles, les mesurer et choisir le plus court.

Sur le papier, le principe est simple. Mais ce n'est pas réalisable.



Faisons un peu de dénombrement.

* Considérons que le voyageur se situe dans l'une des villes pour commencer le circuit.
* Pour sa première étape, il lui reste 19 possibilités : le nombre de trajets possibles à ce stade est donc $19$
* Pour sa deuxième étape, il lui reste 18 possibilités : le nombre de trajets possibles à ce stade est donc $ 19 \times 18$
* etc ...
* Pour son avant dernière étape, il ne lui reste que 2 possibilités : le nombre de trajets possibles à ce stade est donc $ 19 \times 18 \times ... \times 2 = 19!$
* Il n'a plus qu'à relier la ville restante pour terminer son périple.
* Enfin, comme tout trajet peut se faire dans les deux sens avec la même distances, il y a $`\frac {19!} 2`$ trajets de distances différentes.





> *    $\frac {19!} 2$ =                                             
>
> * Combien de temps faudrait-il à un algorithme capable de calculer 100000 trajets par seconde, ce qui serait considérable, pour déterminer tous les trajets possibles ?
>
>     ```python
>     
>     ```
>
>     



Puisqu'il n'est pas possible de lister toutes les possibilités, il nous faut trouver une méthode conduisant à une réponse convenable. Celle-ci consistera à relier la ville la plus proche de la position du voyageur à chaque étape, c'est à dire à choisir une solution **optimale localement** en espérant que la solution finale soit **proche de la solution optimale globale**.

Notre algorithme va donc prendre, à chaque étape, le meilleur cas possible. C'est pour cela qu'on dit qu'il est glouton, gourmand en quelque sorte. Notons que le terme anglais "greedy" se traduit également par cupide, avide, qui illustre bien l'idée de cet algorithme qui est de choisir la grande valeur possible à chaque étape.



Il faut bien comprendre que choisir toujours le meilleur parti à chaque étape n'a pas forcément pour conséquence d'aboutir au meilleur résultat possible, c'est à dire à un résultat **optimal**. Regarder l'arbre ci-dessous. Si on le parcours de façon gloutonne de manière à obtenir la plus grande somme possible, on ne peut parvenir à ce résultat optimal :

![glouton - source wikipédia - CCBYSA](media/Greedy-search-path-example.gif)







## Le problème des pièces de monnaie

### Ennoncé



On dispose de pièces de monnaie dont les montants sont des nombres entiers de l’ unité de monnaie : $p_0, p_1, p_2, ..., p_n$.

Pour payer une certain somme $S$ (entier naturel), on aimerait utiliser le nombre minimal de pièces possibles.



_Exemple : l'euro_

Dans le système monétaire européen on utilise les montants suivants :

$p_1= 50€$, $p_2 = 20€$, $p_3 = 10€$, $p_4= 5€$, $p_6 =2€$ et $p_7 = 1€$


*  Peut-on payer n'importe quelle somme entière avec ce système ? Pourquoi ?

    ```txt
    
    ```

    

* Comment payer la somme $S$=528€ en utilisant le moins de pièces possibles ?

    ```txt
    
    ```

    

* N'y-a-t-il qu'une seule façon de payer cette somme ?

    ```txt
    
    ```

    

* Pouvez-vous calculer le nombre de façon différentes de payer cette somme ?

    ```txt
    
    ```

    

### Implémentation



* Proposer un algorithme, exprimé en langage naturel, permettant de résoudre ce problème .

    ```txt
    
    ```

    

    _Remarque :_

    Il s’agit effectivement d’un algorithme glouton, la plus grande valeur de pièce étant systématiquement choisie si sa valeur est inférieure à la somme à rendre. Ce choix ne garantit en rien l’optimalité globale de la solution. Le choix fait est considéré comme pertinent et permet d’avancer plus avant dans le calcul. 



* Nous allons maintenant implémenter l'algorithme en Python :
    * Le programme s'appellera `monnaie.py`.

    * nous aurons besoin de deux fonctions de paramètre `somme_a_rendre`:

        * l'une renverra la plus grosse pièce utilisable pour rembourser la `somme_a_payer`.
        * l'autre renverra un dictionnaire dont les clés sont les sommes disponibles et les valeurs le nombre de fois qu'on les utilise pour rendre la monnaie.

    * Les deux fonctions seront documentées et auront leurs Doctest ( utilisera l'exemple de la partie précédente comme Doctest)

    * utilisera une liste nommée `SYSTEME` définie globalement et constante (c'est pour cela qu'on la nomme en MAJUSCULE),  triée dans le sens décroissant pour les montants utilisés.

        ```python
        SYSTEME = [50, 20, 10, 5, 2, 1]
        ```

        

### Optimal ?



On se pose la question de savoir si cet algorithme est forcément optimal, c'est à dire utilise toujours aussi peu de pièces que possible.



_Exemple : ancien système monétaire britannique_

Ce système utilisait les montants suivants : $p_0 = 60$, $p_1 = 30$ et $p_2 = 24$, $p_3=12$ , $p_4=6$, $p_5 = 2$ et $p_6=1$

* Avec ces montants, peut-on  payer n'importe quelle somme entière ?

    ```txt
    
    ```

* Comment payer $S=48$ avec le moins de pièces possibles ?

    ```txt
    
    ```

* Adaptez `monnaie.py` afin que cette fonction détermine une façon de payer. Quel est le résultat fourni ? Est-ce optimal ?

    ```txt
    
    ```

    

_Remarques :_ 

Pour éviter ce phénomène, les pays émetteurs des jeux de monnaie utilisent toujours des jeux tels que le résultat de l'algorithme glouton soit toujours optimal. On dit que ces systèmes sont **canoniques**.

  

## Problème du sac à dos



### Enoncé



Vous devez ranger dans un sac des objets de valeurs différentes afin d'emporter avec vous une cargaison aussi précieuse que possible. Hélas, les objets sont plus au moins lourds et le poids maximum que vous pouvez porter dans votre sac sous peine de le déchirer est limité...



Voici, par exemple une liste d'objets à ranger dans un sac pouvant au maximum contenir **20kg**.

| Objets         | TV   | Tableau | bijoux | livres | meubles | Ordinateur |
| -------------- | ---- | ------- | ------ | ------ | ------- | ---------- |
| effectif       | 3    | 5       | 2      | 10     | 2       | 2          |
| valeur         | 500€ | 250€    | 80€    | 10€    | 2500€   | 500€       |
| poids unitaire | 10kg | 4kg     | 0,2kg  | 0.5kg  | 30kg    | 7kg        |



Il s'agit à nouveau d'un algorithme glouton. Quelle optimisation locale pourrait-on choisir pour sélectionner les objets à emporter ?

```txt

```

Compléter le tableau ci-dessous :

| Objets         |  TV  | Tableau | Bijoux | Livres | Meubles | Ordinateur |
| -------------- | :--: | :-----: | :----: | :----: | :-----: | :--------: |
| effectif       |  3   |    5    |   2    |   10   |    2    |     2      |
| valeur         | 500€ |  240€   |  20€   |  10€   |  2500€  |    490€    |
| poids unitaire | 10kg |   4kg   | 0,2kg  | 0.5kg  |  30kg   |    7kg     |
| densité (€/kg) |      |         |        |        |         |            |



### Implémentation



* En vous inspirant du problème de la monnaie, proposez un algorithme permettant de résoudre ce problème en utilisant l'optimisation locale proposée précédemment.

    ```txt
    
    ```

* En Python,

    * Nous modéliserons les objets en utilisant trois dictionnaires :

        ```python
        VALEUR = {  'TV' : 500,
                    'Tableaux' : 240,
                    'Bijoux' : 80,
                    'Livres' : 10,
                    'Meubles' : 2500,
                    'Ordinateurs' : 500
                }
        
        
        POIDS = {   'TV' : 10,
                    'Tableaux' : 4,
                    'Bijoux' : 0.2,
                    'Livres' : 0.5,
                    'Meubles' : 30,
                    'Ordinateurs' : 7
                }
        
        effectif = {'TV' : 3,
                    'Tableaux' : 5,
                    'Bijoux' : 2,
                    'Livres' : 10,
                    'Meubles' : 2,
                    'Ordinateurs' : 2
                    }
        
        ```

    * Définissez et complétez la fonction `lister_candidats`, de paramètre _poids_max_, le poids maximum autorisé qui renvoie la liste des objets qu'il est possible de mettre dans le sac sans dépasser le poids indiqué.

        ```python
        def lister_candidats(poids_max):
            '''
            renvoie  la liste des objets qu'il est possible de mettre dans le sac sans dépasser le poids indiqué.
            : param poids_max le poids à ne pas dépasser
            : return la liste des candidats possibles
            type list
            >>> lister_candidats(1)
            ['Bijoux', 'Livres']
            >>> lister_candidats(9)
            ['Tableaux', 'Bijoux', 'Livres', 'Ordinateurs']
            '''
        ```

    * Définissez et complétez la fonction `selectionner_meilleur_candidat`, de paramètre _candidats_, une liste **non vide** d'objets possibles, qui renvoie le meilleur candidat de la liste selon le critère retenu plus haut.

        ```python
        def selectionner_meilleur_candidat(candidats):
            '''
            renvoie le meilleur candidat de la liste selon le critère de densité maximale.
            : param candidats une liste de candidats possibles
            type liste
            : return le meilleur candidat
            type str
            >>> l = lister_candidats(30)
            >>> selectionner_meilleur_candidat(l)
            'Bijoux'
            '''
        ```

    * Définissez et complétez la fonction `remplir_sac`, de paramètre _poids_max_, le poids maximum autorisé, qui renvoie un dictionnaire représentant le sac rempli selon notre critère.

        ```python
        def remplir_sac(poids_max):
            '''
            renvoie une dictionnaire correspondant au sac rempli avec les objets 
            : param poids_max le poids max autorisé
            type int
            : return un dictionnaire représentant le sac rempli
            type dict    
            '''
            sac = { 'TV' : 0,
                    'Tableaux' : 0,
                    'Bijoux' : 0,
                    'Livres' : 0,
                    'Meubles' : 0,
                    'Ordinateurs' : 0
            }
            liste_candidats = lister_candidats(poids_max)
            while liste_candidats != [] :
        ```
        

        

    

    





____



