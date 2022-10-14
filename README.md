# HuBMAP + HPA - Hacking the Human Body
### Segment multi-organ functional tissue units- My Solution
<img src = "https://user-images.githubusercontent.com/51321172/195871522-cea7bd96-ef9c-4ec0-ae04-5df791f417ac.png" height="250" width="250" align="left">
In this competition, the aim was to identify and segment functional tissue units (FTUs) across five human organs by building models using a dataset of tissue section images, with the best submissions segmenting FTUs as accurately as possible. If successful, it would help accelerate the world’s understanding of the relationships between cell and tissue organization. With a better idea of the relationship of cells, researchers will have more insight into the function of cells that impact human health.<br /><br />
This was my first semantic segmentation competition and it was an amazing learning opportunity, I tried several approaches, trying different combinations of models, data augmentations, data patches and loss functions. My final rank in this competition is 620/ 1175 teams. The competition was evaluated on mean dice coefficient metric. <br /><br />

## Important terminologies:
1) Semantic segmentation: It is a computer vision algorithm that aims to classify each and every pixel of the input image to a particular class by generating a mask.
2) Instance segmentation: Unlike semantic segmentation, instance segmentation aims to segment all the instances that belong to the same class. 
3) Precision: It is described as the ratio of true positives to the sum of true positives and false positives. Precision = T.P. / (T.P. + F.P.)
4) Recall: It is described as the ratio of true positives to the sum of true positives and false negatives. Recall = T.P. / (T.P. + F.N.)
5) Dice coefficient: It is the harmonic mean of precision and recall given by the formula<br />
   {2 * T.P. / (T.P. + F.P. + T.P. + F.N.)}.<br />
   T.P. + F.P. = Predicted mask = Y <br />
   T.P. + F.P. = Ground Truth = X <br />
   Therefore, {T.P. = X ∩ Y} and <br />
   Dice coefficient = (X ∩ Y) / (X + Y)
6) IoU coefficient: Intersection-over-Union(IoU) is given by (X ∩ Y) / (X ∪ Y).
   
<img src="https://user-images.githubusercontent.com/51321172/195859655-7dcd30da-827e-4abd-bf8b-51bb762703ca.png" height="250" width="350" align = "left">
<img src="https://user-images.githubusercontent.com/51321172/195866069-a5c3ccc3-0092-4485-95ce-ed598f1c7c9c.png" height="250" width="350">
