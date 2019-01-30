![Banner](https://rawgit.com/visipedia/iwildcam_comp/sara/assets/iwildcam_2019_banner.jpg)

# iWildCam 2019 Competition
Camera Traps (or Wild Cams) enable the automatic collection of large quantities of image data. Biologists all over the world use camera traps to monitor biodiversity and population density of animal species.  We have recently been making strides towards automating the species classification challenge in camera traps, but as we try to globalize we are faced with an interesting open-set probem: how do you classify a species in a new region that you may not have seen in previous areas?

In order to tackle this problem, we have prepared an open-set challenge where the training data and test data are from different regions, namely The American Southwest and the American Northwest.  The species seen in each region overlap, but are not identical, and the challenge is to classify the test species correctly.  To this end, we will allow training on our American Southwest data (from [CaltechCameraTraps](https://beerys.github.io/CaltechCameraTraps/)), on iNaturalist 2017/2018 data, and on simulated data generated from [Microsoft AirSim](https://github.com/Microsoft/AirSim) using our new TrapCam environment.  We have provided a taxonomy file mapping our classes into the iNat taxonomy. 

This is an FGVCx competition as part of the [FGVC^6 workshop](https://sites.google.com/view/fgvc6/home) at [CVPR 2019](http://cvpr2019.thecvf.com/). Please open an issue if you have questions or problems with the dataset.


## Kaggle
We are using Kaggle to host the leaderboard. Checkout the competition page [here](https://www.kaggle.com/c/iwildcam2019).


## Dates
|||
|------|---------------|
Competition Starts |March, 2019|
Submission Deadline|June 7th, 2019|


## Details and Evaluation

There are a total 106,428 training images from 65 different camera locations and 12,719 validation images from 10 new locations not seen at training time. The test set contains 124,040 images from 65 locations that are not present in the training or validation sets. The location id (`location`) is given for all images. 

The evaluation metric is top-1 accuracy i.e. correctly predicting the class of each animal, or predicting "empty" if no animal is present.


## Guidelines

The general rule is that participants should only use the provided training and validation images for training models to classify the test images. We have decided to allow the use of iNaturalist 2017/2018 data during training. We do not want participants crawling the web in search of additional data or using previous versions of this dataset. Pretrained models may be used to construct the algorithms (e.g. ImageNet pretrained models, or iNaturalist 2017/2018 pretrained models). Please specify any and all external data used for training when uploading results.

Participants are allowed to collect additional annotations (e.g. bounding boxes, keypoints) on the provided training and validation sets. Teams should specify that they collected additional annotations when submitting results.


## Annotation Format
We follow the annotation format of the [COCO dataset](http://mscoco.org/dataset/#download) and add additional fields. Each training images has a `category_id` that is either `0` indicating no animal present or `1` indicating animal present. The annotations are stored in the [JSON format](http://www.json.org/) and are organized as follows:
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
  "category_id" : int
}
```

## Submission Format

The submission format for the competition is a csv file with the following format:
```
id,animal_present
58857ccf-23d2-11e8-a6a3-ec086b02610b,1
591e4006-23d2-11e8-a6a3-ec086b02610b,0
...
```
The `id` column corresponds to the test image id. The `animal_class` is an integer value that indicates the class of the animal, or `0` to represent the absence of an animal.


## Terms of Use

By downloading this dataset you agree to the following terms:

1. You will use the data only for non-commercial research and educational purposes.
2. You will NOT distribute the above images.
3. The California Institute of Technology makes no representations or warranties regarding the data, including but not limited to warranties of non-infringement or fitness for a particular purpose.
4. You accept full responsibility for your use of the data and shall defend and indemnify the California Institute of Technology, including its employees, officers and agents, against any and all claims arising from your use of the data, including but not limited to your use of any copies of copyrighted images that you may create from the data.


## Data

Download the dataset files here:
  * Training and validation images 50.43GB zipped  
      * Links for different parts of the world:
        * [North America](https://storage.googleapis.com/iwildcam_2018_us/train_val.tar.gz)
        * [Asia](https://storage.googleapis.com/iwildcam_2018_asia/train_val.tar.gz)
        * [Europe](https://storage.googleapis.com/iwildcam_2018_eu/train_val.tar.gz)
      * Running `md5sum train_val.tar.gz` should produce `7dcbbee870c407c69fd6012e5f3dd16c`
  * Test images 52.52GB zipped  
     * Links for different parts of the world:
        * [North America](https://storage.googleapis.com/iwildcam_2018_us/test.tar.gz)
        * [Asia](https://storage.googleapis.com/iwildcam_2018_asia/test.tar.gz)
        * [Europe](https://storage.googleapis.com/iwildcam_2018_eu/test.tar.gz)
    * Running `md5sum test.tar.gz` should produce `22423c6896b536b93e95334b544c4c0a`
  * Train and validation annotations 3.65MB
     * Links for different parts of the world:
        * [North America](https://storage.googleapis.com/iwildcam_2018_us/iwildcam2018_annotations.tar.gz)
        * [Asia](https://storage.googleapis.com/iwildcam_2018_asia/iwildcam2018_annotations.tar.gz)
        * [Europe](https://storage.googleapis.com/iwildcam_2018_eu/iwildcam2018_annotations.tar.gz)
    * Running `md5sum iwildcam2018_annotations.tar.gz` should produce `62437e267340c0a0ccc801eb9a041564`

We also provide a smaller version of the dataset where the image width is resized to 1024 pixels:
  * Smaller training and validation images 20.23GB
      * Links for different parts of the world:
        * [North America](https://storage.googleapis.com/iwildcam_2018_us/train_val_sm.tar.gz)
        * [Asia](https://storage.googleapis.com/iwildcam_2018_asia/train_val_sm.tar.gz)
        * [Europe](https://storage.googleapis.com/iwildcam_2018_eu/train_val_sm.tar.gz)
      * Running `md5sum train_val.tar.gz` should produce `6d0b494a4c115f833aac2850567d1da9`
  * Smaller test images 21.02GB
     * Links for different parts of the world:
        * [North America](https://storage.googleapis.com/iwildcam_2018_us/test_sm.tar.gz)
        * [Asia](https://storage.googleapis.com/iwildcam_2018_asia/test_sm.tar.gz)
        * [Europe](https://storage.googleapis.com/iwildcam_2018_eu/test_sm.tar.gz)
    * Running `md5sum test.tar.gz` should produce `9a6257a3ac7d298ef5bdc324a3e0efc5`
 
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
Poor weather, including rain or dust, can obstruct the lens and cause false triggers.

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

Data is primarily provided by Erin Boydston (USGS), Justin Brown (NPS), and the Idaho Department of Fish and Game (IDFG).
