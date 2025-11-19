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

We also did not have to worry about part 107 certification, since we were working with a couple of supervisors that were supervised. We also did not have to worry about collecting data in private locations since we had full permission.
