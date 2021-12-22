# Fault injection on different deep learning models on imagenet dataset

## Installation

Prerequisites for installing the project,
1. Ubuntu OS (Tested with Ubuntu 18.04)
2. Python 2.7 (Not python 3)

Run the following commands to install cmake, protobuf and python dev tools if not 
present(Required mainly for caffe-tensorflow)
```
sudo apt-get install -y gcc python-dev musl-dev libjpeg-dev zlib1g zlib1g-dev libprotobuf-dev protobuf-compiler cmake
```

We highly recommend making a virtualenv. Run the following command to create a virtual env.
```
virtualenv -p python2 venv
source venv/bin/activate
```
Install the requirements,
```
pip install -r requirements.txt
```

# Run

Although this project is intended to make inference over complete validation set of ilsvrc12,
for ease of portability we are providing first 10 sample images and labels in this repository
from ilsvrc12 validation set. For experiment with full dataset, please download it from
imagenet website(size around 6.8GB). And, download complete labels from this link:
<http://dl.caffe.berkeleyvision.org/caffe_ilsvrc12.tar.gz>

We are interested to convert caffe pretrained weights to tensorflow and apply inference with
it. The prototexts and caffemodel for GoogleNet is downloaded from
<https://github.com/BVLC/caffe/tree/master/models/bvlc_googlenet> and stored in googlenet
folder

Run the following command to convert caffe weights into tensorflow weights:
```
python convert.py googlenet/deploy.prototxt --caffemodel googlenet/bvlc_googlenet.caffemodel --data-output-path googlenet/googlenet_imagenet.npy --code-output-path googlenet/googlenet_imagenet.py
```
** Generated tensorflow npy file and model class files are already provided with this
repository inside googlenet folder.

The validation script for testing inference accuracy was taken from
<https://github.com/ethereon/caffe-tensorflow/tree/master/examples/imagenet>.

Run the following Command to inference over full dataset. This provides similar accuracy mentioned in caffe-tensorflow repository.
```
python validate.py googlenet/googlenet_imagenet.npy val.txt val_data/ --model GoogleNet
```
** This won't work readily, because val_data folder and val.txt file is missing in this repository. You need to download full validation set and extract to val_data folder and provide labels for this 50000 images in val.txt file. Full download procedure is mentioned above.

Run the following command to apply fault injection with partial data.
```
python validate_fi.py googlenet/googlenet_imagenet.npy val2.txt val_data_2/ --model GoogleNet
```