## Useful Links
- [Inside Airbnb Madrid](https://insideairbnb.com/madrid/)

## Introduction
Spatial econometrics is a sub-discipline within the field of econometrics whose analytical techniques are designed to incorporate dependence among observations (regions or points in space) into traditional econometric models. spatial dependence exists between the observations and spatial heterogeneity is what distinguishes Spatial econometrics from traditional econometrics (James, 1999). Econometrics is an analysis based on economic theory mathematical and statistical models to understand economical relationship and forecast future trends. In short, it’s an economic analysis on Madrid with spatial and other economic factors.

This study is based on Airbnb data of Madrid, primary goal is to build a model that could predict the pricing based on the attributes which will be discussed in further session. To achieve this model there will be series of analysis on spatial and non-spatial attributes and will have look on how spatial attribute has a significant influence on the model. Selection of predictors and outcome variables as well. And further session will break down the complexity of the terms and methods mentioned. And to mention to avoid overwhelming of code I’ve hidden all the code so please click the play button near to “Code” to view code for this entire study.

## Spatial dependence 
According to the concept of spatial dependence, observations obtained at different locations are not independent of one another; that is, the values of a given variable at one location affect the values of the same variable at other neighbouring locations. This effect can manifest itself in a variety of ways. Examples of this include geographical heterogeneity, which is the variability of a variable across space, and spatial autocorrelation, which is the similarity of neighbouring data (Anselin, 2010).

### 1. Kernel density estimation(KDE)
“Kernel density estimation is a nonparametric technique for density estimation which takes as its input a sample of data and, by associating a suitable non-negative function (the ‘kernel’) with each point of the sample, produces a continuous estimate of the probability density from which the sample is assumed to be drawn” (Silverman, 2018). Through the identification of patterns of clustering or dispersion within a specific area, kernel density estimation(KDE) can yield insights into spatial dependency. From the figure () the difference between a Hexbin map representing number of data points in a hexagonal polygon and KDE map can be visualized, were KDE represents hotspots were the possibility of clusters would be high

![image](https://github.com/user-attachments/assets/7a683540-094f-47fb-8171-325482ead8c7)

### 2.Inverse distance weighting
“Inverse distance weighting is a method for interpolation that computes the values at unsampled locations as weighted averages of observed values, with the weights being inversely related to the distance between the sampled and unsampled locations. (Shepard, 1968)”. This is a spatial interpolation method used to estimate values at unmeasured locations based on the values at surrounding measured locations. It assumes that the influence of nearby points on the estimation decreases as distance increases. Spatial interpolation was performed using Inverse Distance Weighting (IDW) methodology. A regular grid is created on the study area (100*100) with 10000 equally spaced cells. IDW estimated property prices at these grid points based on the values at nearby observed locations from the dataset. The method assigns weights inversely proportional to distances, with closer points exerting greater influence.

![image](https://github.com/user-attachments/assets/7e3288d7-8854-43b3-b359-c9d07537122a)

The IDW map reveals spatial patterns in property prices, with areas of higher and lower values clearly delineated. Closer proximity to observed locations results in more accurate estimates, while areas further away exhibit greater uncertainty. The map highlights spatial trends, such as clusters of high or low values, indicating localized variations in property prices across the study area. These insights are valuable for understanding spatial dependence and guiding decision-making processes related to real estate investment or resource allocation.

## Exploring Non-Spatial Variables in a Regression Modeling
Now that we have a clear understanding of the spatial distribution of point data throughout the research area, let’s examine non-spatial features and their importance in this model. Choosing the model’s outcome variable is the first step. The Airbnb pricing list will serve as our outcome variable since the study’s main objective is to forecast the economic factors influencing the study area. Since Airbnb’s price listing is provided in the numerical continuous category, let’s begin by creating a basic linear model.

### 1. Logarithmic transformation
To solve problems like skewness, heteroscedasticity, and nonlinearity, logarithmic transformation in regression entails taking a variable’s natural logarithm. This produces a more symmetrical distribution, stabilised variance, and linearized correlations. Better model performance and interpretation are made possible by this transition, especially in econometric analysis (Wooldridge, 2008). It would be easier to grasp if we compare the price and log-price. Let’s build a basic linear regression model using non-spatial attributes. The number of beds, bathrooms, and accommodates (the number of guests staying) were chosen as predictors. Given that these characteristics directly affect hotel rates and that most travellers choose opulent stays to enjoy their vacations, premium pricing tactics are justified (GILBERT et al., 2013).


![image](https://github.com/user-attachments/assets/aae3fdc5-fb89-4ae3-a5e9-8a719568de09)

We can understand the pricing and log-pricing distribution from the above plots. The price distribution plot is highly skewed, suggesting a high frequency of low-priced Airbnbs and a low frequency of high-priced Airbnbs. The log-price distribution plot, on the other hand, is more likely to be symmetric, suggesting an even distribution of price values following logarithmic transformation. Being highly skewed causes the model to fit poorly and increase the likelihood of making an incorrect prediction since the quantity of distributed values differs significantly. This can be identified in predictive check plots were the predicted values are halted between 0-2500, though there missing values in log-price model yet they are evenly distributed.


![image](https://github.com/user-attachments/assets/2b0482dd-f726-43bb-82c4-739b5de08227)

Results from linear regression model of pricing and log-pricing are presented in the above table, now lets break the complexity of the above results,

### The Akaike Information Criterion (AIC)
The Akaike Information Criterion (AIC) is a metric used to evaluate statistical models’ relative quality. Models with more parameters are penalised as it assesses the trade-off between the model’s complexity and goodness of fit. A model that fits the data well without going overboard in terms of complexity is indicated by a lower AIC value (Akaike, 1974).

AIC = (-2)log(maximum likelihood) + 2(number of independently adjusted parameters within the model).

Here AIC value of log-price model is nearly 6 times lower than the price model which implies a better fit of the model

### The Bayesian Information Criterion (BIC)
BIC is a statistical criterion for choosing models that strikes a compromise between model complexity and goodness of fit. In comparison to AIC, it applies a larger penalty for model complexity, encouraging the use of simpler models. A model that favours parsimony and fits the data well is indicated by a lower BIC value (Schwarz, 1978).

BIC=−2⋅log(L)+k⋅log(n)

L is the maximized value of the likelihood function of the model.

k is the number of parameters in the model.

n is the sample size.

Same as AIC the BIC values are to 6 times smaller in log-price model.

R-squared
The percentage of the dependent variable’s variance that the independent variables in a regression model account for is expressed as the coefficient of determination (R-squared).

R2  = 1- SSresidual /SStotal

SS​residual is the sum of squared residuals (also known as the sum of squared errors).

SS total is the total sum of squares, which measures the total variance in the dependent

The independent variables in the model—accommodations, bathrooms, bedrooms, and beds—account for around 24% of the variance in the natural logarithm of a hotel room’s price (log1p_price_usd). This indicates that around 24% of the variability seen in the logarithm of hotel room prices can be explained by the model, which uses the number of beds, bathrooms, and bedrooms on the property as predictors.

### Root Mean Squared Error (RMSE)
The RMSE values provide insight into the accuracy of the regression models. A lower RMSE indicates better predictive performance. In this comparison, the model with log-transformed price (RMSE = 0.7765709) outperforms the model with original price (RMSE = 358.8435800). The drastically lower RMSE of the log-price model suggests that it produces predictions much closer to the actual values compared to the price model. Log-transforming the price likely stabilized variance and improved model fit. Additionally, using log-price may have addressed issues such as skewness and heteroscedasticity, common in pricing data. Consequently, the log-price model provides more reliable predictions and better captures the underlying relationships between predictors and price, making it the preferred model for predicting hotel prices.

### Skewness
The skewness of log-price model is lower explains even distribution data points which we discussed in previous session.

## Spatial model
In order to further refine the study, let’s employ a spatial model for analysis. This model incorporates spatial referral, such as neighbourhood and distance, which has a significant impact on the dependent variable. Let’s unwind this theory by adding a spatial attribute to the log-price model called neighbourhood, which contains the neighbourhood location of Airbnb.
After adding spatial attributes to the model the AIC and BIC value got lowered to 41,420.21 and 42,452.46 implying better fit than previous non-spatial model, R-squared value is increased to 0.313, which has near 8% more variance in the natural logarithm of a hotel room’s price. Thus we can conclude that adding a spatial attribute enhanced the performance of the model let do further spatial analysis to this model.

### 1. Fixed Effect
Fixed effects are a kind of parameter used in regression analysis that shows how particular category factors affect the dependent variable. Within the framework of the study, these variables are not prone to change or randomness because they are considered fixed and preset. By incorporating distinct parameters for every category, fixed effects models effectively adjust for unobserved variation related to these variables and take into consideration their influence.

Unobserved heterogeneity between neighbourhoods is taken into account using the fixed effects model (spatial_model1). Neighborhood-specific effects on pricing are captured by the model, which treats each neighborhood as a fixed effect by incorporating neighborhood as a categorical variable. By accounting for regional changes, this method enables a more accurate investigation of the impact of lodging characteristics on hotel rates. Thus, the model helps players in the hotel sector and regulators make well-informed decisions that are relevant to their geographic contexts by offering insightful information about the complex interaction between neighborhood features and price dynamics.
The first model(spatial_model2) omits the intercept term and includes neighborhood as a categorical variable, thereby treating each neighborhood as a fixed effect. This model captures neighborhood-specific effects on pricing, providing insights into the heterogeneity of pricing patterns across different areas.

The second model(spatial_model3) incorporates fixed effects through interaction terms between neighborhood and accommodation attributes. By allowing the effects of accommodation attributes to vary across neighborhoods, this model offers a nuanced understanding of how neighborhood characteristics influence the relationship between accommodation features and pricing.

### 2. Weighted regression model
constructing a weighted regression model to predict property prices, leveraging spatial relationships among observations. Initially, the k-nearest neighbors of each property in the dataset are computed, here K = 50 means 50 nearest neighbourhood of each data point. Subsequently, a spatial weights matrix is generated from these neighbors, facilitating the quantification of spatial autocorrelation. The spatial lag of a selected variable, in this case, the number of bedrooms (w_bedrooms), is then calculated as the weighted average of the variable across neighboring properties. This spatial lag variable is integrated into the regression model alongside other property characteristics, such as the number of accommodates, bathrooms, bedrooms, and beds.

The rationale behind this methodology lies in the recognition that neighboring properties are likely to exhibit similar characteristics due to factors such as location, neighborhood amenities, and building styles. By incorporating spatial relationships through the spatial lag variable, the model aims to capture and account for these spatial dependencies, potentially leading to improved model performance and more accurate predictions.


![image](https://github.com/user-attachments/assets/cdf1a8a4-1524-4a4a-b92b-28e1bada7cce)

## Conclusion
Based on various critical metrics, spatial_model3 is the most effective model among the results presented. In comparison to other models, it has the lowest AIC (41046.98) and BIC (45942.33), showing better model fit and parsimony. Additionally, spatial_model3 has the best R-squared value (0.362), which indicates that the model explains the highest percentage of variance. Better overall predictive performance is shown by its enhanced goodness of fit, even if its RMSE is slightly larger than that of spatial_model2. Moreover, the model’s skewness is similar to that of other models, suggesting that its distribution properties are similar. The graph illustrated above as well explains why spatial_model3 is the best were the density of the predicted variables are evenly distributed not over fitted. Overall, spatial_model3 is clearly the best option since it strikes the optimal balance between explanatory power and model complexity, making it appropriate for both practical use in spatial analysis and reliable inference.
