---
layout: post
title: Image Manipulation
image: "/posts/camaro.jpg"
tags: [Python, Image]
---


In this post, I will demonstrate how to manipulate images using Python.

--- 

First, we need to import the necessary libraries: NumPy, scikit-image, and Matplotlib. 


```ruby
import numpy as np
from skimage import io
import matplotlib.pyplot as plt
```


The following code allows us to read an image and import it as a NumPy array.

```ruby
camaro = io.imread("camaro.jpg")
print(camaro)
```


You can use the 'shape' attribute to examine the image's structure. 

```ruby
camaro.shape
>>> (1200, 1600, 3)
```

The output (1200, 1600, 3) means 1200 rows of pixels, 1600 columns of pixels, and a 3-dimensional array. It is a 3D array because it is a colour image with an intensity value, which is an integer between 0 and 255 for each of the three colour channels. 

Now, let's take a look at the image.

```ruby
plt.imshow(camaro)
plt.show()
```

--- 

Let's try cropping the image. We only want to crop or slice images along the first two axes, rather than the third, which corresponds to the colour channels.    


The first axis is the image's y-axis; slicing it crops vertically.  

```ruby
cropped = camaro[0:500, :, :]
plt.imshow(cropped)
plt.show()
```

The second axis is the image's x-axis; slicing it crops horizontally. 

```ruby
cropped = camaro[:, 400:1000, :]
plt.imshow(cropped)
plt.show()
```

To crop out just the vehicle, here's what we can do.

```ruby
cropped = camaro[350:1100, 200:1400, :]
plt.imshow(cropped)
plt.show()
```

![camaro_cropped](https://github.com/user-attachments/assets/d23650ec-fc81-4e3c-9d74-a93f749c09d7)

This looks good! To save the image to my disk, we can use another function from scikit-image: io.imsave.

```ruby
io.imsave("camaro_cropped.jpg", cropped)
```

--- 

Now, let's try doing something a bit different, flipping images. When slicing an array, we can use the start-stop-step logic to specify exactly what we want to slice out. If we use -1 for the step, we will reverse the image.

1) To flip our image vertically: Here, we reverse the rows and keep the columns and colour channels untouched.

```ruby
vertical_flip = camaro[::-1, :, :]
plt.imshow(vertical_flip)
plt.show()

io.imsave("camaro_vertical_flip.jpg", vertical_flip)
``` 

![camaro_vertical_flip](https://github.com/user-attachments/assets/d6c9f681-54f8-406a-91ca-34c4be5f8394)

2) To flip our image horizontally: Here, we reverse the columns and keep the rows and colour channels untouched.

```ruby
horizontal_flip = camaro[:, ::-1, :]
plt.imshow(horizontal_flip)
plt.show()

io.imsave("camaro_horizontal_flip.jpg", horizontal_flip)
```

![camaro_horizontal_flip](https://github.com/user-attachments/assets/0ef1898c-e4f4-4183-ab6c-1aa2a99513b5)

---

We can also manipulate colour channels. For example, we can extract a red version, a blue verion, and a green version of our image by setting the other colour channels to zero. Here, we do not want to crop out the other channels; we want to make them zero instead.

The easiest way to do this is to create an array of zeros that is the same size as our image, and then fill only the red colour channel, for example, with the red values from the actual image.
We can use numpy.zeros to create an array of zeros, and we want it to be the same dimensions as our original image. So, instead of manually inputting these values, we can use the 'shape' attribute, which provides the precise values. We also need to ensure that the array's data type is equal to uint8, an unsigned 8-bit integer, which is the data type we often use when working with images in NumPy.

1) Red

To create a red version of our image, we first create a NumPy array of the same size and dimensions as the original image, filled with zeros. Then, we fill in the rows and columns with the values for the red channel only, leaving the other two colour channels as zeros. 

```ruby
red = np.zeros(camaro.shape, dtype = "uint8")
red[:,:,0] = camaro[:,:,0]
plt.imshow(red)
plt.show()
```

2) Green
```ruby
green = np.zeros(camaro.shape, dtype = "uint8")
green[:,:,1] = camaro[:,:,1]
plt.imshow(green)
plt.show()
```

3) Blue
```ruby
blue = np.zeros(camaro.shape, dtype = "uint8")
blue[:,:,2] = camaro[:,:,2]
plt.imshow(blue)
plt.show()
```


Cool! Let us use NumPy's stacking functionality to vertically stack these three images (red, green, blue) on top of each other and save the resulting image.

```ruby
camaro_rainbow = np.vstack((red,green,blue))
plt.imshow(camaro_rainbow)
plt.show()

io.imsave("camaro_camaro_rainbow.jpg", camaro_rainbow)
```

![camaro_camaro_rainbow](https://github.com/user-attachments/assets/116000d8-6f7e-4981-a37a-96fa4c2ad52c)
