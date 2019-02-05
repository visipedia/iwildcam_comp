![Banner](https://rawgit.com/visipedia/iwildcam_comp/sara/assets/iwildcam_2019_banner.jpg)

# iWildCam 2019 Competition
Camera Traps (or Wild Cams) enable the automatic collection of large quantities of image data. Biologists all over the world use camera traps to monitor biodiversity and population density of animal species.  We have recently been making strides towards automating the species classification challenge in camera traps, but as we try to expand the scope of these models from specific regions where we have collected training data to nearby areas we are faced with an interesting probem: how do you classify a species in a new region that you may not have seen in previous training data?

In order to tackle this problem, we have prepared a challenge where the training data and test data are from different regions, namely The American Southwest and the American Northwest.  The species seen in each region overlap, but are not identical, and the challenge is to classify the test species correctly.  To this end, we will allow training on our American Southwest data (from [CaltechCameraTraps](https://beerys.github.io/CaltechCameraTraps/)), on [iNaturalist 2017/2018](https://github.com/visipedia/inat_comp) data, and on simulated data generated from [Microsoft AirSim](https://github.com/Microsoft/AirSim) using our new TrapCam environment.  We have provided a taxonomy file mapping our classes into the iNat taxonomy. 

This is an FGVCx competition as part of the [FGVC^6 workshop](https://sites.google.com/view/fgvc6/home) at [CVPR 2019](http://cvpr2019.thecvf.com/). Please open an issue if you have questions or problems with the dataset.


## Kaggle
We are using Kaggle to host the leaderboard. Competition page coming soon.


## Dates
|||
|------|---------------|
Competition Starts |March, 2019|
Submission Deadline|June 7th, 2019|


## Details and Evaluation

There are a total 196,157 camera trap training images from 138 different camera locations in Southern California. iNaturalist 2017 and 2018 are also allowed to be used during training time. Competitors are encouraged to construct a validation set from the training datasets as they see fit.

The test set contains 153,730 images from 100 locations in Idaho. The location id (`location`) is given for all images. 

The set of training classes is:
'bobcat', 'opossum', 'coyote', 'raccoon', 'dog', 'cat', 'squirrel', 'rabbit', 'skunk', 'rodent', 'deer', 'fox', 'mountain_lion', 'empty'

The set of test classes is:
'bobcat', 'opossum', 'coyote', 'raccoon', 'dog', 'cat', 'squirrel', 'rabbit', 'skunk', 'rodent', 'deer', 'fox', 'mountain_lion', 'moose', 'small_mammal', 'elk', 'pronghorn', 'bighorn_sheep', 'black_bear', 'wolf', 'bison', 'mountain_goat', 'empty'

Not all test classes are guaranteed to be in the test set.  The list of test classes was determined based on species that have been seen in Idaho on iNaturalist, and an explicit mapping between all classes and the iNat taxonomy will be provided.

The evaluation metric is top-1 accuracy i.e. correctly predicting the class of each animal, or predicting "empty" if no animal is present.


## Guidelines

The general rule is that participants should only use the provided training images for training models to classify the test images. We have decided to allow the use of iNaturalist 2017/2018 data during training. We do not want participants crawling the web in search of additional data or using previous versions of this dataset. Pretrained models may be used to construct the algorithms (e.g. ImageNet pretrained models, or iNaturalist 2017/2018 pretrained models). Please specify any and all external data and/or models used for training when uploading results.

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
  "location": int
}

category{
  "id" : int,
  "name" : str
}

annotation{
  "id" : str,
  "image_id" : str,
  "category_id" : int,
  "bbox" : [x, y, width, height], #only present in a subset of the training images
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


## Terms of Use

By downloading the training datasets you agree to the following terms:

1. You will use the data only for non-commercial research and educational purposes.
2. You will NOT distribute the above images.
3. The California Institute of Technology makes no representations or warranties regarding the data, including but not limited to warranties of non-infringement or fitness for a particular purpose.
4. You accept full responsibility for your use of the data and shall defend and indemnify the California Institute of Technology, including its employees, officers and agents, against any and all claims arising from your use of the data, including but not limited to your use of any copies of copyrighted images that you may create from the data.

By downloading the test dataset you agree to the following terms:


## Data
All datasets will be released soon

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

### Acknowledgements

Data is primarily provided by Erin Boydston (USGS), Justin Brown (NPS), iNaturalist, and the Idaho Department of Fish and Game (IDFG).
