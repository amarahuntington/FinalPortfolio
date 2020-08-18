```python
import imageio as im
import numpy as np
import scipy.ndimage as ndi
import matplotlib.pyplot as plt


## Load image as assign as 'im'
im = im.imread('SCD2001_MR_117.dcm')


# Use a filter to smooth intensity values
im_filt = ndi.median_filter(im, size=3)

# Create mask to only select high-intensity pixels
mask_start = np.where(im_filt > 60, 1, 0)
mask = ndi.binary_closing(mask_start)

# Label the objects in "mask"
labels, nlabels = ndi.label(mask)

# Create a `labels` overlay
overlay = np.where(labels > 0, labels, np.nan)

#Plot the overlay ontop of the image
plt.imshow(overlay, cmap='rainbow', alpha= 0.75)
format_and_render_plot()

<img width="597" alt="heart" src="https://user-images.githubusercontent.com/69179367/90495286-cd6b9300-e11a-11ea-87bb-3db7629ea242.png">


# We want to segment the left ventricle from this image. To do this, I first find the index value for the left venticle label
lv_val = labels[128, 128] 
lv_mask = np.where(labels == lv_val, 1, 0)

# Find bounding box of left ventricle
bboxes = ndi.find_objects(lv_mask)

# Crop to the left ventricle (index 0)
im_lv = im[bboxes[0]]

# Plot the cropped image
plt.imshow(im_lv) 
format_and_render_plot()

<img width="376" alt="zoom" src="https://user-images.githubusercontent.com/69179367/90495300-d2304700-e11a-11ea-8f71-9bff83218a43.png">


```


    ---------------------------------------------------------------------------

    FileNotFoundError                         Traceback (most recent call last)

    <ipython-input-1-5f0fd47f03cb> in <module>
          6 
          7 ## Load image as assign as 'im'
    ----> 8 im = im.imread('SCD2001_MR_117.dcm')
          9 
         10 


    /usr/local/lib/python3.6/dist-packages/imageio/core/functions.py in imread(uri, format, **kwargs)
        263 
        264     # Get reader and read first
    --> 265     reader = read(uri, format, "i", **kwargs)
        266     with reader:
        267         return reader.get_data(0)


    /usr/local/lib/python3.6/dist-packages/imageio/core/functions.py in get_reader(uri, format, mode, **kwargs)
        170 
        171     # Create request object
    --> 172     request = Request(uri, "r" + mode, **kwargs)
        173 
        174     # Get format


    /usr/local/lib/python3.6/dist-packages/imageio/core/request.py in __init__(self, uri, mode, **kwargs)
        119 
        120         # Parse what was given
    --> 121         self._parse_uri(uri)
        122 
        123         # Set extension


    /usr/local/lib/python3.6/dist-packages/imageio/core/request.py in _parse_uri(self, uri)
        255                 # Reading: check that the file exists (but is allowed a dir)
        256                 if not os.path.exists(fn):
    --> 257                     raise FileNotFoundError("No such file: '%s'" % fn)
        258             else:
        259                 # Writing: check that the directory to write to does exist


    FileNotFoundError: No such file: '/home/user/Assignments/Assignment_4/SCD2001_MR_117.dcm'



```python

```
