# Deep Learning Road Segmentation 

![combined](https://user-images.githubusercontent.com/41071502/126913371-bc3b2a3b-90bf-48bf-b4e9-e082cfd46e76.PNG)

The purpose of this project is to detect roads from sattelite imagery. To do this semantic segmentation is used, which in this case predicts the classification of each pixel in a satellite image as a road or not a road. This can be applied to any type of land classification. Features can be automatically detected and mapped using this type of deep learning model.

## Data 

[Data Set](https://www.kaggle.com/insaff/massachusetts-roads-dataset)

I used the Massachusetts Roads Dataset to train the model and below is an example inputs on the left and the resulting mask layer on the right. The dataset contained around 800 classified mask images which corresponded to a satellite image with the same image id. The images in this dataset are 1500 x 1500 pixels.


## Network Architecture

#### U-net

<img src="https://user-images.githubusercontent.com/41071502/126908484-b9609c33-9f56-4f72-ab10-97cb0a311151.png" width="550" height="350">

U-net is a convolutional neural network that was designed originally for biomedical image segmentation. However, I learned that it worked well with sattelite image segmentation.
Two types of images are required to run the U-net architecture. One is the original satteltite imagery and the second is a classified image (mask). This mask layer can contain multiple classification for different land uses, but in this there are only 2 classifications, roads and everything else that is not a road. 

#### Contraction

#### Expansion

## Data Cleaning

The original dataset was 1500 x 1500 pixel images. I needed to make the inputs 128 x 128 to run the model. Originally I took the inputs and resized them on the fly, however, this took a lot of time, so I took multiple 128 x 128 cropped sections of the original image. Some of the original images had white spaces around the edges, so I deleted all of these. The issue now was the there were mask layers that did not have a matching pair, so I made a new dataframe and checked that each input image had a corresponding mask image. 

![image](https://user-images.githubusercontent.com/41071502/126913653-e0855b6f-1a50-4c71-a2cb-084e213b1f93.png)


## Modelling

I used 1350 training images and 150 images for validation to run the model. The batch size I used is 275, this was limited by the ammount of ram I have. The prediction threshold is set to 50%, meaning that the model will only classify a pixel as a road if it is over 50% confident that the pixel value represents a road. 

### Optimizer - adam
### los - binary cross entropy

### Callbacks
#### Early stopping

The early stopping callback monitors the val_loss of the training set, a trigger can be set to stop the model when a certain number of epochs have passed without improvement in the val_loss. This stops the model when it begins to overfit the dataset. Below is a gif showing the model outputs without the early stopping callback and the loss graph to show the deviation in loss for the training and test samples. 

![first_gif_image_id_10](https://user-images.githubusercontent.com/41071502/126907247-6dd71604-4d22-4de3-8bbf-78b8ecb79959.gif)


![loss graph](https://user-images.githubusercontent.com/41071502/126908359-d1cd6bc6-5b16-4d69-87d4-575e46373026.png)

#### Check point
The Checkpoint callback is required in order to save the best model. The model does not save the last epoch is saves the epoch with the best value. To confirm this I compared the results of the last epoch with the results after applying the model to the test set. The 100th epoch is overfit and missing a large portion of the road while the best results output looks more similar to the true mask. 

##### Final Epoch

![image](https://user-images.githubusercontent.com/41071502/126908609-ca79dde2-152b-4eff-9f74-3b81d13fc7cb.png)

##### Best Scoring Epoch

![image](https://user-images.githubusercontent.com/41071502/126908786-3da61d75-c938-43d5-a413-09037e70260c.png)


#### Custom Callback 

I made a custom callback using the on_epoch_end function in tf.keras.callbacks. At the end of each epoch the results of the model are displayed using a test image, this allowed me to monitor the model's performance visually while it was still running. This callback also saves an image of the results after each epoch, which I then used to create a gif of the models progress.


## Results

I set aside 30 images that were not used in the model, so I could see how well the model performed on images it has never seen before. The model performed very well some images and very poorly on others, below are a few examples of both. The results will likely improve by adding more training images. 

### Good Results

<img src="https://user-images.githubusercontent.com/41071502/126912791-98ac45aa-ed58-44e2-abc7-f1efd4529ac4.png" width="430" height="150">
<img src="https://user-images.githubusercontent.com/41071502/126912793-d3d33305-97dc-4edd-88cd-87a081d3bef7.png" width="430" height="150">
<img src="https://user-images.githubusercontent.com/41071502/126912795-124423d3-eeb5-42dd-8fd9-53756878e1e6.png" width="430" height="150">

### Bad Results 
<img src="https://user-images.githubusercontent.com/41071502/126912803-02ed8c85-def3-4f39-bc74-092f8f746e99.png" width="430" height="150">
<img src="https://user-images.githubusercontent.com/41071502/126912808-dd9dfb23-30db-42d0-b1e3-3643f7680bc5.png" width="430" height="150">
<img src="https://user-images.githubusercontent.com/41071502/126912817-26dc308b-c4d9-4e3c-9758-a3ce7f8858db.png" width="430" height="150">

## Future Improvements

One major change that will improve the model is increasing the number of images in the data set. I originally cropped the 128 x 128 images from a 1500 x 1500 images, but another way to quickly increase the number of images is using image augmentation. Simple image augmenatations such as rotating or flipping an image can create multiple new images from one initial image. Tensorflow does this on the fly, so it is not necessary to save the new images. However, this will increase processing time when running the model. 

Another improvement to the model will be increasing the batch size, I was limited to around 250 for my batch size because of ram limitations.

Lastly I want to run this model on a larger image to see the results for a larger region. Since the model is limited to a size of 128 pixels I plan on using patchify to segment the larger image into 128 x 128 images and then using unpatchify to stitch them back together.

## References

#### Massachusetts Roads Dataset - 
https://www.kaggle.com/insaff/massachusetts-roads-dataset

#### Biomedical Image Segmentation Example - 
https://www.kaggle.com/vbookshelf/simple-cell-segmentation-with-keras-and-u-net

#### U-Net Architecture Example -
https://www.kaggle.com/keegil/keras-u-net-starter-lb-0-277


