# **Playing Rock, Paper, and Scissors with Computer**

<img width="404" alt="image" src="https://user-images.githubusercontent.com/109146037/234909253-a0ef6d78-196b-4b5d-be18-ad688d0e2cc0.png">

CEGE 0018 Coursework Project

Author: Yiqi Huang

GitHub Link: https://github.com/sophuang/CASA0018CourseworkProject

Edge Impulse Link: https://studio.edgeimpulse.com/public/196617/latest

IMG_0460.MOV contains a short demo of the program built for this project.

File ei-gesture_recognition-arduino-1.0.20.zip is the initial model deployment

File 0018_project_trainable_updated.ino is the file that can run the program in Arduino IDE.

File gesture_recognition-export.zip is the exported dataset

File 0018_Report.pdf ia the final pdf version of the report

**1. Introduction**

**1.1 Background of Gesture Recognition**

Gesture recognition is a significant field of computer vision technique since it is a way of human communicating with computers and machines. Most important and vital applications of gesture recognition nowadays include giving commands or talking to computers without using mouse to click, interacting with medical instruments during medical operation and surgery, controlling vehicles functions during driving, controlling smart home devices and etc. All these applications allow the use to control the device or system without touching a button or screen physically, which provide the convenience, safety and accuracy when the user need a multitasking process.

Another popular appliable aspect of gesture recognition is the controlling in gesture-based games. Computer games are a particularly technologically promising and commercially rewarding arena for innovative interfaces due to the entertaining nature of the interaction. (Admin, 2022) This provides more interactive, immersive, and fast-response gaming experiences and interfaces for the user. And this is also the field this study will be mainly focused on.

**1.2 Research Question**

The inspiration of this project is a Switch game called 'How to win paper rock and scissors in Dr. Kawashima's Brain Training'. In this game, the infrared (IR) sensors are used to detect user's gesture, which have advantages in clearly detecting how many fingers the user is holding up and whether the user is holding a fist or not. (Jones, n.d.)

To have a better understanding about computer vision, object detection and classification lectures taught in this module, this study aims to create a simple gaming program in Arduino IDE that is deployed on the microcontroller, Arduino Nano BLE33 to complete the following tasks:

- Recognize which of the gestures, Rock, Paper and Scissors the user gives, using the built-in camera on Arduino Nano BLE33.
- Output a randomly generated gesture choice by the computer program.
- Compare the recognized and generated choices to determine whether the user wins or loses. Also use the built-in LED light as an output signal of winning and losing status.
- The program can recognize the user's gesture up to 80%

**1.3 Methodology and Application Overview**

This study primarily utilizes Edge Impulse, a comprehensive platform that seamlessly facilitates data collection, labeling, train-test set splitting, model building, testing, and program deployment in a consistent and streamlined manner.

In more details, the transfer learning technique will be employed in the model building progress. This is a deep learning approach that enables the use of pre-trained models as a foundation for training customized models with a limited dataset to accomplish new objectives. This method not only facilitates a more efficient training process but also often results in improved performance. (Brownlee, 2019)

Multiple pre-trained base models are available for object detection, with popular and powerful options such as Faster R-CNN, YOLO, and MobileNet. However, when deploying a program on a microcontroller, it is essential to consider factors such as model size and computational complexity to ensure optimal performance on resource-constrained devices. (Admin, 2023) (Shinya, et al., 2019)

MobileNet is a family of lightweight neural network architectures designed specifically for mobile and embedded vision applications. It uses depthwise separable convolutions, which greatly reduce the number of parameters and computations compared to traditional convolutional layers. This makes MobileNet a suitable choice for running on resource-constrained devices while still delivering reasonable accuracy. (Howard, 1027)

**2. Data**

**2.1 Data Collection**

In this study, Edge Impulse was utilized to collect, label, and split the dataset, with images captured using the OV7675 camera module provided in the Arduino Nano 33 BLE KIT. The camera module offers a resolution of 640 x 480 VGA. The final dataset consists of 525 images, divided into 450 training and 75 testing samples, following an 85%:15% split. The dataset comprises four labels:

• Rock: 165 training / 25 testing

• Paper: 120 training / 25 testing

• Scissors: 165 training / 25 testing

