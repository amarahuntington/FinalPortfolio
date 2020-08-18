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




```


  



```python

```
<img width="597" alt="heart" src="https://user-images.githubusercontent.com/69179367/90495286-cd6b9300-e11a-11ea-87bb-3db7629ea242.png">

<img width="376" alt="zoom" src="https://user-images.githubusercontent.com/69179367/90495300-d2304700-e11a-11ea-8f71-9bff83218a43.png">
