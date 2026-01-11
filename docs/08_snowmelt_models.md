# Physical Process Models
## Snowmelt Models
### Learning Module 7

#### Background:

Understanding snowmelt runoff is crucial for managing water resources and assessing flood risks, as it plays a significant role in the hydrologic cycle. Annual runoff and peak flow are influenced by snowmelt, rainfall, or a combination of both. In regions with a snowmelt-driven hydrologic cycle, such as the Rocky Mountains, snowpack acts as a natural reservoir, storing water during the winter months and releasing it gradually during the spring and early summer, thereby playing a vital role in maintaining water availability for various uses downstream.  By examining how snowmelt interacts with other factors like precipitation, land cover, and temperature, we can better anticipate water supply fluctuations and design effective flood management strategies.

Learning objectives: 

In this module, our primary focus will be modeling snowmelt as runoff, enabling us to predict when it will impact streamflow timing. We will consider some factors that may influence runoff timing. However, the term 'snowmelt modeling' is a field in itself and can represent a lifetime worth of work. There are many uses for snowmelt modeling (e.g., climate science and avalanche forecasting). If you are interested in exploring more on this subject, there is an excellent Snow Hydrology: Focus on Modeling series offered by [CUAHSI's](https://www.cuahsi.org){target="_blank"} Virtual University on YouTube.

Helpful terms:

The most common way to measure the water content of the snowpack is by the Snow Water Equivalent or **SWE**. The SWE is the water depth resulting from melting a unit column of the snowpack.

#### Model Approaches

Similar to the model development structure we discussed in the last module, snowmelt models are generally classified into three different types of [abalation](https://nsidc.org/learn/cryosphere-glossary/ablation#:~:text=Ice%20Data%20Center-,ablation,melting%2C%20evaporation%2C%20wind%20and%20avalanches){target="_blank"} algorithms 

**Empirical and Data-Driven Models:** 
These models use historical data and statistical techniques to predict runoff based on the relationship between snow characteristics (like snow area) and runoff. They use past patterns to make predictions about the future. The emergence of data-driven models has benefited from the growth of massive data and the rapid increase in computational power. These models simulate the changes in snowmelt runoff using machine learning algorithms to select appropriate parameters (e.g., daily rainfall, temperature, solar radiation, snow area, and snow water equivalent) from different data sources.

**Conceptual Models:** 
These models simplify the snowmelt process by establishing a simple, rule-based relationship between snowmelt and temperature. These models use a basic formula based on temperature to estimate how much snow will melt.
          
**Physical Models:** 
The physical snowmelt models calculate snowmelt based on the energy balance of snow cover. If all the heat fluxes toward the snowpack are considered positive and those away are considered negative, the sum of these fluxes is equal to the change in heat content of the snowpack for a given time period. Fluxes considered may be

  - net solar radiation (solar incoming minus albedo),
  - thermal radiation,
  - sensible heat transfer of air (e.g., when air is a different temperature than snowpack),
  - latent heat of vaporization from condensation or evaporation/sublimation,
heat conducted from the ground,
  - advected heat from precipitation
  
examples: layered snow thermal model (SNTHERM) and physical snowpack model (SNOWPACK), 

Many effective models may incorporate elements from some or all of these modeling approaches. 

#### Spatial complexity

We may also identify models based on the model architecture or spatial complexity. The architecture can be designed based on assumptions about the physical processes that may affect the snowmelt to runoff volume and timing. 

**Homogenous basin modeling:** You may also hear these types of models referred to as 'black box' models. Black-box models do not provide a detailed description of the underlying hydrological processes. Instead, they are typically expressed as empirical models that rely on statistical relationships between input and output variables. While these models can predict specific outcomes effectively, they may not be ideal for understanding the physical mechanisms that drive hydrological processes. In terms of snow cover, this is a simplistic case model where we assume:

  - the snow is consistent from top to bottom of the snow column and across the watershed
  - melt appears at the top of the snowpack
  - water immediately flows out the bottom 
  
This type of modeling may work well if the snowpack is isothermal, if we are interested in runoff over large timesteps, or if we are modeling annual water budgets in lumped models. 

**Vertical layered modeling:** Depending on the desired application of the model, snowmelt may be modeled in multiple layers in the snow column (air-snow surface to ground). Climate models, for example, may estimate phase changes or heat flux and consider the snowpack in 5 or more layers. Avalanche forecasters may need to consider grain evolution, density, water content, and more over hundreds of layers! Hydrologists may also choose variable layers, but many will choose single- or two-layer models for basin-wide studies, as simple models can be effective when estimating basin runoff. [Here](https://journals.ametsoc.org/view/journals/hydr/13/2/jhm-d-11-072_1.xml) is a study by Dutra et al. (2012) that looked at the effect of the number of snow layers, liquid water representation, snow density, and snow albedo parameterizations within their tested models. Table 1 and figures 1-3 will be sufficient to understand the effects of changes to these parameters on modeled runoff and SWE. In this case, the three-layer model performed best when predicting the timing of the SWE and runoff, but density improved more by changing other parameters rather than layers (Figure 1). 

**Lateral spatial heterogeneity:** 
The spatial extent of the snow cover determines the area contributing to runoff at any given time during the melt period. The more snow there is, the more water there will be when it melts. Therefore, snow cover tells us which areas will contribute water to rivers and streams as the snow melts. 
In areas with a lot of accumulated snow, the amount of snow covering the ground gradually decreases as the weather warms up. This melting process can take several months. How quickly the snow disappears depends on the landscape. For example, in mountainous ecosystems, factors like elevation, slope aspect, slope gradient, and [forest structure](https://repository.library.noaa.gov/view/noaa/45144){target="_blank"} affect how the snow can accumulate, evaporate or sublimate and how quickly the snow melts. 

For mountain areas, similar patterns of depletion occur from year to year and can be related to the snow water equivalent (SWE) at a site, accumulated ablation, accumulated degree-days, or to runoff from the watershed using depletion curves from historical data. [Here](https://www.mdpi.com/2076-3263/8/12/484#:~:text=Snow%20depletion%20curves%20(SDC)%20are,snow%20depth%20or%20water%20equivalent){target="_blank"} is an example of snow depletion curves developed using statistical modeling and remotely sensed data. The use of remotely sensed data can be incredibly helpful to providing estimates in remote areas with infrequent measurements. 
Observing depletion patterns may not be effective in ecosystems where patterns are more variable (e.g., prairies). However, stratified sampling with snow surveys, snow telemetry networks (SNOTEL) or continuous precipitation measurements can be used with techniques like [cluster analyses](https://hess.copernicus.org/articles/27/3525/2023/){target="_blank} or interpolation, to determine variables that influence SWE and estimate SWE depth or runoff over heterogeneous systems. 

You can further explore all readings linked in the above section. These readings may assist in developing the workflow for your term project, though they are optional for completing this assignment. However, it is recommended that you review the figures to grasp the concepts and retain them for future reference if necessary.

#### Model choices: My snow is different from your snow 

When determining the architecture of your snow model, your model choices will reflect the application of your model and the processes you are trying to represent. Recall that parsimony and simplicity often make for the most effective models. So, how do we know if we have a good model? Here are a few things we can check to validate our model choices:

**Model Variability:** A good model should produce consistent results when given the same inputs and conditions. Variability between model runs should be minimal if the watershed or environment is not changing. 

**Versatility:** Check the model under a range of scenarios different from the conditions under which it was developed. The model should apply to similar systems or scenarios beyond the initial scope of development

**Sensitivity Analysis:** We reviewed this a bit in the Monte Carlo module. How do changes in model inputs impact outputs? A good model will show reasonable sensitivity changes in input parameters, with outputs responding as expected. 

**Validation with empirical data:** Comparison with real-world data checks whether the model accurately represents the actual system

**Applicability and simplicity:** A good model should provide valuable insights or aid in decision-making processes relevant to the problem it addresses. It strikes a balance between complexity and simplicity, avoiding unnecessary intricacies that can lead to overfitting or computational inefficiency while sufficiently capturing the system's complexities. 

### Labwork (20 pnts)

#### [Download the repo for this lab HERE](https://github.com/tpcovino/06_snowmelt_models.git){target="_blank"}

In this module, we will simulate snowmelt in a montane watershed in central Colorado with a simple temperature model. The Fool Creek watershed is located in the Fraser Experimental Forest (FEF), 137 km west of Denver, Colorado. The FEF contains several headwater streams (Strahler order 1-3) within the Colorado Headwaters watershed which supplies much of the water to the populated Colorado Front Range. This Forest has been the site of many studies to evaluate the effects of land cover change on watershed hydrology. The Fool Creek is a small, forested headwater stream, with an approximate watershed area of 2.75 km^2. The watershed elevation ranges from approximately 2,930m (9,600ft) at the USFS monitoring station to 3,475m (11,400ft) at the highest point. There is a SNOTEL station located at 3,400m. 

We will refer to data from the SNOTEL station as 'high elevation' or 'upper watershed' data. Data collected from the outlet may be referred to as 'low elevation', 'lower watershed' or 'outlet' data.

![Fool Creek delineated watershed](images/fool_studyarea.png)

Let's look at some data for Fool Creek:



This script collects [SNOTEL](https://www.nrcs.usda.gov/wps/portal/wcc/home/aboutUs/monitoringPrograms/automatedSnowMonitoring/){target="_blank"} input data using snotelr. The SNOTEL automated collection site in Fool Creek supplies SWE data that can simplify our runoff modeling. Lets explore the sites available through snotelr and select the site of interest.



#### Import data: 
First, we will generate a conceptual runoff model using the date, daily mean temperature, SWE depth in mm, and daily precipitation (mm) data from the SNOTEL station. We will use select() to keep the desired columns only. Then we will use mutate() to add a water year, and group_by() to add a cumulative precipitation column for each water year. 



We will also download flow data collected by USFS at the Fool Creek outlet at 2,930m to compare to simulated runoff. 



Let's look at the data for the SNOTEL station at 3400m in elevation.



#### Calculate liquid input 
to the ground by analyzing the daily changes in Snow Water Equivalent (SWE) and precipitation. This script incorporates temperature conditions to determine when to add changes to liquid input.





Let's visualize the timing disparities between precipitation and melt input in relation to discharge.





#### Modeling SWE 
Now we are going to shift focus. Instead of modeling runoff, let’s assume we have precipitation data from 3400m (SNOTEL, upper watershed) in the Fool Creek watershed, but no SWE data. How could we model SWE using temperature data?

Next, we will create a model to estimate SWE and then compare the results to the actual SWE data from the SNOTEL station in the upper Fool watershed.



The model below is a variation of the [degree-day method](https://www.researchgate.net/publication/229698275_Revisiting_the_Degree-day_Method_for_Snowmelt_Computations){target="_blank"}.
In this simulation, temperature determines whether precipitation contributes to snowpack or becomes rain. When temperatures fall below a certain threshold and precipitation occurs, that amount is added to the snow water equivalent (SWE) accumulation.

To find the optimal parameters for the upper watershed, we will run 100 Monte Carlo simulations and analyze the results. The key parameters we are testing include: <br>
Pack threshold – The SWE level at which melting begins. <br>
Degree-day factor – The rate at which snow melts per degree above freezing. <br>
Threshold temperature – The temperature that separates snow from rain.<br>
By adjusting these parameters, we aim to refine our model and improve its accuracy in simulating snowpack dynamics. 





This simple model seems to simulate SWE well if assessed with the NSE. 

Let's go through the procedure again, but this time we'll model SWE at the Fool Creek outlet (lower elevation), where we (really) don't have daily SWE data. First, we’ll import the precipitation data collected from a USFS meteorological station near the Fool Creek outlet.



We also need temperature data. This is also collected at the USFS Lower Fool meteorological station. 



Even though this meteorological station is fairly close to the SNOTEL station, the sites differ in elevation. Lets compare the observed cumulative precipitation between the upper watershed and the lower.



Now let's estimate SWE for the Lower Fool Creek.
Again, we'll just simulate a single year.



Run another set of simulations and compare the values of the best performing parameters across sites.



Since we don't have SWE measurement for Lower Fool Creek, let's see how the simulated values for the lower watershed compare to the observed SWE from the high elevation SNOTEL station.





