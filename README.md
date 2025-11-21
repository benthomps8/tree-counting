# tree-counting
Project using drone imagery and counting the number of trees in an orthoimage

## Collaborators
Ben Thompson, Cayden Doherty, Finn Olson, Josh Mason

## Description
We did this project as a self-guided final for Geography 489 - Mapping with Drones at the University of Oregon. For this project, our group travelled to Baumann Tree farm in Lane County, Oregon and collected over 100 drone images of the landscape. From there, we processed the imagery in Agisoft Metashape and made an orthomosaic. With that orthomosaic, we used a variety of methods to count the number of trees in the farm, or at least the segment that we observed for our project. These methods included manual counting (to use as reference), a pre-trained neural network, ArcGIS calculate geometry tool, and calculating points from a created Lidar point cloud

## Preperation
Before you fly a drone for the purpose of a project, you have to have a really clear plan and end goal that you set out to achieve. Our goal was to create a high quality orthomosaic for the purpose of counting trees. This means that we did not need a high spectral or temporal resolution. Additionally, our goal was not to map the entire tree farm, but rather a smaller portion of the farm so as to test our methods out. We also had a collection of drone imagery from 2024, so we wanted to utilize that data as well and compare the trees from 2024 to 2025 in order to quantify how many trees had grown or been destroyed since then. This information would be very useful to local governments because it can help to measure carbon emission or vegetation loss as a result of natural disasters in certain areas. 

We belived that drones would be requirement for a project of this scale. Aerial photography or satellite imagery would not have good enough spatial resolution to be able to accurately differentiate individual trees, and ground-level photography would be much too difficult to process and track. 

## Data Collection
The drone we flew was the Mavic 3 with an RGB camera. We flew at a height of 45 meters and took photos with 80% front and side overlap. We calculated these numbers before flying to ensure that our collected data would work for our end goal. We mapped a specific area of the farm, a total of 19354.9 square meters of land with a ground sampling distance of 1.62cm/pixel. We decided not to use ground control points because we did not think that high spatial accuracy was necessary. However, if we were to redo the project, we likely would have to ensure a more accurate orthomosaic. 

<img width="513" height="385" alt="image" src="https://github.com/user-attachments/assets/c95dbaa3-d4bc-45f2-bac7-fb7f53c9bb19" />

We also did not have to worry about part 107 certification, since we were working with a couple of supervisors that were certified, and our goal was not business related. We also did not have to worry about collecting data in private locations since we had full permission.

## Data Processing
Once the imagery was collected, we compiled the images into an orthomosaic. We used Agisoft Metashape to align the photos, create a point cloud, and from there the orthophoto. Were we to redo the project, we would have first created a DEM from the point cloud, which would have resolved some of our issues. We also should have used ground control points for more accurate alignment. Here is the workflow to create our orthomosaics:

  * Create new chunk in Agisoft Metashape
  * Import the drone images
  * Workflow -> Align Photos (medium accuracy) to generate sparse cloud
  * Workflow -> Build Point Cloud
  * Workflow -> Build DEM
  * Workflow -> Build Orthomosaic

Here are our two orthomosaics, one from the 2024 imagery and one from 2025:

<img width="358" height="431" alt="image" src="https://github.com/user-attachments/assets/e36d9f12-bc41-45c6-b0a6-db336a3c387e" />


<img width="359" height="429" alt="image" src="https://github.com/user-attachments/assets/69176c1f-2a3e-4def-aae0-3ab450dd0f89" />

The 2024 orthomosaic turned out much better than our 2025 one. In 2025, we captured the images later in the afternoon, which we believe caused a lower contrast, and lower quality images as a result. We also found motion blur in some images, which likely contributed to the pastelle appearance of our ortho.

## Tree Counting Method 1: Manual Counting
Once we had our orthoimage, we wanted to begin counting the trees! Our first method was to simply count by hand, so we could partially gauge the accuracy of our latest methods. We used the Windows snipping tool markup feature and counted about 1700 trees in each orthomosaic. 

<img width="442" height="527" alt="image" src="https://github.com/user-attachments/assets/f971b7e0-84be-4c01-8c9d-b5f35a9d18e4" />

## Tree Counting Method 2: Pre-trained Neural Network
Our second method to count trees was to use a pre-trained neural network. The model we used was called "DeepForest" and it was coded in Python. More details of the model can be found here: https://doc.arcgis.com/en/pretrained-models/latest/imagery/introduction-to-tree-detection.htm

<img width="991" height="564" alt="image" src="https://github.com/user-attachments/assets/813eb8f0-5a4c-4a68-bda7-06d2ae0896d6" />


The model is very simple to run. We just had to download it and import into ArcGIS Pro, along with a raster file of our orthomosaic. The model takes in an input (that raster) and chugs along to classify the trees. Here is the workflow:

* Install the necessary deep learning packages from GitHub(PyTorch, TensorFlow, scikit-learn, etc…). 254 total
* Install the model framework online
* Import the orthomosaic into ArcGIS Pro
* In the geoprocessing pane, search for “Detect Objects Using Deep Learning”
* Select the orthomosaic as the input raster, and the downloaded model as the Model Definition
* Adjust settings (padding, batch size, coordinate system, etc…) to your liking
* Select processor type and run the model

<img width="289" height="502" alt="image" src="https://github.com/user-attachments/assets/1406a97b-41d3-4448-a591-42f5b05eb18a" />

