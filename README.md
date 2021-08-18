The updated problem statement for this assignment can be found at https://github.com/LS-Computer-Vision/image-stitching

# Image Stitching

We will now turn to image stitching and panoramic stitching using CV techniques, and learn some theory in that process.

## Theory

We need to learn how a camera is typically modelled in Computer Vision.
For simplicity, we start with a pinhole camera model. There is a small point through which all the rays pass and form an image on the camera plane. Real life cameras are more complex than that, and have complicated lens systems to focus the rays instead of a pinhole, but a pinhole camera actually works quite well to model even real life cameras. We do need to correct for deviations, but that is besides the point. As you will learn, the way we model a pinhole camera is through a matrix called the Camera Matrix. This is often split into two parts, the intrinsic camera matrix (has data about the focal length, pixel lengths etc), and the extrinsic camera matrix (has data about the position and rotation of the camera in the world)

The camera matrix is simply a matrix which when multiplied with a 3d point gives a 2d point (ie the 2d coordinates of the projection of the 3d point onto the camera image plane). We often represent 3d points with a 4 vector (4d homogenous coordinates), and 2d points with a 3 vector (3d homogenous coordinates), which makes calculations with matrices much simpler.

* http://www.e-cartouche.ch/content_reg/cartouche/graphics/en/html/Transform_learningObject2.html
	* Intro to homogenous coordinates
* https://www.cs.usfca.edu/~cruse/math202s11/homocoords.pdf
	* More detail about homogenous coordinates
* http://www.cs.cmu.edu/~16385/s17/Slides/11.1_Camera_matrix.pdf
	* This is a good starter to learn about the camera matrix mathematics
* https://ksimek.github.io/2012/08/13/introduction/
	* Some more math behind the decomposition of the camera matrix
* https://docs.opencv.org/4.5.2/dc/dbb/tutorial_py_calibration.html
	* Using OpenCV to find the camera matrix of the camera used to click an image (also called camera calibration)

Now we move on to the next part of the theory. What happens when we move our camera position (and orientation), but don't change the camera itself? Simple! Only the extrinsic camera matrix is changed. Therefore, the projection of the 3d point changes simply due to the change in the extrinsic part of the camera matrix, which is tantamount to some matrix multiplications. Thus the transformation of an image from one camera angle to another is simply a matrix multiplication, also called a homography.

* https://en.wikipedia.org/wiki/Homography_(computer_vision)
	* Homography intro
* https://docs.opencv.org/4.5.2/d9/dab/tutorial_homography.html
	* Homography intro with OpenCV code

The way image stitching works is we use some feature detection to find matching parts of the image, and then use that to find a homography which must be the relation between the two camera angles. Once we know that, it is a simple task to reproject the second image onto the first image's camera angle, and then superpose them on top of each other

* https://towardsdatascience.com/image-panorama-stitching-with-opencv-2402bde6b46c
	* Image stitching using OpenCV

## Part 0: Setup

Open up your terminal and execute the following commands:

	pip install virtualenv
	python -m virtualenv venv
	venv/Scripts/activate      # For Windows Users
	source venv/bin/activate   # For OSX/Linux Users
	pip install -r requirements.txt

## Part 1: Image Stitching Basics
Write a script called ```stitcher.py``` that takes takes as input the path to 2 images and outputs an image which is the stitched version of the two input images

	python assets/campus/ # Stitches the two images in the assets/campus directory and creates the output as assets/campus/output.jpg

You can roughly follow these steps

* Perform feature detection using an algorithm of your choice to find keypoints in the two images. One popular algorithm to perform feature detection is ORB. Others can be SIFT, SURF etc.

* Try to find matching keypoints. OpenCV has some methods to do this too, you can use the brute force matcher, or something called the K-nearest neighbors matcher.

* Once the matching keypoints have been found, use an algorithm like RANSAC to estimate a homography which transforms the keypoints of one image to the keypoints of the other. This is a good estimation of the homography between the two camera orientations which were used to click the images

* Use the homography to warp the second image into the perspective of the first.

* Now that you have a version of both images from the perspective of the first's camera angle, paste the second image onto the first. This gives you the resultant larger image which contains parts of both images, from the perspective of the first's camera angle

There are some things to look out for

* To perform the warping successfully, you need to know the dimensions of the final image. The final image will have black portions where neither the first nor the second image has parts, and its dimensions will be larger than both the images.

* Keep in mind that OpenCV's ```(0,0)``` means the top left corner. Sometimes the warped second image may have some part of the image going well into negative coordinates. You may need to translate both images by an appropriate amount such that the final image has no loss of data.

* You need some mathematics to calculate these dimensions and the translation required, so be careful while doing this. OpenCV will not automatically do this for you

## Part 2: ```Seam Removal```

You may have noticed that the final image has a seam. This is because of differences in ambient light levels of the same region in the two images, which leads to a visible seam.

Find methods that help in making the final image as seamless as possible.

Some ideas may be histogram equalization to make light levels equal. Another idea which has been explored, as this instead of pasting the pixels of the second image onto the first's, try to take a weighted average of the two image's pixels in the overlap region, the weight being a function of the distance of the pixel to the corresponding image centre.

This is your chance to explore and research, and most importantly implement!

## Part 3: ```Analysis```

Write down the theory of all the algorithms that you used in ```explanation.pdf```. This should include the feature detection algorithms like ORB, keypoint matching algorithms like KNN, homography finding algorithms like RANSAC etc. Any non trivial algorithm you use, you have to write the theory behind it.

## Submission Instructions

You can add any other test images if you feel like it. It is better if your code works against a large sample of test images!

Your assignment repository (https://github.com/LS-Computer-Vision/opencv-basics-{username}) should have the following contents pushed to it

	repository root
	├── assets
	│   ├── campus
	│   │   ├── Campus Images
	│   │   └── output.jpg
	│   ├── yard
	│   │   ├── Yard Images
	│   │   └── output.jpg
	│   └── Any other test
	│       ├── Test Images
	│       └── output.jpg
	├── .gitignore
	├── README.md
	├── requirements.txt
	├── sticher.py
	└── (Not pushed, ignored by git) venv

## Deadline
The deadline for this assignment is kept at 22 August 11:59 PM