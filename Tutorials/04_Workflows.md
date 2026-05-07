# Workflows and Batch Analysis in QuPath

A great feature of QuPath is that you can interactively create and fine tune an analysis pipeline without risk of losing track of the perfect parameters (with some exceptions). In this tutorial, we will build out a reusable workflow to run on a small batch of images.

## The Workflow Tab

Many of the built in functions in QuPath are automatically recorded in the order they were used with the parameters under the workflow tab. Extensions are not usually recorded in the workflow tab (unless the extension author added the feature), but can be integrated into the script generated using `Create Workflow`.

Open the LuCa image with the classified cell detections and select the Workflow Tab. You should see all of the functions run on the image so far. Click on any of them to see the parameters used.

![Workflow tab](/Tutorials/PNGs/WorkflowTab.png)

## Creating a workflow script

Click on `Create Workflow`. A new window should appear:

![Create Workflow](/Tutorials/PNGs/Create_Workflow.png)

Click on the commands to check the parameters. Right click on the commands to remove repeat commands (keep the one with the parameters you liked the most) and reorder them as needed. Create a workflow that runs the following commands in the listed order: `Cell detection`, `Run object classifier`, `Select objects by class`, and `Compute intensity features` (the composite classifier). Your workflow should look similar to this:

![Workflow example](/Tutorials/PNGs/Example_workflow.png)

 After finalizing, click on `Create script`. A Script Editor window will open with the commands written in groovy and in the correct order. Create a duplicate of LuCa, delete all of the objects, create a new set of rectangles and try running the script. You will probably get a result like this in the Script Editor:

![Test workflow](/Tutorials/PNGs/Test_Workflow.png)

Why do we have 0 objects? What are we missing from the workflow that we did previously when making cell detections?

<details>
  <summary>What is missing?</summary>
    We need to have selected annotations!
    Select the rectangles using <code>Select Annotations</code>. This will record the command in the Workflow Tab (added to the bottom). Instead of creating a whole new workflow as before, right click on the command and select `Copy Command`. In the script editor, move all the code down a line and paste this command as the first line in the script. Make sure the rectangles are not selected (double click outside of them) and run the script again.
</details>

Save the final script. QuPath creates a new folder in the project folder called "scripts" by default. This is a great place to save scripts that are specific for a project. You can easily access project specific scripts from `Project scripts` in the `Automate` menu. If you know the name of the script, you can also search for it using `Ctrl + L`.

### Running a batch script (reference only)

To run a script on a batch of images, select `Run for Project` from the `Run` dropdown menu in the script editor window. A new window will appear that allows you to select the images you want to have the script applied to. QuPath will save the results of the script after each image is processed.

If you are running a script on a batch of images, add the following code snippet to the end of the script. QuPath can hog RAM on computing clusters potentially causing an out of memory problem. This snippet is a good safeguard to prevent out of memory issues.

```groovy
//Add to end of any script running in batch to help clear up memory space. Very important for running batch scripts on HPC
Thread.sleep(100)
//Try to reclaim whatever memory we can, including emptying the tile cache
javafx.application.Platform.runLater {
    getCurrentViewer().getImageRegionStore().cache.clear()
    System.gc()
}
Thread.sleep(100)
```

Congratulations on creating your first batch analysis script! Don't forget to save it!
