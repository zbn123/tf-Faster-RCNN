# tf-Faster-RCNN: *Work in progress (both the code and this README)*
A Python 3.5 + TensorFlow implementation of Faster R-CNN ([paper](https://arxiv.org/abs/1506.01497)). See official implementations here:
- [Python + Caffe](https://github.com/rbgirshick/py-faster-rcnn)
- [MATLAB + Caffe](https://github.com/ShaoqingRen/faster_rcnn)

The deep models in this implementation are built on [TensorBase](https://github.com/dancsalo/TensorBase), a minimalistic framework for end-to-end TensorFlow applications created by my good friend and collaborator [Dan Salo](https://github.com/dancsalo). Check it out. [My personal fork](https://github.com/kevinjliang/TensorBase) (whose changes are typically regularly pulled into Dan's) is a submodule of this tf-Faster-RCNN repo.

### Contents
1. [Requirements: Software](#requirements-software)
2. [Installation](#installation)
3. [Repo Organization](#repo-organization) 
4. [Training and Testing](#training-and-testing)


### Requirements: Software
1. Python 3.5: I recommend Anaconda for your Python distribution and package management. See (2) below.
2. TensorFlow v0.12: See [TensorFlow Installation with Anaconda](https://www.tensorflow.org/get_started/os_setup#anaconda_installation). Note: upgrading to TensorFlow v1.0 is on our to-do list.
3. Some additional python packages you may or may not already have: `cython`, `easydict`, `scipy`, `pickle`, `Pillow`, `tqdm`. These should all be pip installable within your Anaconda environment (pip install [package]) 
4. TensorBase: Tensorbase is used as a submodule, so you can get this recursively while cloning this repo. See [Installation](#installation) below.


### Installation
1. Clone this repository (tf-Faster-RCNN) 
  ```Shell
  # Make sure to clone with --recursive. This'll also clone TensorBase
  git clone --recursive https://github.com/kevinjliang/tf-Faster-RCNN.git
  ```
  
2. We'll call the directory that you cloned tf-Faster-RCNN into `tf-FRC_ROOT`

   *Ignore notes 1 and 2 if you followed step 1 above.*

   **Note 1:** If you didn't clone tf-Faster-RCNN with the `--recursive` flag, then you'll need to manually clone the `TensorBase` submodule:
    ```Shell
    git submodule update --init --recursive
    ```
    **Note 2:** The `TensorBase` submodule needs to be on the `faster-rcnn` branch (or equivalent detached state). This will happen automatically *if you followed step 1 instructions*.

3. Build the Cython modules
  ```Shell
  cd $tf-FRC_ROOT/Lib
  make
  ```


### Repo Organization
- Data: Scripts for creating, downloading, organizing datasets. For your local copy, the actual data also resides here
- Development: Experimental code still in development
- Lib: Library functions necessary to run Faster R-CNN
- Models: Runnable files that create a Faster R-CNN class with a specific convolutional network and dataset
- Network: Neural networks or components that form parts of a Faster R-CNN network


### Training and Testing
If you would like to try training and/or testing the Faster R-CNN network, we currently have models available for translated and cluttered MNIST. Translated MNIST is an MNIST digit embedded into a larger black image. Cluttered MNIST is the same, but with random pieces of other MNIST digits scattered throughout. Both serve as simple datasets for detection, as the algorithm must find the digit and classify it.

To run one of these models:

1. Generate the data:
  ```Shell
  cd $tf-FRC_ROOT/Data/scripts
  
  # This will create a folder $tf-FRC_ROOT/Data/data_[trans/clutter] with the images and bounding box data
  python [trans/clutter]MNIST.py
  ```
2. Run the model:
  ```Shell
  cd $tf-FRC_ROOT/Models
  
  # Change flags accordingly
  python faster_rcnn_[trans/clutter].py -n [Model num, ex 1] -e [# of epochs, ex 3]
  ```
  
3. To reload a previously trained model and test
  ```Shell
  python faster_rcnn_[trans/clutter].py -r 1 -m [Model num] -f [epoch to restore] -t 0
  ```
