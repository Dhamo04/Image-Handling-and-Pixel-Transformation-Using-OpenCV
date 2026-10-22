# Image-Handling-and-Pixel-Transformations-Using-OpenCV

## AIM:
Write a Python program using OpenCV that performs the following tasks:

1. Read and Display an Image.
2. Adjust the brightness of an image.
3. Modify the image contrast.
4. Split and merge the colour channels (B, G, R and H, S, V).

## Software Required:
- Anaconda - Python 3.7
- Jupyter Notebook (for interactive development and execution)

## Algorithm:

### Step 1:
Load an image from your local directory and display it.

### Step 2:
Create a matrix of ones (with data type float64) to adjust brightness.

### Step 3:
Create brighter and darker images by adding and subtracting the matrix from the original image.
Display the original, brighter, and darker images.

### Step 4:
Modify the image contrast by creating higher and lower contrast images using scaling factors of 0.8, 1.1 and 1.2 (without overflow fix).
Display the original, lower contrast, and higher contrast images.

### Step 5:
Split the image (boy.jpg) into B, G, R components and display the channels.
Merge the channels back and display them along with the original image.

### Step 6:
Convert the image to HSV, split it into H, S, V components and display the channels.
Merge the H, S, V channels, convert back to RGB, and display along with the original image.

## Program Developed By:
- **Name:** DHAMODHARAN S
- **Register Number:** 212225230049
- **Slot No:** 26OD2199

## Program:

### Import libraries
```python
import cv2
import numpy as np
import matplotlib.pyplot as plt
```

### 1. Read the image ('Eagle_in_Flight.jpg') using OpenCV imread() as a grayscale image.
```python
img_gray = cv2.imread('Eagle_in_Flight.jpg', cv2.IMREAD_GRAYSCALE)

# Show the image that was read
plt.imshow(img_gray, cmap='gray')
plt.title('Grayscale Image')
plt.axis('off')
plt.show()
```

### 2. Print the image width, height & Channel.
```python
print("Image shape:", img_gray.shape)
h, w = img_gray.shape[:2]
channels = 1 if img_gray.ndim == 2 else img_gray.shape[2]
print("Width :", w)
print("Height:", h)
print("Channels:", channels)
```

### 3. Display the image using matplotlib imshow().
```python
plt.imshow(img_gray, cmap='gray')
plt.title('Grayscale Image')
plt.axis('off')
plt.show()
```

### 4. Save the image as a PNG file using OpenCV imwrite().
```python
cv2.imwrite('Eagle_in_Flight.png', img_gray)
print("Image saved as Eagle_in_Flight.png")
```

### 5. Read the saved image above as a color image using cv2.cvtColor().
```python
saved_img = cv2.imread('Eagle_in_Flight.png')
img_color = cv2.cvtColor(saved_img, cv2.COLOR_BGR2RGB)
```

### 6. Display the Colour image using matplotlib imshow() & Print the image width, height & channel.
```python
plt.imshow(img_color)
plt.title('Colour Image (RGB)')
plt.axis('off')
plt.show()

h, w, c = img_color.shape
print("Width   :", w)
print("Height  :", h)
print("Channels:", c)
```

### 7. Crop the image to extract any specific (Eagle alone) object from the image.
```python
# Read the original colour image and crop the eagle
eagle = cv2.cvtColor(cv2.imread('Eagle_in_Flight.jpg'), cv2.COLOR_BGR2RGB)
cropped = eagle[20:410, 200:545]   # [y1:y2, x1:x2]

plt.imshow(cropped)
plt.title('Cropped Image (Eagle)')
plt.axis('off')
plt.show()
print("Cropped size:", cropped.shape)
```

### 8. Resize the image up by a factor of 2x.
```python
resized = cv2.resize(cropped, None, fx=2, fy=2, interpolation=cv2.INTER_CUBIC)

plt.imshow(resized)
plt.title('Resized Image (2x)')
plt.axis('off')
plt.show()
print("Resized size:", resized.shape)
```

### 9. Flip the cropped/resized image horizontally.
```python
flipped = cv2.flip(resized, 1)   # 1 = horizontal flip

plt.imshow(flipped)
plt.title('Flipped Image (Horizontal)')
plt.axis('off')
plt.show()
```

### 10. Read in the image ('Apollo-11-launch.jpg').
```python
apollo = cv2.imread('Apollo-11-launch.jpg', cv2.IMREAD_COLOR)
apollo_rgb = cv2.cvtColor(apollo, cv2.COLOR_BGR2RGB)

plt.imshow(apollo_rgb)
plt.title('Apollo 11 Launch (Original)')
plt.axis('off')
plt.show()
print("Image size:", apollo.shape)
```

