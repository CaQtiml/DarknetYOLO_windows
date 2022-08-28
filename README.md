# Darknet YOLO Installation for Windows OS

This is a note about how I set Darknet so that it can run on my Windows PC. If you use Linux/MacOS or not use CUDA GPU, get my main references and you can close this note. This blog will guide how to read blogs that tell you how to install Darknet. 

Each blog already goods, but not perfect. Combining a good part of each blog together makes an almost perfect installation guide.

## Main References
- https://github.com/AlexeyAB/darknet
- https://medium.com/geekculture/yolov4-darknet-installation-and-usage-on-your-system-windows-linux-8dec2cea6e81#db4e
- https://medium.com/@quangnhatnguyenle/how-to-train-yolov3-on-google-colab-to-detect-custom-objects-e-g-gun-detection-d3a1ee43eda1#d727

## Other Random Blogs I found during doing this work.
- https://manivannan-ai.medium.com/how-to-train-yolov3-to-detect-custom-objects-ccbcafeb13d2

For training, I use Gun dataset to create gun detection model. The dataset which has both images and labels is [here](http://www.mediafire.com/file/pvfircmboaelkxc/Gun_data_labeled.zip/file) (this is from the third reference link). After downloading dataset, read his article so that you know what he already provides. For now, you can read only about the data. Darknet configuration can be ignored for now.

For the base code, I recommend you to clone from either AlexeyAB Darkent github (first reference) or this repository as I already do some cleaning. Then, manually creates `backup` folder.

After cloning the repository, you have to compile the project. Read the step 4 of [the second reference](https://medium.com/geekculture/yolov4-darknet-installation-and-usage-on-your-system-windows-linux-8dec2cea6e81#db4e) to install OpenCV. I use method 1. Then, [This](https://github.com/AlexeyAB/darknet#how-to-compile-on-windows-using-cmake) is how to compile. Step 5-6 also helps. For MSVC, I use VC16 (2019). 
- To make your PC can use CUDA GPU, follow [this](https://towardsdatascience.com/the-ultimate-tensorflow-gpu-installation-guide-for-2022-and-beyond-27a88f5e6c6e). After that, add `CMAKE_CUDA_COMPILER` environment variable with the value `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.2\bin\nvcc.exe`. For the CUDA version, I recommend to use [this](https://stackoverflow.com/questions/50622525/which-tensorflow-and-cuda-version-combinations-are-compatible) as a guideline.
- Before installing CUDA, read [this](https://stackoverflow.com/a/64704718) to not get `No CUDA toolset found` error. But if you already install CUDA but facing this error, add `CMAKE_CUDA_COMPILER` environment variable. If the error still occurs, reinstall the same version of CUDA without uninstalling the old one.

After building the project, copy all files in `Release` folder to the root folder of this project. Then, follow step 7 of [the second reference](https://medium.com/geekculture/yolov4-darknet-installation-and-usage-on-your-system-windows-linux-8dec2cea6e81#db4e).

To test that our setting is complete, try giving a picture to model and let the model to use the COCO weight downloaded in step 7 to detect the object. Use this command.
```
darknet.exe detect cfg/yolov4.cfg yolov4.weights data/dog.jpg
```
If it shows the picture same as this,

![dog_predict](/blogpicture/dog_predict.png)

it means everything is alright.

Now, we wil include our dataset to the Darknet. Copy all contents in `images` and `labels` from the dataset folder and paste into the `images` folder of Darknet. Both image and label will be in the same folder, `images`. This is the [reason](https://github.com/pjreddie/darknet/issues/1726).

Next, you have to set the configuration file in `cfg` folder. Follow [the first reference](https://github.com/AlexeyAB/darknet#how-to-train-to-detect-your-custom-objects) from step 0 to 3. However, the difference is that you can create `obj.names` and `obj.data` in `data` folder. When reading, if you don't use gaussian YOLO, ignore `[Gaussian Yolo]`. Why `(classes+5)*3`?, search the google with `filters = (classes + 5)*3 yolo`. In case you are lazy, [here](https://stackoverflow.com/questions/60427903/why-is-the-filters-set-as-classes-5-3-in-this-article) is one the the answer.

Finally, follow step 7 and 8 of the first reference to train the model. Check the file path in the command `darknet.exe detector train data/obj.data yolo-obj.cfg yolov4.conv.137` carefully. You can change the file path and file name. For example, I change the command to `darknet.exe detector train data/yolo.data cfg/yolov4_custom_update.cfg supplementary/yolov4.conv.137` because
- My configuration file is `yolov4_custom_update.cfg` locating in `cfg` folder, not `yolo-obj.cfg`.
- My DATA file is `yolo.data`, not `obj.data`.

This note is written in a short amount of time, and I haven't even reread all of my notes. Just think and type. So, I believe there are a lot of grammar mistakes and vocab misuses. I apologize. However, any suggestion is appreciated. I hope this note help!