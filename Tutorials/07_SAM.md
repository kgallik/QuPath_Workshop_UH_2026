# Segment Anything Model

Segment Anything Model (SAM) was [originally created](https://arxiv.org/abs/2304.02643) by a group at Meta for segmenting natural images (i.e., pictures of people, cityscapes, animals, etc.) and then repurposed by Ko Sugawara to [segment biological images](https://www.biorxiv.org/content/10.1101/2023.06.13.544786v1) and created a QuPath extension for using SAM in the software.

SAM requires a [separate python environment](https://github.com/ksugar/samapi) in addition to the [QuPath extension](https://github.com/ksugar/qupath-extension-sam?tab=readme-ov-file). We installed these earlier in the course. If you have a computer with a GPU, SAM can take advantage of GPU acceleration. Instructions for setting up GPU support are in the SAMAPI GitHub repo linked above. For today, we will use the CPU version.

## Starting the SAMAPI server

Open an anaconda prompt, activate the SAMAPI python environment, and start the SAM server:

`conda activate SAMAPI`

`uvicorn samapi.main:app --workers 2`

When you see these messages in the terminal window, SAM is ready to use:

![SAM server running](/Tutorials/PNGs/SAMserverstart.png)

## Using SAM in QuPath

In QuPath, open the CMU-1 image. Open the `SAM` extension in QuPath. You should see this new window open:

![SAM menu](/Tutorials/PNGs/SAMAPI_Menu.png)

You can select different models, prompt type, output type, and how to run SAM (for selected prompts or live). Keep the model on `vit_l(large)`, change the output type to `Single Mask` and use the Rectangle tool to draw a rectangle around one of the tissues in the CMU-1 image.

![rectangle prompt](/Tutorials/PNGs/SAM_rectangle_prompt.png)

Make sure the rectangle remains highlighted and then click on `Run for selected` in the SAM menu. You should get something similar to this after SAM finishes (with CPU this may take a few minutes):

![SAM result](/Tutorials/PNGs/SAM_result.png)

SAM segmentation is context dependent. I go by the guideline that if I can see the structure I want to have SAM segment, SAM should be able to see it too. Try zooming in and finding a structure in the tissue and using the rectangle prompt to segment it.

![SAM structure prompt](/Tutorials/PNGs/SAM_rectangle_prompt2.png)

![Sam Structure result](/Tutorials/PNGs/SAM_result2.png)

The Points annotation tool can also be used as prompts for SAM.

![SAM points prompt](/Tutorials/PNGs/SAM_points_prompt.png)

![SAM points results](/Tutorials/PNGs/SAM_points_results.png)

![SAM multi points prompt](/Tutorials/PNGs/SAM_points_prompt2.png)

![SAM multi points result](/Tutorials/PNGs/SAM_points_results2.png)

Take a few minutes to test out different models, prompts at different zoom levels, and the out put type on both CMU-1 and LuCa. Try toggling channel visibility to see how it impacts results.
