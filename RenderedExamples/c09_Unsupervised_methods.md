---
output: github_document
---



00289_example_9.1_of_section_9.1.2.R



```r
# example 9.1 of section 9.1.2 
# (example 9.1 of section 9.1.2)  : Unsupervised methods : Cluster analysis : Preparing the data 
# Title: Reading the protein data 

protein <- read.table("../Protein/protein.txt", sep = "\t", header=TRUE)
summary(protein)
```

```
##            Country      RedMeat         WhiteMeat           Eggs      
##  Albania       : 1   Min.   : 4.400   Min.   : 1.400   Min.   :0.500  
##  Austria       : 1   1st Qu.: 7.800   1st Qu.: 4.900   1st Qu.:2.700  
##  Belgium       : 1   Median : 9.500   Median : 7.800   Median :2.900  
##  Bulgaria      : 1   Mean   : 9.828   Mean   : 7.896   Mean   :2.936  
##  Czechoslovakia: 1   3rd Qu.:10.600   3rd Qu.:10.800   3rd Qu.:3.700  
##  Denmark       : 1   Max.   :18.000   Max.   :14.000   Max.   :4.700  
##  (Other)       :19                                                    
##       Milk            Fish           Cereals          Starch     
##  Min.   : 4.90   Min.   : 0.200   Min.   :18.60   Min.   :0.600  
##  1st Qu.:11.10   1st Qu.: 2.100   1st Qu.:24.30   1st Qu.:3.100  
##  Median :17.60   Median : 3.400   Median :28.00   Median :4.700  
##  Mean   :17.11   Mean   : 4.284   Mean   :32.25   Mean   :4.276  
##  3rd Qu.:23.30   3rd Qu.: 5.800   3rd Qu.:40.10   3rd Qu.:5.700  
##  Max.   :33.70   Max.   :14.200   Max.   :56.70   Max.   :6.500  
##                                                                  
##       Nuts           Fr.Veg     
##  Min.   :0.700   Min.   :1.400  
##  1st Qu.:1.500   1st Qu.:2.900  
##  Median :2.400   Median :3.800  
##  Mean   :3.072   Mean   :4.136  
##  3rd Qu.:4.700   3rd Qu.:4.900  
##  Max.   :7.800   Max.   :7.900  
## 
```

```r
##            Country      RedMeat         WhiteMeat           Eggs
##  Albania       : 1   Min.   : 4.400   Min.   : 1.400   Min.   :0.500
##  Austria       : 1   1st Qu.: 7.800   1st Qu.: 4.900   1st Qu.:2.700
##  Belgium       : 1   Median : 9.500   Median : 7.800   Median :2.900
##  Bulgaria      : 1   Mean   : 9.828   Mean   : 7.896   Mean   :2.936
##  Czechoslovakia: 1   3rd Qu.:10.600   3rd Qu.:10.800   3rd Qu.:3.700
##  Denmark       : 1   Max.   :18.000   Max.   :14.000   Max.   :4.700
##  (Other)       :19
##       Milk            Fish           Cereals          Starch
##  Min.   : 4.90   Min.   : 0.200   Min.   :18.60   Min.   :0.600
##  1st Qu.:11.10   1st Qu.: 2.100   1st Qu.:24.30   1st Qu.:3.100
##  Median :17.60   Median : 3.400   Median :28.00   Median :4.700
##  Mean   :17.11   Mean   : 4.284   Mean   :32.25   Mean   :4.276
##  3rd Qu.:23.30   3rd Qu.: 5.800   3rd Qu.:40.10   3rd Qu.:5.700
##  Max.   :33.70   Max.   :14.200   Max.   :56.70   Max.   :6.500
##
##       Nuts           Fr.Veg
##  Min.   :0.700   Min.   :1.400
##  1st Qu.:1.500   1st Qu.:2.900
##  Median :2.400   Median :3.800
##  Mean   :3.072   Mean   :4.136
##  3rd Qu.:4.700   3rd Qu.:4.900
##  Max.   :7.800   Max.   :7.900
```




00290_example_9.2_of_section_9.1.2.R



```r
# example 9.2 of section 9.1.2 
# (example 9.2 of section 9.1.2)  : Unsupervised methods : Cluster analysis : Preparing the data 
# Title: Rescaling the dataset 

vars_to_use <- colnames(protein)[-1]              	# Note: 1 
pmatrix <- scale(protein[, vars_to_use])    
pcenter <- attr(pmatrix, "scaled:center")         	# Note: 2 
pscale <- attr(pmatrix, "scaled:scale")

rm_scales <- function(scaled_matrix) {            	# Note: 3 
  attr(scaled_matrix, "scaled:center") <- NULL
  attr(scaled_matrix, "scaled:scale") <- NULL
  scaled_matrix
}

pmatrix <- rm_scales(pmatrix)                     	# Note: 4

# Note 1: 
#   Use all the columns except the first 
#   (Country). 

# Note 2: 
#   Store the scaling attributes. 

# Note 3: 
#   Convenience function to remove scale attributes from a scaled matrix. 

# Note 4: 
#   Null the scale attributes out for safety. 
```




00291_example_9.3_of_section_9.1.3.R



```r
# example 9.3 of section 9.1.3 
# (example 9.3 of section 9.1.3)  : Unsupervised methods : Cluster analysis : Hierarchical clustering with hclust 
# Title: Hierarchical clustering 

distmat <- dist(pmatrix, method = "euclidean")       	# Note: 1 
pfit <- hclust(distmat, method = "ward.D")           	# Note: 2 
plot(pfit, labels = protein$Country)                 	# Note: 3
```

![plot of chunk 00291_example_9.3_of_section_9.1.3.R](figure/00291_example_9.3_of_section_9.1.3.R-1.png)

```r
# Note 1: 
#   Create the distance matrix. 

# Note 2: 
#   Do the clustering. 

# Note 3: 
#   Plot the dendrogram. 
```




00293_example_9.4_of_section_9.1.3.R



```r
# example 9.4 of section 9.1.3 
# (example 9.4 of section 9.1.3)  : Unsupervised methods : Cluster analysis : Hierarchical clustering with hclust 
# Title: Extracting the clusters found by hclust() 

groups <- cutree(pfit, k = 5)

print_clusters = function(data, groups, columns) {               	# Note: 1 
  groupedD = split(data, groups)
  lapply(groupedD,
         function(df) df[, columns])
}

cols_to_print = wrapr::qc(Country, RedMeat, Fish, Fr.Veg)
print_clusters(protein, groups, cols_to_print)
```

```
## $`1`
##       Country RedMeat Fish Fr.Veg
## 1     Albania    10.1  0.2    1.7
## 4    Bulgaria     7.8  1.2    4.2
## 18    Romania     6.2  1.0    2.8
## 25 Yugoslavia     4.4  0.6    3.2
## 
## $`2`
##        Country RedMeat Fish Fr.Veg
## 2      Austria     8.9  2.1    4.3
## 3      Belgium    13.5  4.5    4.0
## 9       France    18.0  5.7    6.5
## 12     Ireland    13.9  2.2    2.9
## 14 Netherlands     9.5  2.5    3.7
## 21 Switzerland    13.1  2.3    4.9
## 22          UK    17.4  4.3    3.3
## 24   W Germany    11.4  3.4    3.8
## 
## $`3`
##           Country RedMeat Fish Fr.Veg
## 5  Czechoslovakia     9.7  2.0    4.0
## 7       E Germany     8.4  5.4    3.6
## 11        Hungary     5.3  0.3    4.2
## 16         Poland     6.9  3.0    6.6
## 23           USSR     9.3  3.0    2.9
## 
## $`4`
##    Country RedMeat Fish Fr.Veg
## 6  Denmark    10.6  9.9    2.4
## 8  Finland     9.5  5.8    1.4
## 15  Norway     9.4  9.7    2.7
## 20  Sweden     9.9  7.5    2.0
## 
## $`5`
##     Country RedMeat Fish Fr.Veg
## 10   Greece    10.2  5.9    6.5
## 13    Italy     9.0  3.4    6.7
## 17 Portugal     6.2 14.2    7.9
## 19    Spain     7.1  7.0    7.2
```

