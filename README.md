The updated problem statement for this assignment can be found at https://github.com/LS-Computer-Vision/opencv-basics

# OpenCV Basics

Now we begin with using OpenCV, which is an excellent open source library with all sorts of functionality specially suited for Computer Vision tasks

OpenCV has bindings in many languages, including C++ and Python. We will be using the Python bindings, since we will deal exclusively with Python

## Resources to get you started

* https://towardsdatascience.com/opencv-complete-beginners-guide-to-master-the-basics-of-computer-vision-with-code-4a1cd0c687f9
* https://docs.opencv.org/4.5.2/
	* This is the OpenCV documentation, any time you have a doubt about how to call certain functions or which function does what, this is the site to help you out
* https://docs.opencv.org/4.5.2/d6/d00/tutorial_py_root.html
	* Set of official OpenCV tutorials

* https://www.pyimagesearch.com/2018/07/19/opencv-tutorial-a-guide-to-learn-opencv/

## Part 0: Setup

Open up your terminal and execute the following commands:

	pip install virtualenv
	python -m virtualenv venv
	venv/Scripts/activate      # For Windows Users
	source venv/bin/activate   # For OSX/Linux Users
	pip install -r requirements.txt

## Part 1: ```Image Editor```
We will attempt to build a very simple image editor with OpenCV.
The application needs to have the following features

* The application should run indefinitely until closed. This means that you should have an infinite loop (or a loop that detects closing condition) of some sorts, where in each iteration the updated image is displayed
	* That's essentially how we display videos using OpenCV, we display each frame as an image, every loop iteration

* 
		python editor.py image.jpg 1280 720

	Should launch an OpenCV window with the image, with the dimensions 1280 x 720. The width and height parameters can be kept optional (with some default values if not stated)
	* Use the ```argparse``` module to make your life easier and parse command line arguments easily
* If the image file does not exist, launch a window with the image completely white

* The user can edit the image by clicking on it to place points. The points placed will have the color red, green, or blue, depending on what the ```current color selection ```is

* The ``current color selection`` can be changed to 
	* red by pressing the ```R``` key
	* green by pressing the ```G``` key
	* blue by pressing the ```B``` key

* Print the ```current color selection``` value to the console every time it is changed

* Pressing ```Q``` should save the image to the file and exit

## Part 2: ```Video Chat Application```

We will now harness OpenCV's power to make the frontend of a video chat application.

* https://www.geeksforgeeks.org/python-opencv-capture-video-from-camera/
	* This will help in understanding how video from webcam (and video in general) can be captured and displayed in OpenCV

* 
		python video.py video.mp4 1280 720
		
	Should launch an OpenCV window with the video playing (loop it around once the video ends), with the dimensions 1280x720. Again, the dimensions can be kept optional arguments

* Display your webcam feed in a small rectangle on the upper left corner on top of the already playing video
	* If your webcam is not working for some reason, you can use a default video feed

* Your webcam feed should have a red border around it
* There should be the following modes to display the webcam feed
	* RGB mode (default), select by pressing ```1```
	* Grayscale mode, select by pressing ```2```
	* Blurred RGB mode, select by pressing ```3```

* Draw a small blue cross at the center of the screen

* Pressing ```Q``` should quit the application

## Submission Instructions

Your assignment repository (https://github.com/LS-Computer-Vision/opencv-basics-{username}) should have the following contents pushed to it

	repository root
	├── assets
	│   ├── videos
	│   │   └── Videos you need
	│   └── images
	│       └── Images you need
	├── .gitignore
	├── README.md
	├── requirements.txt
	├── video.py
	├── editor.py
	└── (Not pushed, ignored by git) venv

## Deadline
The deadline for this assignment is kept at 18 July 11:59 PM