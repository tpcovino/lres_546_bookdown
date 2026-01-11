## Monte Carlo Simulation 

### Learning Module 5
#### Background:

Monte Carlo Simulation is a method to estimate the probability of the outcomes of an uncertain event. It is based on a law of probability theory that says if we repeat an experiment many times, the average of the results will get closer to the true probability of those outcomes. 

First check out this video:
<iframe src="https://www.youtube.com/embed/7TqhmX92P6U" width="672" height="400px" data-external="1"></iframe>

#### Reading
Then read this: https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2924739/ for a understanding of the fundamentals.

#### How does this apply to hydrological modeling?

When modeling watershed hydrological processes, we often attempting to quantify<br><br>

  * watershed inputs <br>
      + e.g., precipitation <br>
  - watershed outputs <br>
      - e.g., [evapotranspiration](https://www.usgs.gov/special-topics/water-science-school/science/evapotranspiration-and-water-cycle#:~:text=Evapotranspiration%20includes%20water%20evaporation%20into,to%20the%20atmosphere%20via%20plants), [sublimation](https://www.usgs.gov/media/images/sublimation-occurs-snow-covered-mountains), runoff/discharge <br>
  - watershed storage <br>
      - precipitation that is not immediately converted to runoff or ET rather stored as snow or subsurface water. <br>

Imagine we are trying to predict the percentage of precipitation stored in a watershed after a storm event. We have learned that there are may factors that affect this prediction, like antecedent conditions, that may be difficult to measure directly. Monte Carlo Simulation can offer a valuable approach to estimate the probability of obtaining certain measurements when those factors can not be directly observed or measured. We can approximate the likelihood of specific measurement by simulating a range of possible scenarios. <br>

Monte Carlo Simulation is not only useful for estimating probabilities, but for conducting sensitivity analysis. In any model, there are usually several input parameters. Sensitivity analysis helps us understand how changes in these parameters affect the predicted values. To perform a sensitivity analysis using a Monte Carlo Simulation we can:

  - Define the realistic ranges for each parameter we want to analyze
  - Using Monte Carlo Simulation, randomly sample values from the defined ranges for each parameter
  - Analyze output to understand how different input sample values affect the predicted output

#### Example

Let's consider all of this in an example model called [WECOH - Watershed ECOHydrology](https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/2014WR016719). In this study, researchers (Nippgen et al.) were interested in the change in subsurface water storage through time and space on a daily and seasonal basis.

![Evolution of watershed connectivity](./images/wrcr21497-fig-0009-m.jpg){width=50%}

<br/><br/>
The authors directly measured runoff/discharge, collected precipitation data at several points within the watershed, used remote sensing to estimate how much water was lost to evapotranspiration, and used digital elevation models to characterize the watershed topography. As we learned in the hydrograph separation module, topographic characteristics can have a significant impact on storage. <br>
Though resources like [USDA's Web Soil Survey](https://websoilsurvey.sc.egov.usda.gov/app/) can provide a basic understanding underlying geology across a large region, characterizing the heterogeneous nature of soils within a watershed can be logistically unfeasible. To estimate the soil characteristics like storage capacity (how much water the soil can hold) and hydraulic conductivity (how easily water can move through soil) in the study watershed, the authors used available resources to determine the possible range of values for each of their unknown parameters. They then tested thousands of model simulations using randomly selected values with the predetermined range for each of the soil characteristics. They compared the simulated discharge from these simulations to the actual discharge measurements. The simulations that predicted discharge patterns that closely matched reality helped them to estimate the unknown soil properties. Additionally, from the results of these simulations, they could identify which model inputs had the most significant impact on the discharge predictions, and how sensitive the output was to changes in each parameter. In this case, they determined that the model and system were most sensitive to precipitation. This type of sensitivity analysis can help us interpret the relative importance of different parameters and understand the overall sensitivity of the model or system. The study is linked for your reference but a thorough reading is not required. 

#### Optional Reading
[This methods paper](https://www.researchgate.net/publication/262765548_Development_of_probability_distributions_for_urban_hydrologic_model_parameters_and_a_Monte_Carlo_analysis_of_model_sensitivity) by Knighton et al. is an example of how Monte Carlo Simulation was used to estimate hydraulic conductivity in an urban system with varied land-cover..

#### Generate a simulation with code 

For a 12-minute example in RStudio, check this out. If you are still learning the basics of R functionality, it may be helpful to code along with this video, pausing as needed. Note that this instruction is coding in an Rscript (after opening RStudio > File > New File > R Script), rather than an Rmarkdown that we use in this class. 

<iframe src="https://www.youtube.com/embed/inJsu-ygBfM" width="672" height="400px" data-external="1"></iframe>

### Repo link
[Download the repo for this lab HERE](https://github.com/tpcovino/05_monte_carlo.git){target="_blank"}

### Labwork (20 pnts)

In this lab/homework, you will use the transfer function model from the previous module for some sensitivity analysis using Monte Carlo simulations. The code is mostly the same as last module with some small adjustments to save the parameters from each Monte Carlo run. 
In this Monte Carlo simulation, we're testing how different parameter values affect simulated streamflow (discharge). Since we don’t know the true values of certain model parameters (like b1, b2, b3, a, and b that you may recall from the transfer function), we randomly sample them within a reasonable range and run the model many times to see which parameter combinations best match observed streamflow data. Each iteration of the loop represents one possible "guess" at the true parameter values, and we evaluate how good that guess is using metrics like Nash-Sutcliffe Efficiency (NSE), Kling-Gupta Efficiency (KGE), Root Mean Square Error (RMSE), and Mean Absolute Error (MAE).
 After the completion of the MC runs, we will use the Generalized Likelihood Uncertainty estimation (GLUE) method to evaluate parameter sensitivity. In other words, we sort all the runs to find the best-fitting parameter combinations, which helps us understand the most likely values of these unknown parameters in our hydrological model.
 
 There are three main objectives for this homework:  
1) Set up the Monte Carlo analysis<br>
2) Run the MC simulation for ONE of the years in the study period and perform a GLUE sensitivity analysis  
3) Compare the different objective functions.

A lot of the code in this homework will be provided.

#### Setup
Import packages, including the "progress" and "tictoc" packages. These will allow us to time our loops and functions



Read data - this is the same PQ data we have worked with in previous modules.



Define variables



#### Parameter initialization
This chunk has two purposes. The first is to set up the number of iterations for the Monte Carlo simulation. The entire model code is essentially wrapped into the main MC for loop. Each iteration of that loop is one full model realization: loss function, TF, convolution, model fit assessment (objective function). For each model run (each MC iteration), we will save the model parameters and respective objective functions in a dataframe. This will be the main source for the GLUE sensitivity analysis at the end. The model parameters are sampled in the main model chunk, this is just the preallocation of the dataframe.  
You will both run your own MC simulation, to set up the code, but will also receive a .Rdata file with 100,000 runs to give you more behavioral runs for the sensitivity analysis and uncertainty bounds. As a tip, while setting up the code, I would recommend setting the number of MC iterations to something low, for example 10 or maybe even 1. Once you have confirmed that your code works, crank up the number of iterations. Set it to 1000 to see how many behavioral runs you get. After that, load the file with the provided runs. 



#### MC Model run
This is the main model chunk. The tic() and toc() statements measure the execution time for the whole chunk. There is also a progressbar in the chunk that3 will run in the console and inform you about the progress of the MC simulation. The loss function parameters are set in the loss function, the TF parameters in the loss function code. For each loop iteration, we will store the parameter values and the simulated discharge in the "param" dataframe. So, if we ran the MC simulation 100 times, we would end up with 100 parameter combinations and simulated discharges.



**Q1 (2 pt) How are the loss function and transfer function parameters being sampled? That is, what is the underlying distribution and why did we choose it? (2-3 sentences)**  
ANSWER: 


**Extra point: What does the while loop do in the transfer function calculation? And why is it in there? (1-2 sentences)**  
ANSWER:  



Save or load data

After setting up the MC simulation, we will actually use a pre-created dataset. Running the MC simulation tens of thousands of times will take multiple hours. For that reason, we have done this for you and saved it as an importable data set.



Best run

Now that we have the dataframe with all parameter combinations and all efficiencies, we can plot the best simulation and compare it to the observed discharge


### Sensitivity analysis

We will use the GLUE methodology to assess parameter sensitivity. We will use GLUE for two purposes: 
1) Assessing parameter sensitivity – Understanding how different parameter values influence model performance
2) Creating an envelope of model simulations – Identifying a range of possible model outputs that fit the observed data 

