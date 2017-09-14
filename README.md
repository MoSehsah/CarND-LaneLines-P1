# **Finding Lane Lines on the Road** 

## Writeup

### This is the writeup for the finding lane lines project which is part of Udacity self driving car Nano Degree

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

2. Setting parameters for each one of the use algorithms 
```python
# Parameters
# Blur
kernel_size = 5
# Canny
low_threshold = 50
high_threshold = 150
# Hough
rho = 2 # distance resolution in pixels of the Hough grid
theta = np.pi/180 # angular resolution in radians of the Hough grid
threshold = 15 # minimum number of votes (intersections in Hough grid cell)
min_line_length = 40 #minimum number of pixels making up a line
max_line_gap = 20 # maximum gap in pixels between connectable line segments
```

3. Applying Canny Edge detection on blur grayscale copy of the image 
```python
gray = grayscale(image)
blur_gray = gaussian_blur(gray,kernel_size)
canny_blur_gray = canny(blur_gray,low_threshold,high_threshold)
```

4. Masking the viewport to a region of interest 
```python
imshape = image.shape
vertices = np.array([[(imshape[1]/7,imshape[0]),(imshape[1]/2-20, imshape[1]/3), (imshape[1]/2+20, imshape[1]/3), (imshape[1],imshape[0])]], dtype=np.int32)
masked_canny_blur_gray = region_of_interest(canny_blur_gray,vertices)
```

5. Applying Hough Transform as well as 
```python
hough_masked_canny_blur_gray = hough_lines(masked_canny_blur_gray, rho, theta, threshold, min_line_length, max_line_gap)  
final = weighted_img(hough_masked_canny_blur_gray, image)
```

---
#### Line extrapolation Mechanism

1. using slope caclulation to determine if the line segment is part of the left or right lane line
```python
# define empty points collections for left line and right line
xl = []
yl = []
xr = []
yr = []
    
#loop all lines and add based on slope to either lines points collection
for line in lines:
	for x1, y1, x2, y2 in line:
		if x2==x1:
			continue
		slope = (y2-y1)/(x2-x1)
		# line looks like \
		if slope > 0.5 and slope < 0.9 :
			xl += [x1, x2]
            yl += [y1, y2]
		elif slope < - 0.5 and slope > -0.9 : 
			xr += [x1, x2]
			yr += [y1, y2]
```
2. using numpy.polyfit as a straight line fit algorithm for both lines
```python
# straight line fit for the points
coeffl = np.polyfit(xl, yl, 1)
l = np.poly1d(coeffl)
        
# get points for line 
x_newl = np.linspace(min(xl), (l - img.shape[0]).r, 20).astype(int)
y_newl = l(x_newl).astype(int)
points_newl = list(zip(x_newl, y_newl))
```

---


### 2. Potential shortcomings

* Currently any change in the angle for the camera would need further calibration for the region of interest


### 3. Suggest possible improvements to your pipeline

* Currently with the shadows in the last video affects the edge detection algorithm. - Needs Improvement - 