```r
## $`1`
##       Country RedMeat Fish Fr.Veg
## 1     Albania    10.1  0.2    1.7
## 4    Bulgaria     7.8  1.2    4.2
## 18    Romania     6.2  1.0    2.8
## 25 Yugoslavia     4.4  0.6    3.2
## 
## $`2`
##        Country RedMeat Fish Fr.Veg
## 2      Austria     8.9  2.1    4.3
## 3      Belgium    13.5  4.5    4.0
## 9       France    18.0  5.7    6.5
## 12     Ireland    13.9  2.2    2.9
## 14 Netherlands     9.5  2.5    3.7
## 21 Switzerland    13.1  2.3    4.9
## 22          UK    17.4  4.3    3.3
## 24   W Germany    11.4  3.4    3.8
## 
## $`3`
##           Country RedMeat Fish Fr.Veg
## 5  Czechoslovakia     9.7  2.0    4.0
## 7       E Germany     8.4  5.4    3.6
## 11        Hungary     5.3  0.3    4.2
## 16         Poland     6.9  3.0    6.6
## 23           USSR     9.3  3.0    2.9
## 
## $`4`
##    Country RedMeat Fish Fr.Veg
## 6  Denmark    10.6  9.9    2.4
## 8  Finland     9.5  5.8    1.4
## 15  Norway     9.4  9.7    2.7
## 20  Sweden     9.9  7.5    2.0
## 
## $`5`
##     Country RedMeat Fish Fr.Veg
## 10   Greece    10.2  5.9    6.5
## 13    Italy     9.0  3.4    6.7
## 17 Portugal     6.2 14.2    7.9
## 19    Spain     7.1  7.0    7.2

# Note 1: 
#   A convenience function for printing out the 
#   countries in each cluster, along with the values 
#   for red meat, fish, and fruit/vegetable 
#   consumption. We???ll use this function throughout 
#   this section. Note the function assumes that the  
#   data is in a data.frame (not a matrix). 
```




00294_example_9.5_of_section_9.1.3.R



```r
# example 9.5 of section 9.1.3 
# (example 9.5 of section 9.1.3)  : Unsupervised methods : Cluster analysis : Hierarchical clustering with hclust 
# Title: Projecting the clusters on the first two principal components 

library(ggplot2)
princ <- prcomp(pmatrix)                                       	# Note: 1 
nComp <- 2
project <- predict(princ, pmatrix)[, 1:nComp]                  	# Note: 2 
project_plus <- cbind(as.data.frame(project),                  	# Note: 3 
                     cluster = as.factor(groups),
                     country = protein$Country)

ggplot(project_plus, aes(x = PC1, y = PC2)) +                  	# Note: 4 
  geom_point(data = as.data.frame(project), color = "darkgrey") + 
  geom_point() +
  geom_text(aes(label = country),
            hjust = 0, vjust = 1) + 
  facet_wrap(~ cluster, ncol = 3, labeller = label_both)
```

![plot of chunk 00294_example_9.5_of_section_9.1.3.R](figure/00294_example_9.5_of_section_9.1.3.R-1.png)

```r
# Note 1: 
#   Calculate the principal components of the 
#   data. 

# Note 2: 
#   The predict() function will rotate the data 
#   into the coordinates described by the principal 
#   components. The first two columns of the rotated data 
#   are the projection of the data on the first two principal  
#   components. 

# Note 3: 
#   Create a data frame with the transformed 
#   data, along with the cluster label and country 
#   label of each point. 

# Note 4: 
#   Plot it. Put each cluster in a separate facet for legibility. 
```




00295_example_9.6_of_section_9.1.3.R



```r
# example 9.6 of section 9.1.3 
# (example 9.6 of section 9.1.3)  : Unsupervised methods : Cluster analysis : Hierarchical clustering with hclust 
# Title: Running clusterboot() on the protein data 

library(fpc)                                                	# Note: 1 
kbest_p <- 5                                                      	# Note: 2 
cboot_hclust <- clusterboot(pmatrix,
                           clustermethod = hclustCBI,    	# Note: 3  
                           method = "ward.D",
                           k = kbest_p)
```

```
## boot 1 
## boot 2 
## boot 3 
## boot 4 
## boot 5 
## boot 6 
## boot 7 
## boot 8 
## boot 9 
## boot 10 
## boot 11 
## boot 12 
## boot 13 
## boot 14 
## boot 15 
## boot 16 
## boot 17 
## boot 18 
## boot 19 
## boot 20 
## boot 21 
## boot 22 
## boot 23 
## boot 24 
## boot 25 
## boot 26 
## boot 27 
## boot 28 
## boot 29 
## boot 30 
## boot 31 
## boot 32 
## boot 33 
## boot 34 
## boot 35 
## boot 36 
## boot 37 
## boot 38 
## boot 39 
## boot 40 
## boot 41 
## boot 42 
## boot 43 
## boot 44 
## boot 45 
## boot 46 
## boot 47 
## boot 48 
## boot 49 
## boot 50 
## boot 51 
## boot 52 
## boot 53 
## boot 54 
## boot 55 
## boot 56 
## boot 57 
## boot 58 
## boot 59 
## boot 60 
## boot 61 
## boot 62 
## boot 63 
## boot 64 
## boot 65 
## boot 66 
## boot 67 
## boot 68 
## boot 69 
## boot 70 
## boot 71 
## boot 72 
## boot 73 
## boot 74 
## boot 75 
## boot 76 
## boot 77 
## boot 78 
## boot 79 
## boot 80 
## boot 81 
## boot 82 
## boot 83 
## boot 84 
## boot 85 
## boot 86 
## boot 87 
## boot 88 
## boot 89 
## boot 90 
## boot 91 
## boot 92 
## boot 93 
## boot 94 
## boot 95 
## boot 96 
## boot 97 
## boot 98 
## boot 99 
## boot 100
```

```r
summary(cboot_hclust$result)                               	# Note: 4 
```

```
##               Length Class  Mode     
## result         7     hclust list     
## noise          1     -none- logical  
## nc             1     -none- numeric  
## clusterlist    5     -none- list     
## partition     25     -none- numeric  
## clustermethod  1     -none- character
## nccl           1     -none- numeric
```

```r
##               Length Class  Mode     
## result         7     hclust list     
## noise          1     -none- logical  
## nc             1     -none- numeric  
## clusterlist    5     -none- list     
## partition     25     -none- numeric  
## clustermethod  1     -none- character
## nccl           1     -none- numeric

groups <- cboot_hclust$result$partition                        	# Note: 5 
print_clusters(protein, groups, cols_to_print)                               	# Note: 6 
```

```
## $`1`
##       Country RedMeat Fish Fr.Veg
## 1     Albania    10.1  0.2    1.7
## 4    Bulgaria     7.8  1.2    4.2
## 18    Romania     6.2  1.0    2.8
## 25 Yugoslavia     4.4  0.6    3.2
## 
## $`2`
##        Country RedMeat Fish Fr.Veg
## 2      Austria     8.9  2.1    4.3
## 3      Belgium    13.5  4.5    4.0
## 9       France    18.0  5.7    6.5
## 12     Ireland    13.9  2.2    2.9
## 14 Netherlands     9.5  2.5    3.7
## 21 Switzerland    13.1  2.3    4.9
## 22          UK    17.4  4.3    3.3
## 24   W Germany    11.4  3.4    3.8
## 
## $`3`
##           Country RedMeat Fish Fr.Veg
## 5  Czechoslovakia     9.7  2.0    4.0
## 7       E Germany     8.4  5.4    3.6
## 11        Hungary     5.3  0.3    4.2
## 16         Poland     6.9  3.0    6.6
## 23           USSR     9.3  3.0    2.9
## 
## $`4`
##    Country RedMeat Fish Fr.Veg
## 6  Denmark    10.6  9.9    2.4
## 8  Finland     9.5  5.8    1.4
## 15  Norway     9.4  9.7    2.7
## 20  Sweden     9.9  7.5    2.0
## 
## $`5`
##     Country RedMeat Fish Fr.Veg
## 10   Greece    10.2  5.9    6.5
## 13    Italy     9.0  3.4    6.7
## 17 Portugal     6.2 14.2    7.9
## 19    Spain     7.1  7.0    7.2
```

```r
## $`1`
##       Country RedMeat Fish Fr.Veg
## 1     Albania    10.1  0.2    1.7
## 4    Bulgaria     7.8  1.2    4.2
## 18    Romania     6.2  1.0    2.8
## 25 Yugoslavia     4.4  0.6    3.2
## 
## $`2`
##        Country RedMeat Fish Fr.Veg
## 2      Austria     8.9  2.1    4.3
## 3      Belgium    13.5  4.5    4.0
## 9       France    18.0  5.7    6.5
## 12     Ireland    13.9  2.2    2.9
## 14 Netherlands     9.5  2.5    3.7
## 21 Switzerland    13.1  2.3    4.9
## 22          UK    17.4  4.3    3.3
## 24   W Germany    11.4  3.4    3.8
## 
## $`3`
##           Country RedMeat Fish Fr.Veg
## 5  Czechoslovakia     9.7  2.0    4.0
## 7       E Germany     8.4  5.4    3.6
## 11        Hungary     5.3  0.3    4.2
## 16         Poland     6.9  3.0    6.6
## 23           USSR     9.3  3.0    2.9
## 
## $`4`
##    Country RedMeat Fish Fr.Veg
## 6  Denmark    10.6  9.9    2.4
## 8  Finland     9.5  5.8    1.4
## 15  Norway     9.4  9.7    2.7
## 20  Sweden     9.9  7.5    2.0
## 
## $`5`
##     Country RedMeat Fish Fr.Veg
## 10   Greece    10.2  5.9    6.5
## 13    Italy     9.0  3.4    6.7
## 17 Portugal     6.2 14.2    7.9
## 19    Spain     7.1  7.0    7.2

cboot_hclust$bootmean                                       	# Note: 7 
```

