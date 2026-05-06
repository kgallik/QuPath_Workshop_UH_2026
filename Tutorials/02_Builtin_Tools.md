# QuPath's Built in Analysis Tools

There are three main sets of analysis tools built into QuPath:

- [Cell Detection](#cell-detection)
- [Object Classifiers](#object-classifiers)
- [Pixel Classifiers](#pixel-classifiers)

**Cell Detection** uses traditional methods of segmentation to find objects like nuclei, cell bodies, or other blob-like objects. The interactive tool allows the user to adjust parameters to smooth the image, correct background, and set a threshold that work well with the data.

**Object Classifiers** allow the user to group their existing detections into different categories. For example, if two different populations of cells existed in a fluorescent image - positive or negative for a marker of interest - an object classifier could be taught with examples of what a positive or negative cell looks like to then classify the rest of the detected cells.

**Pixel Classifiers** are similar to object classifiers in that the user is teaching the software how to group the pixels in the image by providing examples. Pixel classifiers are great for segmenting complex shapes or structures that may not be easily defined by a single channel or signal in the image data.

## Cell Detection

Open the LuCa-7color image in QuPath by double clicking on the thumbnail in the Project Tab. You will get a prompt to select the image type if it has not already been assigned. Create a rectangle that is 1024x1024 pixels using `Specify Annotation`

Open Cell Detection (Analyze > Cell detection > Cell detection)

<img src="/Tutorials/PNGs/Cell_Detection_Menu.png" width="417" height="161"><br>

Adjust the parameters until you are satisfied with the results. Below are the settings I found to work reasonably well.

*Note: this will not be perfect and that's okay!*

<img src="/Tutorials/PNGs/Cell_Detection_Parameters.png" width="545" height="330"><br>

## Object Classifiers
Using the same objects above, we will create two types of object classifiers to group objects as positive or negative for EGFP. 

**Important! You need measurements in order to classify objects, make sure that `Make measurements` was selected when detecting the nuclei.**

### Train an Object Classifier
Before we train the object classifier, we need to mark some examples of positive and negative cells. Add two Points annotations by clicking the points annotation tool and then clicking `Add` on the popup menu twice. These annotations will be visible under the Annotations tab.

<img src="/Tutorials/PNGs/Points_Annotations.png" width="372" height="379"><br>

Then set the class of one points annotation to Positive and the other to Negative.

<img src="/Tutorials/PNGs/Setting_Class_of_Annotations.png" width="294" height="378"><br>

Using the brightness and contrast menu, turn off the DAPI and AF568 channel so that only the EGFP channel is visible. This will make finding positive and negative examples easier.

Using the Positive and Negative Points Annotations, mark examples of cells positive and negative for EGFP. Aim to have roughly equal numbers of examples.

<img src="/Tutorials/PNGs/PositiveNegative_Examples.png" width="766" height="425"><br>

Open the Object classifier (Classify > Object Classification > Train Object Classifier) and then change the Feature parameters from `All Measurements` to `Selected Measurements`. Click select and search for EGFP in the new window. Select all of the EGFP related measurements and then click apply.

<img src="/Tutorials/PNGs/Object_Classifier_Settings.png" width="406" height="310"><br>

In the Training option, change it from `Unlocked annotations` to `Points only`. 

*Note: if you were using training data across multiple images, or a specific training image, then you would select Load training and specify which images to use for training the object classifier. In this case, **only the examples of classes from the selected images would be used**.*

Once the examples are annotated and the object classifier parameters are set, click on the Live update button to preview. You can also name and save the classier for use on other images.

<img src="/Tutorials/PNGs/Preview_Classifier.png" width="618" height="349"><br>

The benefit of training an object classifier with a machine learning algorithm is that it is more flexible to natural variation that can be seen across samples and batches because it uses multiple sets of measurements to make decisions on how a detection should be classified.

## Creating a Single Measurement Classifier
Another approach to classifying cells is to use a hard coded threshold using a single measurement. This is less flexible to natural variation that may be seen across your data, but a great option for small and highly consistent datasets or single images. Do not set different threshold values for different images/data you plan to compare, instead [train a classifier like in the section above](#train-an-object-classifier).

Open the Single Measurement Classifier (Classify > Object classification > Create single measurement classifier). Change the Channel filter to EGFP, and set Above Threshold to Positive and Below Threshold to Negative. Then click on Live Preview. QuPath's default option is the mean intensity value of either the nucleus compartment or the cell and will make a good guess where the threshold should be. In this case, the suggested threshold does a reasonable job separating the positive and negative cells. Test out other measurement options or threshold values to get a feel for how this tool works.

<img src="/Tutorials/PNGs/Single_Measurement_Classifier.png" width="630" height="418"><br>

Like in [Training an Object Classifier](#train-an-object-classifier), you can save the single measurement classifier to use on other images.

## Pixel Classifiers
Training pixel classifiers is similar to training object classifiers, but use different annotations to mark examples. In this example, we are going to make a pixel classifier to automatically segment whole glomeruli.

### Train the Pixel Classifier
First, create a new class and name it `glomerulus`. You can change the color of any class by double clicking the name and selecting a new color.

<img src="/Tutorials/PNGs/Add_Class.png" width="338" height="367"><br>

Using the Open Polygon annotation tool, start marking examples of a couple glomeruli. Leave some unmarked so we can assess how the classifier is performing. Give attention to the edge of the objects so the classifier learns the boundary of the objects. In the Annotation menu, Ctrl+Click all annotations that are on glomeruli and then set their class to `glomerulus`.

*Tip: Instead of creating unclassified annotations, and then classifying them, highlight the class you are going to mark and then click Auto Set. All subsequent annotations will automatically be assigned that class. Just remember to change the class as you are marking different examples.*

<img src="/Tutorials/PNGs/Adding_Annotations.png" width="1273" height="738"><br>

After marking some glomeruli, add in some examples of what is not a glomeruli. These will be classified as `Ignore*`.

<img src="/Tutorials/PNGs/Adding_Annotations2.png" width="1155" height="666"><br>

Open Train Pixel Classifier (Classify > Pixel Classification > Train Pixel Classifier) and adjust the parameters to the following:

- Classifier: RTrees
- Resolution: High (1.38 um/pixel)
- Features:
  - Channels: DAPI, EGFP, AF568
  - Scales: 1.0, 2.0, 4.0, 8.0
  - Features: Select All
  - Local Normalization: Mean and Variance 
  - Local Normalization scale: 8
- Output: Classification
- Region: Everywhere
  
<img src="/Tutorials/PNGs/Pixel_classifier_parameters.png" width="615" height="375"><br>

Preview the results with Live Prediction and add in open polygon annotations for the `glomerulus` and `Ignore*` classes until the preview of the results seem reasonable. Below example has the annotations hidden for easier visibility.

<img src="/Tutorials/PNGs/Pixel_classifier_preview.png" width="480" height="755"><br>

### Make Objects From the Pixel Classifier
Name and save the classifier to use later. 

Create objects from the pixel classifier (may need to be a trial and error process to find the minimum size for excluding false-positives). Select Annotations as the object type if you would like to add cell detections within the glomeruli. Select Detections for a lightweight option and if you don't plan to detect cells inside the glomeruli.

These parameters worked well for this data and the pixel classifier I generated:

<img src="/Tutorials/PNGs/Pixel_classifier_create_objects2.png" width="543" height="399"><br>

*Tip: The Split Objects option can be used if you want to know information about each individual object detected. If you are only interested in the class as a whole, then this option does not have to be used and will be less computationally intense. Be careful of the Delete Existing Objects Option as it will remove everything, use with caution.*