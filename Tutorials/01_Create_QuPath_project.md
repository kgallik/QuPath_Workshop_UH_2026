# Create a QuPath Project

1. Create a new folder on the desktop. Name the folder something descriptive, like "Practice_QuPath_Project" (do not use spaces or special characters in the path of the location, this will cause issues with certain extensions).

2. Open QuPath. The first time you open the software you will get a few pop up windows:

![QuPath Start 1](/Tutorials/PNGs/QuPath_Start.png)

Check `Don't show this again (always agree)` then press `Agree`

![QuPath Start 2](/Tutorials/PNGs/QuPath_Start2.png)

Uncheck `Show this on startup` to disable this from appearing each time QuPath is started.

3. Set up the extension manager. The last popup will ask about setting up the extension manager. Today we will use the default location. Press `Use default`.

![QuPath Extension Manager](/Tutorials/PNGs/QuPath_Start_Extensions_new.png)

QuPath will check for updates, a popup window will let you know QuPath v7.0 is available, press the `Okay` button, this will not automatically install anything. You should see the extension manager window, click on `Manage Extension Catalogs`.

![Extension catalog manager](/Tutorials/PNGs/Extension_Manager.png)

In the new window, add the following websites to access the catalogs:

- `https://github.com/ksugar/qupath-catalog-ksugar`
- `https://github.com/BIOP/qupath-biop-catalog`

![Catalog Manager](/Tutorials/PNGs/Catalog_Manager.png)

After adding, close the window and then in the Extension Manger window find the StarDist, SAM, and CellPose extensions, click on their respective plus buttons and install the lastest version (selected by default).

![Extensions to add](/Tutorials/PNGs/Add_Extensions.png)

Restart QuPath so the extensions are read in completely and are ready to use.

4. Drag and drop the empty folder into the open instance of QuPath.

    <img src="/Tutorials/PNGs/Creating_Project.png" width="775" height="428"><br>

5. Now you're ready to add in images! Drag and drop images you would like to add to the project into the open QuPath window.

     <img src="/Tutorials/PNGs/Adding_Images.png" width="775" height="428"><br>
    
    Select `Bio-Formats` as the image server and the type of image (e.g., fluorescent, H&E, HDAB, Other). This can be corrected if a mistake is made. You can select multiple images and add them at the same time.

[Link to the slide deck presented, covers the layout of QuPath's Graphic User Interface](https://vanandelinstitute-my.sharepoint.com/:p:/g/personal/kristin_gallik_vai_org/EW2hmxe3mDJBmM5PNXSkau4BSGk77gxMTWqw_CFqQ10eiw?e=b7hiwj)