Image Clipper
-------------

This is a fork of the [imageclipper](https://code.google.com/p/imageclipper/)
program initially written by Naotoshi Seo.

This fork was made initially for myself to be able to compile the program on OSX.
Since I used [CMake](http://www.cmake.org/) it was easy to make one build that
works on all platforms.

From the original description:

> It is often required to crop images manually fast for computer vision researchers
> to gather training and testing image sets, e.g., cropping faces in images.
>
> This simple multi-platform (Windows, OSX and Linux were verified) software helps
> you to clip images manually fast.

## Building

To build you need [CMake](http://www.cmake.org/), [OpenCV](http://opencv.org/) and [Boost](http://www.boost.org/)
on your system

### Linux & OSX

Install requirements using your favorite package system or download the sources yourself.

```bash
$ sudo apt-get install cmake libopencv-dev libboost-all-dev
```

Using [MacPorts](http://www.macports.org/).

```bash
$ sudo port install cmake boost opencv
```

And then build:

```bash
$ git clone <repo url>
$ cd imageclipper
$ mkdir build && cd build
$ cmake ..                                     # This should work in most cases.

# (Optional) Or specifying your own builds of OpenCV or Boost.
$ cmake -DOpenCV_DIR=/path/to/opencv/build/ -DBOOST_ROOT=/path/to/boost ..

$ cmake --build .                              # Builds the program.
```

### Windows

On windows you need to download [Visual Studio for Windows Desktop](http://www.visualstudio.com/), [CMake](http://www.cmake.org/cmake/resources/software.html) and [git](http://git-scm.com/).

###### Boost

Has been tested with this verison on Windows 7, but should work with whatever you like:
[Boost 1.55.0 32-bit MSVC12](http://sourceforge.net/projects/boost/files/boost-binaries/1.55.0-build2/)
The default unpack location is **c:\local\boost_1_55_0**, if you change it you need to know the path when building.

###### OpenCV

Download and unpack [OpenCV](http://opencv.org/downloads.html) and remember the path.

From the **git bash** console:
(use cmd.exe if you prefer)

```bash
$ git clone <repo url>
$ cd imageclipper
$ mkdir build && cd build
$ cmake -DOpenCV_DIR=/path/to/opencv/build/ -DBOOST_ROOT=/c/local/boost_1_55_0/ ..
$ cmake --build . --config Release 
$ start imageclipper.sln # If you want to open in Visual Studio instead.
```

The executable can then be found under **build/bin/Release/imageclipper.exe**


Application Usage
(Windows) Drag and drop a folder or an image file or a video file on imageclipper.exe.

(Windows and Linux) Execute as a command line tool. See Command Usage section too.



Mouse Usage
    Left  (select)          : Select or initialize a rectangle region.
    Right (move or resize)  : Move by dragging inside the rectangle.
                            : Resize by draggin outside the rectangle.
Keyboard Usage
    s (save)                : Save the selected region as an image.
    f (forward)             : Forward. Show next image.
    SPACE                   : Save and Forward.
    b (backward)            : Backward. 
    q (quit) or ESC         : Quit.
    r (rotate) R (counter)  : Rotate rectangle in clockwise.
    e (expand) E (shrink)   : Expand the recntagle size.
    h (left) j (down) k (up) l (right) : Move rectangle. (vi-like keybinds)
    y (left) u (down) i (up) o (right) : Resize rectangle (Move right-bottom boundaries).
    n (left) m (down) , (up) . (right) : Shear deformation.
Saved file format
The clipped images are saved as

<dirname>/imageclipper/<basename>_<rotation_degree>_<left_x>_<top_y>_<width>_<height>.png
such as

../myimages/imageclipper/lena.jpg_0359_0109_0093_0065_0090.png
where the original filename is

../myimages/lena.jpg. 
A directory named "imageclipper" is created under the image file's directory and cropped images are stored on there.

This filename format can be configured via command line options. See Command Usage section.

Experimental Feature (Continuous Color Region)
You can choose a rectangular region surrounding a continuous color region.

    Middle or SHIFT + Left  : Initialize the watershed marker. Drag it.
You can use Right mouse and hotkeys to move or resize the watershed marker.



Command Usage
ImageClipper - image clipping helper tool.
Command Usage: imageclipper.exe [option]... [reference]
  <reference = .>
    <reference> would be a directory or an image or a video filename.
    For a directory, image files in the directory will be read sequentially.
    For an image (a file having supported image type extensions listed below),
    it starts to read a directory from the specified image file.
    For a video (a file except images), the video is read.

  Options
    -f <output_format = imgout_format or vidout_format>
        Determine the output file path format.
        This is a syntax sugar for -i and -v.
        Format Expression)
            %d - dirname of the original
            %i - filename of the original without extension
            %e - filename extension of the original
            %x - upper-left x coord
            %y - upper-left y coord
            %w - width
            %h - height
            %r - rotation degree
            %. - shear deformation in x coord
            %, - shear deformation in y coord
            %f - frame number (for video)
        Example) ./$i_%04x_%04y_%04w_%04h.%e
            Store into software directory and use image type of the original.
    -i <imgout_format = %d/imageclipper/%i.%e_%04r_%04x_%04y_%04w_%04h.png>
        Determine the output file path format for image inputs.
    -v <vidout_format = %d/imageclipper/%i.%e_%04f_%04r_%04x_%04y_%04w_%04h.png>
        Determine the output file path format for a video input.
    -h
    --help
        Show this help

  Supported Image Types
      bmp|dib|jpeg|jpg|jpe|png|pbm|pgm|ppm|sr|ras|tiff|exr|jp2
You may make a batch file (Windows) or a shell script or an alias (Linux) to setup default values. Use of -i and -v rather than -f would be useful in that case.

How to Compile on Linux
You need to install OpenCV and Boost libraries.

Install OpenCV
Download OpenCV source codes (.tar.gz)

root user

tar zxvf opencv-1.0.0.tar.gz; cd opencv-1.0.0
./configure
make
make install
general user

tar zxvf opencv-1.0.0.tar.gz; cd opencv-1.0.0
./configure --prefix=$HOME/usr
make
make install
export PATH=$HOME/usr/bin/:$PATH
export LD_LIBRARY_PATH=$HOME/usr/lib:$LD_LIBRARY_PATH
export PKG_CONFIG_PATH=$HOME/usr/lib/pkgconfig:$PKG_CONFIG_PATH
export MANPATH=$HOME/usr/man:$MANPATH
Install Boost
root user and Red Hat (Fedora)

yum install boost
yum install boost-dev
root user and Debian (Ubunts)

apt-get install boost
apt-get install boost-dev
general user

Download Boost source codes (.tar.gz)

tar zxvf boost_1_36_0.tar.gz; cd boost_1_36_0
./configure --prefix=$HOME/usr
make install
Reference: Boost Getting Started on Unix Variants

Compile
Unarchive imageclipper.zip. Go to src/ directory. A Makefile is attached. Modify as follows:

If you installed boost not on $HOME/usr/: modify ~/usr/ to your path such as /usr/
If your boost version is not boost_1_36_0: modify boost-1_36 to your version.
Modify -lboost_system-gcc41-mt -lboost_filesystem-gcc41-mt to yours. Find names by $ ls /path/to/yourboost/lib. If you find names as libboost_filesystem-gcc41-mt, then => -lboost_filesystem-gcc41-mt
$ make check
I don't know why boost library uses different names under different system. It requires us to modify Makefile as above in different system.... anyway,

Finally

make
You should get an executable file "imageclipper".

How to Compile on Windows
A binary file for Windows is already attached. This section explains how to compile for those who want to modify source codes and create a modified software.

You need to install OpenCV and Boost libraries.

Install OpenCV
Download OpenCV installer and install it. I assume you have installed onto C:\Program Files\OpenCV.

Install Boost
Download boost installer and install it. I assume you have installed onto C:\Program Files\boost\boost_1_35_0.

Reference: Boost Getting Started on Windows

Setup MS Visual C++
I write details based on MS Visual Studio 2005. Change boost_1_35_0 if your version is different. Follow menus of MS Visual C++ window as:

Tools > Options > Projects and Solutions > VC++ directories >
Show Directory for: > Include Files. Now add
C:\Program Files\boost\boost_1_35_0
C:\Program Files\OpenCV\cv\include
C:\Program Files\OpenCV\cvaux\include
C:\Program Files\OpenCV\cxcore\include
C:\Program Files\OpenCV\otherlibs\highgui
Show Directory for: > Library Files. Now add
C:\Program Files\boost\boost_1_35_0\lib
C:\Program Files\OpenCV\lib
Compile
Unarchive imageclipper.zip and open src\imageclipper.sln, then Build for Release.

Creating a text file to locate clipped region
A text file containing locations of clipped region is sometimes required rather than clipped images themselves, e.g., as a ground truth text for testing experiments in computer vision.

Assume there exist files as follows:

$ \ls imageclipper/*_*_*
image.jpg_0068_0047_0089_0101.png
image.jpg_0087_0066_0090_0080.png
image.jpg_0095_0105_0033_0032.png
image.jpg_0109_0093_0065_0090.png
image.jpg_0117_0097_0052_0095.png
By executing the following command,

$ find imageclipper/*_*_* -exec basename \{\} \; | perl -pe \
's/([^_]*).*_0*(\d+)_0*(\d+)_0*(\d+)_0*(\d+)\.[^.]*$/$1 $2 $3 $4 $5\n/g' \
| tee clipping.txt
you can obtain a text file "clipping.txt" as follows:

image.jpg 68 47 89 101
image.jpg 87 66 90 80
image.jpg 95 105 33 32
image.jpg 109 93 65 90
image.jpg 117 97 52 95
I think this file format is a typical format for testing experiments in many computer vision tools.

For OpenCV haartraing Users
Use haartrainingformat.pl as

$ perl haartrainingformat.pl clipping.txt | tee info.dat
where clipping.txt is the text file created at the previous section to create a OpenCV haartraining suitable format such as

image.jpg 5 68 47 89 101 87 66 90 80 95 105 33 32 109 93 65 90 117 97 52 95
Or you can generate a file without creating "clipping.txt" as

$ \ls imageclipper/*_*_* | perl haartrainingformat.pl --ls --trim --basename | tee info.dat
Note that clipped images themselves are enough and better for training phase (See Tutorial: OpenCV haartraining). Therefore, you may use this only for creation of testing data.

Usage

Helper tool to convert a specific format into a haartraining format.
Usage: perl haartrainingformat.pl [option]... [filename]
    filename
        file to be read. STDIN is also supported.
Options:
    -l
    --ls
        input is an output of ls.
    -t
    --trim
        trim heading 0 of numbers like from 00xx to xx.
    -b
    --basename
        get basenames from filenames.
    -h
    --help
        show this help.
        
