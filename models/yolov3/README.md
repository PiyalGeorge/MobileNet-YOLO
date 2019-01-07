## Training MobileNet-YOLO-v3-lite with Custom dataset

`Please kindly check last date of modification of this repo's original before proceeding`

To perform training of your own dataset for MobileNet-YOLO-v3-lite, first you need to create a LMDB database of your own dataset of specific classes. For this follow any of the links, which both are actually same.

> - https://github.com/jinfagang/kitti-ssd
> - https://github.com/PiyalGeorge/MobileNet-SSD/tree/master/create_lmdb

Here for example i'm dealing with 5 classes, so i'll explain with 5 classes.
My 5 classes are person, dog, cat, car, bus. All the below stuffs are modified acccording to 5 classes. 

Once you have LMDB, Modify certain files as follows for training.

In **MobileNet-YOLO/models/yolov3/mobilenet_yolov3_lite_train.prototxt**, modify:

- lmdb file path
- label_map.prototxt
- Modify 'num_output: 75' in the file as follows:-
  the above 75 was obtained from the calculation (5+classno)*3, where classno is the number of classes(without considering background in the count), and here eric's classno is 20.
  So if you have 5 classes(for eg:- person, dog, cat, car, bus), then the value is (5+5)*3=30, so modify 'num_output: 75' in file to 'num_output: 30'.
- Search for 'num_class: 20' in the file and modify the value as follows:-
  if you have 5 classes(for eg:- person, dog, cat, car, bus), then modify the value into 'num_class: 5'

In **MobileNet-YOLO/models/yolov3/mobilenet_yolov3_lite_test.prototxt**, modify:

- lmdb file path
- label_map.prototxt
- Modify 'num_output: 75' in the file as follows:-
  the above 75 was obtained from the calculation (5+classno)*3, where classno is the number of classes(without considering background in the count), and here eric's classno is 20.
  So if you have 5 classes(for eg:- person, dog, cat, car, bus), then the value is (5+5)*3=30, so modify 'num_output: 75' in file to 'num_output: 30'.
- Modify 'num_classes: 20' to 'num_classes: 5'
- Modify 'num_classes: 21' to 'num_classes: 6'

Once Training is done:

Modify the **MobileNet-YOLO/examples/yolo/yolo_detect.cpp** . You need to modify the line
char* CLASSES[21] = { "__background__", ....., "train", "tvmonitor" };
with our classes like this:
char* CLASSES[6] = { "__background__", "person", "dog", "cat", "car", "bus" };

Then once again run make commands:
> cd build

> cmake ..

> make -j4

> make pycaffe

In **MobileNet-YOLO/demo_yolo_lite.sh**, modify: 

- Modify 'models/yolov3/mobilenet_yolov3_lite_deploy.prototxt' with 'models/yolov3/mobilenet_yolov3_lite_bn_deploy.prototxt' 

In **MobileNet-YOLO/models/yolov3/mobilenet_yolov3_lite_bn_deploy.prototxt** modify:-

- Modify 'num_output: 75' to 'num_output: 30'
- Modify 'num_classes: 20' to 'num_classes: 5' 

Now run the command `bash demo_yolo_lite.sh` to get the results.




## Deploy model

The bias_term error was cause from dismatch caffemodel , please make sure your model was merged batchnorm or not 

* mobilenet_yolov3_lite_deploy.prototxt : merged 

* mobilenet_yolov3_lite_bn_deploy.prototxt : not merged

See issue :

https://github.com/eric612/MobileNet-YOLO/issues/42

https://github.com/eric612/MobileNet-YOLO/issues/50
