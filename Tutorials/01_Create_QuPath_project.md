# Create a QuPath Project

1. Create a new folder on the desktop. Name the folder something descriptive, like "Practice_QuPath_Project" (do not use spaces or special characters in the path of the location, this will cause issues with certain extensions).

2. Drag and drop the empty folder into the open instance of QuPath.

    ![Creating Project](/Tutorials/PNGs/Creating_Project.png)

3. Now you're ready to add in images! Drag and drop the images from the P drive into the open QuPath window. You will get a prompt asking if you would like to make a project in the directory. **An empty folder must be used to start a QuPath Project.**

    ![Adding Images](/Tutorials/PNGs/Adding_Images.png)

    Select `Bio-Formats` as the image server. Because we are working with a fluorescence and H&E image, leave the image type blank. We will set this after. Auto-generate pyramids can be left checked. Click `Import`. After the images are imported, you should see two entries in the Project tab on the left.

    Note: If all the images imported are the same type, it can be selected in this import window.

QuPath does not create copies of any images when they are added into a project. Instead, QuPath records the location of where the images are stored and references this path each time the project is opened. If the QuPath project is moved somewhere (location that does not have access to the images, or a different OS that uses a different path configuration) or if the images are moved from their original location, QuPath will not be able to load them in and a window will open showing which images are impacted, you can use the search button on this window to navigate to the location of the images to automatically update the broken paths. If QuPath is accessing images over a network, the performance will be greatly impacted. I always recommend having the images copied onto the same computer that will be used for analysis.

Let's take a tour of the QuPath GUI.
