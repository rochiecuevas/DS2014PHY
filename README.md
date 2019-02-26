# Machine learning using data from the DS2014PHY IRRI rice samples

## Background
The data is publicly available and can be found in the supplementary section of the article "Multivariate-based classification of predicting cooking quality ideotypes in rice (*Oryza sativa* L.) indica germplasm" by Rosa Paula Cuevas (me), Cyril John Domingo, and Nese Sreenivasulu (2018) published in Rice 11: 56.

In that paper, R was used in data analyses. I used Ward's cluster analysis to classify the rice varieties into quality types. I then used multinomial logistic regression to create a model that can be used to differentiate the different quality types based on the non-collinear variables used to characterise the rice samples. A random forest algorithm was applied to determine the variables that were most important in classifying the rice samples.

## Further data exploration 
I am now exploring the dataset using different approaches. The results, of course, are quite different.

There are 25 continuous variables in the dataset and these could be categorised as follows:

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
</table>

First, I calculate the Pearson correlation coefficient and determine variable pairs with high coefficients (r > 0.70). From these variable pairs,