{:title "Multi Target Regression"
 :layout :post
 :page-index 0
 :tags ["Regression" "Multi target regression"]
 }

####  Multi Target Regression

I recently had an opportunity to work on a supervised machine learning problem where the target variable is multi valued. An initial search of the machine learning libraries revealed that there is little support in popular Open source ML libraries for the same. 

So here's a quick intro in the topic, and a dive into some of the libraries from the JVM ecosystem that support the same. 

Machine Learning classifiers usually support a single target variable. In the case of regression models, the target is real valued, whereas in classification model, the target is binary or multivalued. 

In the realm of classification models, a problem with multiple target variables is called multi-label classification. The Weka toolkit unfortunately doesn't support any kind of multi label output. The Mulan toolkit which extends Weka, implements several algorithms for the same. 

In the realm of regression models, as a beginner, I found the nomenclature a bit confusing. 

* A Simple regression model is one that attempts to fit a linear regression model with a single explanatory variable.
* 




