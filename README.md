# [AIDAO 2024](https://education.yandex.ru/aidao)
[Solution](./AIDAO.ipynb) to the selection round case. The task was to develop an algorithm capable of determining whether two given fMRI time-series samples belong to the same individual or not. To see 3D graphics, it's important to run the solution in tools that support [plotly](https://github.com/plotly/plotly.py). 

MORE DETAILED DESCRIPTION:

1.	We read this [paper](https://onlinelibrary.wiley.com/doi/10.1002/hbm.26561) which was attached in references, from there we took the idea of using the correlation matrix by time stamps
   
2.	We ran the algorithm K-means separately for the atlas Brainnetome 246 and for the atlas Schaefer 200. We also added PCA before K-means. At the end, we simply combined it independently into the final labelling. PCA was used to reduce the dimensionality of the data in each of the atlases, and then different components were visualized on a 2D plane in order to find similar patterns of data distribution for the further possibility of correctly matching class labels between themselves from two different atlases

3.	Since the main hypothesis was that clustering within each of the atlases works well and it is only necessary to correctly map the labels

4.	The main problem was the mapping between two atlases. We tried a lot of steps to map them into one object with same dimension. Autoencoder, LSTM, PCA… But all these methods did not outperform the score of the previously described solution in the second point. So the main problem was mapping! We found the description of Brainnetome 246 in this [article](https://www.researchgate.net/publication/303558797_The_Human_Brainnetome_Atlas_A_New_Brain_Atlas_Based_on_Connectional_Architecture) and [description](http://braph.org/download/schaefer200-atlas-txt/?wpdmdl=147989&refresh=670a4c0265cd91728728066) of the Schaefer 200. We understood that every index of two atlases was at a specific 3D coordinate. So for every point from Schaefer 200 we found the nearest point from Brainnetome according to the coordinates. So we found the right mapping (see in the notebook). We thought that was a victory because we had all objects in the same dimension, but score did not outperform too, we were very upset.

5.	Similar to the following [article](https://arxiv.org/pdf/2006.09928), the Sparse Dictionary Learning method was used to identify patterns common to all people and to isolate purely individual characteristics

