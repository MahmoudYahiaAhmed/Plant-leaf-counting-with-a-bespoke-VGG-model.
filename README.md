# Plant-leaf-counting-with-a-bespoke-VGG-model.
![image](https://user-images.githubusercontent.com/109751694/209490416-1fd16fc6-647e-48f2-a987-9a3d321dd311.png)
# OverView
This model counts the leaves in the input photos, and it can count images with 1, 2, 3, 4, or 6 leaves. Here, we will examine the two approaches for configuring this problem as a classification or regression problem.

# Dataset
## Leaf counting

Description: The dataset is plant images at different resolutions captured with a variety of cameras. There are images showing plants with approximatelty 1,2,3,4 and 6 leafs. The images are part of a Leaf counting dataset which can be downloaded from the Aarhus University, Denmark.

Link to download: https://vision.eng.au.dk/leaf-counting-dataset/

Samples from the data:
![image](https://user-images.githubusercontent.com/109751694/209490606-5da2f59e-7e25-4d50-84c7-8241e977cfca.png)


# Methods
Preprocessing
resizing and normalising data to prepare the photos for modelling
I transformed the information to grey scale and attempted to segment only the planet and omit the backdrop because the challenge of counting the number of leaves made it clear that colours were not necessary to distinguish between the classes.
The preprocessed dataset is available for download here. Download

data augmentation
We used augmentation techniques including flipping, minor rotation, shear, random cropping, and zoom to increase the amount of pictures so the model could learn more from the data.

Modeling 
Regression using the use of VGG and a modified final layer with a single linear node.
I also utilised some batch normalisation and regularisation in these approaches.

# Results
## VGG Classification without regularization
![image](https://user-images.githubusercontent.com/109751694/209490861-bd55007a-a72b-49b3-806e-b77e931e7e66.png)

Confusion Matrix for train,test,valid

![image](https://user-images.githubusercontent.com/109751694/209490886-5979f43f-d5ea-4ec7-b7e8-efa13a642768.png)
![image](https://user-images.githubusercontent.com/109751694/209490889-27faa795-9a8f-46c1-9f20-4e4be65069f7.png)
![image](https://user-images.githubusercontent.com/109751694/209490896-5a0bb970-fa74-498e-b5b8-2d823bc60053.png)

The classification model achieved overfit on training set 100% acc, while 40% acc on validation set and 33% acc on testing set.
and as shown in the confusion matrix and the classification report the train set clearly overfitted, valid set all classes f-scores ranges in 30-52% with 40% acc, test set f-scores in ranges 14-50%.
The model performs well on the training data but fail to recognize the right classes on the testing and validation sets so it’s clearly overfitting.
The model needs some improvements as it doing bad on the data.

## VGG Regression
![image](https://user-images.githubusercontent.com/109751694/209490946-1b5c3342-e9f4-4ff2-a4cb-0253ffa1d2c0.png)
Confusion Matrix for train,test,valid
![image](https://user-images.githubusercontent.com/109751694/209490955-e46e21f5-628f-4edc-bec3-c705c9116eba.png)
![image](https://user-images.githubusercontent.com/109751694/209490969-a90183a6-9ec9-41f3-a21c-926d985ddfb6.png)
![image](https://user-images.githubusercontent.com/109751694/209490975-9fd65fd5-24fd-4e6c-b7e2-90022ff3d459.png)

The Regression model MSE scores for the training, testing and validation.
MSE of Training Set 0.275    MSE of Testing Set 1.26     MSE of Validation Set 1.23.

that mean the squared distance between the actual points and the predicted ones is quite nearly on the training while on testing and validation was a little far.
The classification report and the confusion show some interesting results as the model achieve 73% acc and the model was able to classify almost 2 classes with high recall and the others ranges in 58-66 on the training set, testing set f-scores ranges in 15-52% with accuracy 40%, validation set f-scores 24-61% with accuracy 41%.
The model couldn’t able to converge and recorded a bad RMSE on validation and testing sets.
The model acceptable and it could be better with some optimization.
Ex: increasing training epochs or adding some layers.
Choosing between the classification and regression requires more experiments as I could say regression is better but still high RMSE could lead into bad predictions. Classification with a good designed network could achieve much better results.

## VGG Classification After performing some Regularization
![image](https://user-images.githubusercontent.com/109751694/209491175-7c850714-881b-43ac-9f61-61de747bcb81.png)
Confusion Matrix for train,test,valid

![image](https://user-images.githubusercontent.com/109751694/209491184-74d66ada-44e6-4a0a-a10a-57045b1107f9.png)
![image](https://user-images.githubusercontent.com/109751694/209491194-f942ed19-7e22-4fe8-b76e-29d1cf4ee5de.png)
![image](https://user-images.githubusercontent.com/109751694/209491199-7725af09-c1e3-4cd9-a265-914bf9680e54.png)

The model got 100% on the training set and 55% on the validation set.
The confusion matrix and the report show that the model overfitted on the training, while on the testing acc 43%, validation acc 49%. 
The model stucked in the 47-53 ranges during training and didn’t converge any more knowledge to be better. 
As Shown in the confusion matrix and the report for validation data the model scored bad accuracies during the training process and model cannot converge on validation data.
It’s clearly overfitting again.
Regularization improve the valid accuracy and the test accuracy from 33% to 43% and the validation from 40% to 49%
##VGG Classification After performing regularization with data augmentation.

![image](https://user-images.githubusercontent.com/109751694/209491324-1036b5f1-c070-4a3a-8a14-17b8f2524fc7.png)

Confusion Matrix for train,test,valid

![image](https://user-images.githubusercontent.com/109751694/209491329-dc5d0f11-bd37-47df-bbe9-c91ce2134768.png)
![image](https://user-images.githubusercontent.com/109751694/209491333-c9b235be-af5b-4488-b8cd-5c267c2ad20d.png)
![image](https://user-images.githubusercontent.com/109751694/209491338-c8647cf5-54e7-449d-a0f8-25efb5890204.png)
The confusion matrix shows a bad result during the classification a huge overlap in the missed classification classes.
The classification report shows bad accuracies on both train and test 22%, but the valid set accuracy was 42%.
That could be due to the concept drift where we changed the data with some processes.
Ex: zooming, shifting, rotating.
Data Augmentation didn't help in improving the accuracy over the training data as the model wasn't able to learn well as the model accuracy keep change between 2 peaks 16-24 %.

## References
[1] N. Teimouri, M. Dyrmann, P. R. Nielsen, S. K. Mathiassen, G. J. Somerville, and R. N. Jørgensen, “Weed growth stage estimator using deep convolutional neural networks,” Sensors, vol. 18, no. 5, 2018.
