![Banner](https://rawgit.com/visipedia/iwildcam_comp/master/assets/iwildcam3.jpg)

# iWildCam 2018 Competition
Camera Traps (or Wild Cams) enable the automatic collection of large quantities of image data. Unfortunately, a large number of the images that end up being captured tend to be false positives. This is typically caused by non-animal motion in the scene e.g. the wind moving trees. 

The goal of this competition is to predict if images from a diverse set of unseen locations that have been captured both during the day and at night contain an animal. The main challenge is generalizing to camera trap locations that are not present in the training set. Another challenge is that some images contain other objects (e.g. people or vehicles) that can trigger the cameras but are not of interest. 

This is an FGVCx competition as part of the [FGVC^5 workshop](https://sites.google.com/view/fgvc5/home) at [CVPR](http://cvpr2018.thecvf.com/). Please open an issue if you have questions or problems with the dataset.




## Dates
|||
|------|---------------|
Data Released|February, 2018|
Submission Server Open |March, 2018|
Submission Deadline|June, 2018|


## Details and Evaluation

There are a total 149,359 training images from 70 different camera locations and 17,784 validation images from a mix of locations in the training set and 5 new locations. The test set contains 125,589 images from 68 locations that are not present in the training or validation sets. The location id (`location`) is given for all images.

The evaluation metric is the area under the ROC curve computed from a predicted continuous value that indicates if a given test image contains an animal. Higher values indicate that an image is more likely to contain an animal. A subset of the images come from short sequences of up to 3 images. We do not provide this meta data but it can be extracted each image.  


## Guidelines

The general rule is that participants should only use the provided training and validation images for training models to classify the test images. We do not want participants crawling the web in search of additional data. Pretrained models may be used to construct the algorithms (e.g. ImageNet pretrained models, or iNaturalist 2017/2018 pretrained models). Please specify any and all external data used for training when uploading results.

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
id,animal_present_score
7b641084-7d68-11e7-884d-7845c41c2c67,0.875
bcc7b4db-7d71-11e7-884d-7845c41c2c67,0.231
...
```
The `id` column corresponds to the test image id. The `animal_present_score` column corresponds a score indicating that an animal is present in the image. Higher values indicate that the image is more likely to contain an animal.


## Terms of Use

By downloading this dataset you agree to the following terms:

1. You will use the data only for non-commercial research and educational purposes.
2. You will NOT distribute the above images.
3. The California Institute of Technology makes no representations or warranties regarding the data, including but not limited to warranties of non-infringement or fitness for a particular purpose.
4. You accept full responsibility for your use of the data and shall defend and indemnify the California Institute of Technology, including its employees, officers and agents, against any and all claims arising from your use of the data, including but not limited to your use of any copies of copyrighted images that you may create from the data.


## Data

Download the dataset files here:
  * Training and validation images [75.7GB]()
    * Running `md5sum train_val.tar.gz` should produce `1f97427c1ea5e46655b7faf8c18e8169`
  * Test images [59.1GB]()
    * Running `md5sum test.tar.gz` should produce `dce9f4ea2dac9ce87bfecbe1935610aa`
  * Train and validation annotations, and test information [XXXMB]()
    * Running `md5sum annotations.tar.gz` should produce `XXX`

We also provide a smaller version of the dataset where the image width is resized to 1024 pixels:      
  * Smaller training and validation images [23.2GB]()
    * Running `md5sum train_val.tar.gz` should produce `7216b6b6b1ef3c4b59980c398400869d`
  * Smaller test images [17.9GB]()
    * Running `md5sum test.tar.gz` should produce `3628bfed32e9c1666002899d66d5def9`
      

### Acknowledgements

Data is primarily provided by Erin Boydston (USGS) and Justin Brown (NPS).



