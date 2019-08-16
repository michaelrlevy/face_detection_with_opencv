## Using OpenCV for Face Detection

OpenCV is a very popular, free and open source software system used for a large variety of computer vision applications. This article is intended to help you get started in experimenting with OpenCV and as an interesting example the article discusses how to detect faces in images.

<p align="center">
  <img src="https://raw.githubusercontent.com/michaelrlevy/face_detection_with_opencv/master/out-2016.477.1_001_001_0010.jpg" width="400" title="out-2016.477.1_001_001_0010">
</p>

Note that face detection is a much different, and much simpler task than facial recognition. Face detection uses a computer vision algorithm to detect the existence and location of face images within an image file. Facial recognition can be used to identify a person or match face images with each other. 

One could imagine a large collection of scanned archival images, some of which contain photographs of people, and an archival user would be interested in locating and viewing these photographs where they exist in the collection.

OpenCV is highly optimized for certain tasks, including face detection. There are many articles and tutorials on using OpenCV in a Python environment, so that is what we will discuss here.

This article assumes you have Linux installed and available to you and you can log in to the command line terminal. These examples were built with Ubuntu 18.04 (LTS), which uses the "apt" (Advanced Packaging Tool) system for installing software packages.

There are some paths in the article that assume you are running Ubuntu under Windows Subsystem for Linux (WSL), but most operations will be very similar if you are running Ubuntu in some other environment such as a Linux server or Linux desktop or VirtualBox environment.

Ensure you have python3 installed by entering "python3" in the command line (and press Enter). If you see something like this, you have Python 3 installed. The exact version of Python may not be critically important, but earlier or later versions may work somewhat differently. Ubuntu 18.04 comes with Python 3 already installed, invoked with "python3".

```
$python3
Python 3.6.5 (default, Apr  1 2018, 05:46:30)
[GCC 7.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>>
```
Please note that You may have to enter
```
quit()
```
in order to exit the Python environment. An alternative method to exit is pressing Ctl-D (Control-D).
You may be prompted to enter your Linux user's password.

Below are output files indicating that the system has identified faces in the images.

https://raw.githubusercontent.com/michaelrlevy/face_detection_with_opencv/master/1997.A.0245_001_001_0001.jpg


In order to use very recent versions of OpenCV, you will need to download and compile the program "from source," and there are many resources available to help you do that. However, the purpose of this article is to help get started with OpenCV, and it is now rather easy to install with precompiled packages, which makes the installation task much easier and less error-prone.

"sudo" is a Linux command that gives your Linux user a higher level of privileges. Some commands below begin with "sudo"; the system may will prompt you for your user's password before proceeding. With many of the "apt" install scripts you will need to respond "Y" when prompted "Do you want to continue? [Y]

If you have a newly installed Ubuntu system, you should update packages by running:

```
sudo apt update
```

followed by

```
sudo apt-get upgrade
```

This may take some time and may also require you to enter "Y" to allow the update package to proceed, and you may at different times be prompted to allow the update to restart Ubuntu Linux. If you are using this in WSL, this will just restart Linux, without restarting your Windows computer.

```
sudo apt install python3-pip
```

This also may take some time and you may have to enter "Y" to allow the update to proceed.

Enter
```
sudo pip3 install opencv-python
```
Note that I found that using the "pip3 install" mentioned above resulted in a recent version of OpenCV, whereas using "sudo apt-get install python-opencv" resulted in a much earlier version.

At this point you should be able to enter a Python environment by entering
```
python3
```
..and then enter these two lines:

```
import cv2 as cv
print(cv.__version__)
..and you should see something like this:
```
```
$ python3
Python 3.6.8 (default, Jan 14 2019, 11:02:34)
[GCC 8.0.1 20180414 (experimental) [trunk revision 259383]] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2 as cv
>>> print(cv.__version__)
4.1.0
>>>
```
This indicates that you can access OpenCV version 4.1.0, which is a relatively new version of OpenCV.

Now install these two packages:
```
sudo apt-get install python-numpy
sudo apt-get install python3-matplotlib
```

Create a directory and change the working directory to work there:
```Bash
mkdir face_detection
cd face_detection
```

There are two face detection algorithms that have been implemented in OpenCV for some time, the Local Binary Pattern(LBP) classifier. and the Haar Classifier. In this article we will use the Haar Classifier. There are also newer face classifiers that are included in newer versions of OpenCV and other methods that can be added if desired, but the Haar produces good results and is easy to implement.