```
## [1] 0.8041667 0.7688452 0.6111627 0.9088690 0.7373333
```

```r
## [1] 0.8090000 0.7939643 0.6247976 0.9366667 0.7815000

cboot_hclust$bootbrd                                        	# Note: 8 
```

```
## [1] 22 16 51  9 40
```

```r
## [1] 19 14 45  9 30

# Note 1: 
#   Load the fpc package. You may have to 
#   install it first. 

# Note 2: 
#   Set the desired number of clusters. 

# Note 3: 
#   Run clusterboot() with hclust 
#   (clustermethod = hclustCBI) using Ward???s method 
#   (method = "ward.D") and kbest_p clusters 
#   (k = kbest_p). Return the results in an object 
#   called cboot_hclust. 

# Note 4: 
#   The results of the clustering are in 
#   cboot_hclust$result. 

# Note 5: 
#   cboot_hclust$result$partition returns a 
#   vector of clusterlabels. 

# Note 6: 
#   The clusters are the same as those produced 
#   by a direct call to hclust(). 

# Note 7: 
#   The vector of cluster stabilities. 

# Note 8: 
#   The count of how many times each cluster was 
#   dissolved. By default clusterboot() runs 100 
#   bootstrap iterations. 
```




00296_example_9.7_of_section_9.1.3.R



```r
# example 9.7 of section 9.1.3 
# (example 9.7 of section 9.1.3)  : Unsupervised methods : Cluster analysis : Hierarchical clustering with hclust 
# Title: Calculating total within sum of squares 

sqr_edist <- function(x, y) {                              	# Note: 1 
  sum((x - y)^2)
}

wss_cluster <- function(clustermat) {                     	# Note: 2 
  c0 <- colMeans(clustermat)                              	# Note: 3 
  sum(apply(clustermat, 1, FUN = function(row) { sqr_edist(row, c0) }))     	# Note: 4 
}

wss_total <- function(dmatrix, labels) {                               	# Note: 5 
  wsstot <- 0
  k <- length(unique(labels))
  for(i in 1:k)
    wsstot <- wsstot + wss_cluster(subset(dmatrix, labels == i))         	# Note: 6 
  wsstot
}

wss_total(pmatrix, groups)                                 	# Note: 7  
```

```
## [1] 71.94342
```

```r
## [1] 71.94342

# Note 1: 
#   Function to calculate squared distance 
#   between two vectors. 

# Note 2: 
#   Function to calculate the WSS for a single 
#   cluster, which is represented as a matrix (one row 
#   for every point). 

# Note 3: 
#   Calculate the centroid of the cluster (the 
#   mean of all the points). 

# Note 4: 
#   Calculate the squared difference of every 
#   point in the cluster from the centroid, and sum 
#   all the distances. 

# Note 5: 
#   Function to compute the total WSS from a set 
#   of data points and cluster labels. 

# Note 6: 
#   Extract each cluster, calculate the 
#   cluster???s WSS, and sum all the values. 

# Note 7: 
#   Calculate the total WSS for the current protein clustering. 
```




00297_example_9.8_of_section_9.1.3.R



```r
# example 9.8 of section 9.1.3 
# (example 9.8 of section 9.1.3)  : Unsupervised methods : Cluster analysis : Hierarchical clustering with hclust 
# Title: Plot WSS for a range of k 

get_wss <- function(dmatrix, max_clusters) {       	# Note: 1 
   wss = numeric(max_clusters)
  
 
  wss[1] <- wss_cluster(dmatrix)                  	# Note: 2 
  
  d <- dist(dmatrix, method = "euclidean")
  pfit <- hclust(d, method = "ward.D")       	# Note: 3 
  
  for(k in 2:max_clusters) {                     	# Note: 4     
    labels <- cutree(pfit, k = k)
    wss[k] <- wss_total(dmatrix, labels)
  }
  
  wss
}

kmax <- 10
cluster_meas <- data.frame(nclusters = 1:kmax,
                          wss = get_wss(pmatrix, kmax))

breaks <- 1:kmax
ggplot(cluster_meas, aes(x=nclusters, y = wss)) +      	# Note: 5 
  geom_point() + geom_line() +
  scale_x_continuous(breaks = breaks)
```

![plot of chunk 00297_example_9.8_of_section_9.1.3.R](figure/00297_example_9.8_of_section_9.1.3.R-1.png)

```r
# Note 1: 
#   A function to get the total WSS for a  
#   range of clusters from 1 to max 

# Note 2: 
#   wss[1] is just the WSS of all the data 

# Note 3: 
#   Cluster the data. 

# Note 4: 
#   For each k, calculate the cluster labels and the cluster WSS 

# Note 5: 
#   Plot WSS as a function of k 
```




00299_example_9.9_of_section_9.1.3.R



```r
# example 9.9 of section 9.1.3 
# (example 9.9 of section 9.1.3)  : Unsupervised methods : Cluster analysis : Hierarchical clustering with hclust 
# Title: Plot BSS and WSS as a function of k 

total_ss <- function(dmatrix) {                              	# Note: 1 
  grandmean <- colMeans(dmatrix)
  sum(apply(dmatrix, 1, FUN = function(row) { sqr_edist(row, grandmean) }))
}

tss <- total_ss(pmatrix)
cluster_meas$bss <- with(cluster_meas, tss - wss)

library(cdata)                                                 	# Note: 2 
cmlong <- unpivot_to_blocks(cluster_meas,                       	# Note: 3 
                           nameForNewKeyColumn = "measure",
                           nameForNewValueColumn = "value",
                           columnsToTakeFrom = c("wss", "bss"))

ggplot(cmlong, aes(x = nclusters, y = value)) +  
  geom_point() + geom_line() + 
  facet_wrap(~measure, ncol = 1, scale = "free_y") +
  scale_x_continuous(breaks = 1:10)
```

![plot of chunk 00299_example_9.9_of_section_9.1.3.R](figure/00299_example_9.9_of_section_9.1.3.R-1.png)

```r
# Note 1: 
#   Calculate total sum of squares TSS. 

# Note 2: 
#   Load the cdata package to reshape the data. 

# Note 3: 
#   Reshape cluster_meas so that WSS and BSS are in the same column. 
```




00302_example_9.10_of_section_9.1.3.R



```r
# example 9.10 of section 9.1.3 
# (example 9.10 of section 9.1.3)  : Unsupervised methods : Cluster analysis : Hierarchical clustering with hclust 
# Title: The Calinski-Harabasz index 

cluster_meas$B <- with(cluster_meas,  bss / (nclusters - 1))                	# Note: 1 

n = nrow(pmatrix)
cluster_meas$W <- with(cluster_meas,  wss / (n - nclusters))                 	# Note: 2 
                                                        
cluster_meas$ch_crit <- with(cluster_meas, B / W)                           	# Note: 3 
                           
ggplot(cluster_meas, aes(x = nclusters, y = ch_crit)) + 
  geom_point() + geom_line() + 
  scale_x_continuous(breaks = 1:kmax)
```

```
## Warning: Removed 1 rows containing missing values (geom_point).
```

```
## Warning: Removed 1 rows containing missing values (geom_path).
```

![plot of chunk 00302_example_9.10_of_section_9.1.3.R](figure/00302_example_9.10_of_section_9.1.3.R-1.png)

```r
# Note 1: 
#   Calculate the between-cluster variance B 

# Note 2: 
#   Calculate the within-cluster variance W 

# Note 3: 
#   Calculate the CH index 
```




00303_example_9.11_of_section_9.1.4.R



```r
# example 9.11 of section 9.1.4 
# (example 9.11 of section 9.1.4)  : Unsupervised methods : Cluster analysis : The k-means algorithm 
# Title: Running k-means with k = 5 

kbest_p <- 5

pclusters <- kmeans(pmatrix, kbest_p, nstart = 100, iter.max = 100)     	# Note: 1 
summary(pclusters)                                                      	# Note: 2 
```

