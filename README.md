# US-yield-curve-analysis

This project consists of two parts. In the first task, I performed Principal Component Analysis (PCA) to reduce the dimensions of the data and validated the stability and reproducibility of the results. In the second part, I implemented both mean square error (MSE) and Dynamic Time Wrapping (DTW) method to find the historical year that has the most similar pattern with Sep 2020 to Sep 2021 time period.

## Dimension Reduction with PCA
  First, I rescaled the 6 features in the dataset before applying PCA because PCA is easily affected by scale. I standardized the datasets with a mean of zero and standard deviation of one. In PCA we are interested in the components that maximize the variance. Without standardization, if one component varies less than another, PCA might determine that the direction of the maximal variance will lean forward to the one that is more fluctuated. 
  
  After scaling, I need to determine how many components to retain. This requires observing the covariance matrix to understand the multi-collinearity among those features. From the covariance matrix, we can see that the 6 features are highly correlated, which makes sense because the 1-year, 2-year, …, 10-year treasury yield have the similar trend. Then I calculate the eigenvalue and eigenvectors of the covariance matrix. Figure 1 shows the histogram of eigenvalues representing the cumulative percentage of variance that is explained by the components. I find that we need two principles to explain more that 95% of the total variation of the total variation in the yield curve. Besides, I believe that by setting the threshold as 95%, we can have a reasonable representation of original data by keeping most of variation in it. Figure 2 shows the histogram of the original data containing 6 features and the modified data with 2 principal components.
 
<p align="center">
 <img width="300" alt="image" src="https://user-images.githubusercontent.com/97713325/157560499-aaec6d1a-3c3b-4099-aa32-6d0a2b81e1de.png">
</p>
<p align="center">
 <em>Figure 1 Cumulative percentage of variance explained by the components</em>
</p>

<p align="center">
<img width="300" alt="image" src="https://user-images.githubusercontent.com/97713325/157560650-6dc04e6a-24eb-4b7f-8414-d9fe93d374f0.png">
</p>

<p align="center">
 <em>Figure 2  (a): Histogram of the original data</em>
</p>

<p align="center">
 <em><img width="300" alt="image" src="https://user-images.githubusercontent.com/97713325/157560708-110858da-8baa-44c2-86ac-06b0fc92fbfb.png">
</p>
  
<p align="center">
 <em>Figure 2  (b): Histogram of the data with reduced dimensions</em>
</p>

## Stability validation of the PCA results
 To assess the stability of results, we can add random perturbations of the data to simulate the natural variability or measurement error. The original data will be changed to:
x_ij=x_ij+N(0,Ks_j)
Where constant K determines the degree of perturbation and s_j id the standard deviation of variable j. Then I can perform PCA on this perturbated data with the same reduced dimensions and derive a correlation between the new extracted components and the components extracted by the original data. Table 1 shows the correlation matrix of adding a random perturbation of 20% of the original standard deviation. We can see that the 2 factors are resistant to the modification implied by the high correlation.

<p align="center">
 <em>Table 1 Correlation matrix of PC between original data and perturbated data</em>
</p>

<p align="center">
<img width="300" alt="image" src="https://user-images.githubusercontent.com/97713325/157560765-6f696447-e4cf-4af0-8910-db6e1b5b9e77.png">
</p>
 

## Evaluate the most similar year using MSE method
Mean Squared Error (MSE) can tell us how close a regression line is to a set of points by taking average of the squared distances from the points to the regression line. In this project, the regression line corresponds to the treasury yield of a historical year and the set of points corresponds to the treasury yield of Sep 2020 to Sep 2021 time period. Figure 3 shows the distance calculated by MSE from 1976 – 2019.
As a result, the most similar year evaluated by MSE method is 2006-2007. 

## Evaluate the most similar year using DTW method 
  Although any two time series can be compared using Euclidean distance, such practice will result into a very poor comparison and similarity score even if the two time series are very similar in shape but out of phase in time. This is because the amplitude of first time series at time T will be compared with the amplitude of second time series at time T. However, DTW method is efficient in tackling this problem. DTW compares amplitude of first signal at time T with amplitude of second signal at time T+1 and T-1 or T+2 and T-2.[1]
	
  DTW computes the optimal alignment between points of two time series. Figure 4 shows the least cumulative distance calculated by DTW algorithm. As a result, the most similar year evaluated by DTW is 2006-2007.
<p align="center">
 <img width="300" alt="image" src="https://user-images.githubusercontent.com/97713325/157560827-f15cd9d5-65a2-424a-96f8-b04945f1ee5f.png">
</p>
<p align="center">
 <em>Figure 3 Distance calculated by MSE method</em>
 </p>
 <p align="center">
 <img width="249" alt="image" src="https://user-images.githubusercontent.com/97713325/157560840-79dd1e56-4d3f-458e-88e8-bfb20eb307be.png">
  </p>
<p align="center">
 <em>Figure 4 Distance calculated by DTW</em>
</p>
  By comparing the distance calculated by MSE method and DTW method, we can find that they share a similar pattern. The top ten most similar time period obtained by both methods are also highly overlapped. In this case, however, we prefer to trust the result derived by DTW, since it is a more robust technique when facing the shifts.

### Reference:
[1] Toni Giorgino. Computing and Visualizing Dynamic Time Warping Alignments in R: The dtw Package. Journal of Statistical Software, 31(7), 1-24. http://www.jstatsoft.org/v31/i07/