For the next step you will need to be able to edit a text file. Unfortunately, there are strong warnings in the current WSL system against using a Windows tool to edit files in the WSL environment. It is not terribly difficult to locate the files, but you should not edit them, currently, except through Linux tools. Many experienced Linux users use a tool call "vi" or "vim". An easy too to learn is "nano" which can be invoked simply with
```
nano
```
or else with a file name. Please enter
```
nano face-detect.py 
```
Nano does take some learning. You will find hints and prompts at the bottom of the nano screen. The carat character ^ indicates the Control key, so for example the hint for "Write Out" is "^D" which indicdates Control-d. Exiting through ^X (Control-X) will prompt you to save the file.

Go to your Windows computer to the C: drive and create these directories:
```
C:\face-detect\
C:\face-detect\inputfiles\
C:\face-detect\outputfiles
```
and copy some image files into those directories. You may use the example files in this project--the ones that do not begin with "out-"--as examples, and download them to C:\face-detect\inputfiles\

You should be able to enter the code below into your editor, save the code, and run it through entering

```
python3 face-detect.py
```

If run successfully, the output files will be written with the same file name as the input file but prepended with "out-", and will be placed in the C:\face-detect\outputfiles\ directory you created.

Note that in WSL Linux, the C: drive is represented as /mnt/c/ and each directory from there should be similar to Windows (although space characters must be handled specially), using slash characters instead of backslash characters. So where you see in the code 

```Python
source_filepattern = '/mnt/c/face-detect/inputfiles/*jpg'
```

it is indicating to the system the Windows location "C:\face-detect\input files" and then the pattern collects all files ending with "jpg".

Here is the code. Note that it can also provide eye detection, but I have commented this out for this example.

```Python
# mostly from:
#https://docs.opencv.org/3.4.1/d7/d8b/tutorial_py_face_detection.html

import os
import glob
import numpy as np
import cv2 as cv
face_cascade = cv.CascadeClassifier('/usr/local/lib/python3.6/dist-packages/cv2/data/haarcascade_frontalface_default.xml')
eye_cascade = cv.CascadeClassifier('/usr/local/lib/python3.6/dist-packages/cv2/data/haarcascade_eye.xml')

source_filepattern = '/mnt/c/face-detect/inputfiles/*jpg'
dest_dir = "/mnt/c/face-detect/outputfiles/out-"

def do_each_file( source_filepattern ):
  for imagefile in glob.glob(source_filepattern):
    filename = os.path.basename( imagefile )
    #print(imagefile)
    #print(filename)
    print(filename)
    outputfile = dest_dir + filename
    detect_face(imagefile, outputfile)
  return;

def detect_face( imgpath, outputfile ):
  img = cv.imread(imgpath)
  #print (img)
  gray = cv.cvtColor(img, cv.COLOR_BGR2GRAY)
  faces = face_cascade.detectMultiScale(gray, 1.3, 5)
  for (x,y,w,h) in faces:
      cv.rectangle(img,(x,y),(x+w,y+h),(0,0,255),4)
      roi_gray = gray[y:y+h, x:x+w]
      roi_color = img[y:y+h, x:x+w]
      #eyes = eye_cascade.detectMultiScale(roi_gray)
      #for (ex,ey,ew,eh) in eyes:
      #    cv.rectangle(roi_color,(ex,ey),(ex+ew,ey+eh),(0,255,0),2)
  cv.imwrite( outputfile, img );
  #cv.imshow('img',img)
  #cv.waitKey(0)
  #cv.destroyAllWindows()
  return;

do_each_file( source_filepattern )
```

Below are some example files indicating that OpenCV has detected faces in these images.

<p align="center">
  <img src="https://raw.githubusercontent.com/michaelrlevy/face_detection_with_opencv/master/out-1997.A.0245_001_001_0001.jpg" width="400" title="1997.A.0245_001_001_0001">
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/michaelrlevy/face_detection_with_opencv/master/out-2016.477.1_001_001_0005.jpg" width="400" title="out-2016.477.1_001_001_0005">
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/michaelrlevy/face_detection_with_opencv/master/out-1997.A.0245_001_001_0001.jpg" width="400" title="1997.A.0245_001_001_0001">
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/michaelrlevy/face_detection_with_opencv/master/out-2016.477.1_001_001_0010.jpg" width="400" title="out-2016.477.1_001_001_0010">
</p>







