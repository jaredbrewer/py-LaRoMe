
# LaRoMe

Jython translation of the outstanding basework done in https://github.com/BIOP/ijp-LaRoMe. This is meant to increase the programmatic nature of these tools by integrating them into Python language work flows in FIJI/ImageJ.

**Label image** (aka Count Masks): An image in which pixels of an object have all the same value. Each object has a unique value.
 
**Measurement image**: An image in which pixels of an object have all the same value, corresponding to a measurement (Area, Angle, Mean...) 

## Scripting

Each script takes a set of arguments that make them ideal for iterative operations over a large number of images. They can, of course, also be used for individual open images, but will minimally require a Jython script containing the requested information. The requirements for each script are listed in their respective sections.

## Installation

These Jython scripts can be installed by placing them into the /Fiji.app/jars/Lib directory (at least on macOS). If this directory does not exist, you can create it. For other locations, open a Jython interpreter in FIJI and run:

```
import sys

for p in sys.path:
	print(p)
```

## labelsToROIs

From a **Label Image**(§), generate ROIs and add them to the ImageJ ROI Manager.

labelsToROIs requires one argument (imp) and has one optional argument (save, as a boolean True or False; default = False). Saving the ROIs will allow them to be used for the inverse approach used by ROIsTolabels

Example:
```
from ij import IJ, ImagePlus
from ij.plugin.frame import RoiManager
from labelsToROIs import labelsToROIs

imp = IJ.getImage()
rm = RoiManager.getRoiManager()
labelsToROIs(imp)
```

## ROIsTolabels

From an (optional) **Image** and some **ROIs**, generates a **Label Image**.

ROIsTolabels has only one mandatory argument (rois) and one optional argument (imp_path). The imp is optional because the entire image can be approximately reconstructed solely from the ROIs if necessary, but this comes with a performance penalty as the ROIs are used to calculate the minimum necessary image dimensions. 

In its current form, this is only useful for iterative processes over directories or specific calls to discrete files. The script will save the resulting labels with a new name in the same folder.

Example:

```
from ij import IJ, ImagePlus
from ROIsTolabels import ROIsTolabels

rois = "/Users/user/Documents/test.zip"

ROIsTolabels(rois, imp_path = None)
```

## ROIsToMap

ROIsToMap has three arguments, all mandatory: imp_path, rois, and column_name. The imp_path should point to a specific image and rois should point to the corresponding ROI (either .roi or .zip). Column_name is one of:

* "area" 
* "angle" 
* "anglevert" 
* "ar" 
* "circumference" 
* "major"
* "minor"
* "mean"
* "median"
* "mode"
* "min"
* "max"
* "perimeter"
* "xcenterofmass"
* "ycenterofmass"

Case insensitive.

Example:

```
from ij import IJ, ImagePlus
from ROIsToMap import ROIsToMap

imp_path = "/Users/user/Documents/test.tif"
rois = "/Users/user/Documents/test.zip"
						
ROIsToMap(imp_path, rois, "AREA")
```

And it will save the resulting measurement image in the source directory.