The model then outputs a feature class that you can overlay with the raster, and you can use the attribute table to see how many features (trees) it identified.

Our 2024 classification turned out alright. The model classified 6294 trees, which is obviously an overestimate based on our manual counting, but the tool seemed to work at classifying individual objects, and I belive had our orthomosaic been better, or had the area been more sparse, the model would have worked great. One major issue with the classification is that some ground debris or branches were classified as separate trees. 

<img width="1109" height="440" alt="image" src="https://github.com/user-attachments/assets/247efe6f-8be3-4456-83e9-1bbf2277453d" />

<img width="1044" height="511" alt="image" src="https://github.com/user-attachments/assets/8838d949-149d-4ec0-810f-63bb43be6972" />

Our 2025 orthomosaic did not have good results. The model classified 52,945 trees. Because our ortho was so low quality, and also because the trees were fully grown and overlapping with each other, the model had difficulty to differentiate each tree. However, the model was trained with forestry in mind, so because our estimate is so far off, we can assume the issue was with our orthomosaic. 

<img width="790" height="526" alt="image" src="https://github.com/user-attachments/assets/792750ed-d7a1-4fb7-bfc3-ed7626fe7a06" />

Overall, this method was fascinating to use and it is awesome seeing how many pre-trained models there are for a variety of tasks in the GIS world. Next time, we would definitely make sure our orthomosaic is higher quality.

## Tree Counting Method 3: ArcGIS Pro Tools

Our second tree counting method was using a hodgepodge of ArcGIS Pro tools. Our workflow is as follows:
  * Load raster into ArcGIS Pro
  * Use “Segment Mean Shift” tool to segment raster into objects based on spectral similarities
  * Classify trees by using “Train ISO Cluster Classifier” on the output
  * Use “Classify Raster” tool on the output
  * Separate raster classification into groups, use “Reclassify” to merge trees/non-trees and keep only trees
  * Convert to polygons with “Raster to Polygon”
  * Aggregate polygons w/ aggregation distance of 1 meter
  * Use “Calculate Geometry” to count the number of trees
After reclassifiying our raster to remove non-tree values, we get a result that looks like this:

<img width="727" height="494" alt="image" src="https://github.com/user-attachments/assets/63a2a80c-63d2-4c1e-ba05-d3e920f8c9ac" />

We then aggregate the polygons and get the count of polygons, or trees in this case. Unfortunately, our results for this method were even more wild than the deep learning method we used before. Our 2024 orthomosaic resulted in 66,217 classified trees, and our 2025 ortho classified 158,286 trees. 

<img width="779" height="499" alt="image" src="https://github.com/user-attachments/assets/29740aa3-0aec-411e-953b-37968e487734" />

To summarize, this method was clearly not meant for an accurate counting of trees. However, it was fun to mess around with ArcGIS tools and see what all you can do to manipulate rasters. 


## Tree Counting Method 4: lidR Package in R
We did not collect Lidar data for this project, but we were still able to create a point cloud in Agisoft Metashape and use it for analysis. The workflow is as follows:
  * Trim the point cloud in Agisoft Metashape to remove edges sticking out
  * Import raster into RStudio. Import lidr package

I unfortunately lost the R file since it was stored in a University fileshare which was deleted when I was no longer enrolled in the class. However, we used readLAS to get the raster data into RStudio. After that, we used crown_segmentation() to extract the high points of the raster, or the tree tops. We then created a canopy height model with the package and used crown_metrics to extract the crowns from the model. 

<img width="939" height="492" alt="image" src="https://github.com/user-attachments/assets/01219573-3fc3-4c99-ae6e-d0192315e017" />

<img width="809" height="527" alt="image" src="https://github.com/user-attachments/assets/dcc8e86e-70d1-4a73-a143-6ae39b646fb8" />

The R lidr package actually counted 1540 crowns, which is by far the closes to our manual count of trees! However, it looks like it may have grouped certain clusters of trees together, so the number may have been a little bit lucky. 

<img width="680" height="598" alt="image" src="https://github.com/user-attachments/assets/a76f6ec3-370e-422e-936a-f5ffa9a446ee" />

Ultimately, this method was very effective, although somewhat convoluted. And it is not easily replicable since Agisoft Metashape has a hefty price. 

## Conclusions
My main takeaway from this project is just how vast the world of remote sensing and GIS is. There are so many tools for such a seemingly unimportant task such as counting trees. Of course, I also learned that the methods all have lots of room for growth. Even the tree-detecting model in Python, with ideally high quality ortho-images, has a precision of only 66%. This makes me excited for potential growth in the GIS space, and I am fascinated with what new techniques will be made or discovered in the coming years. 

## Partner Contributions
This was a group project, so I did not do all of the work. We all worked together equally for methods three and four, each boucning ideas off of each other and walking through the classifications together. We also all worked together on the pre-flight plan, determining ideal flight height and the amount of pictures we should take and at what overlap. I unfortunately did not attend the actualy flying event due to a schedule conflict, so I did not get experience flying any drones. But to make up for this, I did the entirety of method two, finding the deep learning framework and implementing it. All in all though, each partner contributed equally to the project, and the majority of the work happened in a group setting, where we met on campus about five times to discuss and work through the project. 

## Sources
Deep Learning Model Download and Information: https://doc.arcgis.com/en/pretrained-models/latest/imagery/introduction-to-tree-detection.htm
Deep Learning Model Source Code: https://github.com/esri/deep-learning-frameworks















