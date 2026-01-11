# Spatial Models and GIS Integration
## Gridded and tabular data retrieval
**15 pnts**

### Learning Module 9 
#### Objectives
 In this module you will:<br><br>
 -Retrieve flow data using the dataretrieval package<br>
 -Learn to retrieve and manage digital elevation models<br> 
 -Retrieve and import remotely sensed data for the watershed extent<br> 


#### Background
This final section moves toward spatially explicit modeling, integrating everything we have learned so far with geospatial data. 

A refresher in GIS concepts:
  This module assumes familiarity with basic Geographic Information System (GIS) such as coordinate systems (latitude and lognitude, UTM). Some terms to recall: 
  
  **Gridded data** refers to spatial data in raster format. If you have performed any GIS work, you are likely using gridded data. Rasters organize geographical areas into a grid where each regularly spaced cell represents a specific location and contains some attribute representing that location. For example, digital elevation models (DEM) are a form of gridded data. Imagine that we are working in a watershed and we can drop a grid or square celled net over the entire catchment. We can then model the elevation data of that catchment by assigning each cell in the net a mean elevation value for the area represented by the cell. Similarly, we can assign any value we wish to model to the cells. If we were to drop another net and assign a maximum tree height to every cell, we would be adding another 'layer' to our model. The **spatial resolution** of gridded data refers to the length and width of each cell represents. If we want to observe gridded data over time, we can generate a raster stack, where each raster layer represents a point in time. With a raster stack, we can increase the **temporal resolution** of our gridded data. 

  **Vector data** describes spatial features using points, lines, and polygons. These spatial features are represented a geometrical objects with associated attributes, like lines to represent linear features such as rivers, or points to represent single locations. We will work with a .shp file in this module that is a collection of points representing the boundary of a watershed. 

  **Tabular data** refers to data organized in a row-column format. In this class, each of our .csv datasets are a form of tabular data. 

#### A refresher in remote sensing:

Remote sensing is the science of obtaining information about objects or areas from a distance, typically from aircraft or satellites. It involves the collection and interpretation of data gathered by sensors that detect electromagnetic radiation reflected or emitted from the Earth's surface and atmosphere. As ecologists, many of us utilize remote sensing to observe and analyze the Earth's surface and its dynamics, providing valuable insights into environmental processes, land use and land cover changes, natural disasters, and more. 
Some helpful terms to recall:

**spatial resolution:** Remotely sensed data can vary in its spatial resolution, which refers to the scale of the smallest unit, or level of visual detail. 

**temporal resolution:** When used to describe remotely sensed data, we are often talking about how frequently a remote sensing system revisits the same area on the Earth surface. Systems with high temporal resolution revisit the same location frequently, allowing for monitoring rapid changes in phenomena, where a lower resolution may limit a system's ability to capture short-term events. 

We often find that data collection requires a balance of spatial and temporal resolution to select the most appropriate data for the coverage and detail that ecological work requires. 

#### Ethical conduct

When utilizing publicly available datasets, it is important for us as users to:<br><br>
  -Be aware of any copyright or licensing agreements. There may be restrictions or usage terms imposed by data providers. Read provided agreements or terms to be sure we are in compliance with these when using datasets in our research. This may also include requirements for registration or subscription established by providers or agencies like USGS or NASA.<br><br>
  -Cite and acknowledge the contributions of data providers and researchers whose datasets we utilize. This also facilitates transparency and reproducibility in our research.<br><br>
  - Assess the quality and accuracy of geospatial data before using it for decision making purposes. It is important that we consider spatial resolution, temporal resolution and metadata documentation to critically evaluate the reliability of the datasets we use.<br><br> 
  -Promote Open Data Principles: Try to explore and use publicly available datasets that support transparency and collaboration within our scientific and academic communities.<br><br>

### Repo link

