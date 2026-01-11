# Process-Based Modeling - Probabilistic and Process Simulation
\* Modules 3.1 and 3.2 are adapted from Fabian Nipggen (REWM.4500.500)\

## Transfer function rainfall-runoff models 

### Learning Module 4

#### Summary

In previous modules, we explored how watershed characteristics influence the flow of input water through or over hillslopes to quickly contribute to stormflow or to be stored for later contribution to baseflow. Therefore, the partitioning of flow into baseflow or stormflow can be determined by the time it spends in the watershed. Furthermore, the residence time of water in various pathways may affect weathering and solute transport within watersheds. To improve our understanding of water movement within a watershed, it can be crucial to incorporate water transit time into hydrological models. This consideration allows for a more realistic representation of how water moves through various storage compartments, such as soil, groundwater, and surface water, accounting for the time it takes for water to traverse these pathways. 

In this module, we will model the temporal aspects of runoff response to input using a transfer function. First, please read: <br/>

[TRANSEP - a combined tracer and runoff transfer function hydrograph separation model](docs/Weiler_etal_WRR2003.pdf)
<br/>

Then this chapter will step through key concepts in the paper to facilitate hands-on exploration of the rainfall-runoff portion of the TRANSEP model in the assessment. Then, we will introduce examples of other transfer functions to demonstrate alternative ways of representing time-induced patterns in hydrological modeling, prompting you to consider response patterns in your study systems. 


#### Overall Learning Objectives
At the end of this module, students should be able to describe several ways to model and identify transit time within hydrological models. They should have a general understanding of how water transit time may influence the timing and composition of runoff. 


#### Terminology
In modeling a flow system, note that consideration of time may vary depending on the questions being asked. **Transit time** is the average time required for water to travel through the entire flow system, from input (e.g., rainfall on soil surface) to output (e.g., discharge). **Residence time** is a portion of transit time, describing the amount of time water spends within a specific component of the flow system, like storage (e.g., in soil, groundwater, or a lake). 