### 11. Add the following text to the dark area at the bottom of the image (centered on the image):
```python
text = 'Apollo 11 Saturn V Launch, July 16, 1969'
font_face = cv2.FONT_HERSHEY_PLAIN
font_scale = 2.3
font_color = (255, 255, 255)   # white
thickness = 2

img_text = apollo_rgb.copy()
img_h, img_w = img_text.shape[:2]

# Measure the text so that it can be centred horizontally
(text_w, text_h), baseline = cv2.getTextSize(text, font_face, font_scale, thickness)
x = (img_w - text_w) // 2
y = img_h - 20          # dark area at the bottom

cv2.putText(img_text, text, (x, y), font_face, font_scale, font_color, thickness, cv2.LINE_AA)
```

### 12. Draw a magenta rectangle that encompasses the launch tower and the rocket.
```python
magenta = (255, 0, 255)
rect_color = magenta

img_rect = img_text.copy()
cv2.rectangle(img_rect, (480, 55), (770, 640), rect_color, thickness=3)
```

### 13. Display the final annotated image.
```python
plt.figure(figsize=(10, 6))
plt.imshow(img_rect)
plt.title('Final Annotated Image')
plt.axis('off')
plt.show()
```

### 14. Read the image ('Boy.jpg').
```python
boy = cv2.imread('boy.jpg', cv2.IMREAD_COLOR)
boy_rgb = cv2.cvtColor(boy, cv2.COLOR_BGR2RGB)

plt.imshow(boy_rgb)
plt.title('Original Image')
plt.axis('off')
plt.show()
```

### 15. Adjust the brightness of the image.
```python
# Create a matrix of ones (with data type float64) and scale it by the brightness value
matrix_ones = np.ones(boy_rgb.shape, dtype='float64') * 50
matrix = matrix_ones.astype('uint8')   # convert to uint8 so it can be used with cv2.add / cv2.subtract
print("Matrix shape:", matrix.shape, " dtype:", matrix.dtype)
```

### 16. Create brighter and darker images.
```python
img_brighter = cv2.add(boy_rgb, matrix)
img_darker = cv2.subtract(boy_rgb, matrix)
```

### 17. Display the images (Original Image, Darker Image, Brighter Image).
```python
plt.figure(figsize=(15, 5))
plt.subplot(1, 3, 1); plt.imshow(boy_rgb);     plt.title('Original Image'); plt.axis('off')
plt.subplot(1, 3, 2); plt.imshow(img_darker);  plt.title('Darker Image');   plt.axis('off')
plt.subplot(1, 3, 3); plt.imshow(img_brighter);plt.title('Brighter Image'); plt.axis('off')
plt.show()
```

### 18. Modify the image contrast.
```python
# Create two higher contrast images using the 'scale' option with factors of 1.1 and 1.2 (without overflow fix)
matrix0 = np.ones(boy_rgb.shape, dtype='float64') * 0.8
matrix1 = np.ones(boy_rgb.shape, dtype='float64') * 1.1
matrix2 = np.ones(boy_rgb.shape, dtype='float64') * 1.2

img_lower   = np.uint8(cv2.multiply(np.float64(boy_rgb), matrix0))
img_higher1 = np.uint8(cv2.multiply(np.float64(boy_rgb), matrix1))
img_higher2 = np.uint8(cv2.multiply(np.float64(boy_rgb), matrix2))
```

### 19. Display the images (Original, Lower Contrast, Higher Contrast).
```python
plt.figure(figsize=(18, 5))
plt.subplot(1, 4, 1); plt.imshow(boy_rgb);     plt.title('Original');                plt.axis('off')
plt.subplot(1, 4, 2); plt.imshow(img_lower);   plt.title('Lower Contrast (0.8)');    plt.axis('off')
plt.subplot(1, 4, 3); plt.imshow(img_higher1); plt.title('Higher Contrast (1.1)');   plt.axis('off')
plt.subplot(1, 4, 4); plt.imshow(img_higher2); plt.title('Higher Contrast (1.2)');   plt.axis('off')
plt.show()
```

### 20. Split the image (boy.jpg) into the B,G,R components & Display the channels.
```python
b, g, r = cv2.split(boy)   # OpenCV reads in B, G, R order

plt.figure(figsize=(15, 5))
plt.subplot(1, 4, 1); plt.imshow(boy_rgb); plt.title('Original'); plt.axis('off')
plt.subplot(1, 4, 2); plt.imshow(b, cmap='gray'); plt.title('Blue Channel');  plt.axis('off')
plt.subplot(1, 4, 3); plt.imshow(g, cmap='gray'); plt.title('Green Channel'); plt.axis('off')
plt.subplot(1, 4, 4); plt.imshow(r, cmap='gray'); plt.title('Red Channel');   plt.axis('off')
plt.show()
```

