# Class 7: Machine Learning
Noel Lim (PID: A17652474)

Today we are going to learn how to apply differemt machine learning
methods, beginning with clustering:

The goal here is to find groups/clusters in your input data.

First I will make up some data with clear groups. For this I will use
`rnorm()` function

``` r
rnorm(10)
```

     [1]  0.053611456 -0.596208262  0.007045137 -0.338759119 -0.684284344
     [6] -0.824136208 -1.573750985 -1.244249136 -0.092169275 -1.583757951

``` r
hist(rnorm(10000, mean = 3))
```

![](class07_files/figure-commonmark/unnamed-chunk-2-1.png)

``` r
n <- 10000
x <- c(rnorm(n,-3), rnorm(n,+3))
hist(x)
```

![](class07_files/figure-commonmark/unnamed-chunk-3-1.png)

``` r
n <- 30
x<-c(rnorm(n,-3), rnorm(n,+3))
y <- rev(x)

z<- cbind(x,y)
head(z)
```

                 x        y
    [1,] -3.592248 4.170546
    [2,] -2.476393 2.756857
    [3,] -2.857132 4.343722
    [4,] -2.055512 3.465917
    [5,] -2.774974 2.748272
    [6,] -2.062117 1.815521

``` r
plot(z)
```

![](class07_files/figure-commonmark/unnamed-chunk-5-1.png)

Use the kmeans() function setting k to 2 and nstart=20 Inspect/print the
results \> Q. How many points are in each cluster? \> Q. What
‘component’ of your result object details - cluster size? - cluster
assignment/membership? - cluster center? Plot x colored by the kmeans
cluster assignment and add cluster centers as blue points

``` r
km <- kmeans(z, centers = 2)
km
```

    K-means clustering with 2 clusters of sizes 30, 30

    Cluster means:
              x         y
    1  3.021689 -3.155272
    2 -3.155272  3.021689

    Clustering vector:
     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

    Within cluster sum of squares by cluster:
    [1] 43.20567 43.20567
     (between_SS / total_SS =  93.0 %)

    Available components:

    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

cluster size?

``` r
km$size
```

    [1] 30 30

cluster assignment/membership?

``` r
km$cluster
```

     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

cluster center?

``` r
km$centers
```

              x         y
    1  3.021689 -3.155272
    2 -3.155272  3.021689

> Q. Plot z colored by the kmeans cluster assignment and add cluster
> centers as blue points

R will recycle the shorter color vector to be the same length as the
longer (number of data points) in z

``` r
plot(z, col=c("red","blue"))
```

![](class07_files/figure-commonmark/unnamed-chunk-10-1.png)

``` r
plot(z,col=km$cluster)
```

![](class07_files/figure-commonmark/unnamed-chunk-11-1.png)

We can use the `points()` function to add new points to an existing
plot… like the cluster centers.

``` r
plot(z,col=km$cluster)
points(km$centers, col="blue", pch=17, cex=3)
```

![](class07_files/figure-commonmark/unnamed-chunk-12-1.png)

> Q. Can you run kmeans and ask for 4 clusters please and plot the
> results like we have done above