![Figure 6.3. Conceptual diagram of the lumped parameter transit time modeling approach (McGuire & McDonnell, 2006)](https://ars.els-cdn.com/content/image/1-s2.0-S0022169406002150-gr1.jpg)

<br/><br/>

A **transfer function** (TF) is a mathematical representation of how a system responds to input signals. In a hydrological context, it describes the transformation of inputs (e.g. precipitation) to outputs (e.g. runoff). These models can be valuable tools for understanding the time-varying dynamics of a hydrological system.

#### The Linear Time-Invariant TF

We'll begin the discussion in the context of a **linear reservoir**. Linear reservoirs are simple models designed to simulate the storage and discharge of water in a catchment. These models assume that the catchment can be represented as single storage compartments or as a series of interconnected storage compartments and that the change the amount of water stored in the reservoir (or reservoirs) is directly proportional to the inflows and outflows. In other words, the linear relationship between inflows and outflows means that the rate of water release is proportional to the amount of water stored in the reservoir. 

![](images/linear_storage_04.png)

<br/><br/>

##### The Instantaneous Unit Hydrograph: 

The Instantaneous Unit Hydrograph (IUH) represents the linear rainfall-runoff model used in the TRANSEP model. It is an approach to hydrograph separation that is useful for analyzing the temporal distribution of runoff in response to a 'unit' pulse of rainfall (e.g. uniform one-inch depth over a unit area represented by a unit hydrograph). In other words, it is a hydrograph that results from one unit (e.g. 1 mm) of effective rainfall uniformly distributed over the watershed and occurring in a short duration. Therefore, the following assumptions are made when the IUH is used as a transfer function: <br/>
1. the IUH reflects the ensemble of watershed characteristics <br/>
2. the shape characteristics of the unit hydrograph are independent of time <br/>
3. the output response is linearly proportional to the input 

<br/><br/>

![Chow, V.T., Maidment, D.R. and Mays, L.W. (1988) Applied Hydrology. International Edition, McGraw-Hill Book Company, New York.](images/linear_vs_IUH.png)
<br/><br/>

By interpreting the IUH as a transfer function, we can model how the watershed translates rainfall into runoff. In the TRANSEP model, this transfer function is represented as \(g(\tau)\) and thus the rainfall-induced response to runoff. 

$$
g(\tau) = \frac{\tau^{\alpha-1}}{\mathrm{B}^{\alpha}\Gamma(\alpha)}exp(-\frac{\tau}{\alpha})
$$
<br/><br/>

The linear portion of the TRANSEP model describes a convolution of the effective precipitation and a runoff transfer function. 

$$ 
Q(t)= \int_{0}^{t} g(\tau)p_{\text{eff}}(t-\tau)d\tau
$$
<br/><br/>
Whoa, wait...what? Tau, integrals, and convolution? Don't worry about the details of the equations. Check out this video to have convolution described using dollars and cookies, then imagine each dollar as a rainfall unit and each cookie as a runoff unit. Review the equations again after the video.

<br/><br/>


``` r
knitr::include_url("https://www.youtube.com/embed/aEGboJxmq-w")
```

<iframe src="https://www.youtube.com/embed/aEGboJxmq-w" width="672" height="400px" data-external="1"></iframe>

##### The Loss Function: 

The loss function represents the linear rainfall-runoff model used in the TRANSEP model.

$$
s(t) = b_{1} p(t + 1 - b_{2}^{-1}) s(t - \triangle t) 
$$
$$
s(t = 0) = b_{3} 
$$
$$
p_{\text{eff}}(t) = p(t) s(t) 
$$
where \(p_{\text{eff}}(t)\) is the effective precipitation.<br/>
\(s(t)\) is the antecedent precipitation index which is determined by giving more importance to recent precipitation and gradually reducing that importance as we go back in time. <br/>
The rate at which this importance decreases is controlled by the parameter \(b_{2}\). <br/>
The parameter \(b_{3}\) sets the initial antecedent precipitation index at the beginning of the simulated time series. 

In other words, these equations are used to simulate the flow of water in a hydrological system over time. The first equation represents the change in stored water at each time step, taking into account precipitation, loss to runoff, and the system's past state. The second equation sets the initial condition for the storage at the beginning of the simulation. The third equation calculates the effective precipitation, considering both precipitation and the current storage state.

##### How do we code this?

We will use a skeletal version of TRANSEP, focusing only on the rainfall-runoff piece which includes the loss-function and the gamma transfer function. 

We will use rainfall and runoff data from TCEF to model annual streamflow at a daily time step. Then we can use this model as a jump-off point to start talking about model calibration and validation in future modules.

##### Final thoughts:

If during your modeling experience, you find yourself wading through a bog of complex physics and multiple layers of transfer functions to account for every drop of input into a system, it is time to revisit your objectives. Remember that a model is always 'wrong'. Like a map, it provides a simplified representation of reality. It may not be entirely accurate, but it serves a valuable purpose. Models help us understand complex systems, make predictions, and gain insights even if they are not an exact replica of the real world. Check out this paper for more:

https://agupubs.onlinelibrary.wiley.com/doi/10.1029/93WR00877

### Repo link
[Download the repo for this lab HERE](https://github.com/tpcovino/04_transfer_functions.git){target="_blank"}

### Labwork (20 pts)

In this homework/lab, you will write a simple, lumped rainfall-runoff model. The foundation for this model is the TRANSEP model from Weiler et al., 2003. Since TRANSEP (tracer transfer function hydrograph separation model) contains a tracer module that we don't need, we will only use the loss function (Jakeman and Hornberger) and the gamma transfer function for the water routing.  
The data for the model is from the Tenderfoot Creek Experimental Forest in central Montana.  

Load packages with a function that checks and installs packages if needed:







Define input year
We could use every year in the time series, but for starters, we'll use 2006. Use filter() to extract the 2006 water year.



**1) QUESTION: What does the function cumsum() do?(1 pt)** 
ANSWER:

Define the initial inputs <br><br>
This chunk defines the initial inputs for measured precip and measured runoff.



Parameterization <br><br>
We will use these parameters. You can change them if you want, but I'd suggest leaving them like this at least until you get the model to work. Even tiny changes can have a huge effect on the simulated runoff.



Loss function <br><br>
This is the module for the Jakeman and Hornberger loss function where we turn our measured input precip into effective precipitation (p_eff). This part contains three steps.  
1) preallocate a vector p_eff: Initiate an empty vector for effective precipitation that we will fill in with a loop using Peff(t) = p(t)s(t). Effective precipitation is the portion of precipitation that generates streamflow and event water contribution to the stream. It is separated to produce event water and displace pre-event water into the stream. 
2) set the initial value for s: s(t) is an antecedent precipitation index. How much does antecedent precipitation affect effective precipitation?
3) generate p_eff inside of a for-loop  
Please note: The Weiler et al. (2003) paper states that one of the loss function parameters (vol_c) can be determined from the measured input. That is actually not the case.

