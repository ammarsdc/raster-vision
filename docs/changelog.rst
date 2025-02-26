CHANGELOG
=========

Raster Vision 0.13.1
--------------------

Bug Fixes
~~~~~~~~~

* Fix image plot by adding default plot transform `#1144 <https://github.com/azavea/raster-vision/pull/1144>`__

Raster Vision 0.13
------------------

This release presents a major jump in Raster Vision's power and flexibility. The most significant changes are:

Support arbitrary models and loss functions (`#985 <https://github.com/azavea/raster-vision/pull/985>`__, `#992 <https://github.com/azavea/raster-vision/pull/992>`__)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Raster Vision is no longer restricted to using the built in models and loss functions. It is now possible to import models and loss functions from a GitHub repo or a URI or a zip file as long as they interface correctly with RV's learner code. This means that you can now easily swap models in your existing training pipelines, allowing you to take advantage of the latest models or to make customizations that help with your specific task; all with minimal changes.

This is made possible by PyTorch's ``hub`` module.

Currently not supported for Object Detection.

Support for multiband images (even with Transfer Learning) (`#972 <https://github.com/azavea/raster-vision/pull/972>`__)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is now possible to train on imagery with more than 3 channels. Raster Vision automatically modifies the model to be able to accept more than 3 channels. If using pretrained models, the pre-learned weights are retained.

The model modification cannot be performed automatically when using an external model. But as long as the external model supports multiband inputs, it will work correctly with RV.

Currently only supported for Semantic Segmentation.

Support for reading directly from raster sources during training without chipping (`#1046 <https://github.com/azavea/raster-vision/pull/1046>`__)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is no longer necessary to go through a ``chip`` stage to produce a training dataset. You can instead provide the ``DatasetConfig`` directly to the PyTorch backend and RV will sample training chips on the fly during training. All the examples now use this as the default. Check them out to see how to use this feature.

Support for arbitrary Albumentations transforms (`#1001 <https://github.com/azavea/raster-vision/pull/1001>`__)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is now possible to supply an arbitrarily complicated Albumentations transform for data augmentation. In the ``DataConfig`` subclasses, you can specify a ``base_transform`` that is applied every time (i.e. in training, validation, and prediction), an ``aug_transform`` that is only applied during training, and a ``plot_transform`` (via ``PlotOptions``) to ensure that sample images are plotted correctly (e.g. use ``plot_transform`` to rescale a normalized image to 0-1).

Allow streaming reads from Rasterio sources (`#1020 <https://github.com/azavea/raster-vision/pull/1020>`__)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is now possible to stream chips from a remote ``RasterioSource`` without first downloading the entire file. To enable, set ``allow_streaming=True`` in the ``RasterioSourceConfig``.

Analyze stage no longer necessary when using non-uint8 rasters (`#972 <https://github.com/azavea/raster-vision/pull/972>`__)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is no longer necessary to go through an ``analyze`` stage to be able to convert non-``uint8`` rasters to ``uint8`` chips. Chips can now be stored as ``numpy`` arrays, and will be normalized to ``float`` during training/prediction based on their specific data type. See ``spacenet_vegas.py`` for example usage.

Currently only supported for Semantic Segmentation.

Features
~~~~~~~~