``` r
km2 <- kmeans(z, centers = 4)
km2
```

    K-means clustering with 4 clusters of sizes 16, 14, 17, 13

    Cluster means:
              x         y
    1  2.943134 -3.813809
    2  3.111466 -2.402657
    3 -3.526799  2.495111
    4 -2.669428  3.710289

    Clustering vector:
     [1] 4 4 4 4 3 3 4 4 3 4 4 4 4 3 3 3 4 3 3 3 3 3 3 3 3 3 4 4 3 3 1 1 1 1 1 1 1 1
    [39] 1 2 1 2 1 2 2 1 1 2 2 2 1 1 2 2 2 2 2 2 2 1

    Within cluster sum of squares by cluster:
    [1] 18.107358 10.017993 18.007889  8.904604
     (between_SS / total_SS =  95.5 %)

    Available components:

    [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    [6] "betweenss"    "size"         "iter"         "ifault"      

``` r
plot(z,col=km2$cluster)
points(km2$centers, col="blue", pch=17, cex=3)
```

![](class07_files/figure-commonmark/unnamed-chunk-13-1.png)

## Hierarchical Clustering

Let’s take our same First we need a distance matrix for your data to be
clustered

``` r
d <- dist(z)
hc <- hclust(d)
hc
```


    Call:
    hclust(d = d)

    Cluster method   : complete 
    Distance         : euclidean 
    Number of objects: 60 

``` r
plot(hc)
abline(h=8,col="red")
```

![](class07_files/figure-commonmark/unnamed-chunk-15-1.png)

I can get my cluster membership vector by “cutting the tree” with the
`cutree()` function like so

``` r
grps <- cutree(hc, h=8)
grps
```

     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

Can you plot `z` colored by our hclust results:

``` r
plot(z,col=grps)
```

![](class07_files/figure-commonmark/unnamed-chunk-17-1.png)

## PCA of food data

Read data from the UK on food consumption in different parts of the UK

``` r
url <- "https://tinyurl.com/UK-foods"
x <- read.csv(url, row.names=1)
head(x)
```

                   England Wales Scotland N.Ireland
    Cheese             105   103      103        66
    Carcass_meat       245   227      242       267
    Other_meat         685   803      750       586
    Fish               147   160      122        93
    Fats_and_oils      193   235      184       209
    Sugars             156   175      147       139

``` r
barplot(as.matrix(x), beside=T, col=rainbow(nrow(x)))
```

![](class07_files/figure-commonmark/unnamed-chunk-19-1.png)

``` r
barplot(as.matrix(x), beside=F, col=rainbow(nrow(x)))
```

![](class07_files/figure-commonmark/unnamed-chunk-20-1.png)

A so called “Pairs” plot can be useful for this.

``` r
pairs(x, col=rainbow(10), pch=16)
```

![](class07_files/figure-commonmark/unnamed-chunk-21-1.png)

It is hard to see structure and trends in even this small dataset. How
will we ever do this when we have big datasets with thousands or tens of
thousands of things we are measuring…

## PCA to the rescue

Let’s see how PCA deals with this dataset. So main function in base R to
do PCA is called `prcomp()`

``` r
pca <- prcomp( t(x) )
summary(pca)
```

    Importance of components:
                                PC1      PC2      PC3       PC4
    Standard deviation     324.1502 212.7478 73.87622 2.921e-14
    Proportion of Variance   0.6744   0.2905  0.03503 0.000e+00
    Cumulative Proportion    0.6744   0.9650  1.00000 1.000e+00

Let’s see what is inside this `pca` object that we created from running
`prcomp()`

``` r
attributes(pca)
```

    $names
    [1] "sdev"     "rotation" "center"   "scale"    "x"       

    $class
    [1] "prcomp"

``` r
pca$x
```

                     PC1         PC2        PC3           PC4
    England   -144.99315   -2.532999 105.768945 -9.152022e-15
    Wales     -240.52915 -224.646925 -56.475555  5.560040e-13
    Scotland   -91.86934  286.081786 -44.415495 -6.638419e-13
    N.Ireland  477.39164  -58.901862  -4.877895  1.329771e-13

``` r
plot(pca$x[,1],pca$x[,2], 
     col=c("black","red","blue","darkgreen"), pch=16, xlab="PC1 (67.4%)", ylab="PC2(29.05%)")
```

![](class07_files/figure-commonmark/unnamed-chunk-25-1.png)

``` r
plot(pca$x[,1], pca$x[,2],, xlab="PC1", ylab="PC2", xlim=c(-270,500))
text(pca$x[,1], pca$x[,2], colnames(x),col=c("black","red","blue","darkgreen"))
```

![](class07_files/figure-commonmark/unnamed-chunk-26-1.png)