Initially, images of gestures such as thumbs-up or the "OK" sign were collected under a separate label called "Other." However, after several experimental observations, it was determined that including these additional gestures adversely affected the model's performance in classifying rock, paper, and scissors, resulting in decreased accuracy. Given that the project's primary objective is to enable users to play rock, paper, and scissors with a computer, users are not expected to use other gestures during gameplay. As a result, all data in the "Other" label was excluded from the analysis.

As depicted in the figures above, the dataset sizes for the different labels vary. The experimental results in a later section of this study demonstrate that the model has lower accuracy in distinguishing between "Scissors" and "Rock" gestures. Consequently, the dataset sizes for these two classes are slightly larger than that of the "Paper" class. This adjustment allows the model to better learn the specific features of the "Scissors" and "Rock" classes, ultimately improving its overall performance.

**2.2 Dataset Description**

- **Control Factor**

During the data collection process, three primary factors were considered to ensure the robustness of the dataset. The first one is the background variability. To mitigate the impact of varying backgrounds on the sampled images, data was collected across different locations, featuring diverse backgrounds, and at different times of day, encompassing a range of daylight conditions. This approach helps the model generalize better across varied environments.

The second one is hand orientation and position. To account for the differences between left and right hands, as well as the front (palm) and back of hands, the dataset was carefully curated to include samples covering these variations. This consideration ensures the model can accurately detect the target gestures, irrespective of hand orientation and position.

The last one is gesture variations across individuals. Different people may exhibit distinct gesture habits when forming rock, paper, or scissors signs. To address this issue, images were collected with the assistance of friends, thereby introducing a variety of individual styles into the dataset. This diverse data helps the model become more adept at recognizing the gestures, regardless of individual variations. Here are the examples of images input, showing the data in different background, daylight conditions and by different people.

| <img width="125" alt="image" src="https://user-images.githubusercontent.com/109146037/234909355-5d244244-7e37-47d6-9fce-80cc21b88a98.png"> | <img width="119" alt="image" src="https://user-images.githubusercontent.com/109146037/234909383-29daf34f-cca9-4924-be1f-6790ae257a4d.png"> |
| --- | --- |
| Fig.1 Sample of "paper" facing in back | Fig.2 Sample of "paper" facing in front by my friend |
| <img width="125" alt="image" src="https://user-images.githubusercontent.com/109146037/234909459-95137663-49b8-4787-b1e6-3913cc404049.png"> | <img width="128" alt="image" src="https://user-images.githubusercontent.com/109146037/234909494-f972ff28-fb87-4db8-bb9f-b118482e454f.png"> |
| Fig.3 Sample of "scissors" facing in front | Fig.4 Sample of "scissors" facing in back |
| <img width="117" alt="image" src="https://user-images.githubusercontent.com/109146037/234909533-ffafb191-a6de-4d34-8213-e5e32549653c.png"> | <img width="117" alt="image" src="https://user-images.githubusercontent.com/109146037/234909566-f6482723-6a14-40fd-993a-d4c63e7e3e61.png"> |
| Fig.5 Sample of "rock" facing in front | Fig.6 Sample of "rock" by my friend |

**2.3 Features Generation**

The feature generation process began by setting the image size to 96x96 pixels, using the "squash" resize mode and "Transfer Learning" as the learning box.

<img width="446" alt="image" src="https://user-images.githubusercontent.com/109146037/234909609-7b95a5b7-0cad-49b9-8118-654a7c7898f3.png">

Fig.7 Impulse Design

The "squash" resize mode was chosen to ensure the model retains as much information as possible from the original images. Additionally, the color depth was set to RGB to provide the model with a richer representation of the input data. Upon completion, the generated features were visualized as shown below.

As depicted in the Feature Explorer, the features contained within each label are not easily distinguishable or separable. The sample points are clustered into three prominent groups, with each group containing more than one label. Notably, the groups at the bottom left and bottom right primarily consist of rock and scissors labels. This observation indicates that the rock and scissors labels share similar features, which also aligns with the experimental results presented later in this study. Therefore, we can infer that differentiating between rock and scissor poses a significant challenge for this project.

<img width="260" alt="image" src="https://user-images.githubusercontent.com/109146037/234909647-c275883e-d003-4324-8453-90c17477c6ee.png">

Fig.8 Feature explorer

**3. Model Building and Training**

**3.1 Initialization**

In this section, we focus on the process of building and training the machine learning model using the prepared dataset and feature extraction techniques. To initialize the first model, the convolutional neural network MobileNetV2 is employed, with 20 epochs, a 0.005 learning rate, and a 20% validation set. To minimize the overfitting issue, an auto-balanced dataset and data augmentation techniques are also implemented.

