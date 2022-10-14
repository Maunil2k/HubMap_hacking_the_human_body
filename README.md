# HuBMAP + HPA - Hacking the Human Body
### Segment multi-organ functional tissue units- My Solution using tensorflow
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
<img src="https://user-images.githubusercontent.com/51321172/195866069-a5c3ccc3-0092-4485-95ce-ed598f1c7c9c.png" height="250" width="350"><br /><br />

## Approach 1:
Instead of training the models from scratch, all of my approaches work on finetuning models that were pretrained on huge Imagenet dataset. The image sizes in the provided dataset varied from 2230px to 3100px. Due to memory limitations and to avoid data loss, I designed the data pipeline to generate tiles/patches of input image and the mask during the execution of training loop. The pipeline inputs one image and its corresponding mask that were split into 16 patches(hence the batch size was 16) and applied data augmentations. As you could probably guess, there are several downsides to this technique:
1) The training process was extremely time-consuming and resource consuming, since the data pipeline for every new batch would split the image into tiles every single time.
2) This inefficiency leads to a lot of memory consumption, hence, a model with large number of parameters cannot be used for finetuning.
3) The model ended up performing poorly on the test set during submission. Possibly due to the fact that every batch had the patches belonging to the same image.
Due to these reasons, it made no sense in trying different models to improve the public score.<br />

## Approach 2:
In this approach, I decided to focus on improving the efficiency of the data pipeline. Rather than using the patches, I scaled down the images and their corresponding masks(experimented with various sizes i.e. 256p, 384p, 480p, 512p and 768p and adjusting the batch sizes accordingly to avoid OOM error), applied data augmentations and finetuned two models FPN(Feature Pyramid Network) and U-Net trying different backbones for the encoder like resnet34, resnet50, EfficientNetB[0-7]. Interestingly, FPN gave much better results with EfficientNet backbones than UNet with the same backbones(My guess is that UNet probably failed to capture enough features like FPN did. FPN has pyramid type structure in encoder that helps it capture maximum features at different levels). On the similar lines, I experimented with two loss functions; bce_dice_loss and bce_jaccard_loss(IoU loss). The following combination gave the best public score: 
1) Image size: 480p
2) Model: FPN
3) Backbone: EfficientNetB4
4) Batch size: 12
5) Loss: BCE Dice loss<br />

## Approach 3: 
The 2nd approach outperformed its prior one almost in all the aspects, except the only problem with it was huge data loss since I scaled down large images to smaller sizes. To overcome this shortcoming, I created a new dataset out of the already existing one by generating 9 patches in the first attempt and 16 patches of the masks and images in the second. After the dataset was prepared, I used a very similar data pipeline as the one used in 2nd approach. Once again I tried several backbones, experimented different image size, batch sizes, etc. Unlike the first approach, the training loop took much lesser time and the memory management was much efficient. But unfortunately, none of them ever exceeded my previous public score.

## Final word:
These methods are definitely not exhaustive. Infact, there are models that would give better results than FPN or UNet, for instance, SegFormer model. Find the solution of the third placeholder [here](https://www.kaggle.com/competitions/hubmap-organ-segmentation/discussion/354683) and [GitHub repo](https://github.com/VSydorskyy/hubmap_2022_htt_solution) for the same. 

## References:
1) Competition: [HuBMAP + HPA - Hacking the Human Body](https://www.kaggle.com/competitions/hubmap-organ-segmentation/overview)
2) Segmentation models: [Documentation](https://segmentation-models.readthedocs.io/en/latest/tutorial.html) 
