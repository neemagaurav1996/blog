---
layout: post
title:  Image Processing with OpenCV-Python
date:   2021-06-26 02:00:00 +0530
categories: [Blogging, Article]
permalink: image-processing-opencv-python
tags: image-processing opencv python python3 opencv-python computer-vision
image: images/article-images/image-processing-opencv-python.jpg
image_alt: image-processing-opencv-python
comments: true
---

## Image Processing with OpenCV-Python
<div class="article-info muted-text">
    <span class="published-on">Published on June 26, 2021</span>
    <span class="duration"><i class="icon-clock"></i> 5 min read</span>
</div>

Common Operations on Images

<figure>
	<img class="article-image" src="{{ page.image }}" alt="{{ page.image_alt }}" width="200">
	<figcaption class="article-image-caption">Image by <a href="https://pixabay.com/users/activedia-665768/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=4846063">Okan Caliskan</a> from <a href="https://pixabay.com/?utm_source=link-attribution&amp;utm_medium=referral&amp;utm_campaign=image&amp;utm_content=4846063">Pixabay</a></figcaption>
</figure>


Image processing is a fascinating thing. After spending some time on this, I realized that all the image editing you do with the help of 3rd party tools can be done with OpenCV and much more. Many special effects applied by your camera app can be easily done with the help of OpenCV, in very few lines of code.

This is a small attempt made by me to describe the most common operations that are performed using OpenCV.

<!--more-->

### First a bit about OpenCV…

CV in the OpenCV stands for “Computer Vision” and open in the sense that it is “Open source”. So OpenCV is Open source Computer Vision. OpenCV is written in C++ but worries not, it has got its wrappers in Python, MATLAB, Java, etc.

### Installation

Installation in Linux is very easy. If you just want to install OpenCV for python (which will suffice in our case), this is the command:

```python
pip3 install opencv-python
```

And that’s it!! You can start playing with your images now…

### Reading/Writing the Images

Let’s quickly go through reading and writing images through OpenCV:

```python
import cv2
#Reading
input_image = cv2.imread("/path/to/input/image")
#perform some operations on image. Suppose you get 'converted_image'
#Writing
cv2.imwrite("/path/to/output/image",converted_image)
```

### Some Common Operations

- **Converting to Grayscale**
	
	Grayscale images are very useful when you are performing text extraction or OCR of image documents. OCR engines perform very well on grayscale images rather than colored images.

```python
gray_image = cv2.cvtColor(input_image, cv2.COLOR_BGR2GRAY)
```

Here is what the input and output images look like:

<figure>
	<img class="article-image-2" src="images/article-images/grayscale_before.png" alt="Before Grayscale">
	<figcaption class="article-image-caption">Before Grayscale</figcaption>
</figure>
<figure>
	<img class="article-image-2" src="images/article-images/grayscale_after.png" alt="After Grayscale">
	<figcaption class="article-image-caption">After Grayscale</figcaption>
</figure>

A point note here is that colored images (BGR) in OpenCV are represented as a 3-dimensional array (n x m x 3) in memory. The grayscale images are represented as a 2-dimensional array (n x m). 
You can access a pixel value by its row and column coordinates. For the BGR image, it returns an array of Blue, Green, Red values. For grayscale images, just corresponding intensity is returned.

```python
#shape is used to get the dimensions of the image
print (input_image.shape)
print (gray_image.shape)
#This will return [Blue, Green, Red] values of pixel at 50th row and 80th column
bgr_px = input_image[50,80]
```

- **Thresholding**

  In thresholding, every pixel of the image is assigned a value on the basis of the threshold. If the pixel value is smaller than the threshold, it is set to 0, otherwise, it is set to a maximum value (generally 255).
  Thresholding is generally done on grayscale images to convert them to binary images (containing only 0 and 255-pixel values.)

```python
ret, thresh1 = cv2.threshold(gray, 0, 255, cv2.THRESH_OTSU)
cv2.imwrite("thresh.png",thresh1)
```

<figure>
	<img class="article-image-2" src="images/article-images/thresholding_before.png" alt="Before Thresholding">
	<figcaption class="article-image-caption">Before Thresholding</figcaption>
</figure>
<figure>
	<img class="article-image-2" src="images/article-images/thresholding_after.png" alt="After Thresholding">
	<figcaption class="article-image-caption">After Thresholding</figcaption>
