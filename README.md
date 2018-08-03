# Detection-of-pests
Asian University Machine Learning Camp Jeju 2018

## Table of Contents
1. [Overview](#overview)
    *  [Datasets Used](#dataset)
2. [Workflow](#workflow)
    *  [Introduction](#introduction)
    *  [Pre-requsites](#prerequsite)
    *  [Complete Workflow](#complete)
    *  [Steps to Follow](#steps)
3. [Future Direction and Ideas](#future)
4. [References](#ref)

## <a id="overview"> Overview/Motivation
Agriculture is a most important and ancient occupation in India. As economy of India is based on agricultural production,
    utmost care of food production is necessary. Pests like virus, fungus and bacteria causes infection to plants with 
    loss in quality and quantity production. There is large amount of loss of farmer in production. Hence proper care of
    plants is necessary for same. Image processing provides more efficient ways to detect diseases caused by fungus, 
    bacteria or virus on plants. **Mere observations by eyes to detect diseases are not accurate.** Excess use also damages
    plants nutrient quality. It results in huge loss of production to farmer. Hence use of image processing techniques 
    to detect and classify diseases in agricultural applications is helpful.


### <a id="dataset"> Datasets Used

For this project, the datasets are images: you can download images in 2 ways 
1. Fatkun batch Download (chrome Extension)
2. FFmpeg - Command line tool (extracts images from videos)


## <a id="workflow"> Workflow
The workflow is divided into 4 main parts.

## <a id="introduction"> Introduction
[ImageNet](http://www.image-net.org/) is a common academic data set in machine
learning for training an image recognition system. Code in this directory
demonstrates how to use TensorFlow to train and evaluate a type of convolutional
neural network (CNN) on this data set.
    
http://arxiv.org/abs/1512.00567
    
This network achieves 21.2% top-1 and 5.6% top-5 error for single frame
evaluation with a computational cost of 5 billion multiply-adds per inference
and with using less than 25 million parameters. Below is a visualization of the
model architecture.


![image](https://github.com/r-karthik/images/blob/master/detection_of_pests/layers.png)
    
    
## <a id="prerequsite"> Pre-Requsite
* [Python 3.5+](https://www.python.org/downloads/)
* [Tensorflow](https://www.tensorflow.org/install/)
* [Docker Toolbox](https://docs.docker.com/toolbox/)
* [Android Studio](https://developer.android.com/studio/)

### <a id="complete"> Complete Workflow
   
![image](https://github.com/r-karthik/images/blob/master/detection_of_pests/flow1.JPG)

**Why to use pre-trained model when we can build new one??**
*  Computation: Moderate
*  Training: Faster
*  Training Time: Minutes to Hours
*  Accuracy: Good Depends on the pre-trained CNN model.

![image](https://github.com/r-karthik/images/blob/master/detection_of_pests/transferlearningworkflow.png)
    
### <a id="steps"> Steps to Follow
1. Install python 3.5 or above, tensorflow & Docker toolbar.
2. Place all your Datasets in a folder & The classification script uses the folder names as label names, and the images
   inside each folder should be pictures that correspond to that label. Collect as many pictures of each label as you can
   and try it out!
3. Run the training.py in docker with the following command & As it trains, you can start TensorBoard in background. you'll
    see a series of step outputs, each one showing 
   training accuracy, validation accuracy, and the cross entropy:
    *  The **training accuracy** shows the percentage of the images used in the current training batch that were labeled 
    with the correct class.
    *  **Validation accuracy:** The validation accuracy is the precision (percentage of correctly-labelled images) on a 
    randomly-selected group of images from a different set.
    *  **Cross entropy** is a loss function that gives a glimpse into how well the learning process is progressing. 
    (Lower numbers are better.)
    ```shell
        #setting up the architecture
        IMAGE_SIZE=224
        ARCHITECTURE="mobilenet_0.50_${IMAGE_SIZE}"
        #The default training is 4000 iterations.
        python -m scripts.retrain --bottleneck_dir=tf_files/bottlenecks --model_dir=tf_files/models/"${ARCHITECTURE}" --summaries_dir=tf_files/training_summaries/"${ARCHITECTURE}" --output_graph=tf_files/retrained_graph.pb --output_labels=tf_files/retrained_labels.txt --architecture="${ARCHITECTURE}" --image_dir=tf_files/training_dataset
        #TensorBoard
        tensorboard --logdir tf_files/training_summaries &
    ```
    ![image](https://github.com/r-karthik/images/blob/master/detection_of_pests/accu.png)
4. when the training is completed the output is retrained_graph.pb & retrained_labels.txt you can test the model using the graph.
    ```shell
    #Testing
    python -m scripts.label_image --graph=tf_files/retrained_graph.pb --image=tf_files/testing_dataset/image_name.jpg
    ```    
    ![image](https://github.com/r-karthik/images/blob/master/detection_of_pests/Capture2.JPG)
5. If you want to run the model on android device to predict you have to opimize the retrained_graph.pb file so that the model can be compressible. Run the following commands which can compress the model upto 70%.
    ```shell
    #optimized graph
     python -m tensorflow.python.tools.optimize_for_inference --input=tf_files/retrained_graph.pb --output=tf_files/optimized_graph.pb --input_names="input" --output_names="final_result"
    #rounded graph
    python -m scripts.quantize_graph --input=tf_files/optimized_graph.pb --output=tf_files/graph.pb --output_node_names=final_result --mode=weights_rounded
    ```
6. Paste the graph.pb & labels.txt in android/tfmobile/assets & open android studio choose existing project tfmobile. let the Gradle build finish & run your app. Now you can use your andoid mobile to classify things my using the App.
    ![image](https://image.ibb.co/burW0e/ss3.png)                ![image](https://image.ibb.co/j6XAtz/456.png)

## <a id="future"> Future Direction and Ideas
## <a id="ref"> References

[1. https://ieeexplore.ieee.org/document/7155951/](https://ieeexplore.ieee.org/document/7155951/)

[2. https://ieeexplore.ieee.org/document/7916653/](https://ieeexplore.ieee.org/document/7916653/)

[3. https://www.sciencedirect.com/science/article/pii/S0168169917311742?via%3Dihub
](https://www.sciencedirect.com/science/article/pii/S0168169917311742?via%3Dihub)

[4. https://www.researchgate.net/publication/322941653_Deep_learning_models_for_plant_disease_detection_and_diagnosis
](https://www.researchgate.net/publication/322941653_Deep_learning_models_for_plant_disease_detection_and_diagnosis)