```
##              Length Class  Mode   
## cluster      25     -none- numeric
## centers      45     -none- numeric
## totss         1     -none- numeric
## withinss      5     -none- numeric
## tot.withinss  1     -none- numeric
## betweenss     1     -none- numeric
## size          5     -none- numeric
## iter          1     -none- numeric
## ifault        1     -none- numeric
```

```r
##              Length Class  Mode   
## cluster      25     -none- numeric
## centers      45     -none- numeric
## totss         1     -none- numeric
## withinss      5     -none- numeric
## tot.withinss  1     -none- numeric
## betweenss     1     -none- numeric
## size          5     -none- numeric
## iter          1     -none- numeric
## ifault        1     -none- numeric

pclusters$centers                                                   	# Note: 3                                                
```

```
##        RedMeat  WhiteMeat        Eggs       Milk       Fish    Cereals
## 1  1.011180399  0.7421332  0.94084150  0.5700581 -0.2671539 -0.6877583
## 2 -0.807569986 -0.8719354 -1.55330561 -1.0783324 -1.0386379  1.7200335
## 3 -0.570049402  0.5803879 -0.08589708 -0.4604938 -0.4537795  0.3181839
## 4 -0.508801956 -1.1088009 -0.41248496 -0.8320414  0.9819154  0.1300253
## 5  0.006572897 -0.2290150  0.19147892  1.3458748  1.1582546 -0.8722721
##       Starch       Nuts      Fr.Veg
## 1  0.2288743 -0.5083895  0.02161979
## 2 -1.4234267  0.9961313 -0.64360439
## 3  0.7857609 -0.2679180  0.06873983
## 4 -0.1842010  1.3108846  1.62924487
## 5  0.1676780 -0.9553392 -1.11480485
```

```r
##        RedMeat  WhiteMeat        Eggs       Milk       Fish    Cereals
## 1 -0.570049402  0.5803879 -0.08589708 -0.4604938 -0.4537795  0.3181839
## 2 -0.508801956 -1.1088009 -0.41248496 -0.8320414  0.9819154  0.1300253
## 3 -0.807569986 -0.8719354 -1.55330561 -1.0783324 -1.0386379  1.7200335
## 4  0.006572897 -0.2290150  0.19147892  1.3458748  1.1582546 -0.8722721
## 5  1.011180399  0.7421332  0.94084150  0.5700581 -0.2671539 -0.6877583
##       Starch       Nuts      Fr.Veg
## 1  0.7857609 -0.2679180  0.06873983
## 2 -0.1842010  1.3108846  1.62924487
## 3 -1.4234267  0.9961313 -0.64360439
## 4  0.1676780 -0.9553392 -1.11480485
## 5  0.2288743 -0.5083895  0.02161979

pclusters$size                                                      	# Note: 4 
```

```
## [1] 8 4 5 4 4
```

```r
## [1] 5 4 4 4 8

groups <- pclusters$cluster                                         	# Note: 5 

cols_to_print = wrapr::qc(Country, RedMeat, Fish, Fr.Veg)
print_clusters(protein, groups, cols_to_print)                                      	# Note: 6  
```

```
## $`1`
##        Country RedMeat Fish Fr.Veg
## 2      Austria     8.9  2.1    4.3
## 3      Belgium    13.5  4.5    4.0
## 9       France    18.0  5.7    6.5
## 12     Ireland    13.9  2.2    2.9
## 14 Netherlands     9.5  2.5    3.7
## 21 Switzerland    13.1  2.3    4.9
## 22          UK    17.4  4.3    3.3
## 24   W Germany    11.4  3.4    3.8
## 
## $`2`
##       Country RedMeat Fish Fr.Veg
## 1     Albania    10.1  0.2    1.7
## 4    Bulgaria     7.8  1.2    4.2
## 18    Romania     6.2  1.0    2.8
## 25 Yugoslavia     4.4  0.6    3.2
## 
## $`3`
##           Country RedMeat Fish Fr.Veg
## 5  Czechoslovakia     9.7  2.0    4.0
## 7       E Germany     8.4  5.4    3.6
## 11        Hungary     5.3  0.3    4.2
## 16         Poland     6.9  3.0    6.6
## 23           USSR     9.3  3.0    2.9
## 
## $`4`
##     Country RedMeat Fish Fr.Veg
## 10   Greece    10.2  5.9    6.5
## 13    Italy     9.0  3.4    6.7
## 17 Portugal     6.2 14.2    7.9
## 19    Spain     7.1  7.0    7.2
## 
## $`5`
##    Country RedMeat Fish Fr.Veg
## 6  Denmark    10.6  9.9    2.4
## 8  Finland     9.5  5.8    1.4
## 15  Norway     9.4  9.7    2.7
## 20  Sweden     9.9  7.5    2.0
```

```r
## $`1`
##           Country RedMeat Fish Fr.Veg
## 5  Czechoslovakia     9.7  2.0    4.0
## 7       E Germany     8.4  5.4    3.6
## 11        Hungary     5.3  0.3    4.2
## 16         Poland     6.9  3.0    6.6
## 23           USSR     9.3  3.0    2.9
## 
## $`2`
##     Country RedMeat Fish Fr.Veg
## 10   Greece    10.2  5.9    6.5
## 13    Italy     9.0  3.4    6.7
## 17 Portugal     6.2 14.2    7.9
## 19    Spain     7.1  7.0    7.2
## 
## $`3`
##       Country RedMeat Fish Fr.Veg
## 1     Albania    10.1  0.2    1.7
## 4    Bulgaria     7.8  1.2    4.2
## 18    Romania     6.2  1.0    2.8
## 25 Yugoslavia     4.4  0.6    3.2
## 
## $`4`
##    Country RedMeat Fish Fr.Veg
## 6  Denmark    10.6  9.9    2.4
## 8  Finland     9.5  5.8    1.4
## 15  Norway     9.4  9.7    2.7
## 20  Sweden     9.9  7.5    2.0
## 
## $`5`
##        Country RedMeat Fish Fr.Veg
## 2      Austria     8.9  2.1    4.3
## 3      Belgium    13.5  4.5    4.0
## 9       France    18.0  5.7    6.5
## 12     Ireland    13.9  2.2    2.9
## 14 Netherlands     9.5  2.5    3.7
## 21 Switzerland    13.1  2.3    4.9
## 22          UK    17.4  4.3    3.3
## 24   W Germany    11.4  3.4    3.8

# Note 1: 
#   Run kmeans() with five clusters (kbest_p = 5), 
#   100 random starts, and 100 maximum iterations per 
#   run. 

# Note 2: 
#   kmeans() returns all the sum of squares 
#   measures. 

# Note 3: 
#   pclusters$centers is a matrix whose rows are 
#   the centroids of the clusters. Note that 
#   pclusters$centers is in the scaled coordinates, 
#   not the original protein coordinates. 

# Note 4: 
#   pclusters$size returns the number of points 
#   in each cluster. Generally (though not always) a 
#   good clustering will be fairly well balanced: no 
#   extremely small clusters and no extremely large 
#   ones. 

# Note 5: 
#   pclusters$cluster is a vector of cluster 
#   labels. 

# Note 6: 
#   In this case, kmeans() and hclust() returned 
#   the same clustering. This won???t always be true. 
```




00304_example_9.12_of_section_9.1.4.R



```r
# example 9.12 of section 9.1.4 
# (example 9.12 of section 9.1.4)  : Unsupervised methods : Cluster analysis : The k-means algorithm 
# Title: Plotting cluster criteria 

clustering_ch <- kmeansruns(pmatrix, krange = 1:10, criterion = "ch")       	# Note: 1                                                 
clustering_ch$bestk                                                             	# Note: 2 
```

```
## [1] 2
```

```r
## [1] 2

clustering_asw <- kmeansruns(pmatrix, krange = 1:10, criterion = "asw")     	# Note: 3 
clustering_asw$bestk
```

```
## [1] 3
```

```r
## [1] 3

clustering_asw$crit                                                  	# Note: 4 
```

```
##  [1] 0.0000000 0.3271084 0.3351694 0.2617868 0.2639450 0.2734815 0.2471165
##  [8] 0.2429985 0.2412922 0.2388293
```

```r
## [1] 0.0000000 0.3271084 0.3351694 0.2617868 0.2639450 0.2734815 0.2471165 
## [8] 0.2429985 0.2412922 0.2388293

clustering_ch$crit                                                  	# Note: 5 
```

```
##  [1]  0.000000 14.094814 11.417985 10.418801 10.011797  9.964967  9.861682
##  [8]  9.412089  9.166676  9.075569
```

```r
##  [1]  0.000000 14.094814 11.417985 10.418801 10.011797  9.964967  9.861682
##  [8]  9.412089  9.166676  9.075569

cluster_meas$ch_crit                                                        	# Note: 6 
```