To start, we need to format the data so we can create dotty plots and density plots using Nash-Sutcliffe Efficiency (NSE) as our performance metric.

When plotting, each parameter should be displayed in its own panel. You can achieve this using facet_wrap() in ggplot2, and be sure to set the axis scaling to "free" so each parameter is properly visualized



**Q2 (4 pts) Describe the sensitivity analysis with the three different plots. Are the parameters sensitive? Which ones are, which ones are not? Does this affect your "trust" in the model? (5-8 sentences)**    
ANSWER: 



**Q3 (4 pts) What are the differences between the dotty plots and the density plots? What are the differences between the two density plots? (2-3 sentences)**  ANSWER: 
 

Uncertainty bounds
In this section, we will generate uncertainty bounds for our simulation results. Instead of using all model runs, we will only include the behavioral runs—the ones that meet our performance criteria. This ensures that our uncertainty range reflects only the most realistic simulations.



**Q4 (3 pts) Describe what the envelope actually is. Could we say we are dealing with confidence or prediction intervals? (2-3 sentences)**   
Hint: Think about what the envelope is capturing. Does it reflect uncertainty in the model structure and parameters or does it describe the variability in future observations?
ANSWER: 



**Q5 (3 pts) If you inspect the individual model runs (in the Qruns df), you will notice that they all perform somewhat poorly when it comes to the initial baseflow. Why is that and what could you do to change this? (Note: you don't have to actually do this, just describe how you might approach that issue (2-3 sentences)**  
Hint: Consider factors such as parameter initialization, storage effects, or missing processes in the model structure. What tuning or modifications could address this? 
ANSWER: 


**Q6 (4 pts) Run the provided code to generate a plot comparing the best model run for each objective function. Then, analyze the differences in model performance by answering the following:**
Which objective function provides the best match to observed runoff overall?
How do the different functions perform in capturing peak flow events?
Which function best represents baseflow conditions? Which struggles the most?
If you were to prioritize accuracy in low-flow conditions, which objective function might you choose and why?** 
ANSWER: 






The coding steps are: 1) get the simulated q vector with the best run for each objective function, 2) put them in a dataframe/tibble, 3) create the long form, 4) plot the long form dataframe/tibble. These steps are just ONE possible outline how the coding steps can be broken up.


Response surface


