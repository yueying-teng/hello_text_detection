# hello_text_object_detection
detect locatin of hello in the image 

# tensorflow object detection api 
[installation](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md)

# data preparation 
bounding box label generatd using [LabelBox](https://app.labelbox.com/projects)

LabelBox coordinates origin is differnet from that required by the script which convert label csv to TFRecord.
use this [jupyter notebook](https://github.com/yueying-teng/hello_text_object_detection/blob/master/process_labelbox_csv.ipynb) to 
- change bounding box coordiantes origin from bottom left to upper left corner 
- resize all pictures so that the maximum size is 300 x 500, while keeping the aspect ratio
- remove pictures with illogical bounding box coordinates, e.g. xmin > xmax, from training, but keep them in testing
- create train.csv and test.csv 

convert train.csv and test.csv to train.record and test.record using [this script](https://github.com/yueying-teng/hello_text_object_detection/blob/master/generate_tfrecord.py) 

create [hello_label_map](https://github.com/yueying-teng/hello_text_object_detection/blob/master/train/hello_label_map.pbtxt)

# training 
is done on google cloud ml engine by following [this](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/running_pets.md)

files needed are 
- ssd_mobilenet_v2_coco.config, which can be downloaded from [here](https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/detection_model_zoo.md)
  - change num_classes from 90 to 1, see [here](https://github.com/yueying-teng/hello_text_object_detection/blob/master/train/ssd_mobilenet_v2_coco.config)
- [script](https://github.com/yueying-teng/hello_text_object_detection/blob/master/train/train_eval_script) to submit job to google cloud ml engine
  - tar pycocotools-2.0.tar.gz by following [this](https://github.com/tensorflow/models/issues/3470#issuecomment-375985049) to make eval job work on cloud 

tried to train using faster_rcnn_resnet101_coco, but keep getting this error message - **The replica master 0 exited with a non-zero status of 1. Termination reason: Error.**

# model export 
download mdoel checkpoint files to local and use [this script](https://github.com/yueying-teng/hello_text_object_detection/blob/master/train/train_eval_script) to export the model

# demo 
can be found [here](https://github.com/yueying-teng/hello_text_object_detection/blob/master/hello_text_object_detection.ipynb)