```
##  [1]       NaN 12.215107 10.359587  9.690891 10.011797  9.964967  9.506978
##  [8]  9.092065  8.822406  8.695065
```

```r
##  [1]       NaN 12.215107 10.359587  9.690891 10.011797  9.964967  9.506978
##  [8]  9.092065  8.822406  8.695065

 
summary(clustering_ch)                                              	# Note: 7 
```

```
##              Length Class  Mode   
## cluster      25     -none- numeric
## centers      18     -none- numeric
## totss         1     -none- numeric
## withinss      2     -none- numeric
## tot.withinss  1     -none- numeric
## betweenss     1     -none- numeric
## size          2     -none- numeric
## iter          1     -none- numeric
## ifault        1     -none- numeric
## crit         10     -none- numeric
## bestk         1     -none- numeric
```

```r
##              Length Class  Mode   
## cluster      25     -none- numeric
## centers      18     -none- numeric
## totss         1     -none- numeric
## withinss      2     -none- numeric
## tot.withinss  1     -none- numeric
## betweenss     1     -none- numeric
## size          2     -none- numeric
## iter          1     -none- numeric
## ifault        1     -none- numeric
## crit         10     -none- numeric
## bestk         1     -none- numeric

# Note 1: 
#   Run kmeansruns() from 1???10 clusters, and the 
#   CH criterion. By default, kmeansruns() uses 100 
#   random starts and 100 maximum iterations per 
#   run. 

# Note 2: 
#   The CH criterion picks two clusters. 

# Note 3: 
#   Run kmeansruns() from 1???10 clusters, and the 
#   average silhouette width criterion. Average 
#   silhouette width picks 3 clusters. 

# Note 4: 
#   Look at the values of the asw criterion as a function of k. 

# Note 5: 
#   Look at the values of the CH criterion as a function of k. 

# Note 6: 
#   Compare these to the CH values for the 
#   hclust() clustering. They???re not quite the same, 
#   because the two algorithms didn???t pick the same 
#   clusters. 

# Note 7: 
#   kmeansruns() also returns the output of 
#   kmeans for k = bestk. 
```




00305_example_9.13_of_section_9.1.4.R



```r
# example 9.13 of section 9.1.4 
# (example 9.13 of section 9.1.4)  : Unsupervised methods : Cluster analysis : The k-means algorithm 
# Title: Running clusterboot() with k-means 

kbest_p <- 5
cboot <- clusterboot(pmatrix, clustermethod = kmeansCBI,
            runs = 100,iter.max = 100,
            krange = kbest_p, seed = 15555)                	# Note: 1 
```

```
## boot 1 
## boot 2 
## boot 3 
## boot 4 
## boot 5 
## boot 6 
## boot 7 
## boot 8 
## boot 9 
## boot 10 
## boot 11 
## boot 12 
## boot 13 
## boot 14 
## boot 15 
## boot 16 
## boot 17 
## boot 18 
## boot 19 
## boot 20 
## boot 21 
## boot 22 
## boot 23 
## boot 24 
## boot 25 
## boot 26 
## boot 27 
## boot 28 
## boot 29 
## boot 30 
## boot 31 
## boot 32 
## boot 33 
## boot 34 
## boot 35 
## boot 36 
## boot 37 
## boot 38 
## boot 39 
## boot 40 
## boot 41 
## boot 42 
## boot 43 
## boot 44 
## boot 45 
## boot 46 
## boot 47 
## boot 48 
## boot 49 
## boot 50 
## boot 51 
## boot 52 
## boot 53 
## boot 54 
## boot 55 
## boot 56 
## boot 57 
## boot 58 
## boot 59 
## boot 60 
## boot 61 
## boot 62 
## boot 63 
## boot 64 
## boot 65 
## boot 66 
## boot 67 
## boot 68 
## boot 69 
## boot 70 
## boot 71 
## boot 72 
## boot 73 
## boot 74 
## boot 75 
## boot 76 
## boot 77 
## boot 78 
## boot 79 
## boot 80 
## boot 81 
## boot 82 
## boot 83 
## boot 84 
## boot 85 
## boot 86 
## boot 87 
## boot 88 
## boot 89 
## boot 90 
## boot 91 
## boot 92 
## boot 93 
## boot 94 
## boot 95 
## boot 96 
## boot 97 
## boot 98 
## boot 99 
## boot 100
```

```r
groups <- cboot$result$partition
print_clusters(protein, groups, cols_to_print)
```

```
## $`1`
##     Country RedMeat Fish Fr.Veg
## 10   Greece    10.2  5.9    6.5
## 13    Italy     9.0  3.4    6.7
## 17 Portugal     6.2 14.2    7.9
## 19    Spain     7.1  7.0    7.2
## 
## $`2`
##        Country RedMeat Fish Fr.Veg
## 2      Austria     8.9  2.1    4.3
## 3      Belgium    13.5  4.5    4.0
## 9       France    18.0  5.7    6.5
## 12     Ireland    13.9  2.2    2.9
## 14 Netherlands     9.5  2.5    3.7
## 21 Switzerland    13.1  2.3    4.9
## 22          UK    17.4  4.3    3.3
## 24   W Germany    11.4  3.4    3.8
## 
## $`3`
##           Country RedMeat Fish Fr.Veg
## 5  Czechoslovakia     9.7  2.0    4.0
## 7       E Germany     8.4  5.4    3.6
## 11        Hungary     5.3  0.3    4.2
## 16         Poland     6.9  3.0    6.6
## 23           USSR     9.3  3.0    2.9
## 
## $`4`
##    Country RedMeat Fish Fr.Veg
## 6  Denmark    10.6  9.9    2.4
## 8  Finland     9.5  5.8    1.4
## 15  Norway     9.4  9.7    2.7
## 20  Sweden     9.9  7.5    2.0
## 
## $`5`
##       Country RedMeat Fish Fr.Veg
## 1     Albania    10.1  0.2    1.7
## 4    Bulgaria     7.8  1.2    4.2
## 18    Romania     6.2  1.0    2.8
## 25 Yugoslavia     4.4  0.6    3.2
```

```r
## $`1`
##       Country RedMeat Fish Fr.Veg
## 1     Albania    10.1  0.2    1.7
## 4    Bulgaria     7.8  1.2    4.2
## 18    Romania     6.2  1.0    2.8
## 25 Yugoslavia     4.4  0.6    3.2
## 
## $`2`
##    Country RedMeat Fish Fr.Veg
## 6  Denmark    10.6  9.9    2.4
## 8  Finland     9.5  5.8    1.4
## 15  Norway     9.4  9.7    2.7
## 20  Sweden     9.9  7.5    2.0
## 
## $`3`
##           Country RedMeat Fish Fr.Veg
## 5  Czechoslovakia     9.7  2.0    4.0
## 7       E Germany     8.4  5.4    3.6
## 11        Hungary     5.3  0.3    4.2
## 16         Poland     6.9  3.0    6.6
## 23           USSR     9.3  3.0    2.9
## 
## $`4`
##        Country RedMeat Fish Fr.Veg
## 2      Austria     8.9  2.1    4.3
## 3      Belgium    13.5  4.5    4.0
## 9       France    18.0  5.7    6.5
## 12     Ireland    13.9  2.2    2.9
## 14 Netherlands     9.5  2.5    3.7
## 21 Switzerland    13.1  2.3    4.9
## 22          UK    17.4  4.3    3.3
## 24   W Germany    11.4  3.4    3.8
## 
## $`5`
##     Country RedMeat Fish Fr.Veg
## 10   Greece    10.2  5.9    6.5
## 13    Italy     9.0  3.4    6.7
## 17 Portugal     6.2 14.2    7.9
## 19    Spain     7.1  7.0    7.2

cboot$bootmean
```

```
## [1] 0.7540000 0.7441548 0.5965675 0.8716429 0.7971667
```

```r
## [1] 0.8670000 0.8420714 0.6147024 0.7647341 0.7508333

cboot$bootbrd
```

```
## [1] 28 15 53 16 21
```

```r
## [1] 15 20 49 17 32

# Note 1: 
#   We???ve set the seed for the random generator 
#   so the results are reproducible. 
```




00306_example_9.14_of_section_9.1.5.R



```r
# example 9.14 of section 9.1.5 
# (example 9.14 of section 9.1.5)  : Unsupervised methods : Cluster analysis : Assigning new points to clusters 
# Title: A function to assign points to a cluster 

assign_cluster <- function(newpt, centers, xcenter = 0, xscale = 1) { 
   xpt <- (newpt - xcenter) / xscale                                	# Note: 1 
   dists <- apply(centers, 1, FUN = function(c0) { sqr_edist(c0, xpt) })  	# Note: 2 
   which.min(dists)                                                 	# Note: 3 
 }

# Note 1: 
#   Center and scale the new data point. 

# Note 2: 
#   Calculate how far the new data point is from 
#   each of the cluster centers. 

# Note 3: 
#   Return the cluster number of the closest 
#   centroid. 
```




