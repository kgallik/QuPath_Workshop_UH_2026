# Segment Anything Model

Segment Anything Model (SAM) was [originally created](https://arxiv.org/abs/2304.02643) by a group at Meta for segmenting natural images (i.e., pictures of people, city scapes, animals, etc.) and then repurposed by Ko Sugawara to [segment biological images](https://www.biorxiv.org/content/10.1101/2023.06.13.544786v1) and created a QuPath extension for using SAM in the software.

SAM requires a [separate python environment](https://github.com/ksugar/samapi) in addition to the [QuPath extension](https://github.com/ksugar/qupath-extension-sam?tab=readme-ov-file). We installed these earlier in the course. If you have a computer with a GPU, SAM can take advantage of GPU acceleration. Instructions for setting up GPU support are in the SAMAPI GitHub repo. For today, we will use the CPU version.

## Starting the SAMAPI server

Open an anaconda prompt, activate the SAMAPI python environment, and start the SAM server:

`conda activate SAMAPI`

`uvicorn samapi.main:app --workers 2`

When you see these messages in the terminal window, SAM is ready to use:

![SAM server running](/Tutorials/PNGs/SAMserverstart.png)

## Using SAM in QuPath

In QuPath, open the CMU-1 image. Open the `SAM` extension in QuPath.


Select the Rectangle Annotation button in the window and then click Live Mode. Draw a rectangle that roughly covers the full kidney, SAM will automatically create a segmentation outline that outlines the full kidney.

<img src='/Tutorials/PNGs/SAM_Outline.png' width='372' height='239'><br>
<img src='/Tutorials/PNGs/SAM_Outline2.png' width='372' height='239'><br>

SAM segmentation is context dependent. Try zooming in and finding a glomeruli and using the rectangle prompt to segment it.

<img src='/Tutorials/PNGs/SAM_Glomeruli.png' width='343' height='353'><br>

The Points annotation tool can also be used as prompts for SAM.

<img src='/Tutorials/PNGs/SAM_Points.png' width='418' height='357'>
