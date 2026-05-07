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

*Tip: Groovy (QuPath's scripting language) and Python (what StarDist runs on) do not like backslashes, common in Windows OS. Use `$/\Windows\style\path\file/$` in Groovy to avoid issues.*

### Fluorescence model

Create a duplicate of the LuCa image that contains the original cell detections created earlier. Delete the points annotations, if present, and the detections (`Delete all detections`) so the original rectangles remain.

Open `StarDist fluorescence detection script`:

![StarDist Script](/Tutorials/PNGs/StarDist_Script.png)

Enter the path to the dsb2018 heavy augment model in the script on line 21 where it asks for the model path. If you transferred the models to the desktop, the path should be something like:

 `\\ad.helsinki.fi\home\g\**your_username**\Desktop\StarDistModels\dsb2018_heavy_augment.pb`

*Tip: you can get the path to a file by right clicking and selecting "copy as path" in Windows OS.*

Adjust the below parameters to the following:

- Channel: `6` (using the index of the channel is safer than the name)
- Cell expansion: `4`

Select one of the rectangles (should be highlighted) and then press `Run` to run StarDist. It should take approximately 1 min to run on the Short Partition.

Try changing some of the parameters to see how they influence the results.

*Tip: If you are using a script to batch analyze your data, find the parameters that generally work well on your data and then save the script in the QuPath project for documentation and future use.*

### H&E model
Open 44770.svs to test out the H&E model. Create a 2048X2048 rectangle and place it somewhere interesting. Then open `StarDist H&E nucleus detection script`. Like with using `StarDist fluorescence cell detection script`, you will need to add in the path to the `he_heavy_augment.pb` model. Because this model was trained on RGB images of H&E stained samples, you do not need to specify a channel. You can still adjust the `normalizePercentiles`, `threshold`, and `pixelSize` parameters to fine tune the results. 

Use `0.345` for the `pixelSize` and run the script.

![StarDist Results H&E](/Tutorials/PNGs/StarDist_Results2.png)

Try out different parameters to see how they influence the results.

### Common Errors
```
ERROR: No parent objects are selected!
```
This means that the annotation is not selected, double click on the annotation in the image or select it in the Annotation tab and run the script again.

```
ERROR: I couldn't find the model file /typo/or/wrong/path/heheavy_augment.pb in QuPathScript at line number 25
```
Check the path or the slash type used. Groovy (the scripting language for QuPath) does not recognize `\` for paths, use either `\\` or `/`
