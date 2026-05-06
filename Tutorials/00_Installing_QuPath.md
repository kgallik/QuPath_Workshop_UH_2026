# Installing QuPath v6.0 (University of Helsinki)

If you are using a university computer, QuPath v6.0 is already installed. Open the start menu and search for QuPath. I recommend opening the console version of QuPath. 

If you would like to install QuPath on a different university device, QuPath is available in the Software Center or you can follow the steps below.

# Installing QuPath (Personal Computer)

The easiest way to install QuPath is using the installer through QuPath's website (note that the latest version is v7.0, older versions can be accessed through the website below):  
[QuPath Website](https://qupath.github.io/)  

<img src="/Tutorials/PNGs/QuPath_Website.png" width=" 474" height="438"><br>  
Choose the installer for your operating system. The Window's msi installer is like a click-through installer wizard and the portable zip option is the full software contained in a zip file. If you choose the zip option, you will need to unzip the folder and move QuPath to your computer's C: drive.  

When installing on Windows, you may encounter this warning:  
<img src="/Tutorials/PNGs/QuPath_Install_Warning.png" width="236" height="133"><br> 

Click on the `...` button in the upper right and select `Keep`.  
<img src="/Tutorials/PNGs/QuPath_Install_Warning_Keep.png" width="233" height="188"><br>  

And then you may be prompted *again* to make extra sure you want to install QuPath:  
<img src="/Tutorials//PNGs/QuPath_Install_Warning_Keep2.png" width="235" height="405"><br>

**Important Note for Apple Silicon users**  
Due to some incompatibilities between Bio-Formats (an open source server for handling many image file formats including proprietary formats from Zeiss, Leica, and Nikon) and the new M1 Apple computers, Bio-Formats cannot be used to open image files. While you will still be able to open most images using other server options in QuPath, you **will not** be able to open certain Zeiss (.czi) images like those from the AxioScan instruments. To accommodate this potential issue, there will be OME-TIF copies of any .czi files used for this workshop.  

If you are experiencing any issues with installation, please consult the installation documentation [here](https://qupath.readthedocs.io/en/stable/docs/intro/installation.html#).

Once you have QuPath installed, test out opening it up. You will be prompted to agree to the licensing to use the software the first time you open it (i.e., not to be used for diagnostic purposes). You should then see this window:  
<img src="/Tutorials/PNGs/QuPath_Start.png" width="1024" height="568"><br>  
Uncheck the box in the bottom right (magenta arrow) to disable the welcome window from opening each time you start QuPath.  

One more window will pop up asking if you would like to open the extension manager. Click on "no" for now. If we have time in class, we will go over setting up extensions together.  
<img src="/Tutorials/PNGs/QuPath_Start_Extensions.png" width="1024" height="568"><br> 