* Add support for multiband images `#972 <https://github.com/azavea/raster-vision/pull/972>`__
* Add support for vector output to predict command `#980 <https://github.com/azavea/raster-vision/pull/980>`__
* Add support for weighted loss for classification and semantic segmentation `#977 <https://github.com/azavea/raster-vision/pull/977>`__
* Add multi raster source `#978 <https://github.com/azavea/raster-vision/pull/978>`__
* Add support for fetching and saving external model definitions `#985 <https://github.com/azavea/raster-vision/pull/985>`__
* Add support for external loss definitions `#992 <https://github.com/azavea/raster-vision/pull/992>`__
* Upgrade to pyproj 2.6 `#1000 <https://github.com/azavea/raster-vision/pull/1000>`__
* Add support for arbitrary albumentations transforms `#1001 <https://github.com/azavea/raster-vision/pull/1001>`__
* Minor tweaks to regression learner `#1013 <https://github.com/azavea/raster-vision/pull/1013>`__
* Add ability to specify number of PyTorch reader processes `#1008 <https://github.com/azavea/raster-vision/pull/1008>`__
* Make img_sz specifiable `#1012 <https://github.com/azavea/raster-vision/pull/1012>`__
* Add ignore_last_class capability to segmentation `#1017 <https://github.com/azavea/raster-vision/pull/1017>`__
* Add filtering capability to segmentation sliding window chip generation `#1018 <https://github.com/azavea/raster-vision/pull/1018>`__
* Add raster transformer to remove NaNs from float rasters, add raster transformers to cast to arbitrary numpy types `#1016 <https://github.com/azavea/raster-vision/pull/1016>`__
* Add plot options for regression `#1023 <https://github.com/azavea/raster-vision/pull/1023>`__
* Add ability to use fewer channels w/ pretrained models `#1026 <https://github.com/azavea/raster-vision/pull/1026>`__
* Remove 4GB file size limit from VSI file system, allow streaming reads `#1020 <https://github.com/azavea/raster-vision/pull/1020>`__
* Add reclassification transformer for segmentation label rasters `#1024 <https://github.com/azavea/raster-vision/pull/1024>`__
* Allow filtering out chips based on proportion of NODATA pixels `#1025 <https://github.com/azavea/raster-vision/pull/1025>`__
* Allow ignore_last_class to take either a boolean or the literal 'force'; in the latter case validation of that argument is skipped so that it can be used with external loss functions `#1027 <https://github.com/azavea/raster-vision/pull/1027>`__
* Add ability to crop raster source extent `#1030 <https://github.com/azavea/raster-vision/pull/1030>`__
* Accept immediate geometries in SceneConfig `#1033 <https://github.com/azavea/raster-vision/pull/1033>`__
* Only perform normalization on unsigned integer types `#1028 <https://github.com/azavea/raster-vision/pull/1028>`__
* Make group_uris specifiable and add group_train_sz_rel `#1035 <https://github.com/azavea/raster-vision/pull/1035>`__
* Make number of training and dataloader previews independent of batch size `#1038 <https://github.com/azavea/raster-vision/pull/1038>`__
* Allow continuing training from a model bundle `#1022 <https://github.com/azavea/raster-vision/pull/1022>`__
* Allow reading directly from raster source during training without chipping `#1046 <https://github.com/azavea/raster-vision/pull/1046>`__
* Remove external commands (obsoleted by external architectures and loss functions) `#1047 <https://github.com/azavea/raster-vision/pull/1047>`__
* Allow saving SS predictions as probabilities `#1057 <https://github.com/azavea/raster-vision/pull/1057>`__
* Update CUDA version from 10.1 to 10.2 `#1115 <https://github.com/azavea/raster-vision/pull/1115>`__
* Add integration tests for the nochip functionality `#1116 <https://github.com/azavea/raster-vision/pull/1116>`__
* Update examples to make use of the nochip functionality by default  `#1116 <https://github.com/azavea/raster-vision/pull/1116>`__

Bug Fixes
~~~~~~~~~~~~

* Update all relevant saved URIs in config before instantiating Pipeline `#993 <https://github.com/azavea/raster-vision/pull/993>`__
* Pass verbose flag to batch jobs `#988 <https://github.com/azavea/raster-vision/pull/988>`__
* Fix: Ensure Integer class_id `#990 <https://github.com/azavea/raster-vision/pull/990>`__
* Use ``--ipc=host`` by default when running the docker container `#1077 <https://github.com/azavea/raster-vision/pull/1077>`__

Raster Vision 0.12
-------------------

This release presents a major refactoring of Raster Vision intended to simplify the codebase, and make it more flexible and customizable.

