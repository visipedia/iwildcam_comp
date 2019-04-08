![Banner](https://rawgit.com/visipedia/iwildcam_comp/sara/assets/iwildcam_2019_banner.jpg)

# iWildCam 2019 Competition
Camera Traps (or Wild Cams) enable the automatic collection of large quantities of image data. Biologists all over the world use camera traps to monitor biodiversity and population density of animal species.  We have recently been making strides towards automating the species classification challenge in camera traps, but as we try to expand the scope of these models from specific regions where we have collected training data to nearby areas we are faced with an interesting probem: how do you classify a species in a new region that you may not have seen in previous training data?

In order to tackle this problem, we have prepared a challenge where the training data and test data are from different regions, namely The American Southwest and the American Northwest.  The species seen in each region overlap, but are not identical, and the challenge is to classify the test species correctly.  To this end, we will allow training on our American Southwest data (from [CaltechCameraTraps](https://beerys.github.io/CaltechCameraTraps/)), on [iNaturalist 2017/2018](https://github.com/visipedia/inat_comp) data, and on simulated data generated from [Microsoft AirSim](https://github.com/Microsoft/AirSim).  We have provided a taxonomy file mapping our classes into the iNat taxonomy. 

This is an FGVCx competition as part of the [FGVC^6 workshop](https://sites.google.com/view/fgvc6/home) at [CVPR 2019](http://cvpr2019.thecvf.com/). Please open an issue if you have questions or problems with the dataset.

There is $2,000 sponsored by [Microsoft AI for Earth](https://www.microsoft.com/en-us/ai/ai-for-earth) that will be awarded at the FGVC^6 workshop to top challenge solutions at the discretion of the competition hosts.  At the end of the competition we will invite the top-scoring teams to document and open-source their methods and models.  The prize money will be awarded to one or more teams based on their solutions: a combination of competition results, documentation and open-sourcing of methods and models, and method novelty.  

You can find the iWildCam 2018 Competition [here](https://github.com/visipedia/iwildcam_comp/blob/master/2018/readme.md).

## Kaggle
We are using Kaggle to host the leaderboard. Competition page coming soon.


## Dates
|||
|------|---------------|
Competition Starts |March, 2019|
Submission Deadline|June 7th, 2019|


## Details and Evaluation

The Caltech Camera Traps training set includes a total of 196,157 camera trap training images from 138 different camera locations in Southern California. The iNaturalist 2017 and 2018 data and simulated Microsoft AirSim data are also allowed to be used during training time. Competitors are encouraged to construct a validation set from the training datasets as they see fit.

The test set contains 153,730 images from 100 locations in Idaho. The location id (`location`) is given for all images. 

The set of training classes is:
'bobcat', 'opossum', 'coyote', 'raccoon', 'dog', 'cat', 'squirrel', 'rabbit', 'skunk', 'rodent', 'deer', 'fox', 'mountain_lion', 'empty'

The set of test classes is:
'bobcat', 'opossum', 'coyote', 'raccoon', 'dog', 'cat', 'squirrel', 'rabbit', 'skunk', 'rodent', 'deer', 'fox', 'mountain_lion', 'moose', 'small_mammal', 'elk', 'pronghorn', 'bighorn_sheep', 'black_bear', 'wolf', 'bison', 'mountain_goat', 'empty'

Not all test classes are guaranteed to be in the test set.  The list of test classes was determined based on species that have been seen in Idaho on iNaturalist, and an explicit mapping between all classes and the iNat taxonomy is provided.

To reduce the ovehead of entry, we have provided a subset of iNaturalist 2017 and 2018 data containing the classes seen in Idaho, and explicitly mapping them to the test classes.

The evaluation metric is top-1 accuracy i.e. correctly predicting the class of each animal, or predicting "empty" if no animal is present.


## Guidelines

The general rule is that participants should only use the provided training images for training models to classify the test images. We have decided to allow the use of iNaturalist 2017/2018 data and simulated Microsoft AirSim data during training. We do not want participants crawling the web in search of additional data or using previous versions of this dataset. Pretrained models may be used to construct the algorithms (e.g. ImageNet pretrained models, or iNaturalist 2017/2018 pretrained models). Please specify any and all external data and/or models used for training when uploading results.

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
  * CCT images and annotations 87GB zipped
    * [Download Link](http://lilablobssc.blob.core.windows.net/iwildcam2019/iWildCam_2019_CCT.tar.gz)
      * Running `md5sum iWildCam_2019_CCT.tar.gz` should produce `21323ae381653240ff21768658cc44df`
  * IDFG images and annotations 153GB zipped
    * [Download Link](http://lilablobssc.blob.core.windows.net/iwildcam2019/iWildCam_2019_IDFG.tar.gz)
      * Running `md5sum iWildCam_2019_IDFG.tar.gz` should produce `4c2eaeba30cef2d3c13b9dabcdf13c21`
  * iNat Idaho images and annotations 7.2GB zipped
    * [Download Link](http://lilablobssc.blob.core.windows.net/iwildcam2019/iWildCam_2019_iNat_Idaho.tar.gz)
      * Running `md5sum iWildCam_2019_iNat_Idaho.tar.gz` should produce `2fdeec4056134137cdc3dacc39ff1d90`
  * All annotations and iNat taxa map 156MB
    * [Download Link](http://lilablobssc.blob.core.windows.net/iwildcam2019/iWildCam_2019_Annotations.tar.gz)
      * Running `md5sum iWildCam_2019_Annotations.tar.gz` should produce `2829a2c04898739b7fa6bd70b8e34bc2`
  * Sample submission file
    * [Download Link](http://lilablobssc.blob.core.windows.net/iwildcam2019/IDFG_Sample_Submission.csv)

We also provide a smaller version of the dataset where the image width is resized to 1024 pixels:
  * Smaller CCT images 27GB
    * [Download Link](http://lilablobssc.blob.core.windows.net/iwildcam2019/iWildCam_2019_CCT_images_small.tar.gz)
      * Running `md5sum iWildCam_2019_CCT_images_small.tar.gz` should produce `3420db75e1db481a2a6dd4e25c9b6e61` 
  * Smaller IDFG images 18GB
    * [Download Link](http://lilablobssc.blob.core.windows.net/iwildcam2019/iWildCam_IDFG_images_small.tar.gz)
       * Running md5sum `iWildCam_IDFG_images_small.tar.gz` should produce `4ec638281337be6a086c40228e9b706d`

## Camera Trap Animal Detection Model
We are also providing a general animal detection model which competitors are free to use as they see fit.

The model is a tensorflow Faster-RCNN model with Inception-Resnet-v2 backbone and atrous convolution.  It can be downloaded [here](https://lilablobssc.blob.core.windows.net/models/camera_traps/megadetector/megadetector_v2.pb).

Sample code for running the detector over a folder of images can be found [here](https://github.com/Microsoft/CameraTraps/blob/master/detection/run_tf_detector.py).

We have run the detector over the three datasets, and provide the top 100 boxes and associated confidences [here](http://lilablobssc.blob.core.windows.net/iwildcam2019/Detection_Results.tar.gz). Detections are provided in the following format: 
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

By downloading Caltech Camera Traps data you agree to the terms in the [Community Data License Agreement (CDLA)](https://cdla.io/permissive-1-0/).

By downloading iNaturalist data you agree to the terms outlined by [iNaturalist](https://github.com/visipedia/inat_comp).

By downloading IDFG data you agree to the following terms:
1. No representations or warranties are made regarding the data, including but not limited to warranties of non-infringement or fitness for a particular purpose.  Some information shared under this agreement may not have undergone quality assurance procedures and should be considered provisional.
2. Images may not be sold in any format, but may be used for scientific publications.  Please acknowledge the Idaho Department of Fish and Game when using images for publication or scientific communication.

### Acknowledgements

Data is primarily provided by Erin Boydston (USGS), Justin Brown (NPS), iNaturalist, and the Idaho Department of Fish and Game (IDFG).
