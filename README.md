
# Understanding Principal Component Analysis 

## Introduction
PCA could be very counter intuitive to start with. In this lesson, we shall simply attempt to develop an intuition around PCA in terms of dimension reduction and visualization of reduced dimensions. We shall look at a few simple examples in 1D and 2D domains to help you get a clear understanding of what we mean by explained variances, rotation of axis, derived features (components) etc. With this understanding, as we move towards implementing PCA is a number of different contexts, it would be much easier for you to picture what is happening behind the scenes.

## Objectives
You will be able to:
- Develop a clear intuition around how PCA reduces dimensions
- Understand principal components, what do they represent and how many components do we need
- Understand the motivation behind mandatory data normalization prior to PCA

## What is Principal Component Analysis ?

> PCA (principal component analysis) is a popular machine learning technique for engineering and extracting important features as __components__ from a large set of features available in a given dataset. It extracts low dimensional set of features (a __feature Subspace__) from a high dimensional data set with a focus on capturing as much information as possible which is calculated as __captured variance__. 

PCA is also a great visualization tool it allows us to bring a high dimensional dataset into 2 or 3 dimensional subspace which becomes much easier to visualize and interpret.

### PCA Intuition
Let’s understand it using a simple intuitive example. Suppose we have a 3 dimensional dataset with a features set [x1, x2, x3] as shown in the 3D scatter plot below.
<img src="3d.png" width=400>

- The dataset contains 1000 points, and contains two classes, shown as green and red points. 
- The plane created by data points is on a slant of 45 degrees relative to both the x1 and x2 axes.

The purpose of PCA is to __compress__ or __squash__ this dataset into two dimensions so it becomes easier to visualize as a 2D scatter plot. We would, however, like to preserve as much __spread__ or __variation__ from the original data as possible. This makes it a data reduction problem, but with a focus on __maximising the variance__.

### Reducing dimensions without PCA
One (rather naive) way of reducing our data from 3D to 2D, without using PCA or any other analytical reduction technique, would be to pick any 2 combination of the 3 dimensions and plot. Let's pick x1 and x2 and see how it looks. 

<img src="2d1.png" width=400>

That's not so bad. The red and green separation looks very clear and there is a reasonable amount of spread in all axis. This projection of data is __skewed__ though as the variance along the dropped axis has not been captured at all. Other axes combinations: x1, x3 and x2, x3 would produce similar results showing different levels of this skewness due to dropped variance. So this approach will not give us a __good projection__. 

### So what is a good projection ?

The best projection is when we are looking directly at the plane i.e. __orthogonal__ to the plane, from an angle perpendicular to the plane, as shown below.
<img src="2d2.png" width=400>