s(t) is the antecedent precipitation index that is calculated by
exponentially weighting the precipitation backward in time according to the 
parameter b2 is a 'dial' that places weight on past precipitation events. 



**2) QUESTION (3 pts): Interpret the two figures. What is the meaning of "frac"?** 
For this answer, think about what the effective precipitation represents. When is frac "high", when is "frac" low? 

ANSWER: 

**Extra Credit (2 pt): Combine the two plots into one, with a meaningfully scaled secondary y-axis**  

Runoff transfer function<br><br>
Routing module -

This short chunk sets up the TF used for the water routing. This part contains only two steps:
1) the calculation of the actual TF.
2) normalization of the TF so that the sum of the TF equals 1.

tau(0) is the mean residence time 



**3) QUESTION (2 pt): Why is it important to normalize the transfer function?**  
ANSWER: 

**4) QUESTION (4 pt): Describe the transfer function. What are the units and what does the shape of the transfer function mean for runoff generation?**  
ANSWER: 

Convolution<br><br>

This is the heart of the model. Here, we convolute the input with the TF to generate runoff. There is another thing you need to pay attention to: We are only interested in one year (365 days), but since our TF itself is 365 timesteps long, small parts of all inputs except for the first one, will be turned into streamflow AFTER the water year of interest, that is, in the following year. In practice, this means you have two options to handle this.  
1) You calculate q_all and then manually cut the matrix/vector to the correct length at the end, or  
2) You only populate a vector at each time step and put the generated runoff per iteration in the correct locations within the vector. For this to work, you would need to trim the length of the generated runoff by one during each iteration. This approach is more difficult to code, but saves a lot of memory since you are only calculating/storing one vector of length 365 (or 366 during a leapyear).  
We will go with option 1). The code for option 2 is shown at the end for reference.  

Convolution summarized (recall dollars and cookies): Each loop iteration results in a row of the matrix representing the convolution at a specific time step. each time step of p_eff is an iteration, and for each timestep, it multiplies the effective precipitation at that timestep by the entire transfer function. Then each row is summed and stored in the vector q_all. q_all_loop is an intermediate step in the convolution process and can help visualize how the convolution evolves over time. 
As an example, if we have precip at time step 1, we are interested in how this contributes to runoff at time steps 1,2,3 etc, all the way to 365 (or the end of our period). so the first row of the matrix q_all_loop  represents the contribution of precipitation at timestep 1 to runoff at each timestep. the second row represents the contribution of precipitation at time step 2 to runoff at each timestep.  Then when we sum up the rows, we get q_all, where each element represents the total runoff at a specific time step. 



**5) QUESTION (5 pts): We set the TF length to 365. What is the physical meaning of this (i.e., how well does this represent a real system and why)? Could the transfer function be shorter or longer than that?**  

ANSWER:



Plots <br><br>
Plot the observed and simulated runoff. Include a legend and label the y-axis.



**6) QUESTION (3 pt): Evaluate how good or bad the model performed (i.e., visually compare simulated and observed streamflow, e.g., low flows and peak flows).**  
ANSWER:

**7) QUESTION (2 pt): Compare the effective precipitation total with the simulated runoff total and the observed runoff total. What is p_eff and how is it related to q_all? Discuss why there is a (small) mismatch between the sums of p_eff and q_all.**    
ANSWER:  



THIS IS THE CODE FOR CONVOLUTION METHOD 2  
This method saves storage requirements since only a vector is generated and not a full matrix that contains all response pulses. The workflow here is to generate a a response vector for the first pulse. This will take up 365 time steps. On the second time step, we generate another response pulse with 364 steps that starts at t=2. We then add that vector to the first one. On the third time step, we generate a response pulse of length 363 that starts at t=3 and then add it to the existing one. And so on and so forth.


