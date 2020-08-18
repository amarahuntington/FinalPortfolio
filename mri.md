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

<img width="597" alt="Screen Shot 2020-08-18 at 5 38 38 AM" src="https://user-images.githubusercontent.com/69179367/90492865-081ffc00-e118-11ea-9866-48b685f69264.png">



# We want to segment the left ventricle from this image. To do this, I first find the index value for the left venticle label
lv_val = labels[128, 128] 
lv_mask = np.where(labels == lv_val, 1, 0)

# Find bounding box of left ventricle
bboxes = ndi.find_objects(lv_mask)

# Crop to the left ventricle (index 0)
im_lv = im[bboxes[0]]

# Plot the cropped image
plt.imshow(im_lv) 
<img width="376" alt="Screen Shot 2020-08-18 at 5 55 49 AM" src="https://user-images.githubusercontent.com/69179367/90492869-09512900-e118-11ea-8ddb-d9c46638602d.png">
format_and_render_plot()
```


  
