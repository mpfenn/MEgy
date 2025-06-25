![GitHub Release](https://img.shields.io/github/v/release/:user/:repo)

New Test line
# Multiphysical Energy System Simulator - MEgy

MEgy is a simulation framework based on graph theory, using nodes and edges to represent system components and their interconnections. It enables you to calculate the properties of a system and the behaviour of a network — such as heat and electricity — simultaneously. This allows you to study complex systems comprising different interconnected parts.

## Install MEgy

There are two ways to install megy.
The first version is the simplest, but you may not always get the latest patches.
The second version is more complex to install, but you will always get the latest patches, but this can lead to unexpected errors.

### 1. Install MEgy as Matlab toolbox - easy!

1. Download the MEgy version from: [MEgy Releases](https://github.com/mpfenn/megy/releases)
2. Double click on it and install
3. You are done with the megy installation ;)
4. Install a thermodynamic class [see here](#install-a-thermodyamic-class---the-hardes-part). Without it, your software will not run. We have separated the installation process due to licence issues.
5. Install a good solver [see here](#install-a-good-solver). You need a DAE-Solver to run your simulations.


### 2. Installing from the git repository - harder!

1. Clone the git repository on your machine. To do this, you will need to install git on your computer and set up the git-lab server with an SSH key. You can find instructions online, for example here: [Install Git](https://git-scm.com/downloads), [Set up SSH key](https://docs.gitlab.com/ee/user/ssh.html). Now you can clone megy's git repository into a folder on your system: [git@flexhyx-git.h-brs.de:megy/megy.git](git@flexhyx-git.h-brs.de:megy/megy.git). Start Matlab, change to the folder where you want to install megy. Open the Command Window in Matlab and add this line:
    ```cmd
    !git clone git@flexhyx-git.h-brs.de:megy/megy.git
    ```
    This should take some time. If the process was NOT successful, fix the error messages.

2. Install a thermodynamic class [see here](#install-a-thermodyamic-class---the-hardes-part). Without it, your software will not run. We have separated the installation process due to licence issues.

3. Install a good solver [see here](#install-a-good-solver). You need a DAE-Solver to run your simulations.

2. Add megy to the matlab path by running the startup script:
    ```cmd
    cd megy
    startup
    ```

3. Create a new folder on your system for the megy projects, call it ```MEgyProjects```, but you can choose any name you like. Create the folder for the projects outside the megy folder. Do this by typing in the Matlab Comand Window:
    ```cmd
    cd ..
    !mkdir MEgyProjects
    cd MEgyProjects
    ```

4. Go into this folder and create a new megy project. Type:
    ```cmd
    megy project new YourName
    ```

5. Inside the new project folder you will see several files and folders. Open startup.m and edit it. Edit the whole file, but edit the path <pathToMegy> with your path where megy is cloned, for example: ```C:\Users\myUserName\Documents\megy```:
    ```matlab
    function startup()
        pathtoMegy = fullfile("<pathToMegy>");
        try
            addpath(pathtoMegy);
        end
        
        folders = ["Components", "GUI", "Extensions", "Simulation", "Toolbox", "Test", "doc"];
        for i = 1:length(folders)
            try
                addpath(genpath(fullfile(pathtoMegy, folders(i))));
            end
        end

        disp("Startup script finished!");
        try
            MEgyControl.MEgyProject.openProject(pwd);
        catch ex
            addCause(MException("MEgy:openError", "The Project can not be opened, because the class MEgyParameters can not be found."), ex);
        end
    end
    ```

8. You will need to repeat the 7. step for each new project you create!


### Install a Thermodyamic Class - The hardes part
To install a thermodynamic class we use the GERG2008 implementation from National Institute of Standards and Technology (NIST). A link to re repository can be found here: [https://github.com/usnistgov/AGA8](https://github.com/usnistgov/AGA8)

To use a thermodynamic library like AGA8 framework, follow these steps:

1. **Download the Source Code**: Get the AGA8 library source files from the official source.
2. **Make the Source Code Accessible in MATLAB**: We chosse the C library and use the MEX interface in matlab to access the C code in matlab.
3. **Create a Wrapper Class**: Implement a MATLAB class that inherits from Megy's `ThermodynamicInterface` and provides the necessary thermodynamic data to Megy. Make sure you keep the Units the same!
4. **Configure Your Project**: In your `*.megy` project file, add or modify the following fields:
   ```json
    "thermodynamicClassPath": "<path to the thermodynamic class>",
    "thermodynamicClass": "<name of the thermodynamic class>"
    ```
    Ensure that the specified thermodynamicClassPath is also on the MATLAB path.

This schema can be applied to any thermodynamic library you choose.

### Install a good solver
You can use any DAE-Solver you like. Our recommendation for a best performance simulation is to use the rodasp solver [see here](https://uk.mathworks.com/matlabcentral/fileexchange/10354-rodasp).

1. Download the solver on [https://uk.mathworks.com/matlabcentral/fileexchange/10354-rodasp](https://uk.mathworks.com/matlabcentral/fileexchange/10354-rodasp)
2. Move the file ```rodasp.m``` to the folder: ```C:\path to your megy installation\Simulation\Solver```
3. Before starting your simulation, change the solver to the rodasp-solver:
    ```matlab
    simPara = SimulationParameters();
    simPara.setSolver("rodasp2");
    ```


## Getting started

You need to add the installation folder of megy to your MATLAB path. If you miss this step, you will receive an error message. If you have done this, follow these steps:
1. Open the Matlab Command Window and type: ```megy project new myNewProjectName```

Start simulating ;)