00307_example_9.15_of_section_9.1.5.R



```r
# example 9.15 of section 9.1.5 
# (example 9.15 of section 9.1.5)  : Unsupervised methods : Cluster analysis : Assigning new points to clusters 
# Title: Generate and cluster synthetic data for cluster assignment example 

mean1 <- c(1, 1, 1)                                  	# Note: 1 
sd1 <- c(1, 2, 1)

mean2 <- c(10, -3, 5)
sd2 <- c(2, 1, 2)

mean3 <- c(-5, -5, -5)
sd3 <- c(1.5, 2, 1)

library(MASS)                                       	# Note: 2 
clust1 <- mvrnorm(100, mu = mean1, Sigma = diag(sd1))
clust2 <- mvrnorm(100, mu = mean2, Sigma = diag(sd2))
clust3 <- mvrnorm(100, mu = mean3, Sigma = diag(sd3))
toydata <- rbind(clust3, rbind(clust1, clust2))

tmatrix <- scale(toydata)                          	# Note: 3  
tcenter <- attr(tmatrix, "scaled:center")           	# Note: 4 
tscale <-attr(tmatrix, "scaled:scale")
tmatrix <- rm_scales(tmatrix)

kbest_t <- 3
tclusters <- kmeans(tmatrix, kbest_t, nstart = 100, iter.max = 100)     	# Note: 5 

tclusters$size                                   	# Note: 6        
```

```
## [1] 100  99 101
```

```r
## [1] 101 100  99

# Note 1: 
#   Set the parameters for three 3D 
#   Gaussian clusters. 

# Note 2: 
#   Use the mvrnorm() function from MASS package to generate 
#   three-dimensional axis-aligned Gaussian clusters. 

# Note 3: 
#   Scale the synthetic data. 

# Note 4: 
#   Get the scaling attributes, then remove them from the matrix. 

# Note 5: 
#   Cluster the synthetic data into three clusters. 

# Note 6: 
#   The generated clusters are consistent in size with the true clusters. 
```




00308_example_9.16_of_section_9.1.5.R



```r
# example 9.16 of section 9.1.5 
# (example 9.16 of section 9.1.5)  : Unsupervised methods : Cluster analysis : Assigning new points to clusters 
# Title: Unscale the centers 

unscaled = scale(tclusters$centers, center = FALSE, scale = 1 / tscale) 
rm_scales(scale(unscaled, center = -tcenter, scale = FALSE))          
```

```
##         [,1]       [,2]       [,3]
## 1 -4.7554083 -5.0841602 -5.0110629
## 2  9.9278210 -2.9398534  4.8330536
## 3  0.9833241  0.7977014  0.8149083
```

```r
##         [,1]      [,2]       [,3]
## 1  9.8234797 -3.005977  4.7662651
## 2 -4.9749654 -4.862436 -5.0577002
## 3  0.8926698  1.185734  0.8336977
```




00309_example_9.17_of_section_9.1.5.R



```r
# example 9.17 of section 9.1.5 
# (example 9.17 of section 9.1.5)  : Unsupervised methods : Cluster analysis : Assigning new points to clusters 
# Title: An example of assigning points to clusters 

assign_cluster(mvrnorm(1, mean1, diag(sd1)),    	# Note: 1 
                tclusters$centers,
                tcenter, tscale)
```

```
## 3 
## 3
```

```r
## 3 
## 3

assign_cluster(mvrnorm(1, mean2, diag(sd2)),     	# Note: 2 
                tclusters$centers,
                tcenter, tscale)
```

```
## 2 
## 2
```

```r
## 1 
## 1

assign_cluster(mvrnorm(1, mean3, diag(sd3)),        	# Note: 3 
                tclusters$centers,
                tcenter, tscale)
```

```
## 1 
## 1
```

```r
## 2 
## 2

# Note 1: 
#   # This should be assigned to cluster 3. 

# Note 2: 
#   # This should be assigned to cluster 1. 

# Note 3: 
#   # This should be assigned to cluster 2. 
```




00311_example_9.18_of_section_9.2.3.R



```r
# example 9.18 of section 9.2.3 
# (example 9.18 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Reading in the book data 

library(arules)                                                	# Note: 1 
```

```
## Loading required package: Matrix
```

```
## 
## Attaching package: 'arules'
```

```
## The following objects are masked from 'package:base':
## 
##     abbreviate, write
```

```r
bookbaskets <- read.transactions("../Bookdata/bookdata.tsv.gz", 
                                     format = "single",        	# Note: 2 
                                     header = TRUE,           	# Note: 3 
                                     sep = "\t",                    	# Note: 4 
                                     cols = c("userid", "title"),    	# Note: 5 
                                     rm.duplicates = TRUE)       	# Note: 6

# Note 1: 
#   Load the arules package. 

# Note 2: 
#   Specify the file and the file format. 

# Note 3: 
#   Specify that the input file has a header. 

# Note 4: 
#   Specify the column separator (a tab). 

# Note 5: 
#   Specify the column of transaction IDs and of 
#   item IDs, respectively. 

# Note 6: 
#   Tell the function to look for and remove 
#   duplicate entries (for example, multiple entries 
#   for ???The Hobbit??? by the same user). 
```




00312_example_9.19_of_section_9.2.3.R



```r
# example 9.19 of section 9.2.3 
# (example 9.19 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Examining the transaction data 

class(bookbaskets)                              	# Note: 1 
```

```
## [1] "transactions"
## attr(,"package")
## [1] "arules"
```

```r
## [1] "transactions"
## attr(,"package")
## [1] "arules"
bookbaskets                                     	# Note: 2 
```

```
## transactions in sparse format with
##  92108 transactions (rows) and
##  220447 items (columns)
```

```r
## transactions in sparse format with
##  92108 transactions (rows) and
##  220447 items (columns)
dim(bookbaskets)                                	# Note: 3 
```

```
## [1]  92108 220447
```

```r
## [1]  92108 220447
colnames(bookbaskets)[1:5]                      	# Note: 4 
```

```
## [1] " A Light in the Storm: The Civil War Diary of Amelia Martin, Fenwick Island, Delaware, 1861"
## [2] " Always Have Popsicles"                                                                     
## [3] " Apple Magic"                                                                               
## [4] " Ask Lily"                                                                                  
## [5] " Beyond IBM: Leadership Marketing and Finance for the 1990s"
```

```r
## [1] " A Light in the Storm:[...]"
## [2] " Always Have Popsicles"
## [3] " Apple Magic"
## [4] " Ask Lily"
## [5] " Beyond IBM: Leadership Marketing and Finance for the 1990s"
rownames(bookbaskets)[1:5]                      	# Note: 5 
```

```
## [1] "10"     "1000"   "100001" "100002" "100004"
```

```r
## [1] "10"     "1000"   "100001" "100002" "100004"

# Note 1: 
#   The object is of class transactions. 

# Note 2: 
#   Printing the object tells you its 
#   dimensions. 

# Note 3: 
#   You can also use dim() to see the dimensions 
#   of the matrix. 

# Note 4: 
#   The columns are labeled by book 
#   title. 

# Note 5: 
#   The rows are labeled by customer. 
```




00313_informalexample_9.10_of_section_9.2.3.R



```r
# informalexample 9.10 of section 9.2.3 
# (informalexample 9.10 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 

basketSizes <- size(bookbaskets)
summary(basketSizes)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##     1.0     1.0     1.0    11.1     4.0 10253.0
```

```r
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
##     1.0     1.0     1.0    11.1     4.0 10250.0
```




00314_example_9.20_of_section_9.2.3.R



```r
# example 9.20 of section 9.2.3 
# (example 9.20 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Examining the size distribution 

quantile(basketSizes, probs = seq(0, 1, 0.1))         	# Note: 1 
```

```
##    0%   10%   20%   30%   40%   50%   60%   70%   80%   90%  100% 
##     1     1     1     1     1     1     2     3     5    13 10253
```

```r
##    0%   10%   20%   30%   40%   50%   60%   70%   80%   90%  100%
##     1     1     1     1     1     1     2     3     5    13 10253
library(ggplot2)                                     	# Note: 2 
ggplot(data.frame(count = basketSizes)) +
  geom_density(aes(x = count)) +
  scale_x_log10()
```

![plot of chunk 00314_example_9.20_of_section_9.2.3.R](figure/00314_example_9.20_of_section_9.2.3.R-1.png)

```r
# Note 1: 
#   Look at the basket size distribution, in 10% 
#   increments. 

# Note 2: 
#   Plot the distribution to get a better 
#   look. 
```




