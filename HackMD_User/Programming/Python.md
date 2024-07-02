---
title: Python
tags: [Programming]

---

###### tags: `Programming`
# Python
## OpenCV
- `cv2.rectangle(path_img, start_point, end_points, color, space)`
### Read, Display and Write an Image 
- `imread(path_file, flags)`
    - flags: 
        - cv2.IMREAD_UNCHANGED or -1 (original format of the image)
        - cv2.IMREAD_GRAYSCALE or 0
        - cv2.IMREAD_COLOR or 1
- `imshow(window_name, image)`
- `imwrite(filename, image)`
    - filenaem: có extension (ex: .jpg)
- `waitKey()`: close image when press key
- `destroyAllWindows()`:  clear any open image window from the memory 
```python=
# import the cv2 library
import cv2
 
# The function cv2.imread() is used to read an image.
img_grayscale = cv2.imread('test.jpg',0)
 
# The function cv2.imshow() is used to display an image in a window.
cv2.imshow('graycsale image',img_grayscale)
 
# waitKey() waits for a key press to close the window and 0 specifies indefinite loop
cv2.waitKey(0)
 
# cv2.destroyAllWindows() simply destroys all the windows we created.
cv2.destroyAllWindows()
 
# The function cv2.imwrite() is used to write an image.
cv2.imwrite('grayscale.jpg',img_grayscale)
```


```python=
# Importing the OpenCV library
import cv2
# Reading the image using imread() function
image = cv2.imread('road.jpg')

# Show Image
cv2.imshow('window', image)

# Extracting the height and width of an image
h, w = image.shape[:2]

# Displaying the height and width
print("Height = {}, Width = {}".format(h, w))

# height - width - channels(rgb), if image is grayscale => return (height, width)
print("image.shape = {}".format(image.shape))

# Extracting RGB values. 
# Here we have randomly chosen a pixel
# by passing in 100, 100 for height and width.
(B, G, R) = image[100, 100]

# Displaying the pixel values
print("R = {}, G = {}, B = {}".format(R, G, B))

Red = image.item(100, 100, 2)
print("Red = {}".format(Red))
```

- `image.shape`:
    - height, width: grayscale 
    - height, width, channels(rgb): image habe color
- Image datatype is obtained by `img.dtype`: uint8
-  `img.dtype` is very important while debugging because a large number of errors in OpenCV-Python code are caused by invalid datatype.

### Image resizing
- Khi thay đổi kích thước hình ảnh:
    - Điều quan trọng cần lưu ý là tỷ lệ khung hình ban đầu của hình ảnh (nghĩa là chiều rộng theo chiều cao), nếu bạn cũng muốn duy trì tỷ lệ này trong hình ảnh đã thay đổi kích thước.
    - Việc giảm kích thước của hình ảnh sẽ yêu cầu lấy mẫu lại các pixel. 
    - Tăng kích thước của một hình ảnh yêu cầu xây dựng lại hình ảnh. Điều này có nghĩa là bạn cần nội suy các pixel mới.

- `image.shape`: size original size (height * width * channels)
- `image.shape[:2]`: height * width
- **Syntax**: `resize(src, dsize[, dst[, fx[, fy[, interpolation]]]])`
    - `src`: It is the required input image, it could be a string with the path of the input image (eg: ‘test_image.png’).
    - `dsize`: It is the desired size of the output image, it can be a new height and width.
	- `fx`: Scale factor along the horizontal axis.
	- `fy`: Scale factor along the vertical axis.
	- `interpolation`: It gives us the option of different methods of resizing the image.

