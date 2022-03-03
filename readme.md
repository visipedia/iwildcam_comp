<!-- markdownlint-disable MD033 MD041 -->

![header](/assets/iwildcam2022-header.jpg)

<img alt="header" src="/assets/iwildcam2022-header.jpg" width="1200">

# iWildCam 2022

Camera Traps enable the automatic collection of large quantities of image data. Ecologists all over the world use camera traps to monitor biodiversity and population density of animal species. In order to estimate the abundance (how many are there) and population density of species in camera trap data, ecologists need to know not just which species were seen, but also how many of each species were seen. However, because images are taken in motion-triggered bursts to increase the likelihood of capturing the animal(s) of interest, object detection alone is not sufficient as it could lead to over- or under-counting. For example, if you get 3 images taken at one frame per second, and in the first you see 3 gazelles, in the second you see 5 gazelles, and in the last you see 4 gazelles, how many total gazelles have you seen? This is more challenging than strictly detecting and categorizing species, as it requires reasoning and tracking of individuals across sparse temporal samples. For example, in the below sequence of images there are 6 baboons.

<img src="/assets/monkey_count.png" width="1200">

Check out a few hard examples from the training set:

![image](/assets/train_examples_smaller.gif)

We have prepared a challenge where the training data and test data are from different cameras spread across the globe. The set of species seen in each camera overlap, but are not identical. The challenge is to classify species and count individual animals across sequences in the test cameras. To explore multimodal solutions, we allow competitors to train on the following data: (i) our camera trap training set (data provided by the [Wildlife Conservation Society (WCS)](https://www.wcs.org/)), (ii) iNaturalist 2017-2021 data, and (iii) multispectral imagery (from [Landsat 8](https://www.usgs.gov/land-resources/nli/landsat/landsat-8)) for each of the camera trap locations. Below we provide the multispectral data, a taxonomy file mapping our classes into the iNat taxonomy, a subset of iNat data mapped into our class set, and a camera trap detection model (the MegaDetector) along with the corresponding detections.

## Acknowledgements

This competition is part of the [FGVC9](https://sites.google.com/view/fgvc9) workshop at [CVPR 2022](https://cvpr2022.thecvf.com/). Data is primarily provided by the [Wildlife Conservation Society (WCS)](https://www.wcs.org/) and [iNaturalist](https://www.inaturalist.org/), and is hosted on Azure by [Microsoft AI for Earth](https://www.microsoft.com/en-us/ai/ai-for-earth). Count annotations on the test set were generously provided by [Centaur Labs](https://www.centaurlabs.com/).

## Kaggle

We are using Kaggle to host the leaderboard. Competition page is [here](https://www.kaggle.com/c/iwildcam2021-fgvc8).

## Dates

|                     |                |
| ------------------- | -------------- |
| Competition Starts  | March 10, 2021 |
| Submission Deadline | May 28, 2021   |

## Details and Evaluation

The iWildCam 2021 WCS training set contains 203,314 images from 323 locations, and the WCS test set contains 60,214 images from 91 locations. These 414 locations are spread across the globe. A location ID (`location`) is given for each image, and in some special cases where two cameras were set up by ecologists at the same location, we have provided a `sub_location` identifier. Camera traps operate with a motion trigger, and after motion is detected the camera will take a sequence of photos (from 1 - 10 images depending on the camera). We provide a `sequence_id` for each sequence, and the target task is to count the number of individuals of each species across each test sequence.

You may also choose to use supplemental training data from the iNaturalist 2017, iNaturalist 2018, iNaturalist 2019, and iNaturalist 2021 competition datasets. As a courtesy, we have curated all the images from iNaturalist 2017-2018 datasets containing classes that might be in the test set and mapped them into the iWildCam categories.

We provide Landsat-8 multispectral imagery for each camera location as supplementary data. In particular, each site is associated with a series of patches collected between 2013 and 2019. The patches are extracted from a "Tier 1" Landsat product, which consists only of data that meets certain geometric and radiometric quality standards. Consequently, the number of patches per site varies from 39 to 406 (median: 147). Each patch is 200x200x9 pixels, covering an area of 6km^2 at a resolution of 30 meters / pixel across 9 spectral bands. Note that all patches for a given site are registered, but are not centered exactly at the camera location to protect the integrity of the site.

Submissions will be evaluated using Mean Columnwise Root Mean Squared Error (MCRMSE),

<img src="assets/MCRMSE.png" width="400">

where each column `j` represents a species, each row `i` represents a sequence, `x_ij` is the predicted count for that species in that sequence, and `y_ij` is the ground truth count.

We selected this metric out of the options provided by Kaggle in order to capture both species identification mistakes and count mistakes, and to ensure false predictions on empty sequences would contribute to the error. Because many sequences are empty in camera trap data due to false triggers and many species are rare, the error from this normalized metric looks quite small, while the actual errors in counts are still large. To convert the metric to something more interpretable from an ecological standpoint, you can un-normalize the metric from MCRMSE to the Summed Columnwise Root Summed Squared Error (SCRSSE) by multiplying by the number of categories and the square root of the number of test sequences.

<img src="scrsse.png" width="600">

## Guidelines

The general rule is that participants should only use the provided training images for training models to classify the test images. Participants are allowed to use the iNaturalist 2017-2021 competition datasets and the provided Landsat 8 imagery during training. We do not want participants crawling the web in search of additional data or using previous versions of this dataset. Pretrained models trained on specific public datasets may be used to construct the algorithms. We specifically allow ImageNet pretrained models, COCO pretrained models, iNaturalist 2017-2021 pretrained models, and the Microsoft AI for Earth MegaDetector. If you have questions about whether a specific pretrained model is allowed, please ask.

Participants are allowed to collect additional annotations (e.g. bounding boxes, keypoints, counts) on the provided training sets. Participants are not allowed to collect annotations on the test set. Teams should specify any additional annotations they have collected when submitting results.

## Annotation Format

We follow the COCO CameraTraps annotation format and add additional fields. Each training image has at least one associated annotation, containing a `category_id` that that maps the annotation to its corresponding category label. We do not provide any count labels on training data. The annotations are stored in the [JSON format](https://www.json.org/) and are organized as follows:

```json
{
  "images" : [image],
  "categories" : [category],
  "annotations" : [annotation]
}

image{
  "id" : str,
  "width" : int,
  "height" : int,
  "file_name" : str,
  "rights_holder" : str,
  "location": int,
  "sub_location": int,
  "datetime": datetime,
  "seq_id": str,
  "seq_num_frames": int,
  "frame_num": int
}

category{
  "id" : int,
  "name" : str
}

annotation{
  "id" : str,
  "image_id" : str,
  "category_id" : int
}
```

## Submission Format

Submission for the competition is a csv file with the following format:

```csv
Id,Predicted2,Predicted3,[...],Predicted571
58857ccf-23d2-11e8-a6a3-ec086b02610b,0,5,[...],0
591e4006-23d2-11e8-a6a3-ec086b02610b,1,0,[...],3
...
```

The `Id` column corresponds to the test sequence id. `Predicted2` holds an integer value that indicates the number of individuals of species 2 in each test sequence. If you predict there are no animals in the sequence, the entire row after the sequence ID should be populated with `0` values.

## Data

Download the dataset files here:

* WCS training images (84GB zipped)
  * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2021/train.zip)
    * Running `md5sum train.zip` should produce `8c1c8e4bc699c16072ea0ff31ce7967a`
* WCS test images (25GB zipped)
  * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2021/test.zip)
    * Running `md5sum test.zip` should produce `461a9c211058a8c644ec915ae57beafe`
* WCS annotations and MegaDetector Results
  * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2021/metadata.zip)
    * Running `md5sum metadata.zip` should produce `e67aa5c935636360b017dfbf4c8e716f`
* WCS location GPS coordinates
  * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2021/gps_locations.json)
    * Running `md5sum gps_locations.json` should produce `3790fa2bbb138a854632f7ee4c47d789`
* iWildCam Remote Sensing Data (37GB zipped)
  * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/iwildcam_rs_npy.tar.gz)
    * Running `md5sum iwildcam_rs_npy.tar.gz` should produce `f25fbd47535a01139b0ef7b33b964269`
