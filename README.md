# CASA0018 Project: Playing Rock paper scissors with Computers

## Aim 

The idea of this project is to create a simple program on Arduino IDE to complete tasks including recognizing which gesture the user gives; generate a choice randomly; And then compare them and use LED light as an output to tell the user whether they win the computer or not.


The aim of this project is to build a model that can reach an accuray upto 80% in recognising user's gesture.

## Data source

The data for the model training was collected by using Edge Impulse:


Dataset size: 423 training/74 testing (85%/15%) 


Paper: 121 training/21 testing


Rock: 152 training/27 testing


Scissors: 150 training/26 testing 


## Model used

Transfer learning is used for this project. The convolutional neural network MobileNetV1 is used.


The detailed of model building can be found here: https://studio.edgeimpulse.com/studio/196617 

## Presentation
The presentation vedio for this project can be found here: https://youtu.be/Ud4RSV2X0dc 

## File in this repository
IMG_0460.MOV contains a short demo of the program built for this projrct.

File 0018_project_mobileV1_trainable.ino is the file that can run the model in Arduino IDE.
