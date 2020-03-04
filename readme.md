![Banner](https://rawgit.com/visipedia/iwildcam_comp/2020/assets/iwildcam2020header.jpg)

# iWildCam 2020
Camera Traps (or Wild Cams) enable the automatic collection of large quantities of image data. Biologists all over the world use camera traps to monitor biodiversity and population density of animal species. We have recently been making strides towards automating the species classification challenge in camera traps, but as we try to expand the scope of these models we are faced with an interesting problem: how do you train a model that will perform well on new (unseen during training) camera traps? Can you leverage data from other modalities, such as citizen science data and remote sensing data?

In order to tackle this problem, we have prepared a challenge where the training data and test data are from different cameras spread across the globe. The species seen in each camera overlap, but are not identical, and the challenge is to classify species in the test cameras correctly. To explore multimodal solutions, we allow competitors to train data from our camera trap training set (data provided by Wildlife Conservation Society via LILA.science), on iNaturalist 2017-2019 data, and on the provided Landsat 8 remote sensing data across all 10 spectral bands, matched to the camera trap locations. We have provided a taxonomy file mapping our classes into the iNat taxonomy, and a subset of iNat data mapped into our class set. 

This is an FGVCx competition as part of the [FGVC^7 workshop](https://sites.google.com/view/fgvc7/home) at [CVPR 2020](http://cvpr2019.thecvf.com/). Please open an issue if you have questions or problems with the dataset.

You can find the iWildCam 2018 Competition [here](https://github.com/visipedia/iwildcam_comp/blob/master/2018/readme.md), and the iWildCam 2019 Competition [here](https://github.com/visipedia/iwildcam_comp/blob/master/2019/readme.md).

## Kaggle
We are using Kaggle to host the leaderboard. Competition page [here](https://www.kaggle.com/c/iwildcam-2020-fgvc7/overview).


## Dates
|||
|------|---------------|
Competition Starts |March 9, 2020|
Submission Deadline|May 11, 2020|


## Details and Evaluation

The WCS training set contains 217,959 images from 441 different global locations. You may also choose to use supplemental training data from iNaturalist 2017, iNaturalist 2018, iNaturalist 2019, and the provided remote sensing data from Landsat-8 which is matched to the camera locations. As a courtesy, we have curated all the images from iNaturalist 2017-2019 containing classes that might be in the test set and mapped them into the iWildCam categories (we call this set iNat_WCS). 

The test set contains 62,894 images from 111 locations globally. The location id (`location`) is given for all images. 

Submissions will be evaluated based on their macro F1 score - i.e. F1 will be calculated for each class of animal (including "empty" if no animal is present), and the submission's final score will be the unweighted mean of all class F1 scores.

## Guidelines

The general rule is that participants should only use the provided training images for training models to classify the test images. We have decided to allow the use of iNaturalist 2017-2019 data and the provided Landsat 8 tiles data during training. We do not want participants crawling the web in search of additional data or using previous versions of this dataset. Pretrained models may be used to construct the algorithms (e.g. ImageNet pretrained models, or iNaturalist 2017-2019 pretrained models). Please specify any and all external data and/or models used for training when uploading results.

Participants are allowed to collect additional annotations (e.g. bounding boxes, keypoints) on the provided training sets. Participants are not allowed to collect annotations on the test set. Teams should specify that they collected additional annotations when submitting results.


## Annotation Format
We follow the annotation format of the [COCO dataset](http://mscoco.org/dataset/#download) and add additional fields. Each training image has at least one associated annotation, containing a `category_id` that that maps the annotation to it's corresponding category label. The annotations are stored in the [JSON format](http://www.json.org/) and are organized as follows:
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
  "category_id" : int,
  # bounding boxes are in absolute, floating-point coordinates, with the origin at the upper-left
  # they are only present in a subset of the training images
  "bbox" : [x, y, width, height], 
  "area" : float #only present in a subset of the training images (those that also contain "bbox")
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
  * WCS training images 87GB zipped
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/train.zip)
      * Running `md5sum train.zip` should produce `21323ae381653240ff21768658cc44df`
  * WCS test images 153GB zipped
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/test.zip)
      * Running `md5sum test.zip` should produce `4c2eaeba30cef2d3c13b9dabcdf13c21`
  * WCS annotations and MegaDetector Results
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam2020/metadata.zip)
      * Running `md5sum metadata.zip` should produce `2fdeec4056134137cdc3dacc39ff1d90`
  * iWildCam Remote Sensing Data
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2020/iwildcam_rs_npy.tar.gz)
      * Running `md5sum iwildcam_rs_npy.tar.gz` should produce `2829a2c04898739b7fa6bd70b8e34bc2`

We also provide a smaller version of the camera trap datasets where the image width is resized to 1024 pixels:
  * Smaller WCS train images 27GB
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2019/iWildCam_2019_CCT_images_small.tar.gz)
      * Running `md5sum iWildCam_2019_CCT_images_small.tar.gz` should produce `3420db75e1db481a2a6dd4e25c9b6e61` 
  * Smaller WCS test images 18GB
    * [Download Link](https://lilablobssc.blob.core.windows.net/iwildcam2019/iWildCam_IDFG_images_small.tar.gz)
       * Running md5sum `iWildCam_IDFG_images_small.tar.gz` should produce `4ec638281337be6a086c40228e9b706d`
       
## Camera Trap Animal Detection Model
We are also providing a general animal detection model which competitors are free to use as they see fit.

The model is a tensorflow Faster-RCNN model with Inception-Resnet-v2 backbone and atrous convolution.  It can be downloaded [here](https://lilablobssc.blob.core.windows.net/models/camera_traps/megadetector/megadetector_v2.pb).

Sample code for running the detector over a folder of images can be found [here](https://github.com/Microsoft/CameraTraps/blob/master/detection/run_tf_detector.py).

We have run the detector over the three datasets, and provide the top 100 boxes and associated confidences along with the metadata for WCS. Detections are provided in the following format: 
```
{
'images':[list of image ids],
'detections':[list of lists of bounding boxes, 100 bounding boxes for each image]
'detection_labels':[list of lists of labels, all "1" for "animal" in this case],
'detection_scores':[list of lists of scores, 1 score for each detected box]
}
```

## Data Challenges
Camera trap data provides several challenges that can make it difficult to achieve accurate results.  

#### Illumination:
Images can be poorly illuminated, especially at night.  The example below contains a skunk to the center left of the frame.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/illumination.png" width="400">

#### Motion Blur:
The shutter speed of the camera is not fast enough to eliminate motion blur, so animals are sometimes blurry. The example contains a blurred coyote.

<img src="https://rawgit.com/visipedia/iwildcam_comp/master/assets/blur.png" width="400">

#### Small ROI:
Some animals are small or far from the camera, and can be difficult to spot even for humans.  The example image has a mouse on a brance to the center right of the frame.

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

By downloading iNaturalist data you agree to the terms outlined by [iNaturalist](https://github.com/visipedia/inat_comp).

### Acknowledgements

Data is primarily provided by the Wildlife Conservation Society (WCS), iNaturalist, and Microsoft AI for Earth.
