# CellPose3 in QuPath

CellPose is a deep learning model developed by the Stringer Lab. It excels at detecting diverse shapes that can be seen in different cell types. It is also structured to add training to any of the pre-trained models to fine tune the performance to your data. I only recommend this if you plan to use a custom model across a large dataset or if the shapes of the objects are a challenge for the current cellpose models to detect.

The most recent release of CellPose integrates the SAM model: <https://github.com/MouseLand/cellpose/tree/v4.0.4>

We will be using CellPose v3 for this workshop: <https://github.com/MouseLand/cellpose/tree/v3.1.1.2>

The BIOP group created the Extension for using CellPose in QuPath: <https://github.com/BIOP/qupath-extension-cellpose>

## Setting up CellPose

To use CellPose in QuPath, the CellPose python environment needs to be set up, the extension needs to be installed, and QuPath needs to be directed to the location of the CellPose python environment. Your cellpose python.exe file should be in a location similar to this path: `C:\Users\**your_username**\AppData\Local\miniconda3\envs\cellpose\python.exe`

AppData is a hidden folder, to view hidden folders in file explorer on Windows, select `Hidden items` in the `Show` submenu of `View`.

![Hidden items](/Tutorials/PNGs/Hidden_Items.png)

In QuPath, open the preferences menu (cog wheel or `Ctrl + ,`), you should see an option for Cellpose/Omnipose. In `Cellpose 'python.exe' location` paste in the path to your cellpose python.exe file:

![Menu](/Tutorials/PNGs/CellPose_Preferences_Menu.png)

Remove any quotes, if present.

Cellpose is now ready to use in QuPath.

## Running CellPose

Make a copy of both the LuCa and CMU-1 images. Remove any existing objects and create a 2048x2048 rectangle on each image.

Open `Cellpose detection script template`.

There are several parameters that can be adjusted to fine tune the results from CellPose:

- `pathModel` - many pre-trained models are available from Cellpose that work with different data types (cyto3, nuclei, cellpose, tissuenet_cp3, livecell_cp3, yeast_PhC_cp3, yeast_BF_cp3, bact_phase_cp3, bact_fluor_cp3, deepbacs_cp3, cyto, cyto2, CPx)
- `pixelSize` - minimum value is the pixel size of the image, larger values will use down sampling
- `preprocess( ImageOps.Filters.filtertype( filter size ) )` - apply filters like median, gaussian, mean, variance, closing, or opening to pre-process the image
- `channels()` - name or index (recommended) of up to two channels to use for CellPose; common to use a nuclear and cell body channel
- `cellprobThreshold(-6.0 - 6.0)` - lower/negative values are more forgiving, higher/positive values are more stringent
- `flowThrehsold(0.0 - 1.4)` - lower values have less "flow" and higher values have more "flow"
- `diameter(pixels)` - CellPose resizes an image so that the objects are about 30 pixels in diameter. If your objects are much larger or smaller than 30 pixels in diameter, it is crucial to put in an approximate diameter so the objects are detected accurately. Keep in mind that this value is influenced by how much down sampling may be occurring if `pixelSize` is larger than the pixel size of the image.

*Tip: use the straight line tool to get an estimate of the diameter of objects. The length will to scale so you will need to convert to pixels. Conversion: length(um) / pixel size(um/px).*

Use the following parameters for running CellPose on the LuCa image:

- `pathModel = 'cyto3'`
- `pixelSize(0.5)`
- `channels(6)`
- `tileSize(1024)`
- `diameter(20)`
- uncomment the following options: `measureShape()`, `measureIntensity()`, and `disableGPU()` (not necessary but helps the software know it needs to use the CPU faster)

The first time CellPose runs, it downloads the pre-trained models first. So it may take slightly longer to run. Downloading the models only needs to occur once.

You should get results similar to this:

![Cellpose results](/Tutorials/PNGs/CellPose_Results.png)

Cellpose can be used to segment things other than cell nuclei. Test out different models and/or parameters to segment the to segment the following:

- CD8 cells
- FoxP3 cells
- PDL1 cells

### Using CellPose to make more accurate cell outlines

Open `Detect nuclei and cells using cellpose`. We are going to segment the PDL1 positive cells to get more accurate cell boundaries. Use the following parameters in the script:

```groovy
// Create a Cellpose detectors for cyto and nuclei
def pathModel_cyto = 'cyto3'
def cellpose_cyto = Cellpose2D.builder( pathModel_cyto )
        .channels( 0 )
        .pixelSize( 0.5 )              // Resolution for detection
        .diameter(40)                  // Median object diameter. Set to 0.0 for the `bact_omni` model or for automatic computation
        .measureShape()                // Add shape measurements
        .measureIntensity()            // Add cell measurements (in all compartments) 
        .build()

def pathModel_nuc = 'cyto3'
def cellpose_nuc = Cellpose2D.builder( pathModel_nuc )
        .channels(6)
        .pixelSize( 0.5 )              // Resolution for detection
        .diameter(20)                  // Median object diameter. Set to 0.0 for the `bact_omni` model or for automatic computation
        .build()
```

Example of results:

![Cells from cellpose](/Tutorials/PNGs/CellPose_createcells.png)