To learn about how to upgrade existing experiment configurations, perhaps the best approach is to read the `source code <https://github.com/azavea/raster-vision/tree/0.12/rastervision_pytorch_backend/rastervision/pytorch_backend/examples>`__ of the :ref:`rv examples` to get a feel for the new syntax. Unfortunately, existing predict packages will not be usable with this release, and upgrading and re-running the experiments will be necessary. For more advanced users who have written plugins or custom commands, the internals have changed substantially, and we recommend reading :ref:`architecture`.

Since the changes in this release are sweeping, it is difficult to enumerate a list of all changes and associated PRs. Therefore, this change log describes the changes at a high level, along with some justifications and pointers to further documentation.

Simplified Configuration Schema
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We are still using a modular, programmatic approach to configuration, but have switched to using a ``Config`` base class which uses the `Pydantic <https://pydantic-docs.helpmanual.io/>`__ library. This allows us to define configuration schemas in a declarative fashion, and let the underlying library handle serialization, deserialization, and validation. In addition, this has allowed us to `DRY <https://en.wikipedia.org/wiki/Don%27t_repeat_yourself>`__ up the configuration code, eliminate the use of Protobufs, and represent configuration from plugins in the same fashion as built-in functionality. To see the difference, compare the configuration code for ``ChipClassificationLabelSource`` in 0.11 (`label_source.proto <https://github.com/azavea/raster-vision/blob/0.11/rastervision/protos/label_source.proto>`__ and `chip_classification_label_source_config.py <https://github.com/azavea/raster-vision/blob/0.11/rastervision/data/label_source/chip_classification_label_source_config.py>`__), and in 0.12 (`chip_classification_label_source_config.py <https://github.com/azavea/raster-vision/blob/0.12/rastervision_core/rastervision/core/data/label_source/chip_classification_label_source_config.py>`__).

Abstracted out Pipelines
~~~~~~~~~~~~~~~~~~~~~~~~~

Raster Vision includes functionality for running computational pipelines in local and remote environments, but previously, this functionality was tightly coupled with the "domain logic" of machine learning on geospatial data in the ``Experiment`` abstraction. This made it more difficult to add and modify commands, as well as use this functionality in other projects. In this release, we factored out the experiment running code into a separate :ref:`rastervision.pipeline <pipelines plugins>` package, which can be used for defining, configuring, customizing, and running arbitrary computational pipelines.

Reorganization into Plugins
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The rest of Raster Vision is now written as a set of optional plugins that have  ``Pipelines`` which implement the "domain logic" of machine learning on geospatial data. Implementing everything as optional (``pip`` installable) plugins makes it easier to install subsets of Raster Vision functionality, eliminates separate code paths for built-in and plugin functionality, and provides (de facto) examples of how to write plugins. See :ref:`codebase overview` for more details.

More Flexible PyTorch Backends
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The 0.10 release added PyTorch backends for chip classification, semantic segmentation, and object detection. In this release, we abstracted out the common code for training models into a flexible ``Learner`` base class with subclasses for each of the computer vision tasks. This code is in the ``rastervision.pytorch_learner`` plugin, and is used by the ``Backends`` in ``rastervision.pytorch_backend``. By decoupling ``Backends`` and ``Learners``, it is now easier to write arbitrary ``Pipelines`` and new ``Backends`` that reuse the core model training code, which can be customized by overriding methods such as ``build_model``. See :ref:`customizing rv`.

Removed Tensorflow Backends
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Tensorflow backends and associated Docker images have been removed. It is too difficult to maintain backends for multiple deep learning frameworks, and PyTorch has worked well for us. Of course, it's still possible to write ``Backend`` plugins using any framework.

Other Changes
~~~~~~~~~~~~~~

