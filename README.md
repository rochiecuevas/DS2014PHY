# Machine learning using data from the DS2014PHY IRRI rice samples

## Background
The data is publicly available and can be found in the supplementary section of the article "Multivariate-based classification of predicting cooking quality ideotypes in rice (*Oryza sativa* L.) indica germplasm" by Rosa Paula Cuevas (me), Cyril John Domingo, and Nese Sreenivasulu (2018) published in (<a href = "https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6179975/">Rice 11: 56</a>).

In that paper, R was used in data analyses. I used Ward's cluster analysis to classify the rice varieties into quality types. I then used multinomial logistic regression to create a model that can be used to differentiate the different quality types based on the non-collinear variables used to characterise the rice samples. A random forest algorithm was applied to determine the variables that were most important in classifying the rice samples.

## Further data exploration 
I am now exploring the dataset using different approaches. The results, of course, are quite different.

There are 25 continuous variables in the dataset:

<table>
    <tr>
        <th>Variable</th>
        <th>Meaning</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>AC</td>
        <td>Amylose content (%)</td>
        <td>Predicts hardness and stickiness of cooked rice based on the relative concentration of amylose (starch type with straight chains)</td>
    </tr>
    <tr>
        <td>GT_DSC</td>
        <td>Gelatinisation temperature (ºC)</td>
        <td>Indicates the temperature range at which rice begins to cook based on the melting of amylopectin (crystalline starch type with hyperbranched chains)</td>
    </tr>
    <tr>
        <td>PC</td>
        <td>Protein content (%)</td>
        <td>Indicates the relative amount of proteins inside the rice endosperm based on Kjeldahl N measurements</td>
    </tr>
    <tr>
        <td>HRD</td>
        <td>Hardness (g)</td>
        <td>Force required to bite onto a sample, simulated by compression</td>
    </tr>
    <tr>
        <td>ADH</td>
        <td>Adhesiveness (g•sec)</td>
        <td>Degree of stickiness of a sample, simulated by the work required to separate the probe from the base platform</td>
    </tr>
    <tr>
        <td>COH</td>
        <td>Cohesiveness</td>
        <td>Capacity of a sample to remain intact rather than to break during compression</td>
    </tr>
    <tr>
        <td>SPR</td>
        <td>Springiness</td>
        <td>Capacity of a sample to return to its original shape after compression</td>
    </tr>
    <tr>
        <td>SMMAX</td>
        <td>Maximum storage modulus (Pa)</td>
        <td>Maximum elastic response of a sample (solid-like behaviour</td>
    </tr>
    <tr>
        <td>TEMP_SMMAX</td>
        <td>Temperature at maximum storage modulus (ºC)</td>
        <td>Temperature reading when a sample exhibits maximum solid-like behaviour</td>
    </tr>
    <tr>
        <td>TD_SMMAX</td>
        <td>Tan delta at max storage modulus</td>
        <td>Ratio of loss to storage modulus at maximum storage modulus</td>
    </tr>
    <tr>
        <td>LM_SMMAX</td>
        <td>Loss modulus at max storage modulus (Pa)</td>
        <td>Viscous response of a sample at the maximum storage modulus</td>
    </tr>
    <tr>
        <td>TEMP_GELPT</td>
        <td>Temperature at Gel Point (ºC)</td>
        <td>Temperature reading when the loss and the storage moduli are equal (tan delta = 1)</td>
    </tr>
    <tr>
        <td>TROUGH_SM</td>
        <td>Lowest storage modulus (Pa)</td>
        <td>Lowest storage modulus value after reaching the maximum</td>
    </tr>
    <tr>
        <td>SLOPE1_SM</td>
        <td>Increasing storage modulus</td>
        <td>Measured from gel point to SMMAX</td>
    </tr>
    <tr>
        <td>SLOPE2_SM</td>
        <td>Decreasing storage modulus</td>
        <td>Measured from the highest to the lowest points of storage modulus after SMMAX</td>
    </tr>
    <tr>
        <td>SLOPE3_LM</td>
        <td>Increasing loss modulus</td>
        <td>Measured from gel point to maximum loss modulus</td>
    </tr>
    <tr>
        <td>SLOPE4_LM</td>
        <td>Decreasing loss modulus</td>
        <td>Measured from maximum loss modulus until it levelled off</td>
    </tr>
    <tr>
        <td>PV</td>
        <td>Peak viscosity (RVU)</td>
        <td>Highest viscosity recorded as the sample is cooked</td>
    </tr>
    <tr>
        <td>TV</td>
        <td>Trough viscosity (RVU)</td>
        <td>Lowest viscosity recorded as the sample is kept at a high temperature</td>
    </tr>
    <tr>
        <td>FV</td>
        <td>Final viscosity (RVU)</td>
        <td>Last viscosity reading when the sample is cooled</td>
    </tr>
    <tr>
        <td>BD</td>
        <td>Breakdown (RVU)</td>
        <td>Difference between peak viscosity and trough viscosity</td>
    </tr>
    <tr>
        <td>SB</td>
        <td>Setback (RVU)</td>
        <td>Difference between final viscosity and peak viscosity</td>
    </tr>
    <tr>
        <td>LO</td>
        <td>Lift-off (RVU)</td>
        <td>Difference between final viscosity and trough viscosity</td>
    </tr>
    <tr>
        <td>PASTEMP_RECALC</td>
        <td>Pasting temperature (ºC)</td>
        <td>Temperature when a sample starts thickening (as temperature is increased)</td>
    </tr>
    <tr>
        <td>PT</td>
        <td>Pasting time (min)</td>
        <td>How long it takes to reach peak viscosity</td>
    </tr>
</table>

First, I calculate the Pearson correlation coefficient and determine the variable pairs with high coefficients (r > 0.70). From these variable pairs, I picked variables to be excluded from the analysis. 

Second, I conduct K-means clustering. To determine the number of clusters, I used the elbow method (calculating the sum of squared distances per cluster number), the silhouette method, and the dendogram method. These methods indicate that a five-cluster solution is the best; hence, the subsequent deep learning neural network algorithm is based on five clusters.

<a href = "http://neuralnetworksanddeeplearning.com/">Neural networks</a> are programming approaches to learn from observational data by loosely imitating the way the brain's neurons connect and process information. <a href = "http://neuralnetworksanddeeplearning.com/">Deep learning</a>, on the other hand, is a set of techniques for learning within neural networks. I used these techniques to classify the samples.

I divide the samples into a test and a training set. Then, the data is scaled so that all variables have values within the same scale. The input layer is composed of the 18 variables. The hidden layer has 100 units and uses ReLU as the activation function. The output layer has five units and softmax as its activation function.

The deep learning algorithm's performance is evaluated using the DS2014PHY data.