00315_example_9.21_of_section_9.2.3.R



```r
# example 9.21 of section 9.2.3 
# (example 9.21 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Count how often each book occurs 

bookCount <- itemFrequency(bookbaskets, "absolute")
summary(bookCount)
```

```
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
##    1.000    1.000    1.000    4.638    3.000 2502.000
```

```r
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
##    1.000    1.000    1.000    4.638    3.000 2502.000
```




00316_example_9.22_of_section_9.2.3.R



```r
# example 9.22 of section 9.2.3 
# (example 9.22 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Finding the 10 most frequent books 

orderedBooks <- sort(bookCount, decreasing = TRUE)   	# Note: 1 
knitr::kable(orderedBooks[1:10])                   	# Note: 2 
```



|                                                |    x|
|:-----------------------------------------------|----:|
|Wild Animus                                     | 2502|
|The Lovely Bones: A Novel                       | 1295|
|She's Come Undone                               |  934|
|The Da Vinci Code                               |  905|
|Harry Potter and the Sorcerer's Stone           |  832|
|The Nanny Diaries: A Novel                      |  821|
|A Painted House                                 |  819|
|Bridget Jones's Diary                           |  772|
|The Secret Life of Bees                         |  762|
|Divine Secrets of the Ya-Ya Sisterhood: A Novel |  737|

```r
# |                                                |    x|
# |:-----------------------------------------------|----:|
# |Wild Animus                                     | 2502|
# |The Lovely Bones: A Novel                       | 1295|
# |She's Come Undone                               |  934|
# |The Da Vinci Code                               |  905|
# |Harry Potter and the Sorcerer's Stone           |  832|
# |The Nanny Diaries: A Novel                      |  821|
# |A Painted House                                 |  819|
# |Bridget Jones's Diary                           |  772|
# |The Secret Life of Bees                         |  762|
# |Divine Secrets of the Ya-Ya Sisterhood: A Novel |  737|

orderedBooks[1] / nrow(bookbaskets)               	# Note: 3 
```

```
## Wild Animus 
##  0.02716376
```

```r
## Wild Animus
##  0.02716376

# Note 1: 
#   Sort the counts in decreasing order. 

# Note 2: 
#   Display the top 10 books in a nice format 

# Note 3: 
#   The most popular book in the dataset 
#   occurred in fewer than 3% of the baskets. 
```




00317_informalexample_9.11_of_section_9.2.3.R



```r
# informalexample 9.11 of section 9.2.3 
# (informalexample 9.11 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 

bookbaskets_use <- bookbaskets[basketSizes > 1]
dim(bookbaskets_use)
```

```
## [1]  40822 220447
```

```r
## [1]  40822 220447
```




00318_example_9.23_of_section_9.2.3.R



```r
# example 9.23 of section 9.2.3 
# (example 9.23 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Finding the association rules 

rules <- apriori(bookbaskets_use,                                  	# Note: 1 
                 parameter = list(support = 0.002, confidence = 0.75))
```

```
## Apriori
## 
## Parameter specification:
##  confidence minval smax arem  aval originalSupport maxtime support minlen
##        0.75    0.1    1 none FALSE            TRUE       5   0.002      1
##  maxlen target   ext
##      10  rules FALSE
## 
## Algorithmic control:
##  filter tree heap memopt load sort verbose
##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
## 
## Absolute minimum support count: 81 
## 
## set item appearances ...[0 item(s)] done [0.00s].
## set transactions ...[216031 item(s), 40822 transaction(s)] done [1.04s].
## sorting and recoding items ... [1256 item(s)] done [0.03s].
## creating transaction tree ... done [0.02s].
## checking subsets of size 1 2 3 4 5 done [0.04s].
## writing ... [191 rule(s)] done [0.00s].
## creating S4 object  ... done [0.06s].
```

```r
summary(rules)
```

```
## set of 191 rules
## 
## rule length distribution (lhs + rhs):sizes
##   2   3   4   5 
##  11 100  66  14 
## 
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   2.000   3.000   3.000   3.435   4.000   5.000 
## 
## summary of quality measures:
##     support           confidence          lift            count      
##  Min.   :0.002009   Min.   :0.7500   Min.   : 40.89   Min.   : 82.0  
##  1st Qu.:0.002131   1st Qu.:0.8113   1st Qu.: 86.44   1st Qu.: 87.0  
##  Median :0.002278   Median :0.8468   Median :131.36   Median : 93.0  
##  Mean   :0.002593   Mean   :0.8569   Mean   :129.68   Mean   :105.8  
##  3rd Qu.:0.002695   3rd Qu.:0.9065   3rd Qu.:158.77   3rd Qu.:110.0  
##  Max.   :0.005830   Max.   :0.9882   Max.   :321.89   Max.   :238.0  
## 
## mining info:
##             data ntransactions support confidence
##  bookbaskets_use         40822   0.002       0.75
```

```r
## set of 191 rules                                               	# Note: 2 
## 
## rule length distribution (lhs + rhs):sizes                     	# Note: 3 
##   2   3   4   5 
##  11 100  66  14 
## 
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   2.000   3.000   3.000   3.435   4.000   5.000 
## 
## summary of quality measures:                                   	# Note: 4 
##     support           confidence          lift            count      
##  Min.   :0.002009   Min.   :0.7500   Min.   : 40.89   Min.   : 82.0  
##  1st Qu.:0.002131   1st Qu.:0.8113   1st Qu.: 86.44   1st Qu.: 87.0  
##  Median :0.002278   Median :0.8468   Median :131.36   Median : 93.0  
##  Mean   :0.002593   Mean   :0.8569   Mean   :129.68   Mean   :105.8  
##  3rd Qu.:0.002695   3rd Qu.:0.9065   3rd Qu.:158.77   3rd Qu.:110.0  
##  Max.   :0.005830   Max.   :0.9882   Max.   :321.89   Max.   :238.0  
## 
## mining info:                                                  	# Note: 5 
##             data ntransactions support confidence
##  bookbaskets_use         40822   0.002       0.75

# Note 1: 
#   Call apriori() with a minimum support of 
#   0.002 and a minimum confidence of 0.75 

# Note 2: 
#   The number of rules found 

# Note 3: 
#   The distribution of rule lengths (in this 
#   example, most rules contain 3 items???2 on the left 
#   side, X (lhs), and one on the right side, Y 
#   (rhs)) 

# Note 4: 
#   A summary of rule quality measures, 
#   including support and confidence 

# Note 5: 
#   Some information on how apriori() was 
#   called 
```




00319_example_9.24_of_section_9.2.3.R



```r
# example 9.24 of section 9.2.3 
# (example 9.24 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Scoring rules 

measures <- interestMeasure(rules,                            	# Note: 1 
                 measure=c("coverage", "fishersExactTest"),    	# Note: 2 
                 transactions = bookbaskets_use)                	# Note: 3 
summary(measures)
```

```
##     coverage        fishersExactTest    
##  Min.   :0.002082   Min.   : 0.000e+00  
##  1st Qu.:0.002511   1st Qu.: 0.000e+00  
##  Median :0.002719   Median : 0.000e+00  
##  Mean   :0.003039   Mean   :5.080e-138  
##  3rd Qu.:0.003160   3rd Qu.: 0.000e+00  
##  Max.   :0.006982   Max.   :9.702e-136
```

```r
##     coverage        fishersExactTest
##  Min.   :0.002082   Min.   : 0.000e+00
##  1st Qu.:0.002511   1st Qu.: 0.000e+00
##  Median :0.002719   Median : 0.000e+00
##  Mean   :0.003039   Mean   :5.080e-138
##  3rd Qu.:0.003160   3rd Qu.: 0.000e+00
##  Max.   :0.006982   Max.   :9.702e-136

# Note 1: 
#   The first argument to interestMeasure() is 
#   the discovered rules 

# Note 2: 
#   Second argument is a list of interest 
#   measures to apply 

# Note 3: 
#   Last argument is a dataset to evaluate the 
#   interest measures over. This is usually the same 
#   set used to mine the rules, but it needn???t be. For 
#   instance, you can evaluate the rules over the full 
#   dataset, bookbaskets, to get coverage estimates 
#   that reflect all the customers, not just the ones 
#   who showed interest in more than one book. 
```




00320_example_9.25_of_section_9.2.3.R



```r
# example 9.25 of section 9.2.3 
# (example 9.25 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Get the five most confident rules 

library(magrittr)                        	# Note: 1  

rules %>% 
  sort(., by = "confidence") %>%           	# Note: 2  
  head(., n = 5) %>%                       	# Note: 3  
  inspect(.)                             	# Note: 4
```

