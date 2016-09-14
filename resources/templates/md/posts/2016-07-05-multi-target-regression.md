{:title "Multi Target Regression"
 :layout :post
 :page-index 0
 :draft? true
 :tags ["Regression" "Multi target regression"]
 }

####  Multi Target Regression

I recently had an opportunity to work on a supervised machine learning problem where the target variable is multi valued. An initial search of the machine learning libraries revealed that there is little support in popular Open source ML libraries for the same. 

So here's a quick intro in the topic, and a dive into some of the libraries from the JVM ecosystem that support the same. 

Machine Learning classifiers usually support a single target variable. In the case of regression models, the target is real valued, whereas in classification model, the target is binary or multivalued. 

In the realm of classification models, a problem with multiple target variables is called multi-label classification. The Weka toolkit unfortunately doesn't support any kind of multi label output. The Mulan toolkit which extends Weka, implements several algorithms for the same. 

In the realm of regression models, as a beginner, I found the nomenclature a bit confusing. 

* A Simple regression model is one that attempts to fit a linear regression model with a single explanatory/independent variable.
* Multiple regression model is one that attempts to predict a dependent variable which is based on the value of two or more independent variables. Example: can daily cigarette consumption be predicted based on smoking duration, age when started smoking, income, gender etc.
* Multi target regression is the term used when there are multiple dependent variables. If the target variables are categorical, then it is called multi-label or multi-target classification, nd if the target variables are numeric, then multi-target regression is the preferred nomenclature.


From the docs ` most decision tree learners induce classification or regression trees, PCT learns trees that are interpreted as cluster hierarchies.`

[Top down induction of clustering trees](http://arxiv.org/pdf/cs/0011032.pdf) is the paper that discusses use of decision trees for clustering. In a classification role, the leaves of a decision tree are classes or labels, whereas from a clustering perspective, each leaf node corresponds to a concept or a cluster, and the nodes in the tree indicate a taxonomy or hierarchy. 

` A clustering tree is a decision tree where leaves don't contain classes and where each node & leaf correspond to a cluster.

Given a distance measure for clusters and the view that each node of a tree corresponds to a cluster, the decision tree algorithm is then adapted to select in each node the test that will maximize the distance between the resulting clusters in its subnodes.
`

### Applications 

In supervised  learning, the distance measure is the class of the target variable. 

In unsupervised or semi-supervised learning, the clustering is done based on a distance metric (e.g Euclidean). If a small sample is labeled, all the (unlabeled) samples in a node are assigned the class of the majority of the labeled samples. 
The cluster can be used to predict many or all of the target attributes.

### Measuring the quality of the PCT

`For regression an often used criterion is the relative error, which is the mean squared error of predictions divided by the mean squared error of a default hypothesis always predicting the mean.`

### How to build the tree

* Given a Cluster C and a Test T, the best test T is one that maximizes the distance between sub-cluster C1 and C2. If the prototype (TODO) is the mean, maximizing inter-cluster distances corresponds to minimum intra-cluster distance. the resulting clusters would also have the least intra-cluster distance.

* Stopping criteria: Tree growth (or rather node splitting) is stopped when the subtrees differ significantly. For regression, the F-test (which measure difference in variance between 2 populations) is used.

* Pruning: the trees are grown from training data, and a validation set is used to prune trees. 
`For each node of the tree the quality of the tree if it were pruned at that node Q′ is compared with the quality Q of the unpruned tree. If Q′ > Q then the tree is pruned. `


## Comparison of methods for MTR

[This paper](https://pdfs.semanticscholar.org/ca94/c65320c6023bf00ef6db30e6815f3aa07aa4.pdf) discusses the use of ensembles for multi-objective decision trees. 

### Bagging:

* Pick bootstrap samples from the dataset and use these to build one classifier. The bootstrap samples are obtained by sampling with replacement from the training set.

### Random Forest

* In addition to bagging, at each node in the decision tree, a random subset of features is used to split the nodes on, instead of all the features as in Bagging.

The paper's results, in summary

* Ensembles (of MODTs) are better than single decision trees.
* Random Forest Ensembles (of MODTs) perform better than Bagging Ensembles.

#### Discussion of several MTR regression models in current literature

[This paper](http://cig.fi.upm.es/articles/2015/Borchani-2015-WDMKD.pdf) has a good overview of the model approaches to multi-target regression.

It divides methods into these categories

* Problem transformation
  Methods such as single target regression, regressor chains 
* Algorithm transformation
  Mthods such as multi-output SVR, multi-output regression trees and rule methods

### Why use multi-output models instead of a combination of single-output models like regression?

* Models using single outputs take longer to train (computationally complex)
* They optimize for the single target rather than all the targets together
* They do not use relationships between the target variables (Y)
* MTR regression models are simpler than a stack of ST models
* MTR trees provide human-readable predictive models and the models are easily interpreted.








