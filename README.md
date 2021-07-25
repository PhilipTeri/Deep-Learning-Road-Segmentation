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

The early stopping callback monitors the val_loss of the training set, a trigger can be set to stop the model when a certain number of epochs have passed without improvement in the val_loss. This stops the model when it begins to overfit the dataset. Below is a gif showing the model outputs without the early stopping callback and the loss graph to show the deviation in loss for the training and test samples. 

  ![first_gif_image_id_10](https://user-images.githubusercontent.com/41071502/126907247-6dd71604-4d22-4de3-8bbf-78b8ecb79959.gif)

![loss graph](https://user-images.githubusercontent.com/41071502/126908359-d1cd6bc6-5b16-4d69-87d4-575e46373026.png)

#### Check point
The Checkpoint callback is required in order to save the best model. The model does not save the last epoch is saves the epoch with the best value. To confirm this I compared the results of the last epoch with the results after applying the model to the test set. The 100th epoch is overfit and missing a large portion of the road while the best results output looks more similar to the true mask. 

##### Epoch 100

![image](https://user-images.githubusercontent.com/41071502/126908609-ca79dde2-152b-4eff-9f74-3b81d13fc7cb.png)

##### Best Score

![image](https://user-images.githubusercontent.com/41071502/126908786-3da61d75-c938-43d5-a413-09037e70260c.png)


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