The trained model achieves a 91.8% accuracy in the validation set and 86.49% in the testing set, as illustrated in the figures below. This performancedemonstrates that the model isrelatively robust.

However, it is important to note that the PEAK RAM USE is 346.9KB. Even when using the EON compiler, the RAM usage still surpasses the maximum allowable size of 256KB on the Arduino Nano board. Consequently, the EON tuner is employed to experiment with various parameters in order to obtain an optimized model that fits within the 256KB RAM constraint.

<img width="225" alt="image" src="https://user-images.githubusercontent.com/109146037/234909703-cab98d51-2273-4109-86f1-70bd928df66d.png"> <img width="217" alt="image" src="https://user-images.githubusercontent.com/109146037/234909722-6b5e7603-6fbd-4ed6-b5c8-bccaf5b1398c.png">

Fig.9 Validation accuracy and Testing accuracy

**3.2 Experiment**

- **EON tuner**

Utilizing the EON tuner, various models with different parameters were experimented with, as illustrated in the following table:

![image](https://user-images.githubusercontent.com/109146037/234909978-e1c29337-ca1b-4c84-a649-16d6b867022f.png)

As indicated in the table, models employing classification learning type (non-pretrained models, standard convolutional neural networks with three 2D convolutional layers and one dropout layer) exhibit strong performance on the validation set but show a significant decline in performance on the unseen testing set.

In pursuit of the best performance on unseen data, the transfer learning approach using MobileNetV1 (Model No. 5) was chosen. This model features a 96x96 image input size, RGB color depth, 0.005 learning rate, and 50 epochs. These results align with the assumptions and discussions presented in sections 1.3 and 2.3.

To elaborate further, a learning rate of 0.005 strikes an optimal balance between faster convergence and preventing the overshooting of optimal weights during the training process, as compared to learning rates of 0.001 and 0.0005. A higher learning rate might cause the model to overshoot the optimal weights, resulting in unstable training, while a lower learning rate could prolong convergence and potentially cause the model to become stuck in local minima. Incorporating RGB color depth enables the model to distinguish between different gestures more effectively. In contrast, grayscale images would limit the model's learning capacity to brightness variations alone, potentially impairing its ability to differentiate between the classes, thus validating the choices made in sections 1.3 and 2.3.

    ![image](https://user-images.githubusercontent.com/109146037/234910379-89dbfb37-ca88-4162-981e-0bf33b295d53.png)

Fig.10 Keras mode of MobileNetV1 0.1

- **Keras mode**

To achieve better performance, I modified the base model MobileNetV1 to be trainable in Keras, setting the FINE\_TUNE\_EPOCHS to 10 and FINE\_TUNE\_PERCENTAGE to 65.

This means that during the fine-tuning stage of the pre-trained model, the model will iterate 10 times over the dataset to update its pre-trained weights with our gesture dataset. Only 65% of the base model's layers will be fine-tuned, while the remaining 35% will be kept frozen. This approach is designed to retain the valuable features learned by the base, pre-trained model during its initial training while allowing the model to adapt to the new task.

- **Training and testing results**

As a result, the model achieved 97.8% accuracy in the validation set and 90.67% accuracy in the testing set. This significant improvement in model performance can be attributed to allowing the base model to be trainable, enabling it to adapt its pre-trained weights to be more customized and better suited to our dataset.

<img width="208" alt="image" src="https://user-images.githubusercontent.com/109146037/234910533-c4e28955-d2b0-4681-988d-a3428e3e5c9d.png">
<img width="208" alt="image" src="https://user-images.githubusercontent.com/109146037/234910576-488834c2-6158-4404-8b6a-e1397001fcc4.png">Fig.11 Quntized validation set accuracy of trainable MobileNetV1 0.1. Fig.12Unoptimized testing set accuracy of trainable MobileNetV1 0.1

<img width="185" alt="image" src="https://user-images.githubusercontent.com/109146037/234910604-50336f4f-79a6-411f-811c-2ae0c1701f9e.png">

Fig.13 Unoptimized testing set accuracy of trainable MobileNetV1 0.1.

**3.3 Model Deployment**

Next, the model is deployed in the Arduino IDE. Although the quantized version provided by the EON compiler has lower RAM usage, the unoptimized version offers higher accuracy. As a result, the unoptimized version is chosen for deployment. Afterward, the model is exported as a library, allowing it to be easily integrated into the Arduino IDE for use.

<img width="299" alt="image" src="https://user-images.githubusercontent.com/109146037/234910708-8d0c6428-a7da-4204-9b0d-801fb4e0116b.png">

Fig.14 Anlysis of Quantized and Unoptimized versions of model by EON complier

**5. Program Building**

To incorporate the desired functionality, custom code has been added to the deployed script as follows:

- Initialize the built-in LED colors:

<img width="144" alt="image" src="https://user-images.githubusercontent.com/109146037/234910753-e9ed440d-9c73-4299-a657-abe1d7f0f944.png">

- Define a function to gerente a string out of a list of options randomly:

<img width="240" alt="image" src="https://user-images.githubusercontent.com/109146037/234911103-c260a71e-7419-4d01-8f49-e901aaf8c501.png">

- Change the delay time in the program to 5 seconds so that game rounds are not coming too quickly. Turn the image classification delay in the program as a preparing time for the user:

<img width="331" alt="image" src="https://user-images.githubusercontent.com/109146037/234911050-3e53ca80-0856-43c9-a215-ff2e6b19f78d.png">

- In the printing prediction stages, add the additional command to output the most confidence recognition as user's choice

<img width="274" alt="image" src="https://user-images.githubusercontent.com/109146037/234910953-dd0f84bd-0de9-4d69-8e70-c8d1ee0b5c52.png">

- Let the computer randomly generates choices out of {"rock", "paper", "scissors"} using the function defined before.
- Compare the generated choice with user's choice using strcmp() function. The strcmp() function is used to compare two strings character by character and determine their equality or the lexicographic order.
- Output the gaming round result and turn on the LED light as a signal
  - Blue indicates lose
  - Green indicates ties
  - Red indicates win

<img width="351" alt="image" src="https://user-images.githubusercontent.com/109146037/234910927-fe0e80da-60bf-4179-b58e-8ca1df5934a3.png">

**6. Program Testing**

**6.1 testing examples**
![image](https://user-images.githubusercontent.com/109146037/234911750-adc2dd60-db85-4aeb-9b2d-3e4e8fd5760f.png)


**7. Discussion and Conclusion**

To conclude, transfer learning applied on MobileNetV1 performs robustly in this project. However, it still has limitations. The program testing results indicate a case where the paper gesture is mis-recognized as scissors. This might be attributed to the unbalanced dataset size. In earlier experiments (conducted during the presentation process), the primary uncertainty was observed between the rock and scissors gestures, leading to an increased dataset size for rock and scissors. However, doing so inadvertently increased the uncertainty for the paper gesture, as demonstrated in Fig.14. For future work, utilizing a balanced dataset might be a more reasonable approach.

Currently, the program is unable to recognize other gestures and provide appropriate alerts to the user. As previously mentioned, including the "Other" label resulted in decreased model performance. This issue may be due to the limited data collection for the "Other" label. Owing to the time constraints of this project, only 525 samples were collected. For future work, it is recommended to include data for the "Other" label and increase the dataset size to enable this additional functionality.

# Bibliography

Shinya, Y., Simo-Serra, E. & Suzuki, T., 2019. Understanding the effects of pre-training for object detectors via eigenspectrum.

Howard, A. G., 1027. MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications.

Admin, 2022. _Gesture recognition and its application in machine learning._ [Online]
 Available at: https://how2electronics.com/gesture-recognition-application-machine-learning/

Jones, R., n.d. _Dr Kawashima's brain training for Nintendo Switch._ [Online]
 Available at: https://www.trustedreviews.com/reviews/dr-kawashimas-brain-training-for-nintendo-switch

Brownlee, J., 2019. _A gentle introduction to transfer learning for Deep learning._ [Online]
 Available at: https://machinelearningmastery.com/transfer-learning-for-deep-learning/

Admin, 2023. _ESP32 cam object detection & identification with opencv._ [Online]
 Available at: https://iotprojectsideas.com/esp32-cam-object-detection-identification-with-opencv/

## **Declaration of Authorship**

I, AUTHORS NAME HERE, confirm that the work presented in this assessment is my own. Where information has been derived from other sources, I confirm that this has been indicated in the work.

_Yiqi Huang_

ASSESSMENT DATE: 27th, April, 2023