* iNaturalist 2017 subset of images from our classes (2.2GB zipped)
  * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/inaturalist_2017.tar.gz)
    * Running `md5sum inaturalist_2017.tar.gz` should produce `bf9f18c0bc0169c243a8958d3705a0b9`
* iNaturalist 2017 subset metadata
  * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/metadata/inaturalist_2017_to_iwildcam_train.json)
    * Running `md5sum inaturalist_2017_to_iwildcam_train.json` should produce `8664e8f16596995aae3a612d56e6426d`
* iNaturalist 2018 subset of images from our classes (1.5GB zipped)
  * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/inaturalist_2018.tar.gz)
    * Running `md5sum inaturalist_2018.tar.gz` should produce `44c0d20abefb27ec1555bd451d4c8904`
* iNaturalist 2018 subset metadata
  * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/metadata/inaturalist_2018_to_iwildcam_train.json)
    * Running `md5sum inaturalist_2018_to_iwildcam_train.json` should produce `dcdc925a494cb0e58daf9201eb69f595`

## Camera Trap Animal Detection Model

We also allow use of the [Microsoft AI for Earth MegaDetector](https://github.com/microsoft/CameraTraps/blob/master/megadetector.md), paper [here](https://arxiv.org/abs/1907.06772), a general and robust camera trap detection model which competitors are free to use as they see fit. Megadetector V3 detects `animal` and `human` classes, while the MegaDetector V4 adds a `vehicle` class. Any version of the MegaDetector is allowed to be used in this competition. The models can be downloaded [here](https://github.com/microsoft/CameraTraps/blob/master/megadetector.md#downloading-the-models).

Sample code for running the megaDetector detector over a folder of images can be found [here](https://github.com/Microsoft/CameraTraps/blob/master/detection/run_tf_detector.py).

We have run MegaDetector V3 over the WCS dataset, and provide the top 100 boxes and associated confidences along with the metadata. Detections are provided in the following format:

```json
{
  'images':[image],
  'detection_categories': {'1': 'animal', '2': 'person'},
  'info': info
}

image{
  'id': str,
  'max_detection_conf': float,
  'detections':[detection]
}

detection{
  # bounding boxes are in normalized, floating-point coordinates, with the origin at the upper-left
  'bbox' : [x, y, width, height], 
  # note that the categories returned by the detector are not the categories in the WCS dataset
  'category': str,
  'conf': float
}
```

## Class-agnostic Segmentation Model

We are also providing a general weakly-supervised segmentation model which competitors are free to use as they see fit.
We have run the segmentation model over the WCS dataset using the bounding boxes from the MegaDetector, and provide the segmentation for each box.
The segmentations come from [DeepMAC](https://google.github.io/deepmac/), which provides
class agnostic instance segmentation masks and achieves state-of-the-art performance on partially supervised instance segmentation tasks.
Below, we show a sample visualization of instance masks on WCS.

![Instance Masks](/assets/mask_visualization.png)

### Format details

We prodive an instance mask for each detected object by MegaDetector
(which is stored in `metadata/iwildcam2021_megadetector_results.json`).
For each image in the `train` or `test` direcotry with name `<ID>.jpg`, if there
are any objects detected in the image, its corresponding instance masks
will be stored in the `instance_masks/<ID>.png`. The instance mask details
are stored in a single channel PNG image. The pixels in the PNG image
are 1-indexed and indicate which detection they belong to (`0` is reserved
as background). The indices follow the same order as the detections in
MegaDetector's output (addressed by `['images']['detections']`).
When there are overlapping instances, we only preserve the ID of
the instance with the higher detection confidence (`'conf'` field).

### Obfuscated GPS Locations of cameras

We provide GPS locations for the majority of the camera traps, obfuscated within 1km for security and privacy reasons. Some of the obfuscated GPS locations (all from one country) were not released at the request of WCS, but knowing that the locations not listed in the `gps_locations.json` file are all from the same country should help competitors narrow down the set of possible species for those locations based on what is seen in the training data.

### Other Useful links

* [Notebook](https://www.kaggle.com/vighneshbgoogle/iwildcam-visualize-instance-masks) for visualizing iWildCam instance masks.
* [DeepMAC paper](https://arxiv.org/abs/2104.00613).
* [DeepMAC trained model](http://download.tensorflow.org/models/object_detection/tf2/20210329/deepmac_1024x1024_coco17.tar.gz) in TF saved model format (trained on COCO).
* [Colab notebook](https://github.com/tensorflow/models/blob/master/research/object_detection/colab_tutorials/deepmac_colab.ipynb) for using DeepMAC trained model on user-specified boxes.

## Data Challenges

Camera trap data provides several challenges that can make it difficult to achieve accurate results.  

### Illumination

Images can be poorly illuminated, especially at night.  The example below contains a skunk to the center left of the frame.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/illumination.png" width="400">

### Motion Blur

The shutter speed of the camera is not fast enough to eliminate motion blur, so animals are sometimes blurry. The example contains a blurred coyote.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/blur.png" width="400">

### Small ROI

Some animals are small or far from the camera, and can be difficult to spot even for humans.  The example image has a mouse on a branch to the center right of the frame.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/smallroi.png" width="400">

### Occlusion

Animals can be occluded by vegetation or the edge of the frame.  This example shows a location where weeds grew in front of the camera, obscuring the view.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/occlusion.png" width="400">

### Perspective

Sometimes animals come very close to the camera, causing a forced perspective.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/perspective.png" width="400">

### Weather Conditions

Poor weather, including rain, snow, or dust, can obstruct the lens and cause false triggers.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/weather.png" width="400">

### Camera Malfunctions

Sometimes the camera malfunctions, causing strange discolorations.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/malfunctions.png" width="400">

### Temporal Changes

At any given location, the background changes over time as the seasons change.  Below, you can see a single loction at three different points in time.

![alt text](assets/changesovertime.png)

### Non-Animal Variability

What causes the non-animal images to trigger varies based on location.  Some locations contain lots of vegetation, which can cause false triggers as it moves in the wind.  Others are near roadways, so can be triggered by cars or bikers.  

## Terms of Use

By downloading Wildlife Conservation Society data or iWildCam Remote Sensing data you agree to the terms in the [Community Data License Agreement (CDLA)](https://cdla.io/permissive-1-0/).

By downloading iNaturalist data you agree to the terms outlined by [iNaturalist](https://www.inaturalist.org/pages/terms).

## Previous iWildCam Competitions

[iWildCam 2018](https://github.com/visipedia/iwildcam_comp/tree/master/2018)

[iWildCam 2019](https://github.com/visipedia/iwildcam_comp/tree/master/2019)

[iWildCam 2020](https://github.com/visipedia/iwildcam_comp/tree/master/2020)

[iWildCam 2021](https://github.com/visipedia/iwildcam_comp/tree/master/2021)