* For simplicity, we moved the contents of the `raster-vision-examples <https://github.com/azavea/raster-vision-examples>`__ and `raster-vision-aws <https://github.com/azavea/raster-vision-aws>`__ repos into the main repo. See :ref:`rv examples` and :ref:`cloudformation setup`.
* To help people bootstrap new projects using RV, we added :ref:`bootstrap`.
* All the PyTorch backends now offer data augmentation using `albumentations <https://albumentations.readthedocs.io/>`__.
* We removed the ability to automatically skip running commands that already have output, "tree workflows", and "default providers". We also unified the ``Experiment``, ``Command``, and ``Task`` classes into a single ``Pipeline`` class which is subclassed for different computer vision (or other) tasks. These features and concepts had little utility in our experience, and presented stumbling blocks to outside contributors and plugin writers.
* Although it's still possible to add new ``VectorSources`` and other classes for reading data, our philosophy going forward is to prefer writing pre-processing scripts to get data into the format that Raster Vision can already consume. The ``VectorTileVectorSource`` was removed since it violates this new philosophy.
* We previously attempted to make predictions for semantic segmentation work in a streaming fashion (to avoid running out of RAM), but the implementation was buggy and complex. So we reverted to holding all predictions for a scene in RAM, and now assume that scenes are roughly < 20,000 x 20,000 pixels. This works better anyway from a parallelization standponit.
* We switched to writing chips to disk incrementally during the ``CHIP`` command using a ``SampleWriter`` class to avoid running out of RAM.
* The term "predict package" has been replaced with "model bundle", since it rolls off the tongue better, and ``BUNDLE`` is the name of the command that produces it.
* Class ids are now indexed starting at 0 instead of 1, which seems more intuitive. The "null class", used for marking pixels in semantic segmentation that have not been labeled, used to be 0, and is now equal to ``len(class_ids)``.
* The ``aws_batch`` runner was renamed ``batch`` due to a naming conflict, and the names of the configuration variables for Batch changed. See :ref:`aws batch setup`.

Future Work
~~~~~~~~~~~~

The next big features we plan on developing are:

* the ability to read and write data in `STAC <https://stacspec.org/>`__ format using the `label extension <https://github.com/radiantearth/stac-spec/tree/master/extensions/label>`__. This will facilitate integration with other tools such as `GroundWork <https://groundwork.azavea.com/>`__.

Raster Vision 0.11
-------------------

Features
~~~~~~~~~~

- Added the possibility for chip classification to use data augmentors from the albumentations libary to enhance the training data. `#859 <https://github.com/azavea/raster-vision/pull/859>`__
- Updated the Quickstart doc with pytorch docker image and model `#863 <https://github.com/azavea/raster-vision/pull/863>`__
- Added the possibility to deal with class imbalances through oversampling. `#868 <https://github.com/azavea/raster-vision/pull/868>`__

Raster Vision 0.11.0
~~~~~~~~~~~~~~~~~~~~~

Bug Fixes
^^^^^^^^^^

- Ensure randint args are ints `#849 <https://github.com/azavea/raster-vision/pull/849>`__
- The augmentors were not serialized properly for the chip command  `#857 <https://github.com/azavea/raster-vision/pull/857>`__
- Fix problems with pretrained flag `#860 <https://github.com/azavea/raster-vision/pull/860>`__
- Correctly get_local_path for some zxy tile URIS `#865 <https://github.com/azavea/raster-vision/pull/865>`__

Raster Vision 0.10
------------------

Raster Vision 0.10.0
~~~~~~~~~~~~~~~~~~~~~~

Notes on switching to PyTorch-based backends
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The current backends based on Tensorflow have several problems:

* They depend on third party libraries (Deeplab, TF Object Detection API) that are complex, not well suited to being used as dependencies within a larger project, and are each written in a different style. This makes the code for each backend very different from one other, and unnecessarily complex. This increases the maintenance burden, makes it difficult to customize, and makes it more difficult to implement a consistent set of functionality between the backends.
* Tensorflow, in the maintainer's opinion, is more difficult to write and debug than PyTorch (although this is starting to improve).
* The third party libraries assume that training images are stored as PNG or JPG files. This limits our ability to handle more than three bands and more that 8-bits per channel. We have recently completed some research on how to train models on > 3 bands, and we plan on adding this functionality to Raster Vision.

Therefore, we are in the process of sunsetting the Tensorflow backends (which will probably be removed) and have implemented replacement PyTorch-based backends. The main things to be aware of in upgrading to this version of Raster Vision are as follows:

