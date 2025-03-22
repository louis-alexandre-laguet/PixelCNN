# PixelCNN

PixelCNN est un modèle autorégressif de génération d'images publié en 2016 par Aäron van den Oord et al.,  
de l'équipe DeepMind chez Google. Il appartient à la famille des modèles autoregressifs, où chaque élément d'une séquence  
est généré en fonction des éléments précédents.

Les images sont générées pixel par pixel en suivant un ordre défini (généralement de gauche à droite et de haut en bas).  
Chaque pixel est conditionné uniquement sur les pixels déjà générés, ce qui impose une contrainte de dépendance directionnelle.  
Pour garantir cette contrainte, PixelCNN utilise des **couches de convolution masquées**, empêchant l’accès aux pixels  
"futurs" lors du calcul des prédictions.  

Deux types de convolutions masquées sont utilisées :  
- **Masque de type A** : Empêche tout accès au pixel en cours de génération, utile dans la première couche pour s'assurer  
  que chaque pixel est généré indépendamment des valeurs actuelles.  
- **Masque de type B** : Autorise l'accès au pixel en cours de génération, utilisé dans les couches suivantes pour permettre  
  un meilleur passage d'information tout en respectant l’ordre de génération. 

À l’issue de cette convolution, le modèle produit une **distribution discrète** sur les valeurs possibles du pixel,  
et sa valeur est ensuite échantillonnée en fonction de cette distribution.

Afin d'améliorer la capacité du modèle à capturer des relations longues portées dans l'image, des **blocs residuels**
ont été introduits ajoutant ainsi des connexions directes entre les couches profondes et peu profondes.  

Pendant l'entraînement, PixelCNN minimise l'**entropie croisée** entre les pixels prédits et les pixels réels.  

Lors de l'inférence, l’image est générée séquentiellement :
1. On initialise une image vide ou bruitée.  
2. Pour chaque pixel, le modèle prédit une distribution de probabilité sur ses valeurs possibles.  
3. Un échantillon est tiré selon cette distribution, et le pixel est fixé.  
4. Le processus est répété jusqu'à ce que l’image complète soit générée.