</figure>

Thresholding is used to separate the light and dark parts of the images. It is generally used before OCRing the images and also used to deblur the images.

- **Inversion**

  Inversion simply means inverting the pixel values. Inversion is generally done on binary images and after inversion, white pixels will be converted to black, and black pixels will be converted to white.
  We can invert an image using `bitwise_not` function of OpenCV: 
  `image_inv = cv2.bitwise_not(input_image)`

<figure>
	<img class="article-image-2" src="images/article-images/inversion_before.png" alt="Before Inversion">
	<figcaption class="article-image-caption">Before Inversion</figcaption>
</figure>
<figure>
	<img class="article-image-2" src="images/article-images/inversion_after.png" alt="After Inversion">
	<figcaption class="article-image-caption">After Inversion</figcaption>
</figure>

- **Blurring or Smoothing** 
	
	You might be wondering what is the need for blurring or smoothing a sharp image. While blur is undesirable in the images that you capture through your camera, but it’s quite useful in Image processing. 
	- Simply put, blurring an image reduces the noise in the image. 
	- Smoothing or blurring is one of the most common preprocessing steps in computer vision and image processing.
	- By smoothing an image prior to applying techniques such as edge detection or thresholding we are able to reduce the amount of high-frequency content, such as noise and edges (i.e., the “detail” of an image).
	- Here is the syntax to perform **Average Bluring**:

	  `blurImg = cv2.blur(input_image,(10,10))` 

	  It simply takes the average of all the pixels under the kernel area and replaces the central element. Here we have defined the kernel size as (10, 10). You can try different values here and test. 
	- You can explore more [image blurring techniques.](https://medium.com/r?url=https%3A%2F%2Fdocs.opencv.org%2Fmaster%2Fd4%2Fd13%2Ftutorial_py_filtering.html)

- **Morphological Operations**

  Morphological operations are a set of operations that process images based on shapes. They apply a structuring element or **kernel** to an input image and generate an output image. The two most common morphological operations are Erosion and Dilation.

- **Erosion**
  - Erosion  fades away the boundaries of the foreground object. 
  - The kernel slides through the image (as in 2D convolution). A pixel in the original image (either 1 or 0) will be considered 1 only if all the pixels under the kernel are 1, otherwise, it is eroded (made to zero).
  - It is used to diminish the features of an image.
  - It is useful for removing small white noises.

```python
import cv2
import numpy as np

img = cv2.imread('input_img.png')
# Matrix of size 5 as the kernel
kernel = np.ones((5,5), np.uint8) 
img_erosion = cv2.erode(img, kernel, iterations=1)
cv2.imwrite("eroded.png",img_erosion)
```

<figure>
	<img class="article-image-2" src="images/article-images/erosion_before.png" alt="Before Erosion">
	<figcaption class="article-image-caption">Before Erosion</figcaption>
</figure>
<figure>
	<img class="article-image-2" src="images/article-images/erosion_after.png" alt="After Erosion">
	<figcaption class="article-image-caption">After Erosion</figcaption>
</figure>

- **Dilation**

  - It is the opposite of Erosion
  - Here, a pixel element is '1' if atleast one pixel under the kernel is '1'.
  - So it increases the white region in the image or size of foreground object increases.
  - In cases like noise removal, erosion is followed by dilation. Because, erosion removes white noises, but it also shrinks our object. So we dilate it. Since noise is gone, they won’t come back, but our object area increases.
  - It is also useful in joining broken parts of an object.

```python
import cv2
import numpy as np

img = cv2.imread('input_img.png')
# Matrix of size 5 as the kernel
kernel = np.ones((5,5), np.uint8) 
img_dilation = cv2.dilate(img, kernel, iterations=1)
cv2.imwrite("dilated.png",img_dilation)
```

<figure>
	<img class="article-image-2" src="images/article-images/dilation_before.png" alt="Before Dilation">
	<figcaption class="article-image-caption">Before Dilation</figcaption>
</figure>
<figure>
	<img class="article-image-2" src="images/article-images/dilation_after.png" alt="After Dilation">
	<figcaption class="article-image-caption">After Dilation</figcaption>
</figure>


These were some basic operations on images using OpenCV. Hope you find them useful!