# ORML

**Note**: this documentation can also be read on [orml.openrndr.org](https://orml.openrndr.org)

ORML (OPENRNDR Machine Learning) is the machine learning part of OPENRNDR. Within OPENRNDR you can easily connect to a number of widely used machine learning models, such as Facemesh, Posenet and Style transfer networks. You can use OPENRNDR to visualise the data coming from these models in order to create compelling (interactive) experiences. The ORML library includes both models and interface code to make the use of those models simple.

ORML is built on top of [orx-tensorflow](https://github.com/openrndr/orx/tree/master/orx-jvm/orx-tensorflow) which is an OPENRNDR extra that provides tools to wrap and convert between Tensorflow and OPENRNDR primitives. This extra in turn relies on [Tensorflow/Java](https://github.com/tensorflow/java) which are the official Java bindings to Tensorflow 2.x

ORML and its underlying software stack is demonstrated to work on computers running Linux, macOS, and Windows.

## How do I use ORML?
The ORML repository comes with demos included, the easiest way to run the demos is to clone the repository, import the Gradle project into IntelliJ IDEA. The demos reside in the directory src/demo/kotlin of the ORML modules.

To run the demos with a GPU based Tensorflow back-end make sure you have installed CUDA properly and that `orxTensorflowBackend` is set to `orx-tensorflow-gpu` in the root build.gradle.

Using ORML inside stand-alone OPENRNDR projects is easiest when working with openrndr-template. This template can be found in its [Github repository](https://github.com/openrndr/openrndr-template) and provides a convenient way to start-off with a known working setup. After cloning this template repository all that needs to be done is the configuration of `ormlFeatures` in the `build.gradle.kts` file at the project root.
