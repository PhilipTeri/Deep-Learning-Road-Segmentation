# Road-Segmentation

The purpose of this project is to detect roads from sattelite imagery.  To do this I used semantic segmentation, which in this case predicts the classification of each pixel in an image as a road or not a road. Two types of images are required to do this. One is the original satteltite imagery and the second is a classified image (mask). This mask layer can contain multiple classification for different land uses, but in this there are only 2 classifications, roads and everything else that is not a road. I used the Massachusetts Roads Dataset to train the model and below is an example inputs on the left and the resulting mask layer on the right.


## Pre Processing

The model was set up to run images that 

### Network Architecture

#### U-net

<img src="https://user-images.githubusercontent.com/41071502/126908484-b9609c33-9f56-4f72-ab10-97cb0a311151.png" width="500" height="350">



### Callbacks
#### Early stopping

The early stopping callback monitors the val_loss of the training set, a trigger can be set to stop the model when a certain number of epochs have passed without improvement in the val_loss. This stops the model when it begins to overfit the dataset. Below is a gif showing the model outputs without the early stopping callback and the loss graph to show the deviation in loss for the training and test samples. 

![first_gif_image_id_10](https://user-images.githubusercontent.com/41071502/126907247-6dd71604-4d22-4de3-8bbf-78b8ecb79959.gif)


![loss graph](https://user-images.githubusercontent.com/41071502/126908359-d1cd6bc6-5b16-4d69-87d4-575e46373026.png)

#### Check point
The Checkpoint callback is required in order to save the best model. The model does not save the last epoch is saves the epoch with the best value. To confirm this I compared the results of the last epoch with the results after applying the model to the test set. The 100th epoch is overfit and missing a large portion of the road while the best results output looks more similar to the true mask. 

##### Final Output (Epoch 100)

![image](https://user-images.githubusercontent.com/41071502/126908609-ca79dde2-152b-4eff-9f74-3b81d13fc7cb.png)

##### Best Score (Epoch 88)

![image](https://user-images.githubusercontent.com/41071502/126908786-3da61d75-c938-43d5-a413-09037e70260c.png)


#### Custom Callback 

I made a custom callback using the on_epoch_end function tf.keras.callbacks.Callback. At the end of each epoch the results of the model are displayed, this allowed me to monitor the models performance visually while it was running. This callback also saves an image of the results after each epoch, which I then used to create a gif of the models progress.


## Results

## Future Changes

One major change that will improve the model is increasing the number of images in the data set. I originally cropped the 128 x 128 images from a 1500 x 1500 images, but another way to quickly increase the number of images is using image augmentation. Simple image augmenatations such as rotating or flipping an image can create multiple new images from one initial image. Tensorflow does this on the fly, so it is not necessary to save the new images. However, this will increase processing time when running the model. 

Another improvement to the model will be increasing the batch size, I was limited to around 250 for my batch size because of ram limitations.

Lastly I want to run this model on a larger image to see the results for a larger region. Since the model is limited to a size of 128 pixels I plan on using patchify to segment the larger image into 128 x 128 images and then using unpatchify to stitch them back together.

## References

#### Massachusetts Roads Dataset - https://www.kaggle.com/insaff/massachusetts-roads-dataset

#### Biomedical Image Segmentation Example - 
https://www.kaggle.com/vbookshelf/simple-cell-segmentation-with-keras-and-u-net

#### U-Net Architecture Example -
https://www.kaggle.com/keegil/keras-u-net-starter-lb-0-277


