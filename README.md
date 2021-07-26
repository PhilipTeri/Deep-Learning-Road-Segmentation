# Deep Learning Road Segmentation 

![combined](https://user-images.githubusercontent.com/41071502/126913371-bc3b2a3b-90bf-48bf-b4e9-e082cfd46e76.PNG)

The purpose of this project is to detect roads from satellite imagery. To do this I used semantic segmentation, which in this case predicts the classification of each pixel in a satellite image as a road or not a road. This can be applied to any type of land classification. Features can be automatically detected and mapped using this type of deep learning model.

## Data 

[Massachusetts Roads](https://www.kaggle.com/insaff/massachusetts-roads-dataset)

I used the Massachusetts Roads Dataset to train the model. The dataset contains around 800 classified mask images which corresponds to a matching satellite image with the same Id.


## Network Architecture

#### U-net

<img src="https://user-images.githubusercontent.com/41071502/126908484-b9609c33-9f56-4f72-ab10-97cb0a311151.png" width="550" height="300">

U-net is a convolutional neural network that was designed originally for biomedical image segmentation. However, I learned that it worked well with satellite image segmentation as well.
Two types of images are required to run the U-net architecture. One is the original satellite imagery and the second is a classified image (mask). The mask layer can contain multiple classification for different land uses, but in this project there are only 2 classifications, roads and everything else that is not a road. 

## Data Cleaning

The original image size of the dataset is 1500 x 1500-pixels. Originally, I took the inputs and resized them on the fly, however, this added a lot of processing time. As an alternative to resizing, I cropped 128 x 128 images from the original images and masks. Some of the original images had white spaces around the edges, so I removed all of these. This caused a new issue because now some of the masks would not have a matching pair. I made a new data frame to fix this and checked that each input image had a corresponding mask image. 

![image](https://user-images.githubusercontent.com/41071502/126913653-e0855b6f-1a50-4c71-a2cb-084e213b1f93.png)


## Modelling

I used 1350 training images and 150 images for validation to run the model. The batch size I used is 275, this is limited by the amount of ram I have. The prediction threshold is set to 50%, meaning that the model will only classify a pixel as a road if it is over 50% confident that the pixel value represents a road.  The model uses the pair of satellite images and classified masks to make a predicted mask layer.

Optimizer - Adam

Loss - binary_crossentropy 

Metrics - Accuracy

Activation Function - ReLU (Rectified Linear Unit)

#### Early stopping Callback

The early stopping callback monitors the val_loss of the training set.  A trigger can be set to stop the model when a certain number of epochs have passed without improvement in the val_loss. This stops the model when it begins to overfit the dataset. Below is a gif showing the progression of the model outputs. Without the early stopping callback, you can see that the model begins to overfit. The deviation in loss between the training and test samples also shows that the model is overfitting the longer it runs.





![ezgif com-gif-maker](https://user-images.githubusercontent.com/41071502/127062829-32969ceb-f883-453b-b648-b29cf88814cf.gif)
<img src="https://user-images.githubusercontent.com/41071502/127063835-e40f4026-b2a5-4a4c-b2b7-873f36d43c0e.gif" width="150" height="150">


#### Check point Callback
The Checkpoint callback is required in order to save the best model. The model does not save the last epoch it saves the epoch with the best value. To confirm this, I compared the results of the last epoch with the results after applying the model to the test set. The 100th epoch is overfit and missing a large portion of the road while the best result looks more like the true mask.

##### Final Epoch

![image](https://user-images.githubusercontent.com/41071502/126908609-ca79dde2-152b-4eff-9f74-3b81d13fc7cb.png)

##### Best Scoring Epoch

![image](https://user-images.githubusercontent.com/41071502/126908786-3da61d75-c938-43d5-a413-09037e70260c.png)


#### Custom Callback 

I made a custom callback using the on_epoch_end function in tf.keras.callbacks. At the end of each epoch the results of the model are displayed using a test image, this allows me to monitor the model's performance visually while it is still running. This callback also saves an image of the results after each epoch, this is useful to see the models progression over time.


## Results

I set aside 30 images that were not used in the model, so I could see how well the model performed on images it has never seen before. The model performed very well some images and very poorly on others, below are a few examples of both. The results will likely improve by adding more training images. 

#### Good Results

<img src="https://user-images.githubusercontent.com/41071502/126912791-98ac45aa-ed58-44e2-abc7-f1efd4529ac4.png" width="430" height="150">
<img src="https://user-images.githubusercontent.com/41071502/126912793-d3d33305-97dc-4edd-88cd-87a081d3bef7.png" width="430" height="150">
<img src="https://user-images.githubusercontent.com/41071502/126912795-124423d3-eeb5-42dd-8fd9-53756878e1e6.png" width="430" height="150">

#### Poor Results 
<img src="https://user-images.githubusercontent.com/41071502/126912803-02ed8c85-def3-4f39-bc74-092f8f746e99.png" width="430" height="150">
<img src="https://user-images.githubusercontent.com/41071502/126912808-dd9dfb23-30db-42d0-b1e3-3643f7680bc5.png" width="430" height="150">
<img src="https://user-images.githubusercontent.com/41071502/126912817-26dc308b-c4d9-4e3c-9758-a3ce7f8858db.png" width="430" height="150">

## Future Improvements

One major improvement for the future the is increasing the number of images in the data set. One way to quickly increase the number of images is using image augmentation. Simple image augmentations such as rotating or flipping an image can create multiple new images from one initial image. TensorFlow does this on the fly, so it is not necessary to save the new images. However, this will increase processing time.

Another improvement to the model will be increasing the batch size, I was limited to around 250 for my batch size because of ram limitations.  I need to either increase the amount of ram I have or use distributed training and run the model on multiple computers at the same time.

Lastly, I want to run this model on a larger image to see the results for a larger region. Since the model is limited to a size of 128 pixels, I plan on using patchify to segment the larger image into 128 x 128 images and then using unpatchify to stitch them back together.

## References

#### Massachusetts Roads Dataset - 
https://www.kaggle.com/insaff/massachusetts-roads-dataset

#### Simple Cell Segmentation with Keras and U-Net- 
https://www.kaggle.com/vbookshelf/simple-cell-segmentation-with-keras-and-u-net

#### Keras U-Net starter -
https://www.kaggle.com/keegil/keras-u-net-starter-lb-0-277



