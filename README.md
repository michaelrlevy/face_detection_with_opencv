## Using OpenCV for Face Detection

OpenCV is a very popular, free and open source software system used for a large variety of computer vision applications. This article is intended to help you get started in experimenting with OpenCV and as an interesting example the article discusses how to detect faces in images.

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