```
##     lhs                                               rhs                                                support confidence      lift count
## [1] {Four to Score,                                                                                                                        
##      High Five,                                                                                                                            
##      Seven Up,                                                                                                                             
##      Two for the Dough}                            => {Three To Get Deadly : A Stephanie Plum Novel} 0.002057714  0.9882353 165.33500    84
## [2] {Harry Potter and the Order of the Phoenix,                                                                                            
##      Harry Potter and the Prisoner of Azkaban,                                                                                             
##      Harry Potter and the Sorcerer's Stone}        => {Harry Potter and the Chamber of Secrets}      0.002866102  0.9669421  72.82751   117
## [3] {Four to Score,                                                                                                                        
##      High Five,                                                                                                                            
##      One for the Money,                                                                                                                    
##      Two for the Dough}                            => {Three To Get Deadly : A Stephanie Plum Novel} 0.002082211  0.9659091 161.59976    85
## [4] {Four to Score,                                                                                                                        
##      Seven Up,                                                                                                                             
##      Three To Get Deadly : A Stephanie Plum Novel,                                                                                         
##      Two for the Dough}                            => {High Five}                                    0.002057714  0.9655172 180.79975    84
## [5] {High Five,                                                                                                                            
##      Seven Up,                                                                                                                             
##      Three To Get Deadly : A Stephanie Plum Novel,                                                                                         
##      Two for the Dough}                            => {Four to Score}                                0.002057714  0.9655172 167.72062    84
```

```r
# Note 1: 
#   Attach magrittr to get pipe notation. 

# Note 2: 
#   Sort rules by confidence. 

# Note 3: 
#   Get the first 5 rules. 

# Note 4: 
#   Call inspect() to pretty-print the rules. 
```




00321_example_9.26_of_section_9.2.3.R



```r
# example 9.26 of section 9.2.3 
# (example 9.26 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Finding rules with restrictions 

brules <- apriori(bookbaskets_use,
                parameter = list(support = 0.001,                       	# Note: 1 
                                 confidence = 0.6),
                appearance = list(rhs = c("The Lovely Bones: A Novel"),  	# Note: 2 
                                  default = "lhs"))                      	# Note: 3 
```

```
## Apriori
## 
## Parameter specification:
##  confidence minval smax arem  aval originalSupport maxtime support minlen
##         0.6    0.1    1 none FALSE            TRUE       5   0.001      1
##  maxlen target   ext
##      10  rules FALSE
## 
## Algorithmic control:
##  filter tree heap memopt load sort verbose
##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
## 
## Absolute minimum support count: 40 
## 
## set item appearances ...[1 item(s)] done [0.00s].
## set transactions ...[216031 item(s), 40822 transaction(s)] done [0.85s].
## sorting and recoding items ... [3172 item(s)] done [0.03s].
## creating transaction tree ... done [0.02s].
## checking subsets of size 1 2 3 4 5 6 7 8 done [0.22s].
## writing ... [46 rule(s)] done [0.04s].
## creating S4 object  ... done [0.06s].
```

```r
summary(brules)
```

```
## set of 46 rules
## 
## rule length distribution (lhs + rhs):sizes
##  3  4 
## 44  2 
## 
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   3.000   3.000   3.000   3.043   3.000   4.000 
## 
## summary of quality measures:
##     support           confidence          lift           count      
##  Min.   :0.001004   Min.   :0.6000   Min.   :21.81   Min.   :41.00  
##  1st Qu.:0.001029   1st Qu.:0.6118   1st Qu.:22.24   1st Qu.:42.00  
##  Median :0.001102   Median :0.6258   Median :22.75   Median :45.00  
##  Mean   :0.001132   Mean   :0.6365   Mean   :23.14   Mean   :46.22  
##  3rd Qu.:0.001219   3rd Qu.:0.6457   3rd Qu.:23.47   3rd Qu.:49.75  
##  Max.   :0.001396   Max.   :0.7455   Max.   :27.10   Max.   :57.00  
## 
## mining info:
##             data ntransactions support confidence
##  bookbaskets_use         40822   0.001        0.6
```

```r
## set of 46 rules
##
## rule length distribution (lhs + rhs):sizes
##  3  4
## 44  2
##
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
##   3.000   3.000   3.000   3.043   3.000   4.000
##
## summary of quality measures:
##     support           confidence          lift           count      
##  Min.   :0.001004   Min.   :0.6000   Min.   :21.81   Min.   :41.00  
##  1st Qu.:0.001029   1st Qu.:0.6118   1st Qu.:22.24   1st Qu.:42.00  
##  Median :0.001102   Median :0.6258   Median :22.75   Median :45.00  
##  Mean   :0.001132   Mean   :0.6365   Mean   :23.14   Mean   :46.22  
##  3rd Qu.:0.001219   3rd Qu.:0.6457   3rd Qu.:23.47   3rd Qu.:49.75  
##  Max.   :0.001396   Max.   :0.7455   Max.   :27.10   Max.   :57.00  
##
## mining info:
##             data ntransactions support confidence
##  bookbaskets_use         40822   0.001        0.6

# Note 1: 
#   Relax the minimum support to 0.001 and the 
#   minimum confidence to 0.6. 

# Note 2: 
#   Only ???The Lovely Bones??? is allowed to appear 
#   on the right side of the rules. 

# Note 3: 
#   By default, all the books can go into the 
#   left side of the rules. 
```




00322_example_9.27_of_section_9.2.3.R



```r
# example 9.27 of section 9.2.3 
# (example 9.27 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Inspecting rules 

brules %>% 
  sort(., by = "confidence") %>%   
  lhs(.) %>%                                     	# Note: 1  
  head(., n = 5) %>%         
  inspect(.)                                               
```

```
##     items                                                       
## [1] {Divine Secrets of the Ya-Ya Sisterhood: A Novel,           
##      Lucky : A Memoir}                                          
## [2] {Lucky : A Memoir,                                          
##      The Notebook}                                              
## [3] {Lucky : A Memoir,                                          
##      Wild Animus}                                               
## [4] {Midwives: A Novel,                                         
##      Wicked: The Life and Times of the Wicked Witch of the West}
## [5] {Lucky : A Memoir,                                          
##      Summer Sisters}
```

```r
##   items
## 1 {Divine Secrets of the Ya-Ya Sisterhood: A Novel,
##    Lucky : A Memoir}
## 2 {Lucky : A Memoir,
##    The Notebook}
## 3 {Lucky : A Memoir,
##    Wild Animus}
## 4 {Midwives: A Novel,
##    Wicked: The Life and Times of the Wicked Witch of the West}
## 5 {Lucky : A Memoir,
##    Summer Sisters}

# Note 1: 
#   Get the left hand side of the sorted rules. 
```




00323_example_9.28_of_section_9.2.3.R



```r
# example 9.28 of section 9.2.3 
# (example 9.28 of section 9.2.3)  : Unsupervised methods : Association rules : Mining association rules with the arules package 
# Title: Inspecting rules with restrictions 

brulesSub <- subset(brules, subset = !(lhs %in% "Lucky : A Memoir"))  	# Note: 1 
brulesSub %>%
  sort(., by = "confidence") %>%
  lhs(.) %>%
  head(., n = 5) %>%
  inspect(.)
```

```
##     items                                                       
## [1] {Midwives: A Novel,                                         
##      Wicked: The Life and Times of the Wicked Witch of the West}
## [2] {She's Come Undone,                                         
##      The Secret Life of Bees,                                   
##      Wild Animus}                                               
## [3] {A Walk to Remember,                                        
##      The Nanny Diaries: A Novel}                                
## [4] {Beloved,                                                   
##      The Red Tent}                                              
## [5] {The Da Vinci Code,                                         
##      The Reader}
```

```r
brulesConf <- sort(brulesSub, by="confidence")

inspect(head(lhs(brulesConf), n = 5))
```

```
##     items                                                       
## [1] {Midwives: A Novel,                                         
##      Wicked: The Life and Times of the Wicked Witch of the West}
## [2] {She's Come Undone,                                         
##      The Secret Life of Bees,                                   
##      Wild Animus}                                               
## [3] {A Walk to Remember,                                        
##      The Nanny Diaries: A Novel}                                
## [4] {Beloved,                                                   
##      The Red Tent}                                              
## [5] {The Da Vinci Code,                                         
##      The Reader}
```

```r
##   items
## 1 {Midwives: A Novel,
##    Wicked: The Life and Times of the Wicked Witch of the West}
## 2 {She's Come Undone,
##    The Secret Life of Bees,
##    Wild Animus}
## 3 {A Walk to Remember,
##    The Nanny Diaries: A Novel}
## 4 {Beloved,
##    The Red Tent}
## 5 {The Da Vinci Code,
##    The Reader}

# Note 1: 
#   Restrict to the subset of rules where 
#   Lucky is not in the left 
#   side. 
```


