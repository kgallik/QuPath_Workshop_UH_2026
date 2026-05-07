# Installing Miniconda and Creating the CellPose and SAMAPI python environments

Two of the extensions we will use in this workshop require python environments to function: CellPose and SAM. Before starting any analysis tutorials, we will get these environments created so they are ready to use later in the class.

## Installing Miniconda

Miniconda is a light weight version of Anaconda3 (a python environment manager), which we will use for this class. Technically any python manager will work for creating these environments, we happen to be using this one.

Open the Software Center on your workstation (computer) and search for "miniconda", you should see miniconda in the search results:

![Miniconda search](/Tutorials/PNGs/Miniconda.png)

Click on the miniconda icon and then click "Install". Be sure to install **Miniconda** and not Anaconda3. The install should be quick.

Once the Software Center shows that the software is installed, we will open an anaconda prompt. Open the Windows Start Menu and search for "anaconda". You should see an option to launch an anaconda prompt:

![Anaconda prompt start menu](/Tutorials/PNGs/Anaconda_Prompt_StartMenu.png)

When the anaconda prompt is opened, you should see this window, but with your user name instead:

![Anaconda prompt window](/Tutorials/PNGs/Anaconda_Prompt.png)

Now we are ready to create python environments!

There are two files in the P drive that we will use to help create the environments with less coding. They are in P:\h3452\bia2026\QuPath

### CellPose Environment

In the Anaconda prompt window, create the cellpose environment with the following command (hit Enter after writing out the command):

`conda create -n cellpose python=3.10`

*Note: the first time you create an environment with miniconda, you will be asked to agree a few terms and conditions, press the `a` key and `enter` to proceed.*

![terms and conditions](/Tutorials/PNGs/TermsAndConditions.png)

You will be asked if you want to install a set of python packages, press the `y` key and `enter` to continue:

![Base packages](/Tutorials/PNGs/InstallBasePackages.png)

Activate the environment:

`conda activate cellpose`

Install the cellpose environment requirements using the cellpose_requirements.txt file in the P drive.

`pip install -r P:\h3452\bia2026\QuPath\cellpose_requirements.txt`

Launch cellpose to verify installation worked:

`cellpose`

You should see this window open:

![Cellpose gui](/Tutorials/PNGs/cellpose_gui.png)

Close the window afterwards, we do not need to use CellPose through the GUI (graphical user interface) for today.

In the anaconda prompt use the following command to close the CellPose environment:

`conda deactivate`

### SAM Environment

Using the same anaconda prompt window, we will create the python environment for SAM. **The previous cellpose environment must be deactivated prior.**

Create a new python environment with:

`conda create -n SAMAPI python=3.12`

Follow the prompts like before to install the required base packages.

Activate the environment:

`conda activate SAMAPI`

Install the SAMAPI requirements with this command:

`pip install git+https://github.com/ksugar/samapi.git`

After installing completes, start the SAMAPI server with the following command so the model files are downloaded and ready to use later:

`uvicorn samapi.main:app --workers 2`

The server will show these statuses when it is running (sam3 is gated and cannot be accessed, the warning is normal and does not affect the function of the tool):

![SAM server running](/Tutorials/PNGs/SAMserverstart.png)

Use `Crtl + C` to exit out of the server. We will relaunch it later in the class.
