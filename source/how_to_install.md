# How to install MolTwister

## Obtaining MolTwister

The first step is to download MolTwister. Goto [the MolTwister homepage](http://www.moltwister.com) and select Download. You will 
be directed to the GitHub repository of MolTwister. The download can be done in two ways
1. Perform a git clone of the repository
2. Download the zip file of the repository

## Git

Depending on the method used to download MolTwister, it may be necessary to install Git. On Ubuntu this is done by
````text
sudo apt install git
````
On MacOS, Git can be installed using binary installers or through repositories, such as Homebrew. For example,
````text
brew install git
````

## GCC, G++ and CLang

MolTwister is written in C++. Hence, if GCC and G++ has not been installed, they must be installed. On Ubuntu, this can be done by
````text
sudo apt install gcc
sudo apt install g++
````
On MacOS, MolTwister will be compiled using CLang. If CLang is not installed. make sure it is installed. This can be done by installing XCode.

## CMake

To compile MolTwister, version 3.11, or greater, of CMake must be installed. On ubuntu, this can be done by
````text
sudo apt install cmake
````
It is also possible to simplify the compilation process by installing cmake-gui:
````text
sudo apt install cmake-gui
````

## The GNU Readline Library

MolTwister uses the GNU Readline Library. The source code and binary files, in addition to installation instructions, are available at
[The GNU Readline Library](http://www.gnu.org/software/readline/) home page. However, it is also possible (e.g., on Ubuntu) to install
the development version through a repository:
````text
sudo apt install libreadline-dev
````
The library is also avialable on repositories for MacOS and can be installed using Homebrew or MacPorts. For example,
````text
brew install readline
````

## Python/C

Python/C comes with Python and is used by MolTwister. On Ubuntu, this can be installed by
````text
sudo apt install python3
sudo apt install libpython3-dev
````
Note that the Python3 package usually comes pre-installed on newer versions of Ubuntu. For MacOS, the Python3
package can be downloaded from python.org. It may also be necessary to install XCode to obtain the appropriate
development libraries to compile Python/C specific code.

## OpenGL

MolTwister uses OpenGL. Hence, the OpenGL development packages must be installed. On Ubuntu, this can be done by 
````text
sudo apt install libopengl-dev
sudo apt-get install libglu1-mesa-dev freeglut3-dev mesa-common-dev
````
If XCode is installed on MacOS, then development libraries for both OpenGL and the GLUT libraries should already be installed.

## Compiling MolTwister

To compile MolTwister from the command line, goto the MolTwister source folder (where the CMakeLists.txt file resides). Then, type
````text
cmake CMakeLists.txt -DINCLUDE_CUDA_COMMANDS=OFF
make
````

## CUDA

To compile with GPU acceleration for the MD simulator in MolTwister, the latest CUDA development toolkit must be installed, together 
with appropriate drivers for the graphics card. To compile with cuda, use 
````text
cmake CMakeLists.txt -DINCLUDE_CUDA_COMMANDS=ON
make
````
There also exists a command line switch to enable debug code. This is achieved by adding
````text
-DCUDA_DBG_INFO=ON
````
to the cmake call above.

## Using QtCreator

It is recommended to use QtCreator as an IDE for the MolTwister project (since this has been tested extensively, both on MacOS and Ubuntu). The project is
opened by choosing "Open Project" and selecting the CMakeLists.txt file. The first time the project is opened, the IDE will ask to configure the project.
In that case, select "Desktop Qt x.y.z ..." (for MacOS choose the clang 64bit compiler). A file called CMakeLists.txt.user will be created. If it becomes
necessary to redo the configuration process, simply delete this file. To perform the cmake configuration right click the project and select "Run CMake" 
(usually happend automatically the first time the project is configured). After this, it is possible to right click and select "Build" or "Rebuild".

On the menu that by default appears at the left hand side of the IDE window, there are a few options ("Welcome", "Edit", "Debug", "Projects" and "Help").
Click on "Projects" and "Build" (under "Build & Run"). There, it is possible to select and deselect the INCLUDE_CUDA_COMMANDS and CUDA_DBG_INFO options.
Activate the selection by clicking "Apply Configuration Changes". Then, "Run CMake" and "Rebuild".

On MacOS, it is important to goto Projects > Buid & Run > Run, from the left hand menu bar and tick "Run in Terminal".

## Installation of MolTwister

If MolTwister was compiled from the command line, make sure the working folder is the folder containing the source code (same folder as pulled from Git and 
the same folder from where ````make```` was called). In case QtCreater was used to build MolTwister, goto the build output folder created by QtCreator.
Usually, the output folder is created one level below the source folder. However, this is configurable through the QtCreator user interface. Then execute
````text
sudo make install
````
This will install MolTwister as 'moltwister' under /usr/local/bin.

It should now be possible to execute the ````moltwister```` command from any folder from within the command prompt, to start MolTwister.
