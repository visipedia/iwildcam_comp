![Banner](https://rawgit.com/visipedia/iwildcam_comp/2020/assets/iwildcam2020header.jpg)

# iWildCam 2021
Camera Traps (or Wild Cams) enable the automatic collection of large quantities of image data. Biologists all over the world use camera traps to monitor biodiversity and population density of animal species. We have recently been making strides towards automatic species classification in camera trap images. However, as we try to expand the scope of these models we are faced with an interesting problem: how do we train models that perform well on new (unseen during training) camera trap locations? Can we leverage data from other modalities, such as citizen science data and remote sensing data?

In order to tackle this problem, we have prepared a challenge where the training data and test data are from different cameras spread across the globe. The set of species seen in each camera overlap, but are not identical. The challenge is to classify species in the test cameras correctly. To explore multimodal solutions, we allow competitors to train on the following data: (i) our camera trap training set (data provided by WCS), (ii) iNaturalist 2017-2019 data, and (iii) multispectral imagery (from [Landsat 8](https://www.usgs.gov/land-resources/nli/landsat/landsat-8)) for each of the camera trap locations. On the competition [GitHub page](https://github.com/visipedia/iwildcam_comp) we provide the multispectral data, a taxonomy file mapping our classes into the iNat taxonomy, a subset of iNat data mapped into our class set, and a camera trap detection model (the MegaDetector) along with the corresponding detections.

This is an FGVCx competition as part of the [FGVC8](https://sites.google.com/corp/view/fgvc8) workshop at [CVPR 2021](http://cvpr2021.thecvf.com/), and is sponsored by [Microsoft AI for Earth](https://www.microsoft.com/en-us/ai/ai-for-earth) and [Wildlife Insights](https://www.wildlifeinsights.org/). Please open an issue if you have questions or problems with the dataset.

You can find the iWildCam 2018 Competition [here](https://github.com/visipedia/iwildcam_comp/blob/master/2018/readme.md), the iWildCam 2019 Competition [here](https://github.com/visipedia/iwildcam_comp/blob/master/2019/readme.md), and the iWildCam 2020 Competition [here](https://github.com/visipedia/iwildcam_comp/blob/master/2020/readme.md).

## Kaggle
We are using Kaggle to host the leaderboard. Competition page [here](https://www.kaggle.com/c/iwildcam-2020-fgvc8/overview).


## Dates
|||
|------|---------------|
Competition Starts |March 8, 2021|
Submission Deadline|May 31, 2021|


## Details and Evaluation

The WCS training set contains 206,710 images from 441 locations, and the WCS test set contains 61,104 images from 111 locations. These 552 locations are spread across the globe. A location ID (`location`) is given for each image.

You may also choose to use supplemental training data from the iNaturalist 2017, iNaturalist 2018, and iNaturalist 2019 competition datasets. As a courtesy, we have curated all the images from these datasets containing classes that might be in the test set and mapped them into the iWildCam categories. Note that these curated images come only from the iNaturalist 2017 and iNaturalist 2018 datasets because there are no common classes between the iNaturalist 2019 dataset and the WCS dataset. However, participants are still free to use the iNaturalist 2019 data if they wish.

We provide Landsat-8 multispectral imagery for each camera location as supplementary data. In particular, each site is associated with a series of patches collected between 2013 and 2019. The patches are extracted from a "Tier 1" Landsat product, which consists only of data that meets certain geometric and radiometric quality standards. Consequently, the number of patches per site varies from 39 to 406 (median: 147). Each patch is 200x200x9 pixels, covering an area of 6km^2 at a resolution of 30 meters / pixel across 9 spectral bands. Note that all patches for a given site are registered, but are not centered exactly at the camera location to protect the integrity of the site. 

Submissions will be evaluated based on their categorization accuracy.

## Guidelines

The general rule is that participants should only use the provided training images for training models to classify the test images. Participants are allowed to use the iNaturalist 2017-2019 competition datasets and the provided Landsat 8 imagery during training. We do not want participants crawling the web in search of additional data or using previous versions of this dataset. Models pretrained on standard computer vision datasets may be used to construct the algorithms (e.g. ImageNet pretrained models). Please specify any and all external data and/or models used for training when uploading results.

Participants are allowed to collect additional annotations (e.g. bounding boxes, keypoints) on the provided training sets. Participants are not allowed to collect annotations on the test set. Teams should specify any additional annotations they have collected when submitting results.


## Annotation Format
We follow the annotation format of the [COCO dataset](http://mscoco.org/dataset/#download) and add additional fields. Each training image has at least one associated annotation, containing a `category_id` that that maps the annotation to its corresponding category label. The annotations are stored in the [JSON format](http://www.json.org/) and are organized as follows:
```
{
  "info" : info,
  "images" : [image],
  "categories" : [category],
  "annotations" : [annotation]
}

info{
  "year" : int,
  "version" : str,
  "description" : str,
  "contributor" : str
  "date_created" : datetime
}

image{
  "id" : str,
  "width" : int,
  "height" : int,
  "file_name" : str,
  "rights_holder" : str,
  "location": int,
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
The `bbox` units are in pixels, the origin is the upper left hand corner, and the `area` value is approximated as `(width * height) / 2.0` since we did not collect segmentation masks.

## Submission Format

The submission format for the competition is a csv file with the following format:
```
id,animal_class
58857ccf-23d2-11e8-a6a3-ec086b02610b,1
591e4006-23d2-11e8-a6a3-ec086b02610b,5
...
```
The `id` column corresponds to the test image id. The `animal_class` is an integer value that indicates the class of the animal, or `0` to represent the absence of an animal.

## Data
Download the dataset files here:
  * WCS training images (84GB zipped)
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/train.zip)
      * Running `md5sum train.zip` should produce `881d703639ce1df034e32fee1222bdcb`
  * WCS test images (25GB zipped)
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/test.zip)
      * Running `md5sum test.zip` should produce `adac3be1b45e12e062299615386cae05`
  * WCS annotations and MegaDetector Results
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/metadata.zip)
      * Running `md5sum metadata.zip` should produce `3050b2a641ebef259ee73e1476e5e6ae`
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
<!---      
We also provide a smaller version of the camera trap datasets where the image width is resized to 1024 pixels:
  * Smaller WCS train images 27GB
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2019/train_small.zip)
      * Running `md5sum train_small.zip` should produce `` 
  * Smaller WCS test images (9GB zipped)
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2019/test_small.zip)
       * Running md5sum `test_small.zip` should produce `70f68298f4390353e05a50f8b6e122f5`
--->      
## Camera Trap Animal Detection Model
We are also providing a general animal detection model which competitors are free to use as they see fit.

The model is a tensorflow Faster-RCNN model with Inception-Resnet-v2 backbone and atrous convolution.  It can be downloaded [here](https://lilablobssc.blob.core.windows.net/models/camera_traps/megadetector/megadetector_v2.pb).

Sample code for running the detector over a folder of images can be found [here](https://github.com/Microsoft/CameraTraps/blob/master/detection/run_tf_detector.py).

We have run the detector over the WCS dataset, and provide the top 100 boxes and associated confidences along with the metadata. Detections are provided in the following format: 
```
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
  # bounding boxes are in absolute, floating-point coordinates, with the origin at the upper-left
  'bbox' : [x, y, width, height], 
  # note that the categories returned by the detector are not the categories in the WCS dataset
  'category': str,
  'conf': float
}
```

## Class-agnostic Segmentation Model
We are also providing a general weakly-supervised segmentation model which competitors are free to use as they see fit.

We have run the segmentation model over the WCS dataset using the bounding boxes from the MegaDetector, and provide the segmentation for each box 

## Data Challenges
Camera trap data provides several challenges that can make it difficult to achieve accurate results.  

#### Illumination:
Images can be poorly illuminated, especially at night.  The example below contains a skunk to the center left of the frame.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/illumination.png" width="400">

#### Motion Blur:
The shutter speed of the camera is not fast enough to eliminate motion blur, so animals are sometimes blurry. The example contains a blurred coyote.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/blur.png" width="400">

#### Small ROI:
Some animals are small or far from the camera, and can be difficult to spot even for humans.  The example image has a mouse on a branch to the center right of the frame.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/smallroi.png" width="400">

#### Occlusion:
Animals can be occluded by vegetation or the edge of the frame.  This example shows a location where weeds grew in front of the camera, obscuring the view.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/occlusion.png" width="400">

#### Perspective:
Sometimes animals come very close to the camera, causing a forced perspective.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/perspective.png" width="400">


#### Weather Conditions:
Poor weather, including rain, snow, or dust, can obstruct the lens and cause false triggers.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/weather.png" width="400">

#### Camera Malfunctions:
Sometimes the camera malfunctions, causing strange discolorations.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/malfunctions.png" width="400">

#### Temporal Changes: 

At any given location, the background changes over time as the seasons change.  Below, you can see a single loction at three different points in time.

![alt text](https://rawgit.com/visipedia/iwildcam_comp/master/assets/changesovertime.png)

#### Non-Animal Variability:
What causes the non-animal images to trigger varies based on location.  Some locations contain lots of vegetation, which can cause false triggers as it moves in the wind.  Others are near roadways, so can be triggered by cars or bikers.  

## Terms of Use

By downloading Wildlife Conservation Society data or iWildCam Remote Sensing data you agree to the terms in the [Community Data License Agreement (CDLA)](https://cdla.io/permissive-1-0/).

By downloading iNaturalist data you agree to the terms outlined by [iNaturalist](https://www.inaturalist.org/pages/terms).

### Acknowledgements

Data is primarily provided by the Wildlife Conservation Society (WCS), iNaturalist, the U.S. Geological Survey, and Microsoft AI for Earth.
