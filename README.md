# Auto-Crop-And-Limb-Remove-Image-Processing
Final Project of Image Processing Course

# Objective

Develop a program that reads a monkey X-ray image and performs image processing to:

Crop the image so that only the torso region of the monkey remains.

Remove the ribs located in the torso area while preserving the spine and other anatomical structures as much as possible.

Save the processed image in JPG format.

The program may apply appropriate image enhancement or preprocessing techniques before rib removal if necessary.

# Requirements

The program should be designed to work with as many monkey X-ray images as possible without modifying the core algorithm.

Users are allowed to adjust parameter values (e.g., thresholds, kernel sizes, morphological parameters) to suit different images.

After processing, the output image must be saved as a .jpg file.

# Implementation Details

This project is implemented using Python and OpenCV for image processing. The program is designed to automatically crop the monkey X-ray image to the torso region and reduce rib structures while preserving major anatomical components.

I implemented this project on Google Colab
This is my project link
https://colab.research.google.com/drive/1bI9EKPolK6TqVAlsu6lRC42Cl-Czleu-?usp=sharing

# Libraries Used

OpenCV (cv2) – image processing operations

NumPy – numerical operations

Matplotlib – visualization (optional/debugging)

OS – file and directory handling

# Algorithm Overview
1. Image Loading

All images are loaded from a specified folder.
Each image is:

Read in BGR format

Converted to grayscale for further processing

gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)


Only valid image files (.png, .jpg, .jpeg) are processed.

2. Binary Thresholding

To separate the monkey body from the background, a fixed binary threshold is applied:

_, thresh = cv2.threshold(gray, 120, 255, cv2.THRESH_BINARY)


This step highlights high-intensity regions (bones and body) in the X-ray image.

3. Contour Detection and Torso Cropping

All contours are detected from the binary image.

The largest contour is assumed to represent the monkey’s body.

A bounding box is calculated from this contour and used to crop the image.

cnt = max(contours, key=cv2.contourArea)
x, y, w, h = cv2.boundingRect(cnt)
cropped = img[y:y+h, x:x+w]


This step automatically removes background regions and keeps only the torso area.

4. Rib Reduction Using Denoising

To reduce rib visibility while preserving the spine and other structures, Non-Local Means Denoising is applied:

remove_ribs = cv2.fastNlMeansDenoising(cropped, None, 50, 7, 21)


This method smooths repetitive rib patterns while retaining important edges.

Parameter values can be adjusted by the user to suit different X-ray images.

5. Output Saving

The final processed image is saved in JPG format:

cv2.imwrite("final_" + output_name, remove_ribs)


Each input image generates one output file.

# Key Features

Automatic torso cropping using contour detection

Parameter-adjustable rib reduction

Works with multiple images without changing the algorithm

Simple and reusable image processing pipeline

# Notes

The algorithm is designed to be image-independent, but parameter tuning (e.g., threshold value, denoising strength) may be required for different X-ray images.

Intermediate image-saving steps are included in the code (commented out) for debugging and visualization purposes.
