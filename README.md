# Biomedical-Image-Analysis
Fundamentals of image analysis using NumPy, SciPy, and Matplotlib. We'll navigate through a whole-body CT scan, segment a cardiac MRI time series, and determine whether Alzheimer’s disease changes brain structure.

### Loading images : ImageIO package
- One useful feature of ImageIO is that it can read DICOM files, the standard format for human medical image.
- The data is read in as an image object, which is a type of numpy array.

```python
import imageio

im = imageio.imread('body-001.dcm')
```

#### Metadata
- Details of image acquisition `im.meta`

#### Plotting images : matplotlib

```python
import matplotlib.pyplot as plt
plt.imshow(im, cmap='gray')
plt.axis('off')
plt.show()
```

### N-dimensional images are stacks of arrays
- We can feed a list of 3 images into numpy's stack() function to create a 3D volume.

```python
import imageio
import numpy as np

im1 = imageio.imread('chest-000.dcm')
im2 = imageio.imread('chest-001.dcm')
im3 = imageio.imread('chest-002.dcm')

vol = np.stack([im1, im2, im3])

vol.shape
```

#### loading volumes directly
- `imageio.volread()` : ImageIO's volread() function is capable of reading volumes directly from disk, whether the images are stored in their folder or if the dataset is already multi-dimensional. Can read multi-dimesional data directly.
- "chest-data" contains 50 slices of a 3D volume. We simply have to pass the folder name to `volread()`, and it will assemble the volume for us.
- Since these are DICOM images, the function actually checks the available metadata to make sure that the images are placed in the correct order, otherwise it will default to alphabetical order.

```python
import imageio

vol = imageio.volread('chest-data')
vol.shape
```

#### Sampling rate : physical space covered by each element
- The amount of space covered by each element is the sampling rate, and it can vary along each dimension. For DICOM images, the sampling rate is usually encoded in the metadata.

```python
# image shape
n0, n1 ,n2 = vol.shape

# sampling rate
d0, d1, d2 = vol.meta['sampling']

# field of view
n0 * d0, n1 * d1, n2*d2
```

- Image shape : number of elements along each axis
- Field of view : total amount of space covered along each axis.
