### 21. Merged the R, G, B, displays along with the original image.
```python
merged_rgb = cv2.merge([r, g, b])   # merge back in R, G, B order for matplotlib

plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1); plt.imshow(boy_rgb);    plt.title('Original Image'); plt.axis('off')
plt.subplot(1, 2, 2); plt.imshow(merged_rgb); plt.title('Merged Image (R, G, B)'); plt.axis('off')
plt.show()
```

### 22. Split the image into the H, S, V components & Display the channels.
```python
img_hsv = cv2.cvtColor(boy, cv2.COLOR_BGR2HSV)
h, s, v = cv2.split(img_hsv)

plt.figure(figsize=(15, 5))
plt.subplot(1, 4, 1); plt.imshow(boy_rgb); plt.title('Original'); plt.axis('off')
plt.subplot(1, 4, 2); plt.imshow(h, cmap='gray'); plt.title('Hue Channel');        plt.axis('off')
plt.subplot(1, 4, 3); plt.imshow(s, cmap='gray'); plt.title('Saturation Channel'); plt.axis('off')
plt.subplot(1, 4, 4); plt.imshow(v, cmap='gray'); plt.title('Value Channel');      plt.axis('off')
plt.show()
```

### 23. Merged the H, S, V, displays along with original image.
```python
merged_hsv = cv2.merge([h, s, v])
merged_hsv_rgb = cv2.cvtColor(merged_hsv, cv2.COLOR_HSV2RGB)

plt.figure(figsize=(10, 5))
plt.subplot(1, 2, 1); plt.imshow(boy_rgb);        plt.title('Original Image'); plt.axis('off')
plt.subplot(1, 2, 2); plt.imshow(merged_hsv_rgb); plt.title('Merged Image (H, S, V)'); plt.axis('off')
plt.show()
```

## Output:

**i) Read and Display an Image.**

1. Read 'Eagle_in_Flight.jpg' as grayscale and display:

<img width="622" height="506" alt="image" src="https://github.com/user-attachments/assets/cdb30934-ee63-432e-b0f9-5b5dfec3aa1e" />


2. Save image as PNG and display:

<img width="962" height="762" alt="image" src="https://github.com/user-attachments/assets/1c634332-9fcb-4359-9bbb-acbc5ad65039" />


3. Cropped image:

<img width="438" height="517" alt="image" src="https://github.com/user-attachments/assets/81861b1e-9009-4b41-8f74-c0451d21866e" />


4. Resize and flip horizontally:

<img width="438" height="507" alt="image" src="https://github.com/user-attachments/assets/811d9b25-309e-464e-88e7-83c5f6eb3225" />


5. Read 'Apollo-11-launch.jpg' and display the final annotated image:

<img width="651" height="390" alt="image" src="https://github.com/user-attachments/assets/c209acd1-a7cd-462c-be70-0bd44688f3b8" />

<img width="842" height="501" alt="image" src="https://github.com/user-attachments/assets/9fb4428e-fbd8-41f2-b628-5a31e7729197" />


**ii) Adjust Image Brightness.**

1. Create brighter and darker images and display:

<img width="865" height="211" alt="image" src="https://github.com/user-attachments/assets/cbfb1187-bb57-487d-b1a1-d952e3921f17" />


**iii) Modify Image Contrast.**

1. Modify contrast using scaling factors 0.8, 1.1 and 1.2:

<img width="857" height="177" alt="image" src="https://github.com/user-attachments/assets/926b70f2-f9cb-498c-93ee-cee414572888" />


**iv) Split and merge the colour channels (B, G, R and H, S, V).**

1. Split 'boy.jpg' into B, G, R components and display:

   <img width="851" height="170" alt="image" src="https://github.com/user-attachments/assets/6296e172-1d9e-4824-8513-b812fd5efdb3" />


2. Merge the R, G, B channels and display:

<img width="855" height="322" alt="image" src="https://github.com/user-attachments/assets/995f5338-342d-4eef-a375-9aaf5fe443af" />


3. Split the image into H, S, V components and display:

<img width="851" height="177" alt="image" src="https://github.com/user-attachments/assets/80eb2c14-f5d0-4207-88ff-c05abb0744aa" />


4. Merge the H, S, V channels and display:

<img width="851" height="323" alt="image" src="https://github.com/user-attachments/assets/3851449d-99aa-4336-a225-e756fdb407db" />


## Result:
Thus, the images were read, displayed, brightness and contrast adjustments were made, and the colour channels were split and merged successfully using the Python program.
