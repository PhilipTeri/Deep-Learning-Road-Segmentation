# Road-Segmentation



## intro 


## data source 
-link image source
- talk about how I can make my own data

## pre processing

-original images
- cropping images


## modelling

### Network Architecture

#### U-net
![image](https://user-images.githubusercontent.com/41071502/126908484-b9609c33-9f56-4f72-ab10-97cb0a311151.png)


### Callbacks
#### Early stopping

The early stopping callback monitors the val_loss of the training set, a trigger can be set to stopp the model when a certain number of epochs have passed without improvement in the val_loss. This ensures that the model is not being overfit. Below is a gif showing the model outputs without the early stopping callback and the loss graph to show the deviation in loss for the training and test samples. 

    ![first_gif_image_id_10](https://user-images.githubusercontent.com/41071502/126907247-6dd71604-4d22-4de3-8bbf-78b8ecb79959.gif)

![loss graph](https://user-images.githubusercontent.com/41071502/126908359-d1cd6bc6-5b16-4d69-87d4-575e46373026.png)

#### Check point
The Checkpoint callback is

Epoch 100
![image](https://user-images.githubusercontent.com/41071502/126908609-ca79dde2-152b-4eff-9f74-3b81d13fc7cb.png)

Best Score
![image](https://user-images.githubusercontent.com/41071502/126908625-ab5236a7-a7d1-413d-8a94-428788392fb8.png)


Custom Callback 




## Results



## Future changes
- data augmentation
- bigger batch size, more ram

## References

Biomedical Example - 
https://www.kaggle.com/vbookshelf/simple-cell-segmentation-with-keras-and-u-net

U-Net architecture Example -
https://www.kaggle.com/keegil/keras-u-net-starter-lb-0-277
