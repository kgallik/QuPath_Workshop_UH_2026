# CellPose in QuPath
CellPose is a deep learning model developed by the Stringer Lab. It excels at detecting diverse shapes that can be seen in different cell types. It is also structured to add training to any of the pre-trained models to fine tune the performance to your data. I only recommend this if you plan to use a custom model across a large dataset.

The most recent release of CellPose integrates the SAM model: https://github.com/MouseLand/cellpose/tree/v4.0.4

We will be using CellPose v3 for this workshop: https://github.com/MouseLand/cellpose/tree/v3.1.1.2

The BIOP group created the Extension for using CellPose in QuPath: https://github.com/BIOP/qupath-extension-cellpose

## Setting up CellPose
To use CellPose in QuPath, the CellPose python environment needs to be set up, the extension needs to be installed, and QuPath needs to be directed to the location of the CellPose python environment. If you are using the HPC to use QuPath, the CellPose environment is already installed. If you are using your own computer, there are instructions in the CellPose links above for setting up the Python Env.

The CellPose extension can be found [here](/Tutorials/QuPath_Extensions/), if you have not already installed it, drag and drop the qupath-extension-cellpose-0.10.1.jar and run-cellpose-qc.py files into an open instance of QuPath to install and then restart QuPath.

Open the preferences menu, you should now see an option for Cellpose/Omnipose. In `Cellpose 'python.exe' location` paste in the following path: /varidata/research/software/cellpose/cellposeQupathPlugin/QuPathCellposePython

Restart QuPath again so CellPose is ready to use.

**Note: Cellpose on the HPC is setup with GPU support but will still run on CPU nodes. Use a GPU node to run Cellpose for incredibly fast computation (great for big batches of images).**

## Running CellPose
Open/add the kidney_3chan.czi image. Create a 2048x2048 rectangle and move it somewhere interesting (if you ran StarDist earlier, keep those results on the image so you can compare the two models).

Open `Cellpose detection script template` under Extensions > Cellpose.

There are several parameters that can be adjusted to fine tune the results from CellPose:
- `pathModel` - many pre-trained models are available from Cellpose that work with different data types (cyto3, nuclei, cellpose, tissuenet_cp3, livecell_cp3, yeast_PhC_cp3, yeast_BF_cp3, bact_phase_cp3, bact_fluor_cp3, deepbacs_cp3, cyto, cyto2, CPx)
- `pixelSize` - minimum value is the pixel size of the image, larger values will use down sampling
- `preprocess( ImageOps.Filters.filtertype( filter size ) )` - apply filters like median, gaussian, mean, variance, closing, or opening to preprocess the image before Cellpose is run
- `channels()` - name or index of up to two channels to use for Cellpose; common to use a nuclear and cell body channel
- `cellprobThreshold(-6.0 - 6.0)` - lower/negative values are more forgiving, higher/positive values are more stringent
- `flowThrehsold(0.0 - 1.4)` - lower values have less "flow" and higher values have more "flow"
- `diameter(pixels)` - cellpose resizes an image so that the objects are about 30 pixels in diameter. If your objects are much larger or smaller than 30 pixels in diameter, it is crucial to put in an approximate diameter so the objects are detected accurately. Keep in mind that this value is influenced by how much down sampling may be occurring if `pixelSize` is larger than the pixel size of the image.

*Tip: use the straight line tool to get an estimate of the diameter of objects. The length will to scale so you will need to convert to pixels. Conversion: length(um) / pixel size(um/px)*

Use the cyto3 model (default option) with `pixelSize(0.345)` and `channels('DAPI')`. It should take about 2.5 minutes to run.

![Cellpose results](/Tutorials/PNGs/CellPose_Results.png)

Cellpose can be used to segment things other than cell nuclei, try segmenting the tubular structures with `channels(0,1)`, `pixelSize(0.345)`, and `diameter(150)`

![Cellpose results for tubular structures](/Tutorials/PNGs/CellPose_Results2.png)

Test out different models and/or parameters to segment the cells or tissue structures.