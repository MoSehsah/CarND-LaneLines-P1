# **Finding Lane Lines on the Road** 

## Writeup

### This is the writeup for the finding lane lines project which is part of Udacity self driving car Nano Degree

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Pipeline Description

1. Doing some basic OS operations
```python
output_dir = "test_images_output/"
input_dir = "test_images/"
import shutil
if(os.path.exists(output_dir)):
    shutil.rmtree(output_dir)
shutil.copytree(input_dir,output_dir)
```
2. Applying Canny Edge detection on blur grayscale copy of the image 
```python
gray = grayscale(image)
blur_gray = gaussian_blur(gray,kernel_size)
canny_blur_gray = canny(blur_gray,low_threshold,high_threshold)
```
 3. List item

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...

