{:title "Models with multiple output variables"
 :layout :post
 :page-index 0
 :draft? false
 :tags ["Regression" "Multi target regression"]
 }


I recently had an opportunity to work on a supervised machine learning problem where the target variables are real and multi valued. An initial search of the machine learning libraries revealed that popular Open source ML libraries have little support for the same. 

Here's a quick intro in the topic, and (in a later post), a dive into some of the libraries from the JVM ecosystem. 

###  Multi Target Regression

Machine Learning classifiers usually support a single target variable. In the case of regression models, the target is real valued, whereas in a classification model, the target is binary or multivalued. 

For classification models, a problem with multiple target variables is called _multi-label classification_. 
In the realm of regression models, as a beginner, I found the nomenclature a bit confusing. 

* A *Simple regression* model is one that attempts to fit a linear regression model with a single explanatory/independent variable.
* *Multiple regression* model is one that attempts to predict a dependent variable which is based on the value of two or more independent variables. Example: can daily cigarette consumption be predicted based on smoking duration, age when started smoking, income, gender etc.
* *Multi target regression* is the term used when there are multiple dependent variables. If the target variables are categorical, then it is called multi-label or multi-target classification, and if the target variables are numeric, then multi-target (or multi-output) regression is the name commonly used.


### Discussion of several MTR regression models approaches 

[This paper](http://cig.fi.upm.es/articles/2015/Borchani-2015-WDMKD.pdf) has a good overview of the model approaches to multi-target regression.

It divides methods into these categories:

* Problem transformation

 * Methods such as Single Target Regression, Regressor Chains 

* Algorithm transformation

 * Methods such as multi-output Support Vector Regression, Multi-Output Regression Trees and rule methods

 
## Multi target regression(MTR) using Clustering and Decision trees.

For the rest of the discussion, we shall focus on a single method, that is, decision trees and ensembles of decision trees for MTR.

### Using trees for clustering, aka Predictive Clustering Trees (PCT)

We need to first take a look at Predictive Clustering Trees (PCT), which is the foundation on which decision trees for MTR are built on.


[Top down induction of clustering trees](http://arxiv.org/pdf/cs/0011032.pdf) is the paper that discusses use of decision trees for clustering. 
In a classification role, the leaves of a decision tree are classes or labels. 

However, in a clustering perspective 

* each leaf node corresponds to a concept or a cluster, 
* the nodes in the tree indicate a taxonomy or hierarchy. 

The paper states that:

` A clustering tree is a decision tree where leaves don't contain classes and where each node & leaf correspond to a cluster. `

A walk down the tree (starting from the root and then down to a leaf node) can be compared to moving from a large cluster (at the top) to smaller and smaller clusters as we near the leaf nodes.


### Measures for Distance between clusters 

In supervised classification, the distance measure is the class of the target variable. 

In unsupervised or semi-supervised learning, the clustering is done based on a distance metric (e.g Euclidean). If a small sample is labeled, all the (unlabeled) samples in a node are assigned the class of the majority of the labeled samples. 
The cluster can be used to predict many or all of the target attributes.

For a problem with say, 5 target variables, the leaf node would contain a vector of length 5, each representing one of the target variables.

### Measuring the quality of the PCT

For regression, the default metric used is the relative error, which is

* the mean squared error of predictions divided by the mean squared error of a default hypothesis always predicting the mean.

### How to build the tree

To build a tree, we first decide on:

* a distance measure (which compares distances between 2 instances)
* a prototype function (which computes a cluster centroid so that 2 clusters can be compared)

Given that each node of a tree corresponds to a cluster, the decision tree algorithm is then adapted to select in each node the test that will maximize the distance between the resulting clusters in its subnodes.

* Given a Cluster C and a Test T, the best test T is one that maximizes the distance between sub-cluster C1 and C2. 
* If the prototype (something similar to cluster centroid) is the mean, maximizing inter-cluster distances corresponds to minimum intra-cluster distance. This means the tree build clusters that are distant from each other, and there is little dispersion within the cluster. 


* Stopping criteria: Tree growth (or rather node splitting) is stopped when the subtrees differ significantly. For regression, the F-test (which measure difference in variance between 2 populations) is used.

* Pruning: the trees are grown from training data, and a validation set is used to prune trees. 

`For each node of the tree the quality of the tree if it were pruned at that node Q′ is compared with the quality Q of the unpruned tree. If Q′ > Q then the tree is pruned. `


## Comparison of ensemble methods using decision trees for MTR

In the section above, we learned how a single decision tree can be built. However, the performance is greatly improved when a Forest of trees are used.

[This paper](https://pdfs.semanticscholar.org/ca94/c65320c6023bf00ef6db30e6815f3aa07aa4.pdf) discusses the use of ensembles for multi-objective decision trees. 

### Bagging:

* Pick bootstrap samples from the dataset and use these to build one tree. The bootstrap samples are obtained by sampling with replacement from the training set.

### Random Forest

* In addition to bagging, at each node in the decision tree, a random subset of features is used to split the nodes on, instead of all the features as in Bagging.

The paper's results, in summary

* Ensembles (of MODTs) perform better than single decision trees.
* Random Forest Ensembles (of MODTs) perform better than Bagging Ensembles.


## Why use multi-output models instead of a combination of single-output models like regression?

* Models using single outputs take longer to train and are computationally expensive
* They optimize for the single target rather than all the targets together
* They do not use relationships between the target variables (Y)
* MTR regression models are simpler than a stack of Single Target models
* MTR trees provide human-readable predictive models and the models are easily interpreted.


### Tools available for MTR 


I found that the [Clus toolkit](http://Clus.sourceforge.net) has a good blend of high performance and robustness

* The documentation is excellent
* The toolkit has several methods for both multi-target classification and regression
* It also support rule-based induction and clustering.
* The ensemble models (Bagging, RandomForest) that I used can be read and interpreted easily.

Some of the newer methods (post 2012) have been implemented as an extension of the Mulan toolkit, available at this [Github link](https://github.com/lefman/mulan-extended). Although these methods such as Random Linear Target Combinations report better performance than ensemble models, I found that the toolkit is not as mature as the Clus toolkit.

Here's a [discussion on StackExchange](https://stats.stackexchange.com/questions/176515/resources-for-learning-about-multiple-target-techniques) on the same topic.

In a later post, we'll discuss how to use the Clus toolkit to run an MTR model.

