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

# What You Can Do
Open images in a directory sequentially
Open a video, frame by frame
Clip (save) and go to the next image by one button (SPACE)
Move and resize your selected region by vi-like hotkeys or right mouse button
Rotate and shear deform (affine transform) the rectangle region


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

