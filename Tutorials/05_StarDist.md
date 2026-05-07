# Using StarDist in QuPath

[StarDist](https://github.com/stardist), created by Uwe Schmidt and Martin Weigert, is a deep learning instance segmentation tool that excels at detecting round blob objects, like nuclei. It is also excellent at detecting these types of shapes when they are highly clustered.

Three StarDist models have been created and released by Schmidt and Weigert, and are available to use with QuPath's StarDist extension: dsb2018_paper(_heavy_augment) for fluorescence data and he_heavy_augment for H&E stained tissues. You can find the models in the P drive. Copy the `StarDist Models` folder to the desktop.

## Running StarDist

StarDist is run through a script. There are a few parameters available in the script to fine tune the results of the model.

- `channels('name' or index of channel)` The channel that contains blob like objects. If using index value, indexing starts a 0. *Only applies to fluorescence-like images.*
- `threshold(value between 0-1)` The higher the value the more stringent the results.
- `pixelSize(any floating point value)` Minimum value is the pixel size of the image. Increasing the number will down sample the image prior to running StarDist, the more down sampling the less accurate the outlines.
- `normalizePercentiles(lower, upper)` Default is `1,99` which works well for most data.
- (Optional) `cellExpansion(in um)` can be used to create an arbitrary expansion of the specified distance around the detection.

The location of the model also needs to be provided in the variable `modelPath`.

Example of a model path = `'/path/to/location/dsb2018_paper.pb'`

*Tip: Groovy (QuPath's scripting language) and Python (what StarDist runs on) do not like backslashes, common in Windows OS. Use* `$/\Windows\style\path\file/$` *in Groovy to avoid issues.*

### Fluorescence model

Create a duplicate of the training image. Delete the points annotations, if present, and the detections (`Delete all detections`) so only the rectangle remains.

Open `StarDist fluorescence detection script`:

![StarDist Script](/Tutorials/PNGs/StarDist_Script.png)

Enter the path to the dsb2018 heavy augment model in the script on line 21 where it asks for the model path. If you transferred the models to the desktop, the path should be something like:

 `\\ad.helsinki.fi\home\g\**your_username**\Desktop\StarDistModels\dsb2018_heavy_augment.pb`

*Tip: you can get the path to a file by right clicking and selecting "copy as path" in Windows OS. Remember to add* `$/` *before and* `/$` *after the path.*

Adjust the below parameters to the following:

- Channel: `6` (using the index of the channel is safer than the name)
- Cell expansion: `4` to replicate the same expansion we used earlier

Select the full image annotation (should be highlighted) and click `Run` in the script editor to run StarDist. It should only take a few minutes.

Try changing some of the parameters to see how they influence the results. Can you find a set of parameters that reliably segments the clumped and isolated nuclei? How do these results compare to the cell segmentation from QuPath's built in tool?

#### Combining StarDist with QuPath classifier tools (reference only)

Like with the cell detections we made previously, an object classifier can be trained with these detections. The command to classify the detections can be added to the bottom of the script so that after detections are made by StarDist, they are classified. An example script is in the [Example Script folder](/Tutorials/Example_Scripts/) for reference.

### H&E model

Open CMU-1 to test out the H&E model. Create a 1024x1024 rectangle and place it somewhere interesting. oi87Then open `StarDist H&E nucleus detection script`. Like with using `StarDist fluorescence cell detection script`, you will need to add in the path to the `he_heavy_augment.pb` model. Because this model was trained on RGB images of H&E stained samples, you do not need to specify a channel. You can still adjust the `normalizePercentiles`, `threshold`, and `pixelSize` parameters to fine tune the results.

You should get something similar to this with the default parameters:

![StarDist Results H&E](/Tutorials/PNGs/StarDist_HE.png)

Try out different parameters to see how they influence the results. Can you find parameters that keep real nuclei while excluding false detections? If we were unable to get ideal segmentation results, what are some tools we have covered previously that could be used to identify detections to keep and detections to remove?

See an [example script here](/Tutorials/Example_Scripts/HE_StarDist_Filter_Nuclei.groovy) that uses a trained classifier to identify and then remove the false nuclei detections.

### Common Errors

```txt
ERROR: No parent objects are selected!
```

This means that the annotation is not selected or there are no annotations present on the image depending on if `def pathObjects = QP.getSelectedObjects()` or `def pathObjects = QP.getAnnotationObjects()` is used.

```text
ERROR: I couldn't find the model file /typo/or/wrong/path/heheavy_augment.pb in QuPathScript at line number 25
```

Check the path or the slash type used. If Windows OS is used, remember to add `$/` before and `/$` after the path.
