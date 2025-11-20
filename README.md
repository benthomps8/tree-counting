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
Once the imagery was collected, we compiled the images into an orthomosaic. We used Agisoft Metashape to align the photos, create a point cloud, and from there the orthophoto. Were we to redo the project, we would have first created a DEM from the point cloud, which would have resolved some of our issues. We also should have used ground control points for more accurate alignment. Here are our two orthomosaics, one from the 2024 imagery and one from 2025:

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





