About this Repository
-------------------
This repository is based from the NIFTIlib project. It contains changes to allow for building a static/shared library in Windows using Microsoft Visual Studio.

Nifti-1 C libraries
-------------------

Version 2.0.0 beta release Jul  2010
Version 1.1.0 beta release Aug  2008
Version 1.0.0 beta release Dec  2007
Version 0.6 beta release Aug  2007
Version 0.5 beta release May  2007
Version 0.4 beta release Sept. 2006
Version 0.3 beta release April 2006
Version 0.2 beta release August 12, 2005
Version 0.1 beta release March 11, 2005

niftilib code is released into the public domain.


Library directories
-------------------
znzlib   -- low level library for handling read/write of compressed files.

niftilib -- core i/o routines for reading and writing nifti-1 format files.
	    Primarily routines to read/write and manipulate the header field
	    information, including orientation matrices.  Volume-wise,
            timecourse-wise, access to image data.  

nifticdf -- functions to compute cumulative distributions and their inverses

fsliolib -- i/o routines for reading and writing nifti-1 format files, higher
            level than niftilib, includes routines for reading the data blob by
            volume, timecourse, etc., and, addresses image orientation issues.
            *** work in progress, subject to significant revision.....

utils    -- directory containing library utility programs (nifti_tool)




Destination directories
-----------------------
bin      -- destination directory for installed programs
include  -- destination directory for library header files
lib      -- destination directory for compiled libraries
docs     -- destination directory Doxygen html (created via "make doc")



Example directories
-------------------
examples  -- directory containing sample code using nifti reference library
real_easy -- code snippets to read nifti-1 files, not using nifti ref. lib.


Other directories
-----------------
Testing   -- directory containing code to test the libraries
packaging -- spec file for building RPMs, and template package
             description for Dev-Cpp (http://www.bloodshed.net/devcpp.html)



Instructions to build
---------------------
### Linux
There is a Makefile for the project precompiled which can be used.
Run the following command to make the library:
```
make all
```
The results will be in the directories: bin/ include/ lib/

**Note:** If the Makefile causes errors, then you should use CMake to build the library. The details are given below.

### Windows
CMake is the tool that will be used to build the library on Windows.
Follow these instructions to build using CMake on Windows:

1. Start CMake-gui.
2. Select the source folder as the NIFTIclib directory.
3. Select the build folder to be an empty folder where the results of the build will be contained. 
  * The recommended location is the folder build within the source folder. 
4. Click Configure
  * __Note:__ A message box may appear asking if you want to create the build directory. Select Yes.
5. Next, a dialog box should appear requesting the desired generator to use to build the library. Select the compiler you want to use to build this library. 
  * Visual Studio has 32-bit and 64-bit compilers so make sure to select the desired one. (Win64 is for 64-bit) 
6. If everything was successful, then you should have a bunch of red background items in a list. These are the configuration options for building the application. Many of these can be left untouched but I will cover the more common ones you should check.
  * BUILD_SHARED_LIBS - If checked, this creates a shared library. Otherwise, the library is linked statically.
  * CMAKE_INSTALL_PREFIX - When compiling the library, there is usually an install phase where it will take the necessary binaries/libraries and place them in one install directory. This provides a clean directory with just the necessary items from the build. Change this to the location where you would like to keep that information.
  * ZLIB_INCLUDE_DIR and ZLIB_LIBRARY_DEBUG or ZLIB_LIBRARY_RELEASE - Zlib is a third-party library very commonly used for compression of data. It is used in NIFTIclib for extracting GZ files. CMake will make an attempt to search for a Zlib library but it may be unsuccesful. You will want to make sure that the INCLUDE_DIR is set to a valid directory and that either ZLIB_LIBRARY_DEBUG or ZLIB_LIBRARY_RELEASE is set to a valid library file (file extensions: *.lib, *.so, *.a).
7. Once the configuration values are set to desired values, click Configure again to finalize any changes. 
8. Click Generate and this will create the project that is used to make the libraries.
9. This part is compiler specific but I will give a general outline of what needs to be done.
  * In Cygwin, MinGW, and GCC compilers, you will redirect to the build directory and run these commands:
  ```
  make
  make install  (Installs in CMAKE_INSTALL_PREFIX)
  ```
  * In Visual Studio, you will open up a solution containing all of the projects that will be built. Click Build -> Build Solution. This should build the project. Afterwards, go to the INSTALL project in Visual Studio, right-click and select Build. This will install the libraries in the CMAKE_INSTALL_PREFIX.
  
Done! The results will be in the directories: bin/ include/ lib/ of the build directory. It will also be located in the CMAKE_INSTALL_PREFIX if the install command was run.
  
  
Using the Library in Projects
---------------------
Using NIFTIclib in projects is like using any other static/shared library in a project. You include the header files in your project and use them as needed. Next, you link to the library file (.lib, .so, .a, etc). Finally, if a shared library was built, the DLLs are what will contain the actual information. DLLs are searched for at runtime. The necessary DLLs will need to be in your PATH variable or in the directory of the executable.

**Note: ** If you are using static libraries in your project, make sure to define `NIFTILIB_STATIC` before including ANY header file. It is usually best to define NIFTILIB_STATIC in the project settings so that it will be done automatically.

For more information
--------------------

See the niftilib webpage at http://niftilib.sourceforge.net/

See the NIFTI webpage at http://nifti.nimh.nih.gov/

