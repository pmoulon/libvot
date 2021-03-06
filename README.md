#libvot - A C++11 multi-thread library for image retrieval

[![Join the chat at https://gitter.im/hlzz/libvot](https://badges.gitter.im/hlzz/libvot.svg)](https://gitter.im/hlzz/libvot?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)
[![Build Status](https://travis-ci.org/hlzz/libvot.svg?branch=master)](https://travis-ci.org/hlzz/libvot) 
[![Build Status](https://travis-ci.org/hlzz/libvot.svg?branch=feature)](https://travis-ci.org/hlzz/libvot) 
[![License](https://img.shields.io/badge/license-BSD-blue.svg)](LICENSE)
<a href="https://scan.coverity.com/projects/hlzz-libvot">
  <img alt="Coverity Scan Build Status"
       src="https://scan.coverity.com/projects/8983/badge.svg"/>
</a>
[![todofy badge](https://todofy.org/b/hlzz/libvot/master)](https://todofy.org/r/hlzz/libvot/master)
###[中文简介](doc/README_Chinese.md)

##Introduction
*libvot* is a fast implementation of vocabulary tree, which is an algorithm widely used in image retrieval and computer vision. It usually comprises three components to build a image retrieval system using vocabulary tree: build a k-means tree using sift descriptors from images, register images into the database, query images against the database. In this library, we use C++11 standard multi-thread library to accelerate the computation, which achieves fast and accurate image retrieval performance. Currently this library is under active development for research purpose. If you find this repository useful, please star it to let me know. :)

##Installation
The build system of libvot is based on [CMake](http://cmake.org). To take full advantages of the new features in C++11, we require the version of CMake to be 2.8.12 or above. Current we have tested our project under Linux (Ubuntu 14.04, CentOS 7) and MacOS (10.10) using gcc. The common steps to build the library are:

1. Extract source files.
2. Create build directory and change to it.
3. Run CMake to configure the build tree.
4. Build the software using selected build tool.
5. Run "make test"
6. See src/example for the usage of this library.

On Unix-like systems with GNU Make as the build tool, the following sequence of commands can be used to compile the source code.

    $ cd libvot
    $ git submodule init & git submodule update  
    $ mkdir build && cd build
    $ cmake ..
    $ make && make test

Optional dependencies include *OpenCV*, but it is not required to run the core functions in *libvot*.

##First try
Suppose `$LIBVOT_ROOT` represents the root directory of libvot, and it is successfully compiled in `build` subdirectory. You can use `./libvot_feature <image_list>` to first generate a set of descriptor files and use them as inputs to `image_search`. For example, you have some target .jpg image files to generate sift files. Just `cd` into that directory, prepare the `image_list`, and generate sift files in the same directory as the image files:

    $ ls -d $PWD/*.jpg > image_list
    $ $LIBVOT_ROOT/build/bin/libvot_feature <image_list>

Then you can run *image_search* in src/example to generate the image retrieval results
The usage is simply “./image_search <sift_list> <output_dir> [depth] [branch_num] [sift_type] [num_matches] [thread_num]”. 
We add a small image dataset [fountain-P11](http://cvlabwww.epfl.ch/data/multiview/denseMVS.html) to illustrate this process. 
*test_data/fountain_dense* folder contains the sift files generated by `libvot_feature`, while the original images are not included in order to save space. 
If you use the out-of-source build as shown in the installation section and in the *build* directory, 
the following command should work smoothly and generate several output files in `build/bin/vocab_out` directory (you need to change the prefix of filepaths in `test_data/fountain_dense/sift_list`).

    $ cd bin
    $ ls -d $PWD/*.sift > sift_list
    $ ./image_search <sift_list> <output_folder> [depth] [branch_num] [sift_type] [num_matches] [thread_num]  
    $ (e.g.) ./image_search ../../test_data/fountain_dense/sift_list ./vocab_out

Each line in *match.out* contains three numbers “first_index second_index similarity score”. 
Since the library is multi-threaded, the rank is unordered with respect to the first index (they are ordered w.r.t the second index). 
*match_pairs* saves the ordered similarity ranks, from *0*th image to *n-1*th image. Besides, libvot also supports sift files generated by [openMVG](https://github.com/openMVG/openMVG/) (set [sift_type] to 1).

## Documentation
The [homepage](http://hlzz.github.io/libvot/) of libvot is hosted by github-pages. See the documentation [here](http://hlzz.github.io/libvot/doc/html/index.html).

## Contributing
We are working toward the next major release (0.2.0). 
If you are interested in contributing, please have a look at [Roadmap.md](Roadmap.md). 
All types of contributions, including documentation, testing, and new features are welcomed and appreciated.

##License
The BSD 3-Clause License

##Contact and Donation
For inquiries and suggestions, please send your emails to <tshenaa@ust.hk>.

If you would like to support this project, you can contribute to this project, or make a donation via [pledgie](https://pledgie.com/campaigns/30901). Thanks

<a href='https://pledgie.com/campaigns/30901'><img alt='Click here to lend your support to: Open-Source Image Retrieval Project and make a donation at pledgie.com !' src='https://pledgie.com/campaigns/30901.png?skin_name=chrome' border='0' ></a>