#### Resizing With a Scaling Factor
```python=
# Scaling Up the image 1.2 times by specifying both scaling factors
scale_up_x = 1.2
scale_up_y = 1.2
# Scaling Down the image 0.6 times specifying a single scale factor.
scale_down = 0.6
 
scaled_f_down = cv2.resize(image, None, fx= scale_down, fy= scale_down, interpolation= cv2.INTER_LINEAR)
scaled_f_up = cv2.resize(image, None, fx= scale_up_x, fy= scale_up_y, interpolation= cv2.INTER_LINEAR)
```
#### Resizing by Specifying Width and Height
```python=
# let's start with the Imports 
import cv2
import numpy as np
 
# Read the image using imread function
image = cv2.imread('image.jpg')
cv2.imshow('Original Image', image)
 
# let's downscale the image using new  width and height
down_width = 300
down_height = 200
down_points = (down_width, down_height)
resized_down = cv2.resize(image, down_points, interpolation= cv2.INTER_LINEAR)
 
# let's upscale the image using new  width and height
up_width = 600
up_height = 400
up_points = (up_width, up_height)
resized_up = cv2.resize(image, up_points, interpolation= cv2.INTER_LINEAR)
 
# Display images
cv2.imshow('Resized Down by defining height and width', resized_down)
cv2.waitKey()
cv2.imshow('Resized Up image by defining height and width', resized_up)
cv2.waitKey()
 
#press any key to close the windows
cv2.destroyAllWindows()
```

#### Resizing With Different Interpolation Methods
- `INTER_AREA`: INTER_AREA sử dụng mối quan hệ diện tích pixel để lấy mẫu lại. Điều này phù hợp nhất để giảm kích thước của hình ảnh (thu nhỏ). Khi được sử dụng để phóng to hình ảnh, nó sử dụng INTER_NEARESTphương thức.
- `INTER_CUBIC`: Điều này sử dụng phép nội suy nhị phân để thay đổi kích thước hình ảnh. Trong khi thay đổi kích thước và nội suy các pixel mới, phương pháp này hoạt động trên các pixel lân cận 4×4 của hình ảnh. Sau đó, nó lấy trọng số trung bình của 16 pixel để tạo pixel được nội suy mới.
- `INTER_LINEAR`: Phương pháp này hơi giống với - INTER_CUBIC phép nội suy. Nhưng không giống như - INTER_CUBIC, điều này sử dụng các pixel lân cận 2×2 để lấy giá trị trung bình có trọng số cho pixel được nội suy.
- `INTER_NEAREST`: INTER_NEARESTPhương thức sử dụng khái niệm hàng xóm gần nhất để nội suy. Đây là một trong những phương pháp đơn giản nhất, chỉ sử dụng một pixel lân cận từ hình ảnh để nội suy.

### Cropping an Image using OpenCV
- **Syntax**: `cropped = img[start_row:end_row, start_col:end_col]`

```python=
imgheight, imgwidth, c = img.shape

M = 76
N = 104
x1 = 0
y1 = 0
 
for y in range(0, imgheight, M):
    for x in range(0, imgwidth, N):
        if (imgheight - y) < M or (imgwidth - x) < N:
            break
             
        y1 = y + M
        x1 = x + N
 
        # check whether the patch width or height exceeds the image width or height
        if x1 >= imgwidth and y1 >= imgheight:
            x1 = imgwidth - 1
            y1 = imgheight - 1
            #Crop into patches of size MxN
            tiles = image_copy[y:y+M, x:x+N]
            #Save each patch into file directory
            cv2.imwrite('saved_patches/'+'tile'+str(x)+'_'+str(y)+'.jpg', tiles)
            cv2.rectangle(img, (x, y), (x1, y1), (0, 255, 0), 1)
        elif y1 >= imgheight: # when patch height exceeds the image height
            y1 = imgheight - 1
            #Crop into patches of size MxN
            tiles = image_copy[y:y+M, x:x+N]
            #Save each patch into file directory
            cv2.imwrite('saved_patches/'+'tile'+str(x)+'_'+str(y)+'.jpg', tiles)
            cv2.rectangle(img, (x, y), (x1, y1), (0, 255, 0), 1)
        elif x1 >= imgwidth: # when patch width exceeds the image width
            x1 = imgwidth - 1
            #Crop into patches of size MxN
            tiles = image_copy[y:y+M, x:x+N]
            #Save each patch into file directory
            cv2.imwrite('saved_patches/'+'tile'+str(x)+'_'+str(y)+'.jpg', tiles)
            cv2.rectangle(img, (x, y), (x1, y1), (0, 255, 0), 1)
        else:
            #Crop into patches of size MxN
            tiles = image_copy[y:y+M, x:x+N]
            #Save each patch into file directory
            cv2.imwrite('saved_patches/'+'tile'+str(x)+'_'+str(y)+'.jpg', tiles)
            cv2.rectangle(img, (x, y), (x1, y1), (0, 255, 0), 1)
```
### Image Rotation and Translation Using OpenCV

