# Introduction #

This page will provide you detailed steps to build BTK for Octave under Windows, Linux or MacOS X.

# Prerequisites #

You need to get and install [CMake](http://www.cmake.org) and [Octave](http://www.gnu.org/software/octave/). For Linux, the package manager of your distribution should contain them.

You have also to download the sources of BTK which are available in the Downloads section of this website. The file to download is named BTK-x.y.z\_src.zip, where x.y.z is the release number (e.g. BTK-0.1.9\_src.zip). Then, decompress it in a temporary folder easily accessible as you will use it later (for example on the desktop or your download folder).

## Configure your environment to use MinGW (Windows only) ##

Because under Windows, Octave is compiled with MinGW, it is important to use the same compiler for wrapping BTK, otherwise it won't be compatible and the compiling will fail. Luckily, Octave embeds MinGW, but the environment (the OS) doesn't know where it is.

You have to set the PATH environment variable to include the minGW compiler.

**WARNING**: You have to be careful! You need to append the path, not to replace the content of the variable!

For this, right-click on the "Computer" icon, select "Properties", then select the tab "Advanced" and click on the button "Environment Variables". In the system section, scroll to find the variable "PATH", edit it (double click on the value) and APPEND the path of MinGW which is located in `<octave path here>\mingw32\bin`. (don't forget the semicolon before to add the path of MinGW). Click on the button "OK", and apply the modification by clicking on the button "Apply"

These instructions can be summarized as the following:
Set the PATH Environment Variable to include the minGW compiler Computer --> Properties --> Advanced ... -> Environment Variables.   System section:  PATH:  append: `; <octave path here>\mingw32\bin`.

# Configuring CMake #

## Set the source code and the build folder ##
Open CMake (cmake-gui) and enter the path containing the source of BTK for the field "Where is the source code" (e.g. `C:/Users/jdoe/Download/BTK`). Then for the field "Where to build the binaries", copy/paste the same path than the source code and add a new subfolder's name (e.g.
`C:/Users/jdoe/Download/BTK/build-octave`).

## Configure the project ##
You can now click on the "Configure" button. CMake should ask you to create the build folder and then ask you to select the generator of project.

Under Linux and MacOS X, select `Unix Makefiles` and click on the button "Done".

Under Windows, select "MinGW Makefiles" and select the second radio button ("Specify native compilers"). Then click on "Continue" to set the compiler.

### Set the MinGW compiler binaries (Windows only) ###
You need here to set the path for the C/C++/Fortran compiler. Each of these paths must be exact as CMake test the binaries to be sure they are valid compilers.

Depending of the version of Octave installed, the release number of the compilers change, but they follow the same syntax each time. For example if you have Octave 3.2.4 compiled with GCC 4.4.0 in `C:\Octave`, then the subfolder will be `3.2.4_gcc-4.4.0`. These last numbers are also used by MinGW. For each field, click on the "..." button and go to the path of Octave and look for the subfolder `mingw32/bin`. Then you will find some programs starting by `mingw32-`. For the C compiler, select the program starting by `mingw32-gcc-`. For the C++ compiler, select the program starting by `mingw32-g++-`. And for the Fortran compiler, select the program starting by `mingw32-gfortran-`.
In the given example (Octave 3.2.4 compiled with GCC 4.4.0), the binaries to give to CMake are:
  * C compiler: `C:/Octave/3.2.4_gcc-4.4.0/mingw32/bin/mingw32-gcc-4.4.0-dw2.exe`
  * C++ compiler: `C:/Octave/3.2.4_gcc-4.4.0/mingw32/bin/mingw32-g++-4.4.0-dw2.exe`
  * Fortran compiler: `C:/Octave/3.2.4_gcc-4.4.0/mingw32/bin/mingw32-gfortran-4.4.0-dw2.exe`

You can now click on the "Done" button.

## Configure the project to compile BTK for Octave ##
CMake detects the C++ compiler and its available options. At the end, some red lines are displayed. They contains the possible configuration of BTK. For all of these options, you can check the [general documentation](http://b-tk.googlecode.com/svn/doc/API/0.1/_build_instructions.html) of BTK. For Octave, the most important is to select the option BTK\_WRAP\_OCTAVE and be sure that the option CMAKE\_BUILD\_TYPE is set to Release (and not Debug), otherwise, you have to type it.

Click on the "Configure" button and Octave should be found automatically. If it is not the case, you have to set manually the path into the new variable named OCTAVE\_ROOT (in red). The given path must correspond to the root of Octave where you can find the folder `bin` (for example `C:/Octave/3.2.4_gcc-4.4.0` or it could be also `C:\Octave\Octave3.4.3_gcc4.5.2\Octave3.4.3_gcc4.5.2\`).

By default, BTK is installed in the folder `/usr/local/(share|include|lib)` for Linux and MacOS X and under `C:\Program files` or `C:\Program files (x86)` for Windows. If you prefer to install BTK elsewhere, you have to modifiy the content of the variable CMAKE\_INSTALL\_PREFIX

You have to click again on the button "Configure" to create the final configuration (no more red lines). Finally, click on the button "Generate" to generate the Makefiles.

# Code generation #
From this point, you need to open a terminal (Linux / MacOS X) or a command prompt (Windows).
  * Under Ubuntu, the terminal is availble in the submenu "Accessories".
  * Under MacOS X, it is in the folder `/Applications/Utilities`.
  * Under Windows XP and Vista, you can click on the button Start, click on run and type `cmd`.
  * Under Windows 7, you need administrator privileges. Type `cmd` in the Start-->Run box, then wait for `cmd.exe` to show up as an option. Right-click `cmd.exe` and choose "run as administrator".

Change the directory to select the build directory (where the Makefile is). For example, use the command (`cd C:/Users/jdoe/Download/BTK/build-octave`). In most of the OS, you can also drag and drop the build direction to the terminal. Then, you have only to type `cd`, put a space, and drop the build directory.

Compile the code by typing `make` for Unix user (Linux / MacOS X). For Windows user, type `mingw32-make`.

After several time, the Octave functions are compiled and ready to be installed!

To install the function as well as the documentation, Unix users have to type `sudo make install` and give their password. The file will be installed in `/usr/local/share/btk-x.y` (where x.y correspond to the major and minor revision of BTK). For the Windows users, you need to type `mingw32-make install`

That's it!

# Extras #

To add the path of the `btk` toolbox into Octave, you can launch Octave and type the command `addpath('C:\Program Files\BTK\share\btk-0.1\Wrapping\Octave\btk')`. The given path must be the path containing the functions and the documentation.

More specific to Octave and tested only under Windows 7, you can use these commands in Octave when you load it for the first time:
```
pkg rebuild -auto
pkg rebuild -noauto ad windows
pkg rebuild -auto java
```
They will update octave\_packages database with your installation tree
and auto-load most packages (excluding 'ad' and 'windows' which may crash octave when loaded and 'clear all' is executed).

Note: Explanation found in the Octave-help mailing list in the thread [how to instal, or upgrade, the latest octave version on windows 7](http://octave.1599824.n4.nabble.com/how-to-instal-or-upgrade-the-latest-octave-version-on-windows-7-td4165440.html)

Finally, you can create and edit the .octaverc file(s), e.g.:
`C:\Users\Peter\.octaverc`, `C:\Octave\Octave3.4.3_gcc4.5.2\Octave3.4.3_gcc4.5.2\share\octave\site\m\startup\octaverc` and add inside startup functions - e.g. MATLAB startup script, cd to preferred folder, etc.

# Acknowledgments #
Thanks to Peter Gabriel Adamczyk for his help to create this documentation and also Martin Felis for the creation of the patch used to provide the wrapping of BTK in Octave.