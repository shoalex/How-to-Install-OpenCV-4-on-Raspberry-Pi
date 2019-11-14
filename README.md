# How-to-Install-OpenCV-on-Raspberry-Pi
Explain how to install OpenCV on Raspberry Pi

# Features
- Using Raspbian not Noobs version
- Using any Raspberry Pi
- Using OpenCV
- Using numpy

# 1.Install Raspbian

First we need to make SD card look at this video

<a href="https://www.youtube.com/watch?v=LGAMRMrCeeA&t=327s">Clivk Here</a>

Recomended to open all interfaces for VNC that you can remoute control install

# 2.Expend file system

Open the Terminal using Ctrl+Alt+T
Run the command
```
sudo raspi-config
```
And then select the “7 Advanced Options”
Followed by selecting “A1 Expand filesystem”
Press Finish and Reboot
if its not reboot
Run the command
```
sudo reboot
```

# 3.Free memory
Run the commands
```
sudo apt-get purge wolfram-engine -y
sudo apt-get purge libreoffice* -y
sudo apt-get clean -y
sudo apt-get autoremove -y
```
# 4.Install dependencies

Make sure you dont have any errors on the way
Run the commands
```
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install build-essential cmake pkg-config -y
sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng-dev -y
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev -y
sudo apt-get install libxvidcore-dev libx264-dev -y
sudo apt-get install libfontconfig1-dev libcairo2-dev -y
sudo apt-get install libgdk-pixbuf2.0-dev libpango1.0-dev -y
sudo apt-get install libgtk2.0-dev libgtk-3-dev -y
sudo apt-get install libatlas-base-dev gfortran -y
sudo apt-get install libhdf5-dev libhdf5-serial-dev libhdf5-103 -y
sudo apt-get install libqtgui4 libqtwebkit4 libqt4-test python3-pyqt5 -y
sudo apt-get install python3-dev -y
```

# 5.Install virtual environment
We make virtual environment in case of problems don't hurt the RPI
and in case of problems delete only the virtual environment and renew it.

Run the commands
```
wget https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
sudo python3 get-pip.py
sudo rm -rf ~/.cache/pip
sudo pip install virtualenv virtualenvwrapper
nano ~/.bashrc
```
Add to he end of the file the next lines:
```
# virtualenv and virtualenvwrapper
export WORKON_HOME=$HOME/.virtualenvs
export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
source /usr/local/bin/virtualenvwrapper.sh
```

to save and exit press
cntrl+x
y
enter

Would recommend logging into the file again to see that it is saved

Run the commands
```
nano ~/.bashrc
```

To exit press
cntrl+x

When the file is ready
Run the command
```
source ~/.bashrc
```

# 6.Create virtual environment
Run the command
```
mkvirtualenv cv -p python3
```
If you have a Raspberry Pi Camera Module attached to your RPi, you should install
```
pip install "picamera[array]"
pip install matplotlib
pip install scikit-image
pip install scipy
pip install RPi.GPIO
```

# 7.Compile OpenCV
Run the command
```
cd ~
wget -O opencv.zip https://github.com/opencv/opencv/archive/4.1.1.zip
wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.1.1.zip
unzip opencv.zip
unzip opencv_contrib.zip
mv opencv-4.1.1 opencv
mv opencv_contrib-4.1.1 opencv_contrib
sudo nano /etc/dphys-swapfile
```

Change the veriable CONF_SWAPSIZE=100 to CONF_SWAPSIZE=1024
to save and exit press
cntrl+x
y
enter

Run the command
```
sudo /etc/init.d/dphys-swapfile stop
sudo /etc/init.d/dphys-swapfile start
```
## 7.1.Install NumPy
Run the command
```
pip install numpy
cd ~/opencv
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
  -D CMAKE_INSTALL_PREFIX=/usr/local \
  -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
  -D ENABLE_NEON=ON \
  -D ENABLE_VFPV3=ON \
  -D BUILD_TESTS=OFF \
  -D INSTALL_PYTHON_EXAMPLES=OFF \
  -D OPENCV_ENABLE_NONFREE=ON \
  -D CMAKE_SHARED_LINKER_FLAGS=-latomic \
  -D BUILD_EXAMPLES=OFF ..
```

## 7.2.ompile OpenCV and NumPy
Run the command to check number of cpu's
```
lscpu
```

Aftr you check the number
if you have 1 cpu
run the command
```
make
```

else run command
```
make -jx
x-number of cpu's
```

This may take about 1-5 hours depends on number of cpu's
make sure you dont have errors and if you have try again the command

After it finish
Run the command
```
sudo make install
sudo ldconfig
```

## 7.3.Reset Swap
run the command
```
sudo nano /etc/dphys-swapfile
```

Change back the veriable CONF_SWAPSIZE=1024 to CONF_SWAPSIZE=100
to save and exit press
cntrl+x
y
enter

last installation
run the command
```
cd /usr/local/lib/python3.7/site-packages/cv2/python-3.7
sudo mv cv2.cpython-37m-arm-linux-gnueabihf.so cv2.so
cd ~/.virtualenvs/cv/lib/python3.7/site-packages/
ln -s /usr/local/lib/python3.7/site-packages/cv2/python-3.7/cv2.so cv2.so
```

# 8.Testing
run the command
```
python
import cv2
cv2.__version__
```

Check the answer you get
```
'4.1.1'
```

# 9.Optional
defult open to virtual machine
```
deactivate
sudo nano ~/.bashrc
```

add to the end of the file
```
workon cv
```
to save and exit press
cntrl+x
y
enter

# Author

Alex Shoyhit

Contact Information:

Linkedin:<a href="https://www.linkedin.com/in/alexshoyhit/"> Alex Shoyhit</a>
