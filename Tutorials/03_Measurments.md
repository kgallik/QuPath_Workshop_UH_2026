# Measurements in QuPath

While most tools and extensions in QuPath are set to collect measurements at default, there are cases where you need to add measurements in later or add in additional measurements that are not included in the standard measurement settings. Here we will cover adding in measurements and how to export them for downstream statistical analysis.

## Adding more measurements

Open the LuCa image that contains the cell detections we classified previously. Double click on any of the cell detections to view its measurements in the annotation tab.

![Measurements](/Tutorials/PNGs/Cell_Measurements.png)

In addition to shape descriptor measurements, there are compartment specific measurements for each channel (e.g., Nucleus, Cytoplasm, Membrane and Cell). However, the measurements listed here are not the only ones available through QuPath. Intensity measurements that capture the texture and distribution of the fluorescence can be added.

Open `Add Intensity Features`:

![Add features](/Tutorials/PNGs/Measure_Intensity_Features.png)

For today, we will only add Haralick features. When calculating Haralick features on fluorescence images, a minimum and maximum grey value needs to be given. Use the brightness and contrast menu to determine the minimum and maximum values of the channels. If all the channels are fairly similar in the minimum and maximum values, the same values can be used for all channels. In the LuCa image, the CD68 channel has a much lower maximum value compared to the other channels. If this were a real analysis, you would want to consider using a different set of min and max values for this channel, but for today we will use the same values for all channels:

![Haralick Features](/Tutorials/PNGs/Haralick_Features.png)

*Note: the settings for ROIs can be ignored, this only applies to specific ROIs in use which are outside the scope of this tutorial/workshop.*

If you want to read more about how Haralick features are calculated, here is a [nice resource document](https://juliaimages.org/ImageFeatures.jl/stable/tutorials/glcm/).

## Exporting measurements

QuPath has several options for exporting measurements, these are based on the hierarchy of the image and its objects.

- Image level: Gives summary of object classes and counts in the whole image.
- Annotation level: Gives high level information organized by annotations in the image. This includes measurements, and detection counts (by classes if present).
- Detection/Cell level: Gives detailed information about each detection including hierarchy, measurements, and class
- TMA Cores level: Similar to Annotation level, but also includes the assigned coordinate location of the TMA core and a True/False value to indicate if the TMA core is present at a specific location in the grid.

**Make sure the project is saved before exporting measurements.** Open `Export measurements` and select the images to include in the export, the location to save (within the QuPath project folder is the default), delimiter type, and level of measurement to export. Click `Populate` next to the `Columns to include` dropdown menu to enable selecting specific measurements to export. If no measurements are specified, then all measurements are exported.

![Export measurements](/Tutorials/PNGs/Measurement_Export.png)

All images in the selected column will be written to the same data frame file. Detection level measurement files can easily contain hundreds of thousands of entries from just a few images when working with large slide scan datasets. I recommend using Python, R, or other software that a made to work with these types of large datafiles. Excel can be used to view them, but can accidentally cause data loss if you are not careful.

## Viewing Measurements Within QuPath

Open `Show detection measurements`, depending on the number of objects and the power of the computer, it may take a moment for the window to open. Click the `Show Plots` for interactive views of different measurements as histograms (counts of objects per bin) or scatter plots of two values. Take some time to explore this tool. You can also double click on any thumbnail to centre it in the viewer. These plots can be saved. Note that this is only a visual tool and will not provide you statistical significance calculations.

![View measurements](/Tutorials/PNGs/View_Measurements.png)
