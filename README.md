# RTSP_video_stream

This tutorial contains: <br/> <br/>
RTSP video stream on wifi <br/>
gstreamer in opencv


## İnstall gstreamer dependencies

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install gstreamer1.0-tools
sudo apt-get install gstreamer1.0-plugins-good
sudo apt-get install gstreamer1.0-plugins-bad
sudo apt-get install gstreamer1.0-plugins-ugly
sudo apt-get install libglib2.0-dev
sudo apt-get install libgstreamer1.0-dev
sudo apt-get install libgstreamer-plugins-base1.0-dev
```

## İnstall numpy

```
pip3 install numpy
```


## Compile Opencv with Gstreamer
### remove current opencv
```
pip3 uninstall opencv-python
```

### Clone opencv repo
```
git clone https://github.com/opencv/opencv.git
cd opencv/
git checkout 4.1.0
```

### Build

```
mkdir build
cd build

cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D INSTALL_C_EXAMPLES=OFF \
-D PYTHON_EXECUTABLE=$(which python3) \
-D BUILD_opencv_python2=OFF \
-D CMAKE_INSTALL_PREFIX=$(python3 -c "import sys; print(sys.prefix)") \
-D PYTHON3_EXECUTABLE=$(which python3) \
-D PYTHON3_INCLUDE_DIR=$(python3 -c "from distutils.sysconfig import get_python_inc; print(get_python_inc())") \
-D PYTHON3_PACKAGES_PATH=$(python3 -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())") \
-D WITH_GSTREAMER=ON \
-D BUILD_EXAMPLES=ON ..
```

## Check your log

✔ check if it has Python 3: section and all interpreter and path are right.
(If they aren’t there, check the numpy package) <br/>
✔ check the GStreamer section.
(If they aren’t say YES, go check GStreamer lib package)
if something went wrong just remove build folder and try again.


![check_photo](https://github.com/KARAASLAN-AI/RTSP_video_stream/blob/main/Resim1.png)

## Building

```
sudo make -j$(nproc)
```

## Install package

```
sudo make install
sudo ldconfig
```
for install control (opencv)

```
import cv2
print(cv2.getBuildInformation())
```

![get_build](https://github.com/KARAASLAN-AI/RTSP_video_stream/blob/main/Resim3.png)


## Use

This is my Gstreamer pipeline SEND script line:

`gst-launch-1.0 -v v4l2src ! video/x-raw,width=320,height=240 ! videoconvert ! jpegenc ! rtpjpegpay ! udpsink host=192.168.1.101 port=5200`

This is my Gstreamer pipeline RECEIVER script line:

`gst-launch-1.0 -v udpsrc port=5200 ! application/x-rtp, encoding-name=JPEG,payload=26 ! rtpjpegdepay ! jpegdec ! videoconvert ! autovideosink`


### opencv capture use

`cap = cv2.VideoCapture("udpsrc port=5200 ! application/x-rtp,media=video,payload=26,clock-rate=90000,encoding-name=JPEG,framerate=30/1 ! rtpjpegdepay ! jpegdec ! videoconvert ! appsink",cv2.CAP_GSTREAMER)`

### My project. Video stream with opencv in GCS (for information GCS : https://github.com/KARAASLAN-AI/Basic_T_Ground_Station_Control) 
![GCS_photo](https://github.com/KARAASLAN-AI/RTSP_video_stream/blob/main/ezgif.com-gif-maker.gif)

## Contact

Mahmut KARAASLAN : mkaraaslan719@gmail.com

## Soruce Link

https://github.com/PhysicsX/Gstreamer-on-embedded-devices <br/>
https://galaktyk.medium.com/how-to-build-opencv-with-gstreamer-b11668fa09c <br/>
https://answers.opencv.org/question/202017/how-to-use-gstreamer-pipeline-in-opencv/ <br/>

