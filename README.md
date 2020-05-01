[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/yliess86/FaceCrop/blob/master/LICENSE)
[![Python 3.6+](https://img.shields.io/badge/python-3.6+-blue.svg)](https://www.python.org/downloads/release/python-360/)
[![Pytorch 1.4](https://img.shields.io/badge/pytorch-1.4.0-blue.svg)](https://pytorch.org/)

# FaceCrop: a Tool for Face Cropped Videos

![Logo](logo.png)

**FaceCrop** is a **Python** tool for face cropped videos. It allows you to infer face **region** and **landmarks** from **single person videos**. FaceCrop has been implemented with **batch inference** in mind allowing to treat the videos faster. It can be used to generate a dataset for training Deep Learning models and such.

> #### Disclaimer
>
> This tool is **not meant** to be used for **real-time** landmark and face detection. Other tools such as OpenPose, OpenCV, and optimized Dlib already allow you to do so.
>
> This tool has been built to **meet the needs I have at the moment** of creating it for other and more complex projects. FaceCrop is meant to be used with single person videos exclusively. It **isn't perfect** and **will be updated as needed**.

<p align="center">
  <img width="640" height="360" src="joma.gif"></img>
</p>
<p align="center">
    <a href="https://www.youtube.com/watch?v=uxRf7KS3abo">
        Orignal Video
    </a>
</p>

## Install

The installation is pretty straightforward. All you need is a **Python3** environment with **Pip** installed. The library can then be used on its own by installing it on the system or by using it locally.

### System Install

**System install** is done by calling the following command *(may require sudo)*:

```bash
$ (sudo) python3 setup.py install
```

### Local Usage

To use the library without installation on the system, it is mandatory to at least **install the libraries** required and listed in the `requirements.txt` file. You can do that by calling the following command *(may require sudo)*:

```bash
$ (sudo) pip3 install -r requirements.txt
```

## Usage

After installation, the **FaceCrop** can perform two actions: **Create** annotation **CSV** file for a given video and **Visualize** annotation for a given video and its annotation file.

> #### Tips
>
> The speed of processing will highly **benefit** from using an enable **Nvidia GPU** with **Cuda** and **CudNN** support installed. Make sure the number of batches can fit into your device's memory.

### Annotation

The **annotation** command is in the following format:

```bash
$ python3 -m facecrop annotate --help
usage: __main__.py annotate [-h] --video VIDEO --annotations ANNOTATIONS
--dbatch_size DBATCH_SIZE --lbatch_size LBATCH_SIZE --n_process N_PROCESS
[--size SIZE SIZE] [-d DEVICE]

optional arguments:
  -h, --help                  show this help message and exit
  --video VIDEO               video path to be annotated (mp4 by preference `video.mp4`)
  --annotations ANNOTATIONS   annotations saving path (save to csv `annotations.csv`)
  --dbatch_size DBATCH_SIZE   batch_size for the detector inference
  --lbatch_size LBATCH_SIZE   batch_size for the landmark video loader
  --n_process N_PROCESS       number of threads used for lanrmarking
  --size SIZE SIZE            resize the video for the detector
  -d DEVICE, --device DEVICE  device to run detector on, `cpu` or `cuda`
```

The **annotation file** is in `CSV` format and is meant to be loaded with the **Pandas** library. The resulting file spec is the following:

|frame_idx|box_x|box_y|box_w|box_h|landmark_1_x|landmark_1_y|...|landmark_68_x|landmark_68_y|
|--------:|----:|----:|----:|----:|-----------:|-----------:|:-:|------------:|------------:|

The **naming convention** is explicit. `frame_idx` corresponds to the frame id in the video. `box_x`, `box_y`, `box_w`, `box_h` corresponds to the face detection **2D box** coordinates and size. And `landmark_i_x`, `landmark_i_y` correspond to the `ith` (68 in total) facial regressed **2D landmarks** coordinates.


### Visualization

The **visualization** command is in the following format:

```bash
$ python3 -m facecrop visualize --help
usage: __main__.py visualize [-h] --video VIDEO --annotations ANNOTATIONS
[--save SAVE] [--size SIZE SIZE]

optional arguments:
  -h, --help                 show this help message and exit
  --video VIDEO              video path (mp4 by preference `video.mp4`)
  --annotations ANNOTATIONS  annotations path (csv `annotations.csv`)
  --save SAVE                visualization saving path (gif `visualization.gif`)
  --size SIZE SIZE           resize the video to save gif
```

The visualization will **display the processed frames** as the example displayed at the top of this page shows. If you decide to **save** the result into a `GIF`, it will produce one with `25 FPS`. Be careful to used small videos for this usage as the visualization is not optimized to load the video frames in batches compared to the rest of the library. It makes sense as `GIF` needs to be small.
