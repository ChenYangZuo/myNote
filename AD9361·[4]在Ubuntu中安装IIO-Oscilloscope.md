# 在Linux中安装IIO-Oscilloscope

## 安装依赖库

```
sudo apt-get -y install libglib2.0-dev libgtk2.0-dev libgtkdatabox-dev libmatio-dev libfftw3-dev libxml2 libxml2-dev bison flex libavahi-common-dev libavahi-client-dev libcurl4-openssl-dev libjansson-dev cmake libaio-dev libserialport-dev
```

## 安装libiio

```
sudo apt-get install libxml2 libxml2-dev bison flex libcdk5-dev cmake
sudo apt-get install libaio-dev libusb-1.0-0-dev libserialport-dev libxml2-dev libavahi-client-dev doxygen graphviz
git clone https://github.com/pcercuei/libini.git
cd libini
mkdir build && cd build && cmake ../ && make && sudo make install
cd ..
git clone https://github.com/analogdevicesinc/libiio.git
mkdir build && cd build && cmake ../ && make && sudo make install
cd ..
```

## 安装libad9361-iio

```
git clone https://github.com/analogdevicesinc/libad9361-iio.git
cd libad9361-iio
cmake ./
make
sudo make install
```

## 安装IIO-Oscilloscope

```
git clone https://github.com/analogdevicesinc/iio-oscilloscope.git
cd iio-oscilloscope
mkdir build && cd build
cmake ../ && make -j20
sudo make install
export LD_LIBRARY_PATH=./
./osc
```