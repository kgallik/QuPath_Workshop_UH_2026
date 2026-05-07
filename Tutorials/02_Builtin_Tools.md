# QuPath's Built in Analysis Tools

There are three main sets of analysis tools built into QuPath:

- [Cell Detection](#cell-detection)
- [Object Classifiers](#object-classifiers)
- [Pixel Classifiers](#pixel-classifiers)

**Cell Detection** uses traditional methods of segmentation to find objects like nuclei, cell bodies, or other blob-like objects. The interactive tool allows the user to adjust parameters to smooth the image, correct background, and set a threshold that work well with the data.

**Object Classifiers** allow the user to group their existing detections into different categories. For example, if two different populations of cells existed in a fluorescent image - positive or negative for a marker of interest - an object classifier could be taught with examples of what a positive or negative cell looks like to then classify the rest of the detected cells.

**Pixel Classifiers** are similar to object classifiers in that the user is teaching the software how to group the pixels in the image by providing examples. Pixel classifiers are great for segmenting complex shapes or structures that may not be easily defined by a single channel or signal in the image data.

Open the LuCa-7color image in QuPath by double clicking on the thumbnail in the Project Tab. You will get a prompt to select the image type if it has not already been assigned. Create a rectangle that is 1024x1024 pixels using `Specify Annotation` (remember to use `Ctrl + L`). The rectangle will be placed in the middle of the viewer. Pick a place that looks interesting to you. Double click the rectangle in the viewer or select it in the Annotation tab to edit or move the rectangle. If the anchor points are visible when an annotation is selected, the object can be moved or edits to the shape can be made.

![rectangle](/Tutorials/PNGs/Create_Rectangle.png)

Try using the wand or brush tool to edit the shape of the rectangle. To add, left click and drag starting inside the rectangle. To remove, hold `Alt + Left Click`. Fill the rectangle in (`Shift + F`) to better see what parts of the shape are included.

![edited rectangle](/Tutorials/PNGs/Edit_Rectangle.png)

Delete the edited shape and add in a new 1024x1024 rectangle. Select the object in the annotation tab, right click and select `Lock`. This prevents an annotation from being accidentally edited or moved. It also allows for adding separate annotations inside.

*QuPath does not automatically save progress, be sure to save the project after tasks like object creation to prevent loss of time and effort.*

## Cell Detection

Open `Cell Detection`.

![Cell Detection menu](/Tutorials/PNGs/Cell_Detection_Menu.png)

There are options for selecting the channel, pixel size, and pre-processing parameters to optimize detecting cells. Cell expansion creates an arbitrary added boundary to generate a nucleus, cytoplasm, membrane, and cell compartment for measuring image features. This value is set by the user and does not take the context of the image into consideration. If neighbouring cells have boundaries that come into contact, they meet in the middle and end. Overlapping objects are not created with this tool.

Set the threshold value to 3 and keep the rest of the default parameters to get a sense of what the results look like. Then try adjusting each parameter available (smaller and larger values) to see how they impact the end result. Each time the cell detection is run, the previous results will be cleared out (that's okay!). Once you have a better feel for how these parameters affect the cell detection results, try to find an optimized set of parameters you think works well for the data. *Tip: use the show grayscale option in the brightness and contrast menu so it is easier to see the nuclei.*

Question: why would the default threshold value of 100 not work for this dataset?

![Grayscale DAPI](/Tutorials/PNGs/Grayscale_DAPI.png)

Below are the settings I thought performed well.

![parameters](/Tutorials/PNGs/Cell_Detection_Parameters.png)

## Object Classifiers

Using the same objects above, we will create a set of object classifiers to classify cells as positive or negative for FoxP3 and CD8 so we can quantify the number of cells that are single and dual positive for these markers. We will eventually create a composite classifier for identifying the dual positive cells with less bias.

**Important! You need measurements in order to classify objects, make sure that `Make measurements` was selected when detecting the cells. Double click on any cell and look in the annotation tab to verify there are measurements present.**

### Train an Object Classifier

This tool is an example of machine learning, which uses pre-extracted features (i.e., the measurements made when creating cell detections) and statistics to calculate the combination of features to separate the classes.

Before we train the object classifier, we need to mark some examples of positive and negative cells for the two markers. We will start with CD8 and then repeat the same process for FoxP3. In the annotations tab, there is a class list for the project. Add two new classes named "CD8+" and "CD8-" by clicking the plus button to the right of `Class List`. Colours for classes can be changed by clicking on the colour square next to the name. Class visibility can be toggled by clicking the eye button.

![classes](/Tutorials/PNGs/Class_List.png)

Using the points tool (`.`) create two sets of points and set the class of one set to `CD8+` and the other to `CD8-`. Add new point sets with `Add` and set the class by right clicking on the object entry.

![classes](/Tutorials/PNGs/Points_Annotations.png)

The name of the annotation can also be changed by right clicking the object in the annotations tab and selecting `Set Properties`. Sometimes changing the names of objects can help with organizing data and keeping track of objects.

Using the brightness and contrast menu, change the channel visibility so only CD8 is on (can turn off all other channels or use the view grayscale option which is my preferred method).

With the points tool active, select the `CD8+` point set and left click on cell detections that are examples of CD8+. Then repeat the process for cells that are CD8- (be sure to select the `CD8-` point set). Try to give examples of the variability of CD8+ cells to help the classifier become more robust to natural biological variability. Do not mark all the cells as positive or negative, for this dataset start with up to 10 cells for each class. You should end up with something similar to this:

![Points Example](/Tutorials/PNGs/Points_Example.png)

*Tip: Individual points can be moved by left click and drag, and removed by using  `Alt + Left Click`*

Open `Train Object Classifier`. Change the Feature parameters from `All Measurements` to `Selected Measurements`. Click select and search for CD8 in the new window. Click `Select all` and then click apply. This only selects the results from the search.

![Classifier settings](/Tutorials/PNGs/Object_Classifier_Settings.png)

In the Training drop down, change it from `Unlocked annotations` to `Points only`. If you have several sets of classes present on the image, change the Classes from `All Classes` to `Selected Classes`, click select, and select only the classes you want to use in training (important later on). Additionally, click on `Load Training` and select the images to use for training. In our case, we are only using the LuCa image, but in real experiments, you may be using several images. Finally, in `Advanced Options` select `Mean & Variance`, normalizing can help with generalizing the classifier.

![Select Training Image](/Tutorials/PNGs/Training_Image_Selection.png)

*Note: if you were using training data across multiple images, or a specific training image, then you would select Load training and specify which images to use for training the object classifier. In this case, **only the examples of classes from the selected images would be used**.*

Once the examples are annotated and the object classifier parameters are set, click on the Live update button to preview. You can also name and save the classier for use on other images. Once you are satisfied with the results, save the classifier.

![Classifier preview](/Tutorials/PNGs/Classifier_Preview.png)

The pie chart shows the proportion of training annotations for each class, this can be very helpful when checking for class imbalance (can impact the results significantly).

The benefit of training an object classifier with a machine learning algorithm is that it can be more robust to natural variation seen in biological data. Additionally, any bias/error introduced into the training remains fairly consistent across the data it is applied to. However, careful and well planned training strategies are required for best results.

After creating the CD8 classifier, create a new object classifier for FoxP3. Reset the object classes with `Reset detection classification` to make training and previewing results easier. Add two new sets of points, and when setting up the new object classifier, be sure to only select the measurements for the FoxP3 channel and to only use the FoxP3+ and FoxP3- classes. Save the classifier after you are happy with the performance and reset the detection classes again.

### Creating a composite classifier for multiclass classification

To assign multiple classes to objects (i.e., FoxP3+;CD8-, FoxP3+;CD8+, FoxP3-;CD8-, FoxP3-;CD8+), we will use QuPath's `Create composite classifier` to merge the two classifiers we created. Open `Create composite classifier`, select the two classifiers, name the new composite classifier and click `Save & Apply`. You should see something similar to this:

![Composite Classifier Results](/Tutorials/PNGs/Composite_Classifier_Results.png)

In the Class list in the Annotation tab, the new combined classes will not be visible yet. To add these new classes, click on the arrow to the right of Class List and in the populate from existing objects menu, select `All Classes (including sub-classes)`. You can choose if you want to clear out any classes not present.

![Add Classes](/Tutorials/PNGs/Class_Visibility.png)

You should see the new classes added to the class list:

![New Classes Added](/Tutorials/PNGs/New_Classes_Added.png)

Take some time to toggle the visibility of the different classes, were you able to find any dual positive cells?

Try creating a new rectangle in the same image and create a new set of cells using the same cell detection settings from before. Then apply the new composite classifier to these cells (`Load Object Classifier` and then select the composite classifier). Do the results seem reliable to you?

## Creating a Single Measurement Classifier

Another approach to classifying cells is to use a hard coded threshold using a single measurement. This is less flexible to natural variation that may be seen across your data, but a great option for small and highly consistent datasets or single images. Do not set different threshold values for different images/data you plan to compare, instead [train a classifier like in the section above](#train-an-object-classifier).

Duplicate the LuCa image by right clicking on the image entry in the Project Tab and selecting `Duplicate Image`. Rename or add a suffix to the image and keep the option to duplicate the image data (this maintains the cells we have already created).

![Duplicate Image](/Tutorials/PNGs/Duplicate_Image.png)

Open the duplicated image and then open `Single Measurement Classifier`. Change the Channel filter to CD8, and the measurement to `CD8:Nucleus:Mean`. Set `Above Threshold` to `CD8+` and `Below Threshold` to `CD8-`. Then click on `Live Preview`. QuPath will make a good guess where the threshold should be based on the distribution of the values. Review the results and test out other measurements/threshold values to get a feel for how this tool works.

![Single Measurement Classifier](/Tutorials/PNGs/Single_Measurement_Classifier.png)

Like in [Training an Object Classifier](#train-an-object-classifier), you can save the single measurement classifier to use on other images.

Try making a single measurement classifier for FoxP3 and then create a new composite classifier using the single channel classifiers. Apply the classifier and compare the results to the trained object classifier.

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