* Instead of there being CPU and GPU Docker images (based on Tensorflow), there are now tf-cpu, tf-gpu, and pytorch (which works on both CPU and GPU) images. Using ``./docker/build --tf`` or ``./docker/build --pytorch`` will only build the TF or PyTorch images, respectively.
* Using the TF backends requires being in the TF container, and similar for PyTorch. There are now ``--tf-cpu``, ``--tf-gpu``, and ``--pytorch-gpu`` options for the ``./docker/run`` command. The default setting is to use the PyTorch image in the standard (CPU) Docker runtime.
* The `raster-vision-aws <https://github.com/azavea/raster-vision-aws>`__ CloudFormation setup creates Batch resources for TF-CPU, TF-GPU, and PyTorch. It also now uses default AMIs provided by AWS, simplifying the setup process.
* To easily switch between running TF and PyTorch jobs on Batch, we recommend creating two separate Raster Vision profiles with the Batch resources for each of them.
* The way to use the ``ConfigBuilders`` for the new backends can be seen in the `examples repo <https://github.com/azavea/raster-vision-examples>`__ and the :ref:`backend` reference

Features
^^^^^^^^^^^^

- Add confusion matrix as metric for semantic segmentation `#788 <https://github.com/azavea/raster-vision/pull/788>`__
- Add predict_chip_size as option for semantic segmentation `#786 <https://github.com/azavea/raster-vision/pull/786>`__
- Handle "ignore" class for semantic segmentation `#783 <https://github.com/azavea/raster-vision/pull/783>`__
- Add stochastic gradient descent ("SGD") as an optimizer option for chip classification `#792 <https://github.com/azavea/raster-vision/pull/792>`__
- Add option to determine if all touched pixels should be rasterized for rasterized RasterSource `#803 <https://github.com/azavea/raster-vision/pull/803>`__
- Script to generate GeoTIFF from ZXY tile server `#811 <https://github.com/azavea/raster-vision/pull/811>`__
- Remove QGIS plugin `#818 <https://github.com/azavea/raster-vision/pull/818>`__
- Add PyTorch backends and add PyTorch Docker image `#821 <https://github.com/azavea/raster-vision/pull/821>`__ and `#823 <https://github.com/azavea/raster-vision/pull/823>`__.

Bug Fixes
^^^^^^^^^

- Fixed issue with configuration not being able to read lists `#784 <https://github.com/azavea/raster-vision/pull/784>`__
- Fixed ConfigBuilders not supporting type annotations in __init__ `#800 <https://github.com/azavea/raster-vision/pull/800>`__

Raster Vision 0.9
-----------------

Raster Vision 0.9.0
~~~~~~~~~~~~~~~~~~~

