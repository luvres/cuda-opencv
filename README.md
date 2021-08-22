## YOLO Environment
### CUDA 10.2 (cudnn7)
### OpenCV 4.5.3 with cuda suport
### Anaconda 2020.11 (Python 3.8.5)
### Mambaforge 4.10.3-4 (Python 3.9.6)
-----
### Run

#### Darknet - AlexeyAB
```
git clone https://github.com/AlexeyAB/darknet.git

cd darknet

wget -c https://pjreddie.com/media/files/yolov3.weights
```
```
docker run --rm --runtime=nvidia --name OpenCV \
--net=host \
-e DISPLAY=$DISPLAY \
-v /tmp/.X11-unix \
-v $HOME/.Xauthority:/root/.Xauthority \
--device /dev/video0 \
--mount type=bind,src=$PWD,dst=/root/notebooks \
--workdir=/root/notebooks \
-ti izone/yolo:cuda-opencv-conda bash
```
```
mkdir build-release && cd build-release

cmake ..

make

cp darknet .. && cd ..
```
```
./darknet detector test cfg/coco.data cfg/yolov3.cfg yolov3.weights data/dog.jpg

./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights -ext_output videofile.mp4
./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights video.mp4 -out_filename video.avi

./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights -c 0
./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights -c 0 -out_filename camera.avi
```

#### Darkflow
```
git clone https://github.com/thtrieu/darkflow.git

cd darkflow

mkdir bin

wget -c https://pjreddie.com/media/files/yolov2.weights -O ./bin/yolov2.weights

[ download a videofile.mp4 that you want to process ]
```
```
docker run --rm --runtime=nvidia --name OpenCV \
--net=host \
--env=DISPLAY=$DISPLAY \
--volume=/tmp/.X11-unix \
--volume=$HOME/.Xauthority:/root/.Xauthority \
--device /dev/video0 \
--mount type=bind,src=$PWD,dst=/root/notebooks \
--workdir=/root/notebooks \
-ti izone/yolo:cuda-opencv-conda \
jupyter notebook \
	--allow-root \
	--no-browser \
	--ip=0.0.0.0 \
	--port=8888 \
	--notebook-dir=/root/notebooks \
	--NotebookApp.token=''
```
```
http://localhost:8888/
```
```
-> In the noteboook cells

!pip install tensorflow-gpu==1.15

!python setup.py build_ext --inplace

!python flow --model cfg/yolo.cfg --load bin/yolov2.weights --demo videofile.mp4 --gpu 1.0 --saveVideo

!python flow --model cfg/yolo.cfg --load bin/yolov2.weights --demo camera --saveVideo
```

-----
### Build
```
docker build -t izone/yolo:cuda-opencv-conda .
```
```
docker build -t izone/yolo:cuda10.2-conda2020.02-ocv4.5.3 -f ./Dockerfile.cu102ocv453py37 .
```
```
docker build -t izone/yolo:cuda10.2-conda2020.02-ocv4.4.0 -f ./Dockerfile.cu102ocv440py37 .
```
```
docker build -t izone/yolo:cuda10.2-conda2020.02-ocv4.3.0 -f ./Dockerfile.cu102ocv430py37 .
```

