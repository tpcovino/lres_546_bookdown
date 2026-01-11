
## Evapotranspiration

### Repo [here](https://github.com/tpcovino/09_evapotranspiration){target="blank"}

### Learning Module 8

20pnts<br>

#### Background

Suggested reading: [Forest Evapotranspiration: Measurement and Modeling at Multiple Scales](chrome-extension://efaidnbmnnnibpcajpcglclefindmkaj/https://www.srs.fs.usda.gov/pubs/ja/2016/ja_2016_amatya_008.pdf)

Evapotranspiration (ET) encompasses all processes through which water moves from the Earth's surface to the atmosphere, comprising both evaporation and transpiration. This includes water vaporizing into the air from soil surfaces, the capillary fringe of groundwater, and water bodies on land. 
Much like snowmelt modeling, ET modeling and measurements are critical to many fields and could be a full course on its own. We will be focused on the basics of ET, modeling and data retrieval methods for water balance in hydrological modeling. Evapotranspiration is an important variable in hydrological models, as it accounts for much of the water loss in a system, outside of discharge. 
Transpiration, a significant component of ET, involves the movement of water from soil to atmosphere through plants. This occurs as plants absorb liquid water from the soil and release water vapor through their leaves. To gain a deeper understanding of ET, let's review transpiration.

##### Transpiration
Plant root systems to absorb water and nutrients from the soil, which they then distribute to their stems and leaves. As part of this process, plants regulate the loss of water vapor into the atmosphere through stomatal apertures, or transpiration. However, the volume of water transpired can vary widely due to factors like weather conditions and plant traits.

Vegetation type: Plants transpire water at different rates. Some plants in arid regions have evolved mechanisms to conserve water by reducing transpiration. One mechanism involves regulating stomatal opening and closure. These plants can minimize water loss, especially during periods of high heat and low humidity. This closure of stomata can lead to diel and seasonal patterns in transpiration rates. Throughout the day, when environmental conditions are favorable for photosynthesis, stomata open to allow gas exchange, leading to increased transpiration. Conversely, during the night or under stressful conditions, stomata may close to conserve water, resulting in reduced transpiration rates. 

Humidity: As the relative humidity of the air surrounding the plant rises the transpiration rate falls. It is easier for water to evaporate into dryer air than into more saturated air.  

Soil type and saturation: Clay particles, being small, have a high capacity to retain water, while sand particles, being larger, readily release water. During dry periods, transpiration can contribute to the loss of moisture in the upper soil zone.When there is a shortage of moisture in the soil, plants may enter a state of senescence and reduce their rate of transpiration.

Temperature: Transpiration rates go up as the temperature goes up, especially during the growing season, when the air is warmer due to stronger sunlight and warmer air masses. Higher temperatures cause the plant cells to open stomata, allowing for the exchange of CO2 and water with the atmosphere, whereas colder temperatures cause the openings to close. 

The availability and intensity of sunlight have a direct impact on transpiration rates. Likewise, the aspect of a location can influence transpiration since sunlight availability often depends on it.

Wind & air movement: Increased movement of the air around a plant will result in a higher transpiration rate. Wind will move the air around, with the result that the more saturated air close to the leaf is replaced by drier air. 

#### Measurements

In the realm of evapotranspiration (ET) modeling and data analysis, you'll frequently encounter the terms potential ET and actual ET. These terms are important to consider when selecting data, as they offer very different insights into water loss processes from the land surface to the atmosphere.

Potential Evapotranspiration (PET): Potential ET refers to the maximum possible rate at which water could evaporate and transpire under ideal conditions. These conditions typically assume an ample supply of water, unrestricted soil moisture availability, and sufficient energy to drive the evaporative processes. PET is often estimated based on meteorological variables such as temperature, humidity, wind speed, and solar radiation using empirical equations like the Penman-Monteith equation.

Actual Evapotranspiration (AET): Actual ET, on the other hand, represents the observed or estimated rate at which water is actually evaporating and transpiring from the land surface under existing environmental conditions. Unlike PET, AET accounts for factors such as soil moisture availability, vegetation cover, stomatal conductance, and atmospheric demand. It reflects the true water loss from the ecosystem and is often of greater interest in hydrological modeling, as it provides a more realistic depiction of water balance dynamics.

The formula for converting PET to AET is:

AET = PET * Kc

Where:

AET is the actual evapotranspiration,
PET is the potential evapotranspiration, and
Kc is the crop coefficient.
The crop coefficient accounts for factors such as crop type, soil moisture levels, climate conditions, and management practices. It can vary throughout the growing season as well.

##### Direct measurements: 

There are several methods to measure ET directly like lysimeters and gravimetric analysis, but this data rarely available to the public. There has been a concerted effort to enhance the accessibility of Eddy Covariance data, so the dataset mentioned in the video below may expand in the years to come. 

This video focuses on CO2 as an output of eddy covariance data, but among the 'other gases' mentioned, water vapor is included, offering means to estimate actual ET. The video also provides a resource where you might find eddy covariance data for your region of interest.


``` r
knitr::include_url("https://www.youtube.com/embed/CR4Anc8Mkas")
```

<iframe src="https://www.youtube.com/embed/CR4Anc8Mkas" width="672" height="400px" data-external="1"></iframe>

##### Remote sensing: 

Remote sensing of evapotranspiration (ET) involves the use of satellite or airborne sensors to observe and quantify the exchange of water vapor between the Earth's surface and the atmosphere over large spatial scales. This approach offers several advantages, including the ability to monitor ET across diverse landscapes, regardless of accessibility, and to capture variations in ET over time with high temporal resolution.

Remote sensing data, coupled with energy balance models, can be used to estimate ET by quantifying the energy fluxes at the land surface. These models balance incoming solar radiation with outgoing energy fluxes, including sensible heat flux and latent heat flux (representing ET). Remote sensing-based ET estimates are often validated and calibrated using ground-based measurements, such as eddy covariance towers or lysimeters, to ensure accuracy and reliability. It can be helpful to validate these models yourself if you have a data source available in your ecoregion as a 'sanity check'. Keep in mind that there are numerous models available, some of which may be tailored for specific ecoregions, resulting in significant variations in estimated evapotranspiration (ET) for your area among these models. If directly measured ET data is not available, you can check model output in a simple water balance. For example, inputs - outputs for your watershed (Ppt - Q - ET) should be *approximately* 0 (recall from our transfer function module that it is likely not exact). If the ET estimate matches precipitation, it's likely that the selected model is overestimating ET for your region.  

Some resources for finding ET modeled from remote sensing data: 

[ClimateEngine.org](https://app.climateengine.org/climateEngine) - This is a fun resource for all kinds of data. Actual evapotranspiration can be found in the TerraClimate dataset.

[OpenET](https://etdata.org/) - you need a Google account for this one. This site is great if you need timeseries data. You can select 'gridded data' and draw a polygon in your area of interest. You can select the year of interest at the top of the map, and once the timeseries generates, you can view and compare the output of seven different models.

#### Modeling:

### Labwork (20 pts):

For this assignment, we will work again in the Fraser Experimental Forest (same site as snowmelt module). Not only are there meteorological stations in our watershed of interest, but there is a Eddy Covariance tower nearby, in a forest with the same tree community as our watershed of interest. We can use this data to verify our model output for modeled data.

#### The [Evapotranspiration package](https://cran.r-project.org/web/packages/Evapotranspiration/index.html)
We will test a couple of simple methods that require few data inputs. However, it may be helpful to know that this package will allow calculations of PET, AET and Reference Crop Evapotranspiration from many different equations. Your selection of models may depend on the study region and the data available.

#### Import libraries



#### Import data
##### Meteorological data

We'll import three datasets in this workflow, one containing actual data from the eddy covariance tower that has been converted to AET. This dataset ranges from 2018 to 2023 <br>
Also imported is the discharge (Q) data from our watershed. Note that data is not collected during periods of deep snow. We will use this data to estimate the total discharge for 2022. <br>
Additionally, we'll import the meteorological data from our watershed. We are going to estimate ET for our watershed during the 2022 water year, though met data is provided in calendar years, so we will need to extract the desired data from the meteorological dataframe.



#### Data formatting

We will model ET at daily timesteps. Note that all imported data is for different timesteps, units and measurements than what we need for analysis. It is common practice to manipulate the imported data and adjust it to align with our model functions. Ensuring the correct structure of the data can prevent potential issues later in the code





Now we have the RHmax and RHmin values that we will need for our ET models

##### Precipitation 

To add a layer of checks we will also check precipitation totals from the watershed from meteorological stations. This dataframe provides cumulative precipitation, though the accumulation begins on January 1. Therefore we will need to calculate daily precipitation for each day of the water year and generate a new cumulative value that accumulates from October 1 - Sept 30 (water year cumulative total).


This was a lot of work to save one value.

#### Pseudocode

Often when writing code, it is necessary to start with pseudocode. Pseudocode allows us to plan and structure the logic of the script before implementing it in a specific programming language. It is typically a mix of natural language and code that serves as a blueprint for the script(e.g., "I'll used 'merge' to combine my data frames by a shared date column"). Once the workflow is outlined, then we can translate it into actual code using appropriate syntax.  

**Q1. (2 pnts)  In you own words, what did we do at each step in the above chunk and what is cumulative_ppt_2022. Why do we want to know this?**
Hint: if the third step in the above chunk is confusing, look at the new daily_ppt_mm column after running the first two steps to see what happens without it.

ANSWER:

##### Discharge data

Our next step will be to import and format discharge data collected from the weir at Fool Creek. Again, data collection starts on April 20th this year. To estimate discharge between Sept 31 of the previous year and April 20th of 2022, we will assume daily discharge between these dates is the mean of the discharge values from each of these dates. 

**Q2 (3 pnts) Write pseudocode in a stepwise fashion to describe the workflow that will need to happen to estimate the total volume of water in mm/watershed area for 2022 lost to stream discharge. Consider that the units provided by USFS are in cubic feet/second (cfs).**

ANSWER:
1. Import dataframe
2. ...



##### Evapotranspiration data
Import and transform ET data. <br>
Note that ET data is in mmol/m2/s. We want to convert this to mm/day. <br>
Eddy covariance data collected from towers represents the exchange of gases, including water vapor, between the atmosphere and the land surface within a certain area known as the "footprint." This footprint area is not fixed; it changes in size and shape depending on factors like wind direction, thermal stability, and measurement height. Typically, it has a gradual border, meaning that the influence of the tower measurements extends beyond its immediate vicinity.

For our specific tower data, we've estimated the mean flux area, or footprint area, to be approximately 0.4 square kilometers. However, when estimating the total evapotranspiration (ET) for our entire watershed, we need to extrapolate the ET measured by the tower to cover the entire watershed area. This extrapolation involves scaling up the measurements from the tower footprint to the larger area of the watershed to get a more comprehensive understanding of water vapor exchange over the entire region.






This appears to be a fairly well balanced water budget, especially considering that we have made some estimates along the way. Let's see how this ET data compares to modeled data. 

#### [The Priestly Taylor method](https://search.r-project.org/CRAN/refmans/Evapotranspiration/html/ET.PriestleyTaylor.html)

The Priestley-Taylor method is a semi-empirical model that estimates potential evapotranspiration as a function of net radiation. This method requires a list which contains:<br> (climate variables) required by Priestley-Taylor formulation:
Tmax, Tmin (degree Celcius), RHmax, RHmin (percent), Rs (Megajoules per sqm) or n (hour). <br>
We have measurements of relative humidity (RH) data from within our watershed meteorological station. However, RH data can be found through many sources providing local weather data. <br>
n refers to the number of sunshine hours. 



Evapotranspiration comes with a list physical 'constants', but we want to replace important constants with data specific to our cite. 







We can change the function name to specify the desired model. Now we'll try the[the Hamon method](http://data.snap.uaf.edu/data/Base/AK_2km/PET/Hamon_PET_equations.pdf).





Recall that each of these are estimates of potential ET. 
Crop coefficients (Kc values) for specific tree species like those found in the Fraser Experimental Forest (lodgepole pine or Englemann spruce) may not be as readily available as they are for agricultural crops. However, you may be able to find some guidance in scientific literature or forestry publications. From what we have found, Kc estimates for lodgepole pine forests can be between 0.4 and 0.8. These values may vary depending on factors such as climate, soil type, elevation, and other site-specific factors. Our own estimates using water balance data from dates that correspond with the eddy flux tower data suggest seasonal fluctuations, with a mean of 0.55. 

AET = PET * Kc



Let's plot the two modeled ET timeseries with the eddy covaraince tower data. 



**Q3. (4 pnts) Consider the two evapotranspiration (ET) models we have generated, one consistently underestimates ET, while the other consistently overestimates summer values and underestimates winter values. Consider ecosystem specificity in modeling. Why do you think these two methods generate such different estimates? What ecosystem-specific factors might contribute to the discrepancies between models?**

ANSWER:

**Q4 (3 pnts) Let's assume we do not have eddy covariance tower data for this time period, but you have the provided discharge and precipitation measurements. Which model would you choose to estimate an annual ET sum and why?** 

ANSWER:

**Q5 (3 pnts) In our modeling script, while our main objective was to estimate evapotranspiration (ET), which specific aspect required the most labor and attention to detail? Reflect on the tasks involved in data preparation, cleaning, and formatting. How did these preliminary steps impact the overall modeling process? **

ANSWER:

**Q6 (5 pnts) Review the sub-watershed figure and analyze how vegetation characteristics (e.g., height, age), slope, and aspect vary across high-elevation watersheds in the Fraser Experimental Forest. Utilize Google Maps' high-resolution imagery as an additional resource to observe these landscape features. How might these factors interact to influence actual evapotranspiration (AET) in this region? Provide a hypothetical comparison of two sub-watersheds, describing how differences in these variables could lead to variations in AET.**

ANSWER:

![Fool Creek delineated watershed](images/fool_studyarea.png)
