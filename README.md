# tev — The EXR Viewer

[![Travis Build Status](https://travis-ci.org/Tom94/tev.svg?branch=master)](https://travis-ci.org/Tom94/tev)
[![Appveyor Build Status](https://ci.appveyor.com/api/projects/status/y9pdd25v9b8s5cv1/branch/master?svg=true)](https://ci.appveyor.com/project/Tom94/tev)

A high dynamic range (HDR) image comparison tool for graphics people. __tev__ allows viewing images through various tonemapping operators and inspecting the values of individual pixels. Often, it is important to find exact differences between pairs of images. For this purpose, __tev__ allows rapidly switching between images and visualizing various error metrics (L1, L2, and relative versions thereof). To avoid clutter, images and their layers can be filtered by keywords.

While the predominantly supported file format is OpenEXR certain other types of images can also be loaded. The following formats are currently supported:
- __EXR__ (via [OpenEXR](https://github.com/wjakob/openexr))
- __PFM__ (compatible with [Netbpm](http://www.pauldebevec.com/Research/HDR/PFM/))
- __HDR__, BMP, GIF, JPEG, PIC, PNG, PNM, PSD, TGA (via [stb_image](https://github.com/wjakob/nanovg/blob/master/src/stb_image.h))
    - stb_image only supports [subsets](https://github.com/wjakob/nanovg/blob/master/src/stb_image.h#L23) of each of the aforementioned file formats.
    - Low-dynamic-range (LDR) images are "promoted" to HDR through the reverse sRGB transformation.

## Screenshot

![Screenshot](https://raw.githubusercontent.com/Tom94/tev/master/resources/screenshot.png)
_A false-color comparison of two multi-layer OpenEXR images of a beach ball. Image courtesy of [openexr-images](https://github.com/openexr/openexr-images)._

## Usage

### Graphical User Interface

Images can be opened via a file dialog or simply by dragging them into __tev__. They can be reloaded, closed, or filtered at any time, so opening more images than exactly needed is encouraged.

Select an image by left-clicking it, and optionally select a reference image to compare the current selection to by right-clicking. The current selection can be moved with the Up/Down or the 1-9 keys. For a comprehensive list of keyboard shortcuts click the little "?" icon at the top or press "h" on your keyboard.

If the interface seems overwhelming, simply hover over any control to view an explanatory tooltip.

### Command Line

__tev__ understands positional command-line arguments (with and without wildcards).
```sh
$ tev foo.exr bar.exr
```

The filenames can also be supplied using a pipe.

By default, all layers and channels are loaded, but individual layers or channels can also be specified. In the following example, the *depth* layer of *foo.exr* and the *r*, *g*, and *b* channels of *foo.exr* and *bar.exr* are loaded.
```sh
$ tev :depth foo.exr :r,g,b foo.exr bar.exr
```

Other command-line arguments exist (e.g. for starting __tev__ with a pre-set exposure value). For a list of all valid arguments simply invoke
```sh
$ tev -h
```

## Obtaining tev

### macOS / Windows

Pre-built binaries for Windows (32-bit and 64-bit) and macOS (64-bit) are available on the [releases page](https://github.com/Tom94/tev/releases).

### Linux

- Archlinux: available on the [Arch User Repository](https://aur.archlinux.org/packages/tev/)

## Building tev

All that is required for building __tev__ is a C++11-compatible compiler. Begin by cloning this repository and all its submodules using the following command:
```sh
$ git clone --recursive https://github.com/Tom94/tev
```

If you accidentally omitted the `--recursive` flag when cloning this repository you can initialize the submodules like so:
```sh
$ git submodule update --init --recursive
```

__tev__ uses [CMake](https://cmake.org/) as its build system. The following sections detail how it should be used on various operating systems.

### macOS / Linux

On macOS and most Linux distributions [CMake](https://cmake.org/) can be obtained via a package manager ([Homebrew](https://brew.sh/) on macOS, apt on Ubuntu/Debian, etc.). Most Linux distributions additionally require _xorg_, _gl_, and _zlib_ development packages and _zenity_. On Ubuntu/Debian simply call
```sh
$ apt-get install cmake xorg-dev libglu1-mesa-dev zlib1g-dev zenity
```

Once all dependencies are installed, create a new directory to contain build artifacts, enter it, and then invoke [CMake](https://cmake.org/) with the root __tev__ folder as argument as shown in the following example:
```sh
$ mkdir build
$ cd build
$ cmake ..
```

Afterwards, __tev__ can be built and installed via
```sh
$ make -j
$ make install
```

### Windows

On Windows, install [CMake](https://cmake.org/download/), open the included `cmake-gui` application, and point it to the root directory of __tev__. CMake will then offer a selection of compilers. Visual Studio 2019 is officially supported, but __tev__ likely also compiles using Visual Studio 2017 and 2015.

## License

__tev__ is available under the BSD 3-clause license, which you can find in the `LICENSE.md` file. [TL;DR](https://tldrlegal.com/license/bsd-3-clause-license-(revised)) you can do almost whatever you want as long as you include the original copyright and license notice in any copy of the software and the source code.
