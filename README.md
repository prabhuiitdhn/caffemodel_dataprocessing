For creating the caffemodel using custom dataset needs three steps:
1. Data processing
   -In this step of creating caffemodel, data in the form of images needs to be in train.txt and val.txt file for converting the dataset in LMDB dataset format(Lightning Memory-Mapped Database (LMDB) is a software library that provides a high-performance embedded transactional database in the form of a key-value store.)
2. caffe Network file prepation
3. Training and finetuning the parameters 

for Converting to LMDB:
To feed Caffe with large images dataset it’s good choice to use LMDB format for our dataset. We already have an example of a script in Caffe folder (I suppose you have Caffe built on your machine) here caffe/examples/imagenet/create_imagenet.sh.

We need to change following things:

    EXAMPLE=examples/dogs : where we are going to store LMDB
    DATA=data/dogs/dogs_data : folder with dogs train.txt, val.txt
    TRAIN_DATA_ROOT : folder with train images
    VAL_DATA_ROOT : folder with test images (with script above it’s same folder)
    RESIZE=true : we need to resize all photos to same size
    And following piece of code:

GLOG_logtostderr=1 $TOOLS/convert_imageset \
 — resize_height=$RESIZE_HEIGHT \
— resize_width=$RESIZE_WIDTH \
— shuffle \
$TRAIN_DATA_ROOT \
$DATA/train.txt \
$EXAMPLE/dogs_train_lmdb
echo “Creating val lmdb…”

GLOG_logtostderr=1 $TOOLS/convert_imageset \
— resize_height=$RESIZE_HEIGHT \
— resize_width=$RESIZE_WIDTH \
— shuffle \
$VAL_DATA_ROOT \
$DATA/val.txt \
$EXAMPLE/dogs_val_lmdb

    Above we set $DATA/train.txt, $DATA/val.txt and $EXAMPLE/dogs_train_lmdb, $EXAMPLE/dogs_val_lmdb

You can also use (you will need it for some Caffe prototxt’s) make_mean.sh to generate mean file from input images (for further substraction in preprocessing step)

The following program is for converting the images in different-different labess in text format so that caffemodel architecture will be able to access the file and labels directly from .txt file for training and texting.
The following things are:
1. caffe_path //list of images file with labels
2. new_path // assign new path for storing the modified images, train.txt and val.txt file
We're splitting the data; 85% for training and remaining for testing.
for executing the program:
python data_process.py
