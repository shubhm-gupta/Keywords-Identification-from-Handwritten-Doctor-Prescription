# Handwriting to Digital Text

## Introduction

### Problem

As we all know that doctors have illegible handwriting and it is difficult for individuals from non-medical background to understand the disease and the medicines mentioned in the report. Most of the times, even pharmacists find it difficult to decipher the handwriting written in medical reports. This is the problem we have been observing from decades and many have suffered due to this problem.
In the modern era, few doctors have started to provide digitized prescriptions to maintain records, but most doctors still provide traditionally handwritten prescriptions on their printed letterhead. This is one of the main reasons we want to work on this problem.

Moreover, it is difficult to store and access physical documents in an efficient manner, search through them efficiently and to share them with others. Thus, a lot of important data gets lost or does not get reviewed because documents(prescription) never get transferred to digital format.  This is another reason for us to work on this problem.

There are many existing models which convert handwritten images to digital text, but our aim is to go an extra mile and identify the keywords or medical terms from the prescriptions such as the disease, medicines prescribed, medical tests suggested, etc.

### Motivation

Many people face a problem in identifying or understanding keywords written by doctors in their prescriptions and end up purchasing wrong medicines. One of us also faced a similar problem. Naman got a prescription from the doctor when he was suffering from pain in his abdominal area. Even though he got the medication he still didn’t know what it was for. On his next visit to the doctor, he questioned why he was still having pain. The doctor said that he had mentioned in his prescription that he was suffering from a kidney stone and that might take more time for him to recover. Similarly, many people face such problems, and by building this model we are trying to overcome such issues. 

### Challenges
* Medical Data is not readily available due to sensitivity. We had to build our own dataset from scratch.

* A medical prescription consists of printed letter-head and handwritten doctor notes.

* We needed to segregate printed and handwritten text and process the handwritten text.

* Since the text is handwritten, it was skewed and slanted.


### Dataset
We have got the IAM Handwriting Dataset for the initial training of our model. This dataset is very large, and it consists of images of handwritten text segregated by words and sentences which we will be using. Apart from this, we build our own dataset using our Family & Friends Contact. Medical Dataset is very sensitive and kept confidential. So, we built our own dataset and plan not to share it with anyone in the future or upload to this repository.

### Approach

Here is the brief high-level approach to solving this problem. We have discussed our approach for each of the below algorithm/models in detail in the “Methods” section of this report.

•	Build dataset.  
•	Apply Image processing techniques such as:  
  o	Rescaling  
  o	Skew Correction  
  o	Slant Correction  
  o	Erosion and Dilation  
  o	Image Thresholding  
•	Page Segmentation - Segregate the handwritten text from the printed text  
•	Line Segmentation - Segregate each line from the segmented handwritten text   
•	Handwritten Text Recognition - Use Deep CNN and bi-LSTM to convert image text to characters.  
•	Keyword Spotting - Using the predefined python repositories of medical terms, we will be spotting the medical keywords from the medical prescriptions


## Methods

![](https://github.com/shubhm-gupta/Handwriting-to-Digital-Text/blob/master/pipeline.png)

Here is our detailed approach for this project after Literature Survey & reviewing existing implementations. We have classified it into 5 major blocks, and we will be discussing each one of them in detail now:

### A.	Image Preprocessing:
In this section, we tried and tested out various image preprocessing techniques that were required for our dataset. Here is a summary of what we did:

1.	**Rescaling**: Inter cubic interpolation in CV to scale the image to a larger size.

2.	**Grayscale**: Converting the image to grayscale.

3.	**Erosion and Dilation**: To reduce the noise we applied both the techniques with a various combination of kernel sizes.

4.	**Blurring**: In order to reduce the noise, out of 3 methods i.e. Average, Gaussian, Median Blurring and we found that Gaussian, Median Blurring works fine with our image data.

5.	**Image Thresholding**: Segmented the image in foreground and background. We tried various thresholding techniques and found Adaptive Gaussian Thresholding from CV2 provides a good image for our dataset.

6.	Since the documents were manually scanned by inexperienced people, most of them were skewed. Hence, image **Skew Correction** was necessary. Skew correction is one of the first operations to be applied to scanned documents or images when converting data to a digital format. Its aim is to align an image before processing because text segmentation and recognition methods require properly aligned text data.

To skew an image containing a rotated block of text at an unknown angle, we corrected the text skew by:
i.	Detecting the block of text in the image.
ii.	Computing the angle of the rotated text.
iii.	Rotating the image to correct for the skew.

### B.	Paragraph Segmentation:

Once we processed the input image, we tried to segment the handwritten text from the image. A medical prescription consists of printed and handwritten text both. Our aim is to focus on Handwritten text. So, we used Deep CNN network to segment the paragraphs of text from the image. We found out the height and width of the handwritten text and used the bounding box to label the Handwritten data.

### C.	Line Segmentation:

After segmentation of image to paragraphs, we will pass each paragraph bounded by boxes as an input for our Line Segmentation model and it will output the image marking each line bounded by boxes.

For the segmentation of paragraph into the lines, we will be using Single Shot MultiBox Detector (SSD) with horizontal anchor boxes to identify each line from the paragraph. SSD predicts the bounding boxes and class as it processes the image simultaneously.

Our plan for feature extraction is to create a network based on ResNet because the main advantage of Resnet is that it solves the famous problem of Vanishing Gradient. With ResNet, gradients can back propagate by skipping some connections from last to front layer and the size of ResNet depends on how big our layer is and how many layers do we have.

### D. Handwritten Text Recognition
We built a model that generates the image features from the input image through CNN, and fed those features to LSTM sequentially. We used an encoding layer for extracting the image features from CNN and down sampled so that the features of the image are reduced. We also created a feature extraction network. Most of the previous work was done with multi-dimensional LSTM to recognize the handwriting. But we implemented that using single dimensional LSTM as it is computationally much cheaper than multidimensional.


### E. Keyword Spotting

* We have built a repository having common medicine names, medical tests and diseases.

* Looked up closest matching words from the repository using the difflib library.

* Voila! the keyword is spotted.


## Future Work

* The keywords can be correlated with the Doctor's background. Right now, we did not focus much on printed text about Doctor's Qualification.

* The keyword spotting can be done in real-time. Only those words are converted to digital text, which is a medical keyword. This will save cost of iterating the entire list.

* We built a small prototype medical repository for this project. For future, this can be taken up as a different project which will be very helpful working in Medical Domain.


