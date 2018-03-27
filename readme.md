![Banner](https://rawgit.com/visipedia/iwildcam_comp/master/assets/iwildcam3.jpg)

# iWildCam 2018 Competition
Camera Traps (or Wild Cams) enable the automatic collection of large quantities of image data. Unfortunately, a large number of the images that end up being captured tend to be false positives. This is typically caused by non-animal motion in the scene e.g. the wind moving trees. 

The goal of this competition is to predict if images from a diverse set of unseen locations that have been captured both during the day and at night contain an animal. The main challenge is generalizing to camera trap locations that are not present in the training set, large variations in appearance in the same location over time. Another challenge is that some images contain other objects (e.g. people or vehicles) that can trigger the cameras but are not of interest. The animals of interest can be very small, partially occluded, or exiting the frame - you sometimes have to look hard to find them. There may also be a small number of incorrect annotations in the training set.  

This is an FGVCx competition as part of the [FGVC^5 workshop](https://sites.google.com/view/fgvc5/home) at [CVPR](http://cvpr2018.thecvf.com/). Please open an issue if you have questions or problems with the dataset.


## Kaggle
We are using Kaggle to host the leaderboard. Checkout the competition page [here](https://www.kaggle.com/c/iwildcam2018).


## Dates
|||
|------|---------------|
Competition Starts |March, 2018|
Submission Deadline|June 4th, 2018|


## Details and Evaluation

There are a total 106,428 training images from 65 different camera locations and 12,719 validation images from 10 new locations not seen at training time. The test set contains 124,040 images from 65 locations that are not present in the training or validation sets. The location id (`location`) is given for all images. 

The evaluation metric is overall accuracy i.e. correctly predicting which of the test images contain animals.


## Guidelines

The general rule is that participants should only use the provided training and validation images for training models to classify the test images. We do not want participants crawling the web in search of additional data or using previous versions of this dataset. Pretrained models may be used to construct the algorithms (e.g. ImageNet pretrained models, or iNaturalist 2017/2018 pretrained models). Please specify any and all external data used for training when uploading results.

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
The `id` column corresponds to the test image id. The `animal_present` is a binary value that indicates if an animal is predicted to be present in the image.


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
Camera trap data provides several challenges that can make it difficult to get good results.  

Illumination:
Images can be poorly illuminated, especially at night.  The example below contains a skunk to the center left of the frame.

![alt text](https://rawgit.com/visipedia/iwildcam_comp/master/assets/illumination.png)

Motion Blur:
The shutter speed of the camera is not fast enough to eliminate motion blur, so animals are sometimes blurry. The example contains a blurred coyote.

![alt text](https://rawgit.com/visipedia/iwildcam_comp/master/assets/blur.png)

Small ROI:
Some animals are small or far from the camera, and can be difficult to spot even for humans.  The example image has a mouse on a brance to the center right of the frame.

![alt text](https://rawgit.com/visipedia/iwildcam_comp/master/assets/smallroi.png)

Occlusion:
Animals can be occluded by vegetation or the edge of the frame.  This example shows a location where weeds grew in front of the camera, obscuring the view.

![alt text](https://rawgit.com/visipedia/iwildcam_comp/master/assets/occlusion.png)

Perspective:
Sometimes animals come very close to the camera, causing a forced perspective.

![alt text](https://rawgit.com/visipedia/iwildcam_comp/master/assets/perspective.png)

Weather Conditions:
Poor weather, including rain or dust, can obstruct the lens and cause false triggers.

![alt text](https://rawgit.com/visipedia/iwildcam_comp/master/assets/weather.png)

Camera Malfunctions:
Sometimes the camera malfunctions, causing strange discolorations.

![alt text](https://rawgit.com/visipedia/iwildcam_comp/master/assets/malfunctions.png)

### Acknowledgements

Data is primarily provided by Erin Boydston (USGS) and Justin Brown (NPS).
