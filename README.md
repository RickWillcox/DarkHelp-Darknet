# DarkHelp-Darknet
Instructions on how to get Darknet working using DarkHelp Cli

Useful links:

[DarkHelp Server](https://www.ccoderun.ca/darkhelp/api/Server.html)

[DarkHelp ShellScripting](https://www.ccoderun.ca/darkhelp/api/ShellScripting.html)

[DarkHelp Repo - contains instructions](https://github.com/stephanecharette/DarkHelp/)

[DarkHelp Video Tutorial](https://www.youtube.com/watch?v=pJ2iyf_E9PM)


I suggest you follow the instructions in the DarkHelp Repo and DarkHelp help video linked above. Below is very similar but has some adjustsments I had to personally make. 

### Build Darknet

##### Just some issues I had installing and the fix - seems to be common
```
sudo apt-get install build-essential git libopencv-dev`
```
libopencv-dev missing dependancies Run below

```
sudo apt install libopencv-calib3d-dev libopencv-contrib-dev libopencv-features2d-dev libopencv-highgui-dev libopencv-imgcodecs-dev libopencv-objdetect-dev libopencv-shape-dev libopencv-stitching-dev libopencv-superres-dev libopencv-video-dev libopencv-videoio-dev libopencv-videostab-dev libopencv4.2-java libopencv-calib3d4.2 libopencv-contrib4.2 libopencv-features2d4.2 libopencv-highgui4.2 libopencv-imgcodecs4.2 libopencv-videoio4.2 libgdal26 libodbc1
```

```mkdir src
cd src
git clone https://github.com/AlexeyAB/darknet.git
cd darknet
```

Edit MakeFile

```
OPENCV=1
AVX=1
OPENMP=1
LIBSO=1  < Important for this Darkhelp
``` 

Below is if you want to use GPU - Set ARCH to your own GPU, check MakeFile commented section to find yours.
```
GPU=1 

ARCH= -gencode arch=compute_61,code=sm_61 \
      -gencode arch=compute_61,code=compute_61
```
Save MakeFile
in darknet dir
```
make
sudo cp libdarknet.so /usr/local/lib/
sudo cp include/darknet.h /usr/local/include/
sudo ldconfig
```

### Build Darknet Help
```
cd ..` back into src
sudo apt-get install cmake build-essential libtclap-dev libmagic-dev libopencv-dev
git clone https://github.com/stephanecharette/DarkHelp.git
cd DarkHelp
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make
make package
sudo dpkg -i darkhelp*.deb
```

### Usage
```
DarkHelp -j yolo4.cfg yolo4.weights coco.names path_to_images/*.jpg | | sed -e '1,/JSON OUTPUT/ d' > results.json
```

note `path_to_images` can just be a single image path eg: `test.jpg`

Example
```
DarkHelp -j /home/rick/Documents/Programming/lure-face/DarkHelp/src/darknet/cfg/yolov4.cfg /home/rick/Documents/Programming/lure-face/DarkHelp/src/darknet/yolov4.weights /home/rick/Documents/Programming/lure-face/DarkHelp/src/darknet/cfg/coco.names sample_images/*.jpg | sed -e '1,/JSON OUTPUT/ d' > results.json
```