- `getRotationMatrix2D(center, angle, scale)`: tạo matrix chuyển đổi
    - center: tâm xoay, thường = (width/2, height/2)
    - angle: góc quay tính bằng độ, value dương thì xoay ngược chiều kim đồng hồ, value âm thì cùng chiều kim đồng hồ 
    - scale: phóng to hoặc thu nhỏ
- Xoay cần 3 bước
    - xác định tâm xoay, thường là tâm ảnh
    - xác định matrix xoay
    - `warpAffine()`
- `warpAffine(src, M, dsize[, dst[, flags[, borderMode[, borderValue]]]])`
	- M: the transformation matrix
	- dsize: size of the output image
	- dst: the output image
	- flags: combination of interpolation methods such as INTER_LINEAR or INTER_NEAREST
	- borderMode: the pixel extrapolation method
	- borderValue: the value to be used in case of a constant 	- border, has a default value of 0

```python=
# dividing height and width by 2 to get the center of the image
height, width = image.shape[:2]
# get the center coordinates of the image to create the 2D rotation matrix
center = (width/2, height/2)
 
# using cv2.getRotationMatrix2D() to get the rotation matrix
rotate_matrix = cv2.getRotationMatrix2D(center=center, angle=45, scale=1)
 
# rotate the image using cv2.warpAffine
rotated_image = cv2.warpAffine(src=image, M=rotate_matrix, dsize=(width, height))
```
### Translation of Images

- Dịch một hình ảnh có nghĩa là dịch chuyển nó theo một số pixel xác định, dọc theo trục x và y.
![](https://i.imgur.com/JlyeXNq.png)

- tx > 0: rightn, tx < 0: left
- ty > 0: down, ty < 0: up

```python=
import cv2 
import numpy as np
 
# read the image 
image = cv2.imread('image.jpg')
# get the width and height of the image
height, width = image.shape[:2]
# create the translation matrix using tx and ty, it is a NumPy array 
translation_matrix = np.array([
    [1, 0, tx],
    [0, 1, ty]
], dtype=np.float32)
translated_image = cv2.warpAffine(src=image, M=translation_matrix, dsize=(width, height))

```

### Annotating Images

- `line(image, start_point, end_point, color, thickness)`
- `circle(image, center_coordinates, radius, color, thickness)`
- `rectangle(image, start_point, end_point, color, thickness)`
- `ellipse(image, centerCoordinates, axesLength, angle, startAngle, endAngle, color, thickness)`
- `putText(image, text, org, font, fontScale, color)`: add text

```python=
# make a copy of the original image
imageText = img.copy()
#let's write the text you want to put on the image
text = 'I am a Happy dog!'
#org: Where you want to put the text
org = (50,350)
# write the text on the input image
cv2.putText(imageText, text, org, fontFace = cv2.FONT_HERSHEY_COMPLEX, fontScale = 1.5, color = (250,225,100))))
# display the output image with text over it
cv2.imshow("Image Text",imageText)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### Color spaces

- Lab: cv2.cvtColor(src, cv2.COLOR_BGR2YLAB)
    - L – Lightness ( Intensity ).
	- a – color component ranging from Green to Magenta.
	- b – color component ranging from Blue to Yellow.
- YCrCb: cv2.cvtColor(src, cv2.COLOR_BGR2YCrCb)
    - Y – Luminance or Luma component obtained from RGB after gamma correction.
    - Cr = R – Y ( how far is the red component from Luma ).
    - Cb = B – Y ( how far is the blue component from Luma ).
- HSV: cv2.cvtColor(src, cv2.COLOR_BGR2HSV)
    - H – Hue ( Dominant Wavelength ).
    - S – Saturation ( Purity / shades of the color ).
    - V – Value ( Intensity ).