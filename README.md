# MRI-Image-Analysis
Le projet à pour but de détecté des tumeurs cancéreuse sur des images IRM puis d'en révéler les contours via un masque  

# Répertoire

## Repository Files:
```
├── REAMDE.md
├── Code.ipynb
├── ResUnet-archit.json
├── reUsnet-weights.hdf5
├── classifier-resnet-weights.hdf5
├── classifier-resnet-archt.json
├── classifier-resnet-weights.hdf5
├── results.png
└── Brain_MRI
    ├── Imgs
    ├── Mask
    └── Csv
```

# Architecture

- Modèle de Détéction (Classification) :
 - Le modèle de base pour la détection est le modèle ResNet réemployé par Transfert Learning sur le jeu de donné 
![ResNet50-Archit](https://i.stack.imgur.com/gI4zT.png)

  - On a conserver que 2 sorties (la valeur de sortie est "oui" ou "Non"

- Modèle de segmentation
  - Le but de la segmentation est de catégoriser chaque pixel d'une image, la classification se fait donc au pixel près ("Dense Prediction")
  - ![Segmentation](https://miro.medium.com/max/700/1*nXlx7s4wQhVgVId8qkkMMA.png)
  - Le modèle de segmentation est une modèle type Unet (https://arxiv.org/abs/1505.04597).
  - Il se présente sous a forme d'une Full CNN-AE, c-à-d d'un réseau Autoencoeur dont les couches sont des couches Convolutionnelle (+ Pooling) et comme il n'y a aucune Couche Dense, il est Full Cnvolutional (FCN) ce qui permet de s'adapter a tout type de taille d'image.
  - ![Unet](https://miro.medium.com/max/700/1*OkUrpDD6I0FpugA_bbYBJQ.png)  
  - * Notre version est plus particulièrement une version ResUnet ( il y a des passerels entres la partie Encodeur et Décodeur)
  - ![img](https://upload.wikimedia.org/wikipedia/commons/2/2b/Example_architecture_of_U-Net_for_producing_k_256-by-256_image_masks_for_a_256-by-256_RGB_image.png)

# Mask ?

- Le but de la << segmentation d'images >> est de Comprendre l'image au niveau du pixel, et d'associer CHAQUE pixel à une classe. La sortie produite par le modèle de segmentation d'image se nomme "mask"

- Le mask est une version de l'image d'entrer soulignant en pixel blanc le point d'interet (255 s'il existe) et en noir tout le reste (=0)

  -> On pourra flatten la matrice representant le mask et norm les valeurs (Normaliser ces valeurs 255/0 --> 1/0 est un +)

  ![img](https://www.researchgate.net/profile/Svetlana-Yanushkevich/publication/343096300/figure/fig2/AS:915575563890689@1595301633452/Segmentation-masks-are-converted-to-bounding-box-masks-by-fitting-the-smallest-possible.png)
  
# Dataset

- Les données proviennent d'un défi Kaggle de @2019 de https://www.kaggle.com/mateuszbuda/lgg-mri-segmentation
- Le jeu de donnée consiste en des images IRM accompagné de leur mask en pixel : Délimitant une tumeur quand elle est présente / pas de mask si pas de tumeur
- Le csv est de la forme : patient_id	| image_path |	mask_path |	mask

# Résultats

- A des fins démonstration et de rappel, j'ai concervé dans le notebook les différente version d'entrainement des modèles / Voir : Modèle Performance et Pred & Perfs

<table>
  <tr>
    <th> Précision Classification </th>
    <th> 89 % </th>
  </tr>
</table>

*On peut faire mieux avec un peu plus d'entrainement

![Résultats](https://github.com/Soren-Kierkegaard/MRI-Image-Analysis/blob/main/results.png?raw=true)

# Acknowlege

- Originly coded in July 2018
- Recoded in March 2020
