# Natural_Disasters
This project uses machine learning and satellite imagery to better target disaster relief efforts. I focused on Cyclone Phailin, which hit the Odhisa in October of 1999. It broke records for having the highest wind speeds upon landfall and destroyed over 1 million homes.

After natural disasters, it's important to understand which areas suffered the most damage in order to prioritize relief efforts. Often times damage assessment maps are created by volunteers with the Humanitarian Open Street Map team who compare satellite imagery before and after the disaster and manually label each building with their evaluation of damage. However these maps are time and labor intensive to create, and not always accurate.

Studies have found that Open Street Map data often over-estimates damage in areas that are talked about in the news and under-estimates damage elsewhere.

# Goal
My goal is  to create a model that could more quickly and more accurately identify the hardest hit areas in order to better target disaster relief.

Using satellite imagery before and after cyclone Phailin in odhisa, I built a neural network to detect damaged buildings. Using the predictions from the model, I then created density maps of damage, illustrating priority areas for relief efforts.

# Data
I  shall use Landsat8 satellite imagery, which is at 15 meter resolution (and available freely through Google Earth Engine). This resolution is much lower than commercially available satellite imagery (which can get up to 30 cm per pixel!), but the success of this model using publicly available data demonstrates its applicability to organizations that may be limited in funding.
For example the building damage data will be like, with different colors squares indicating different levels of damage. Superimposing these polygons on my satellite imagery, I labeled each pixel in my satellite imagery as either being part of a damaged building or not.

# Random forest baseline model
My baseline model was a random forest pixel-wise classification (with each pixel either being damaged or not damaged). The model features came from the band information from my satellite data, i.e., for each pixel, how much red it has, how much blue, green, UV, infrared, etc.

I shall do  a pixel wise subtraction between pre-cyclone and post-cyclone imagery given that it is a change detection model. I wanted to understand whether there was a building there before but isn't there anymore.

However many things can change — buildings can collapse and vegetation can bloom. Yet vegetation reflects infrared really highly whereas buildings do not. By including the pre and post pixel values in addition to the subtraction values, I was able to better identify buildings.

To deal with a class imbalance, I trained the model on a balanced sample. This made it more sensitive to damaged buildings.

When I trained a random forest on the top half of three satellite images and had it predict on the bottom half, it did very well as shown by the ROC curve which is very close to the upper left hand corner. However when I asked the model to predict on an image it hasn't seen before, the ROC curve dropped precipitously. This indicates that the model does a poor job of generalizing. The model is therefore hard to scale as it would be infeasible to train a model on every part of a country after a natural disaster.

# A superior U-Net model
To create a model that is more generalizable, I built a neural net called a U-Net, named for its structure of convolutional layers. U-Nets are known to be good for object detection (segmentation).

This model did quite well with my data. The ROC on the left is for the validation set, which are images the model has seen. On the right is the ROC curve for the holdout set, which are images the model has never seen before. Unlike the random forest, the ROC curve didn't drop significantly. The fact that this model extends well to new images is important as one would want to feed in satellite imagery for an entire country to generate hotspot predictions after a natural disaster.

# Mapping the density of damage
The U-Net predictions identify where the damaged buildings are, and from this, I created a density map highlighting the areas with the greatest building damage.

# Applications
This sort of change detection model has many applications. For example, it could be used to identify illegal deforestation, monitor rising sea levels caused by global warming, or identify crop loss due to droughts or floods.

In the case of disaster relief as I've written about today, this model enables targeting of limited resources to ensure that structural building assessments, food, water, and other aid are going to the people who need it most.

# Languages: Python, JavaScript
# Libraries: Keras + TensorFlow, numpy, pandas, sklearn, rasterio, geopandas, shapely, opencv, matplotlib, seaborn
# Methods: Deep learning, classification (supervised learning)












