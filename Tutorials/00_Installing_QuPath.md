# Installing QuPath v6.0 (University of Helsinki)

If you are using a university computer, QuPath v6.0 is already installed. Open the start menu and search for QuPath. I recommend opening the console version of QuPath.

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
