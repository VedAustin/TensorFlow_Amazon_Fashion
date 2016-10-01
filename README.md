# TensorFlow_Amazon_Fashion
An alternate version of transfer learning .. this time not using indico - 3rd party api (see Fashion_reco).
A major difference between these two versions is that in ![tensorflow example](https://codelabs.developers.google.com/codelabs/tensorflow-for-poets/?utm_campaign=chrome_series_machinelearning_063016&utm_source=gdev&utm_medium=yt-desc#0) it will only label your test images. Which means you will have to organise your photos into categories. See the 'data' folder. A hack that might work in order to reproduce what you get like at Indico is to put an image in a folder of its own i.e. one folder-one image

Transfer learning using tensorflow

Steps to recreate:

1. If you are starting from clean slate great! Else remove previous versions of docker toolbox and virtual box and download docker and use their default virtualbox installation version. It works.
2. Open docker start terminal
3. `docker run -it -v /$(pwd)/tf_files-local:/tf_files-container gcr.io/tensorflow/tensorflow:latest-devel`

  $(pwd)-> should return C:\Users\<user> - but dont wory about this just use $(pwd)

  tf_files-local-> Local directory tree structure should be like this: C:\Users\<user>\tf_files-local
  
  Ideally you will keep data necessary for training here. In our case it will be like C:\Users\<user>\tf_files-local\data\amazon_train
  
  tf_files-container will be the folder created in the docker container that will contain data\amazon_train and image_label.py
4. `root@<containerId>: cd /tensorflow`
5. `root@<containerId>: git pull`
6. ```
  root@<containerId>:python tensorflow/examples/image_retraining/retrain.py \
  --bottleneck_dir=/tf_files-container/bottlenecks \
  --model_dir=/tf_files-container/inception \
  --output_graph=/tf_files-container/retrained_graph.pb \
  --output_labels=/tf_files-container/retrained_labels.txt \
  --image_dir /tf_files-container/data/amazon_train
   ```


Now test individual images:
  ```
root@<containerId>:~# python /tf_files-container/label_image.py \
/tf_files-container/data/amazon_test/pic\ \(148\).jpg
  ```

Now the image will be classified by a label sorted by probability scores in descending order.

Sample output:
![alt text](https://github.com/VedAustin/TensorFlow_Amazon_Fashion/blob/master/github-tensorflow.png)