The repo for this module can be found [here](https://github.com/tpcovino/07_gridded_data_retrieval){target="_blank"}

### Labwork (20 pnts)

In this module we will import streamflow (tablular) data, using USGS's DataRetrieval package, then visualize vector data to delineate our area of interest and learn how to find spatial gridded data for the area. 

#### Setup:<br>

When importing packages and setting parameters, think about how the local working environment can vary among users and computers. A function that examines and installs packages before importing them enhances the script's ability to be easily used by others. 



#### Selecting a watershed: 

Here, we are using USGS' dataRetrieval package to search and filter hydrologic datasets. There are many ways data can be searched; more can be found in this [dataRetrieval vingette](https://cran.r-project.org/web/packages/dataRetrieval/vignettes/dataRetrieval.html). In this example, we will fetch USGS site numbers for Colorado watersheds with streamflow (discharge) data for a specified time period. 



**Q1** (2 pts). If you were creating this script for your work team's shared collaboration, or for yourself to use again a year from now, what might be the disadvantages of embedding search parameters directly into the function parameters? 

ANSWER: 

**Q2** (3 pts). What is a potential use for whatNWISsites?

ANSWER:

**EXTRA** (1 pt): Why might the above script return an error if dpylr:: is not placed before select()?

ANSWER: 


drain_area_va is a column value returned by readNWISsite. [Other variables can be found here](https://rconnect.usgs.gov/dataRetrieval/reference/readNWISsite.html)

**Q3** (1 pt). In a sentence, what does this dataframe (all_site_data) tell you?

ANSWER: 

Note that while your research interests may not require streamflow data, it can serve as a valuable resource for a wide array of data types. 

For the remainder of this assignment, we'll focus on a single watershed in central Colorado near the Experimental Forest that we explored in the Snowmelt module. There are several dataRetrieval functions could we utilize to see what data is available for a particular site number. Check [dataRetrieval docs](https://waterdata.usgs.gov/blog/dataretrieval/) to learn package functions or tools.



**Q4** (4 pts). Find the longest running dataset in DataAvailable (i.e. either by count or by start and end date). Use https://help.waterdata.usgs.gov to look up the meaning of some of column headers and the codes associated with them.<br> 
a. What kind of data does this long running dataset hold? <br> 
b. What column did you use to learn that? <br> 
c. What does 'dv' mean in data_type_cd? <br> 

ANSWER: 

#### Flow data retrieval:





Let's rename our columns. For this you will need to determine the units of the date 





**Q5** (2 pts). Based on the hydrograph plot, what type of precipitation events appear to influence the observed intra-annual variations in flow rates? 

ANSWER: 

#### Watershed boundary vector data: 

A boundary shapefile has been provided to save time and improve repeatability. Watershed boundaries for gauging stations can be found at [USGS StreamStats](https://streamstats.usgs.gov/ss/) by zooming into the water map and clicking 'Delineate' near your desired gage or point. Once your watershed is delineated, you can click on 'Download Basin' and select from a choice of file types. This can be a great to to delineate river basins if all you require is a shapefile. For smaller streams, other analyses are available. We will cover those in the next module. 

Let's import a pre-created shapefile: 





Let's transform this to a projected CRS (flat surface) rather than a geographic (spherical surface) CRS. A projected CRS will depict the data on a flat surface which is required for distance or area measurements. A more detailed explanation can be [found at the University of Colorado's Earth Lab](https://www.earthdatascience.org/courses/earth-analytics/spatial-data-r/intro-to-coordinate-reference-systems/) if needed.



#### Visualization - watershed boundary 
i.e., sanity check



#### Digital Elevation Models (DEM)
A DEM has been provided for consistency among users.

However, if you are unfamiliar with the process of retrieving a DEM here are three methods that our team commonly uses:<br>

1. USDA's [Natural Resources Conservation Science (NRCS) website](https://datagateway.nrcs.usda.gov/GDGOrder.aspx). Here you can order (free) DEMs by state, county or bounding box. It takes just minutes for them to be emailed to you. <br><br>

2. If you have a USGS account (fast and free) you can also find DEM data at https://earthexplorer.usgs.gov. See [this video](https://youtube.com/watch?v=NQg0g9ObhXE) if interested. This is the method we used to provide this DEM and we will use this resource again to retrieve Landsat data.<br><br>

3. GIS or hydrological software like QGIS and SAGA offer tools for downloading DEMs directly into your project for analysis. This can be optimal when working over large geographic extents to streamline the data acquisition process. Both QGIS and SAGA are free and easy to learn if you have experience with ESRI products.<br><br>

Let's check out our DEM:



If the watershed and raster don't overlap as you expect, be sure the projection and extent is the same for all data files plotted. 



#### Other remotely sensed data:

Now that we have site data spatial data, let's cover a couple of techniques or resources you can use to access satellite data for your site. In the event that you only need a vegetative map layer from a specific point in time, it is not too difficult to download actual Landsat imagery from usgs.gov. In the case of Cabin Creek let's look at a single summer image from 2020.<br><br>

Navigate to https://ers.cr.usgs.gov/, and if you don’t already have an account, you can create one quickly or sign in if you already have one. Once signed in, go to https://earthexplorer.usgs.gov you should see a surface map with tabs on the left. 
Start with the ‘Search Criteria’ tab. For this assignment the shape file we have is too large to upload, so it is easiest to scroll down and choose ‘polygon’ or ‘circle’ to define your area. Under the circle tab you can use Center Latitude 39.9861   Center Longitude -105.719 and a radius of about 3.5 kilometers to capture our study area. 

Let's say we want an NDVI map layer from summer of 2020. In 'date range', select dates for the summer of 2020. In this high elevation area of Colorado, the greenest times of the year without snow cover are likely June and July with senescence beginning sometime in late July. First search from June 1 to July 15 2020. Then click on ‘Data Sets’ either at the bottom or top of the left-hand side menus. From the data sets we will select Landsat Collection 2 level 2, Landsat 8 - 9 OLI TIRS C2L2. 

We can skip ‘Additional Criteria’ for now and go to ‘Results’. You should receive a set of about 11 images. For each image you can check out the metadata. That is the 'pen and notepad' icon in the middle of image specific menu. Here you can look at data like cloud cover. For these images, the values you see are a cloud coverage percent. Cloud coverage measurements and the codes to filter for certain cloud cover thresholds may change across missions. If you are retrieving Landsat data for time series using code, it will be important to explore the different quality control parameters and what they represent. You don't need to download anything for this assignment, we've provided it for you. However to gain familiarity with this process, click on the 'download' icon for July 7, 2020 and click 'select files' on the Reflectance Bands option. 

**Q6** (1 pt): What 2 bands should we download to calculate NDVI?

Answer:

**Q7** (3 pt): Find 2 other indices that can be used as indicators of vegetative health. What might each of these indices indicate. What are the bands used for each index? 

Check out other earthexplorer resources while you are at the site. You will likely find resources here for your MS projects, even if it is just for making maps of your study area for presentations or paper.

##### Landsat 
If you want to learn about Landsat missions or utilize Landsat data, USGS offers all of the reference information you need. The sensors on these missions can be a fantastic resource if characterizing land or water surface over large temporal scales. Landsat data provides (30m) spatial resolution so can capture spatial heterogeneity, even for smaller study areas. If calculating vegetation indices over different time periods, it is important to know which mission you are retrieving data from and check the band assignments for that mission. For example, on the Landsat 6 instrument, Band 4 is near-infrared, whereas Band 4 on Landsat 9's instrument is red.

##### Moderate Resolution Imaging Spectroradiometer (MODIS):
is a sensor aboard NASA's Terra and Aqua satellites which orbit Earth and collect data on a daily basis. MODIS data provides moderate spatial resolution (250m to 1km) and high temporal resolution. Therefore, MODIS data can be an optimal way to monitor or characterize rapid changes to Earth's surface like land cover changes resulting from wildfire. Data can be accessed and downloaded directly through [NASA's Earthdata tool](https://www.earthdata.nasa.gov/).

Let's import our raster data. 



**Q8** (2 pt)  What does 'SR' in the .tif names tell us about the data?

ANSWER: 

**Q9** (2 pts)  What is the difference between 'mask' and 'crop' when filtering large files to fit the area of interest?

ANSWER: 

#### Earth surface changes over time

If considering changes over time, comparing individual rasters from different time periods directly may not always provide meaningful insights because the absolute values of vegetation indices can vary due to factors such as differences in solar angle, atmospheric conditions, and land cover changes. Instead, it's often more informative to analyze trends in vegetation over time.

##### LandTrendr
One fun way to do this is with [Oregon State University's LandTendr](https://emapr.github.io/LT-GEE/landtrendr.html). With a Google account and access to Earth Engine, you can easily interact with the UI Applications in section 8. You can jump to the [pixel timeseries here](https://emaprlab.users.earthengine.app/view/lt-gee-pixel-time-series).
This will allow you to view changes in vegetation indices over a specified time period within a single Landsat pixel. 

##### Google Earth Engine
Another effective way to access evaluate large spatial datasets is through Google Earth Engine (GEE). GEE is a cloud-based platform developed by Google for large scale environmental data analysis. It provides a [massive archive of satellite imagery and other geospatial datasets](https://developers.google.com/earth-engine/datasets/), as well as computational tools to view and analyze them. GEE uses Google's cloud to provide processing of data so you don't need powerful computing ability to run a potentially computationally expensive spatial analysis. 

###### GEE Access

If spatial data is something you may need for your MS project, we recommend exploring this resource

**Sign Up:** Go to the Google Earth Engine website (https://earthengine.google.com/) and sign in with your Google account. If you don't have a Google account, you can sign up for access by filling out a request form.

**Request Access:** If you're signing up for the first time, you'll need to request access to Google Earth Engine. Provide information about your affiliation, research interests, and how you plan to use Earth Engine (learn and access data!).

**Approval:** After submitting your request, you'll need to wait for approval from the Google Earth Engine team. Approval times may vary, but you should receive an email notification once your request has been approved.

**Access Earth Engine:** Once approved, you can access Google Earth Engine through the Code Editor interface in your web browser at code.earthengine.google.com to start exploring the available datasets, running analyses, and visualizing your results.

We have provided a [basic script for retrieving and viewing NDVI data](
https://code.earthengine.google.com/332cc9055781b21d8506620f0a4cb582). This is a code snapshot, so you can manually copy the code from the snapshot and paste it into you own Google Earth Engine Code editor. 
You are welcome to save this to your own GEE files and try changing the area of interest, vegetation indices, time periods or exploring other data. 

Colorado State University also has a [great tutorial here](https://ecology.colostate.edu/google-earth-engine/) that will start you from scratch. 

Note that JavaScript is the default language for the code editor interface in your web browser. If you are familiar with Python, GEE provides a Python API that can allow you to use Python syntax. If neither of these languages are familiar, don't give up yet! You can still use this tool with your R coding background and trial and error. We recommend finding other scripts that suit your need (there are a lot of shared resources online as well as in our data collection assignment) and adapting them to your area of interest. Once you understand coding concepts, like those in R, other languages can be picked up easily. You will find that you are learning JavaScript before you realize it. 