Features
^^^^^^^^
- Add requester_pays RV config option `#762 <https://github.com/azavea/raster-vision/pull/762>`__
- Unify Docker scripts `#743 <https://github.com/azavea/raster-vision/pull/743>`__
- Switch default branch to master `#726 <https://github.com/azavea/raster-vision/pull/726>`__
- Merge GeoTiffSource and ImageSource into RasterioSource `#723 <https://github.com/azavea/raster-vision/pull/723>`__
- Simplify/clarify/test/validate RasterSource `#721 <https://github.com/azavea/raster-vision/pull/721>`__
- Simplify and generalize geom processing `#711 <https://github.com/azavea/raster-vision/pull/711>`__
- Predict zero for nodata pixels on semantic segmentation `#701 <https://github.com/azavea/raster-vision/pull/701>`__
- Add support for evaluating vector output with AOIs `#698 <https://github.com/azavea/raster-vision/pull/698>`__
- Conserve disk space when dealing with raster files `#692 <https://github.com/azavea/raster-vision/pull/692>`__
- Optimize StatsAnalyzer `#690 <https://github.com/azavea/raster-vision/pull/690>`__
- Include per-scene eval metrics `#641 <https://github.com/azavea/raster-vision/pull/641>`__
- Make and save predictions and do eval chip-by-chip `#635 <https://github.com/azavea/raster-vision/pull/635>`__
- Decrease semseg memory usage `#630 <https://github.com/azavea/raster-vision/pull/630>`__
- Add support for vector tiles in .mbtiles files `#601 <https://github.com/azavea/raster-vision/pull/601>`__
- Add support for getting labels from zxy vector tiles `#532 <https://github.com/azavea/raster-vision/pull/532>`__
- Remove custom ``__deepcopy__`` implementation from ``ConfigBuilder``\s. `#567 <https://github.com/azavea/raster-vision/pull/567>`__
- Add ability to shift raster images by given numbers of meters. `#573 <https://github.com/azavea/raster-vision/pull/573>`__
- Add ability to generate GeoJSON segmentation predictions. `#575 <https://github.com/azavea/raster-vision/pull/575>`__
- Add ability to run the DeepLab eval script.  `#653 <https://github.com/azavea/raster-vision/pull/653>`__
- Submit CPU-only stages to a CPU queue on Aws.  `#668 <https://github.com/azavea/raster-vision/pull/668>`__
- Parallelize CHIP and PREDICT commands  `#671 <https://github.com/azavea/raster-vision/pull/671>`__
- Refactor ``update_for_command`` to split out the IO reporting into ``report_io``. `#671 <https://github.com/azavea/raster-vision/pull/671>`__
- Add Multi-GPU Support to DeepLab Backend `#590 <https://github.com/azavea/raster-vision/pull/590>`__
- Handle multiple AOI URIs `#617 <https://github.com/azavea/raster-vision/pull/617>`__
- Give ``train_restart_dir`` Default Value `#626 <https://github.com/azavea/raster-vision/pull/626>`__
- Use ```make`` to manage local execution `#664 <https://github.com/azavea/raster-vision/pull/664>`__
- Optimize vector tile processing  `#676 <https://github.com/azavea/raster-vision/pull/676>`__

Bug Fixes
^^^^^^^^^
- Fix Deeplab resume bug: update path in checkpoint file `#756 <https://github.com/azavea/raster-vision/pull/756>`__
- Allow Spaces in ``--channel-order`` Argument `#731 <https://github.com/azavea/raster-vision/pull/731>`__
- Fix error when using predict packages with AOIs `#674 <https://github.com/azavea/raster-vision/pull/674>`__
- Correct checkpoint name `#624 <https://github.com/azavea/raster-vision/pull/624>`__
- Allow using default stride for semseg sliding window  `#745 <https://github.com/azavea/raster-vision/pull/745>`__
- Fix filter_by_aoi for ObjectDetectionLabels `#746 <https://github.com/azavea/raster-vision/pull/746>`__
- Load null channel_order correctly `#733 <https://github.com/azavea/raster-vision/pull/733>`__
- Handle Rasterio crs that doesn't contain EPSG `#725 <https://github.com/azavea/raster-vision/pull/725>`__
- Fixed issue with saving semseg predictions for non-georeferenced imagery `#708 <https://github.com/azavea/raster-vision/pull/708>`__
- Fixed issue with handling width > height in semseg eval `#627 <https://github.com/azavea/raster-vision/pull/627>`__
- Fixed issue with experiment configs not setting key names correctly `#576 <https://github.com/azavea/raster-vision/pull/576>`__
- Fixed issue with Raster Sources that have channel order `#576 <https://github.com/azavea/raster-vision/pull/576>`__


Raster Vision 0.8
-----------------

Raster Vision 0.8.1
~~~~~~~~~~~~~~~~~~~

Bug Fixes
^^^^^^^^^
- Allow multiploygon for chip classification `#523 <https://github.com/azavea/raster-vision/pull/523>`__
- Remove unused args for AWS Batch runner `#503 <https://github.com/azavea/raster-vision/pull/503>`__
- Skip over lines when doing chip classification, Use background_class_id for scenes with no polygons `#507 <https://github.com/azavea/raster-vision/pull/507>`__
- Fix issue where ``get_matching_s3_keys`` fails when ``suffix`` is ``None`` `#497 <https://github.com/azavea/raster-vision/pull/497>`__