*[Visit here](https://en.wikipedia.org/wiki/Orthogonality) to have a quick read on orthogonality. It refers to "perpendicularity" between two vectors.*


So the loss of variance by dropping features is not ideal. PCA allows to do a much better job. 

> __Note that PCA does not directly select important features and drops less important ones. PCA constructs new features from the existing that capture the most variation in the data.__ 

## Reducing Dimensions the PCA way

One intuitive way you can think of PCA is that it __rotates__ the plane so that it __aligns__ to one of the 3 possible axis combinations. For example, if we could somehow do the following: 
1. __Rotate the plane__ so that it lies perfectly flat along the $(x1,x2)$ space 
2. After rotation, ignore x3 and the variance is still maintained due to rotation

### A simpler Example

This can be a bit counter intuitive. Let's think of an even simpler example we'll see how we can reduce a dataset from 2 to just 1 dimension. 
<img src="ex1.png" width=600>

In the figure above, on the left we have 4 data points shown in red, in two demensional $(x_{1},x_{2})$ space. The purple line represents the 1-dimensional __subspace__ onto which PCA aims to project this data. 

The middle figure is where it all happens. Here, data points in red are being projected on the purple line as new points shown in green. 

PCA picks the 1D purple line while ensuring following:

1. __Maximize__ the variance of the projected green points

2. __Minimize__ the square of red-green distance (i.e. euclidiean squared distance of blue lines), using following formula: $$||x_{n}-\tilde{x_{n}}||^2$$

As a last step, the image on the right shows the original 2D data transformed from $(x_{1},x_{2})$ onto 1D, $u_{1}$ space. This new feature subspace is called __First Principal Component__. 


This intuition of PCA projecting 2D data onto a 1D line with __rotation__ can also generalize similarly to projecting 3D data onto a 2D plane shown below.

<img src="3d22d.png" width=800>



In above figure, PCA requires two principal components $(u_{1},u_{2})$ to project 3D data from $(R,G,B)$ space to 2D, $(u_{1},u_{2})$ space. 

### What are Principal Components

A principal component is a normalized linear combination of the original predictors in a data set. In image above, u1 and u2 are the principal components (otherwise, referred to as PC1 and PC2). IF we have a dataset containing features $X_1, X_2...,X_n$

The principal component can be written as:

$$u_1 = Φ_{11}X_1 + Φ_{21}X_2 + Φ_{31}X_3 + .... +Φ_{n1}X_n$$

- $u_1$ is first principal component
- $Φ$ (pronounced "phi") is called the __Loading Vector__ . It contains set of loadings ($Φ_1, Φ_2..$) of first principal component. It results in a line in n dimensional space which is closest to examples in the dataset. Closeness is measured using average squared euclidean distance as shown earlier.
- $X_1..X_n$ are set of standardized features. 


The __First Principal Component__ ($u_{1}$ above) is found in the direction of highest variability and is a linear combination of the original features that captures the most variation in the data.Larger the variability captured in first component, larger the information captured by component. No other component can have variability higher than first principal component. The first principal component results in a line which is closest to the data i.e. it minimizes the sum of squared distance between a data point and the line.

The __Second Principal Component__ ($u_{2}$ above) is __uncorrealted__ with first principal compoenent $u_{1}$ (that is $u_{1}$ is "ORTHOGONAL" to $u_{2}$). It is also a linear combination of the original features that captures the second most variation in the data. It can be computed as:

$$u_2 = Φ_{12}X_1 + Φ_{22}X_2 + Φ_{32}X_3 + .... +Φ_{n2}X_n$$

*Notice the direction of the components u1 and u2 sbove, as expected they are orthogonal. This suggests the correlation b/w these components in zero.*


### How many principal components can we have ?

>The number of possible principal components is equal to the original dimension of the data. In order to reduce the dimensionality of a dataset, we drop the principal components that explain the lowest amounts of variation in the data. 

For example to covert from 3D to 2D in the above plot, the third and final principal component ($u_{3}$ - not shown in the plot) is dropped.

<img src="scree.png", width=400>

The plot above shows that in a 44 dimensional dataset,  30 components explain around 98.4% variance in the data set. In order words, using PCA can reduce 44 features to 30 without compromising on explained variance too much. This is the power of PCA. 

### Why is feature normalization necessary ?

The principal components are calculated using normalized version of original features as the they may have different scales. For example: Imagine a data set with variables’ measuring units as gallons, kilometers, light years etc. Some features my show a large variance due to the nature of their scale.  Performing PCA on un-normalized variables will lead to insanely large loadings for variables with high variance. In turn, this will lead to dependence of a principal component on the variable with high variance as shown below:
<img src="scale.png" width=700>

The image above shows results of PCA  on a 40 dimensional dataset twice (with unscaled and scaled features). You can see, first principal component is dominated by a variable Item_MRP. And, second principal component is dominated by a variable Item_Weight. This domination prevails due to high value of variance associated with a variable. When the variables are scaled, we get a much better representation of variables in 2D space.

### Back to the original example 
As compared to simply dropping a dimension, applying PCA to the data shown in original example would results a reduced data which can be plotted in 2D as shown below:

<img src="pcafinal.png" width=400>

We can see, the plane now looks like a proper square and the centre circle looks like a circle. PCA finds the maximum variance for each principal component. There might be some loss of information in the process but overall we have the desired orthogonal view of the data. 

Next we shall look into steps necessary to perform PCA to successfully reduce the dimensions of a datset in a computational environment. 


## Additional Resources

- [Interactive PCA visualization demo](http://setosa.io/ev/principal-component-analysis/) - Highly Recommended
- [An intuitive visual exmaple of PCA](https://algobeans.com/2016/06/15/principal-component-analysis-tutorial/)
- [A One-Stop Shop for Principal Component Analysis](https://towardsdatascience.com/a-one-stop-shop-for-principal-component-analysis-5582fb7e0a9c)
- [A Tutorial on PCA](https://www.cs.princeton.edu/picasso/mats/PCA-Tutorial-Intuition_jp.pdf) - For a deep dive into the maths behind PCA


## Summary 

In this lesson we developed an intiution behind dimensionality reduction with PCA. We looked at a few very simple examples to see how PCA creates new dimensions (features) called principal components that attempt to capture the maximum variance in a given dataset. We looked at why normalization prior to PCA is standard approach. Next we shall look at the steps necessary to run a PCA experiment and achieve desired results. 
