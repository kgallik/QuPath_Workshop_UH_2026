# Installing QuPath v6.0 (University of Helsinki)

If you are using a university computer in B425, QuPath v6.0 is already installed. Open the start menu and search for QuPath. I recommend opening the console version of QuPath.

If you would like to install QuPath on a different university device, QuPath is available in the Software Center or you can follow the steps below.

## Installing QuPath (Personal Computer)

The easiest way to install QuPath is using the installer through QuPath's website (note that the latest version is v7.0, older versions can be accessed through the website below):  

[QuPath Website](https://qupath.github.io/)  

![website](/Tutorials/PNGs/QuPath_Website.png)

Choose the installer for your operating system. The Window's msi installer is like a click-through installer wizard and the portable zip option is the full software contained in a zip file. If you choose the zip option, you will need to unzip the folder and move QuPath to your computer's C: drive.  

When installing on Windows, you may encounter this warning:

![install warning](/Tutorials/PNGs/QuPath_Install_Warning.png)

Click on the `...` button in the upper right and select `Keep`.

![keep install](/Tutorials/PNGs/QuPath_Install_Warning_Keep.png)

And then you may be prompted *again* to make extra sure you want to install QuPath:  

![keep install for real](/Tutorials//PNGs/QuPath_Install_Warning_Keep2.png)

**Important Note for Apple Silicon users**  
Due to some incompatibilities between Bio-Formats (an open source server for handling many image file formats including proprietary formats from Zeiss, Leica, and Nikon) and the new M1 Apple computers, Bio-Formats cannot be used to open image files. While you will still be able to open most images using other server options in QuPath, you **will not** be able to open certain Zeiss (.czi) images like those from the AxioScan instruments. To accommodate this potential issue, there will be OME-TIF copies of any .czi files used for this workshop.  

If you are experiencing any issues with installation, please consult the [installation documentation](https://qupath.readthedocs.io/en/stable/docs/intro/installation.html#).

Once you have installed QuPath, move onto the [next tutorial on opening QuPath for the first time and creating a project](/Tutorials/01_Create_QuPath_project.md).

## First Start Up

Open QuPath. The first time you open the software you will get a few pop up windows:

![QuPath Start 1](/Tutorials/PNGs/QuPath_Start.png)

Check `Don't show this again (always agree)` then press `Agree`

![QuPath Start 2](/Tutorials/PNGs/QuPath_Start2.png)

Uncheck `Show this on startup` to disable this from appearing each time QuPath is started.

Set up the extension manager. The last popup will ask about setting up the extension manager. Today we will use the default location. Press `Use default`.

![QuPath Extension Manager](/Tutorials/PNGs/QuPath_Start_Extensions_new.png)

QuPath will check for updates, a popup window will let you know QuPath v7.0 is available, press the `Okay` button, this will not automatically install anything. You should see the extension manager window, click on `Manage Extension Catalogs`.

![Extension catalog manager](/Tutorials/PNGs/Extension_Manager.png)

In the new window, add the following websites to access the catalogs:

- `https://github.com/ksugar/qupath-catalog-ksugar`
- `https://github.com/BIOP/qupath-biop-catalog`

![Catalog Manager](/Tutorials/PNGs/Catalog_Manager.png)

After adding, close the window and then in the Extension Manger window find the StarDist, SAM, and CellPose extensions, click on their respective plus buttons and install the latest version (selected by default).

![Extensions to add](/Tutorials/PNGs/Add_Extensions.png)

Restart QuPath so the extensions are read in completely and are ready to use.
