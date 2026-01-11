## Rational method and NRCS curve number (20 pts)

The repo for this module can be found [here](https://github.com/tpcovino/03_rational_and_CN_methods.git){target="_blank"}

### Learning Module 3

#### Background information - Rational Method

The **Rational Method** is a type of simple hydrological analysis used to estimate the *peak runoff rate* from a small watershed during a rainfall event. It is particularly useful for estimating the amount of water that will flow through a particular area during a storm, like a drainage system or culvert. Despite the existence of more advanced methods, this approach remains widely used in practice for its simplicity and ability to provide quick approximations, making it invaluable for preliminary assessments and as a foundation for understanding more complex hydrological and environmental modeling techniques.

Here is a 5-minute video to get started: 

<iframe src="https://www.youtube.com/embed/brNpLh21UCg" width="672" height="400px" data-external="1"></iframe>

#### Reading - Rational Method.<br> 
Read at least sections 2 and 3 to for the formula and an example <br> 

- [The Rational Method](https://www.waternz.org.nz/Attachment?Action=Download&Attachment_id=846){target="_blank"}

Some helpful terminology:

<u>runoff coefficient</u> - represents how much rainfall actually becomes runoff<br>
<u>time of concentration</u> - the time it takes for some mass of precipitation to travel from the most remote point in a watershed to the outlet or point of interest. e.g., how long it takes a drop of rain to reach a culvert after it falls to the ground. 

#### Background information - Curve Number

The NRCS (Natural Resources Conservation Science) **curve number** (CN) is a tool used to estimate the *total runoff volume* of water that will run off an ungaged watershed during a storm event. The curve number is based on soil type, land use and antecedent moisture conditions. You may also see SCS CN in texts. NRCS was previously known as Soil Conservation Science, they are the same.
It was designed as a simple tool to describe typical watershed response from infrequent rainfall anywhere in the US for watersheds with the same soil type, land use, and surface runoff conditions. The CN method is a single event model to estimate of runoff volume from rainfall events (not peak discharge or a hydrograph). 

To understand the function and derivation of the CN number, let's start with the <br>
the NRCS runoff equation:

$$
Q = \frac{{(P - I_{a})^2}}{{P - I_{a} + S}}
$$
Where Q = runoff(in) <br>
P = rainfall (in) <br>
S = potential maximum retention after runoff begins
I<sub>a</sub> = initial abstraction (initial amount of rainfall that is intercepted by the watershed surface and does not immediately contribute to runoff)

I<sub>a</sub> is assumed to reduce to 0.2S based on empirical observations by NRCS. If:
$$
S = \frac{{1000}}{{CN}} - 10
$$
the runoff equation therefore reduces to:
$$
Q = \frac{{[P - 0.02\left(\frac{{1000}}{{CN}} - 10\right)]^2}}{{P + 0.8\left(\frac{{1000}}{{CN}} - 10\right)}}
$$

#### Reading - Curve numbers<br> 

Curve Number selection tables are available from the
[US Army Corps.](https://www.hec.usace.army.mil/confluence/hmsdocs/hmstrm/cn-tables){target="_blank"}

Slides on selecting curve number start around slide 8. <br>
- [Link](https://cdn.serc.carleton.edu/files/geoinformatics/steps/presentation_selecting_cn_vari.pdf){target="_blank"}

#### Reading - Supporting material

Time of concentration. Up to "other considerations", pages 15-1 to 15-9. <br>
- [Link](https://irrigationtoolbox.com/NEH/Part630_Hydrology/NEH630-ch15draft.pdf){target="_blank"}

We will use the Kirpich method to calculate the time of concentration. Here is the citation for your reference.

Kirpich, Z.P. (1940). "Time of concentration of small agricultural watersheds". Civil Engineering. 10 (6): 362.

### Labwork (20 pts)

In this module, you will apply the Rational Method and the SCS curve number (CN) method to estimate peak flows and effective rainfall/runoff volumes. This lab also introduces two coding techniques; for-loops and functions.

This is knitr settings. Knitr is a package that will turn this Rmd to an html. 

``` r
knitr::opts_chunk$set(echo = TRUE, warning = FALSE, message = FALSE, results = FALSE)
```

Packages



#### Part I - Rational Method
The goal is to calculate peak runoff (cfs) for a small 280-acre rangeland watershed near Bozeman for multiple events with different return periods. You will first have to calculate the time of concentration and then look up the the rainfall values for the different return intervals. The longest flowpath in the watershed is 6300 ft long, average watershed slope is 1.95%. Look at this [table](https://www.dropbox.com/scl/fi/tkofgmpnroug8wwe8x16z/Rational.Method.C.png?rlkey=qgcyayk10j94is3hwjfxuzwz5&dl=0) to select C.

##### Time of Concentration

- Time of concentration is the time it takes water to travel along the longest flowpath in the watershed and exit the watershed. 



tc in this example should be ~ 29.91. If your calculated value is very different, start by checking slope value (S). For the sake of simplicity in next steps, do not change the name of variables.

##### Storm Depths
Now that you have the time of concentration, you need to find the corresponding 1, 2, 5, 10, 25, 50, and 100 year storm depths for a duration that works for the Rational Method in that particular watershed (that is, the duration that is closest to tc). Create a dataframe (or 'tibble' which is the tidyverse data frame) called "storms" that has a column for the return period, Tr, the storm depth, Pin, and the average storm intensity over ONE HOUR, Pin_hr. Typically hourly depths may be determined from a rainfall analysis. For the sake of the assignment, approximate daily depths corresponding to appropriate frequencies are provided here for Bozeman, MT. 
To obtain 1 hour depths by dividing daily depth by 24. 



##### Example for-loop
Now that we have the rainfall intensities, we need to set up a way that calculates Qp for each of those intensities, without us having to go back and manually enter them each time. We do this with a for-loop. Let's first look at how for loops work in R. 

A for-loop will execute the code inside it for a specified number of iterations. The structure looks like:
for(i in some number of iterations) {
  output <- some code/function that needs to be executed
  } 
  In the chunk below, we create the vector x, which contains 6 values (0, 2, 4, 6, 8, 10). Each loop then executes a calculation using each value in 'x' in sequence. 
  In a for-loop, 'i' is called the loop index or iterator. It has a new value in each iteration of the loop. You can think of i as a 'counter' that helps the loop keep track of its progress. In the example below, we want to run the code in the loop for every value in the vector 'x'. Since there are 6 values in vector 'x', then we will tell the loop to run 6 times. However, rather than for (i in 1:6), using 1:length(x) ensures the loop adapts to the size of x, no matter how many values it contains. This improves the flexibility of our code.
  We also need to store the output of loop during each iteration. If we don't store the results of each iteration or loop, the loop will overwrite the output each time, leaving us with only the result from the final iteration. To store the output of each loop iteration, we have to preallocate a vector (like y) to store all of the outputs. In other words, we are creating an empty vector with the same length as 'x' for the for-loop to add values to.



We have created a vector x from 0 to 10 in increments of 2. The for-loop takes each element of that vector and squares it. The "i" is called an index and runs from 1 through 6 (the length of x). During the first iteration i is 1, during the second iteration i is 2, and so on. We are then writing the results from the calculation into a new vector, y. When i is 1, we are squaring the first value in vector x, which is 0. The first value in y will be zero as well. When i is 2, the second value in x gets squared, which is 2^2. The second value in y is going to be 4.

##### Calculate Qp with for-loop
Now let's set up the for-loop for the Rational Method. We need to set up a for-loop that does the same calculation 7 times, the number of precip values in "storms". However, 'length()' for a dataframe returns the number of columns, so you want to use 'nrow()' when referencing a dataframe. (As apposed to using length() for a vector as above). The calculated peak discharges should go into a new column of our "storms" df. However, indexing is slow for dataframes, so we will write the new values into a new vector, Qp, and then after the loop insert the vector as a column into the dataframe.<br>
**If you are using the complete version of the assignment, do not assume that this C value is correct**



##### Plot Tr and Qp
Now plot the return interval against the storm peakflow. You only need three to four lines for this: 1st sets up the data, 2nd defines the theme that removes the gray background and sets the axes labels to a proper size, 3rd plots the data with geom_points, 4th makes the axes labels with labs. Note that axis labels have units. This is an important feature when communicating your findings.



**1. (1 pt) What does the C in the Rational Method do?**  
ANSWER: 

**2. (2 pt) What is the the time of concentration and why does it need to be taken into account for the Rational Method? What is a common issue among many tc methods?**  
ANSWER: 

#### Part II - NRCS CN
In this exercise we will write a function that takes the necessary inputs for the NRCS CN method and returns a value based on the parameters. This is not fully automated, you will still have to look up the CN yourself for a given land use. The goal is to write a function that requires P, CN, and AMC (antecedent moisture condition) as an input in order to calculate Q.

##### AMC Table
#### Function example
Let's look at a simple example for a function. Assume we want a number with an exponent and we want to be able to choose both the base and the exponent. The function 'function()' defines the inputs in (), the actual calculation then follows in {}.



Run  the above function. You will notice that a function was added to the Global Environment all the way at the bottom. Now let's test the function with a base of 2 and an exponent of 3. This should perform the calculation 2x2x2 = 8



##### NRCS CN test
Before we write a function to estimate Q using CN, let's make sure we can set up the correct steps WITHOUT the function. This is almost always a helpful step in developing functions. It can save hours of headaches later. Let's try P = 2.5 inches, CN = 90, and AMC = 3.
The biggest problem here is going to be creating a lookup table for the AMC. I will provide the basic structure of the code for this.


Sanity check: Q should ~ 2.0 for the specified parameters.

**3. (4 pt) What is one of the critical assumptions for the SCS CN method? (1-2 sentences). Why is it important to understand assumptions of models like this one?**  
ANSWER:

Now write a function called "scs_cn" that takes the inputs P, CN, and AMC (all numeric) and returns Q, Ia, Si, and the RR as a df called "scs_out".
Test the function for P = 2.5 in, CN = 90, AMC = 3.



**4. (4 pts) Describe what factors into the curve number (and most notably what is missing). What does it mean when the CN is 100 or 0?**  
ANSWER: 

**5. (3 pts) What is the difference between a deterministic and a stochastic model? Describe one example each.**  
ANSWER:

**6. (6 pts)(4-6 sentences) Let's consider the cross-disciplinary relevance of hydrological modeling. You may not be studying hydrology directly, but environmental resource managers often need to estimate peak flows. Why? (1-2 sentences). In a sentence, what is your specific area of interest, or the topic of your professional paper, if known. Consider broader implications of the hydrologic cycle, like water quality, nutrient or pollution transport, or erosion. Does rainfall/runoff management affect your niche? If so, how? (2-4 sentences)**
ANSWER:
