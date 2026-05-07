# Workflows and Batch Analysis in QuPath

One of the best features of QuPath is that you can interactively create and fine tune an analysis pipeline without risk of losing track of the perfect parameters (with some exceptions). In this tutorial, we will build out a reusable workflow to run on a small batch of images.

## The Workflow Tab

Many of the built in functions in QuPath are recorded in the order they were used with the parameters under the workflow tab. Extensions are not usually recorded in the workflow tab (unless the extension author added the feature), but can be integrated into the script generated using `Create Workflow`.

Open the LuCa image with the classified cell detections and select the Workflow Tab. 
<img src='/Tutorials/PNGs/WorkflowTab.png' width='368' height='210'><br>

Parameters are listed when a command is selected.

<img src='/Tutorials/PNGs/WorkflowTab_parameters.png' width='358' height='932'><br>

These commands can be copied over into a script for running on multiple images by right clicking on any command and clicking copy command.

## Creating a workflow script
Open the BPAE_63x_3x4tile_Airyscan Processing_Stitch_MIP.czi image. We used this image in [02_Builtin_Tools](/Tutorials/02_Builtin_Tools.md) and should have a few commands listed in the workflow tab. Click on the Create Workflow button at the bottom of the workflow tab. You should see a new window open.

<img src='/Tutorials/PNGs/CreateWorkflow.png' width='410' height='634'><br>

Click on the commands to check the parameters. Right click on the commands to remove repeat commands (keep the one with the parameters you liked the most) and reorder them as needed. Your final workflow should look similar to the one above. After finalizing, click on Create script. A Script Editor window will open with the commands written in groovy and in the correct order. If you had multiple images of the same type to analyze in the same way, you could run this script for all (or select) images in the QuPath project (Run > Run for project). 

Test out the script by deleting all objects in the image (Objects > Delete > Delete all objects) and then click Run on the script editor.

If you are running a script on a batch of images, add the following code snippet to the end of the script. QuPath can hog memory on the HPC and cause out of error messages when running a batch analysis (this will cause the node to crash!). This snippet will help clear memory so the script can run without issue.

```
//Add to end of any script running in batch to help clear up memory space. Very important for running batch scripts on HPC
Thread.sleep(100)
// Try to reclaim whatever memory we can, including emptying the tile cache
javafx.application.Platform.runLater {
    getCurrentViewer().getImageRegionStore().cache.clear()
    System.gc()
}
Thread.sleep(100)
```

Congratulations on creating your first batch analysis script! Don't forget to save it!
