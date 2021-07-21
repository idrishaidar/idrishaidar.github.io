---
title: "[WIP] How (not) to use PCA"
layout: post
---

There are several purposes of applying dimensionality reduction into a dataset. We often times need to communicate our high-dimensional dataset into a simpler visualization, either in 2D or 3D. Sometimes our model training seems very slow because of the number of features or the size of the data. Sometimes we just need to drop some unnecessary details and noise from our data. 

(PCA diagram)

If you're someone who have been taking benefits from dimensionality reduction, you might need to revisit the implementations you've done for several problems. In many cases, this technique could look beneficial, but in some particular others, it turns out wouldn't be necessary to use it or there would even be some pitfalls. 

Let's talk about Principal Component Analysis (PCA) as a matter of problem framing. Suppose that you'd like to reduce our dataset dimensionality in order to reduce the training duration of your regression or classification model. While implementing PCA, you suddenly recall about how reducing features from n to k features could simplify the model and help avoiding overfitting. You also wonder how PCA could help you find the so-called "most important features" to predict your label. You then proceed your PCA implementation and finally obtain the principal components.

With something you reflected about a while ago, you then re-train your model with the PCA-reduced dataset. You come up with this comparison (before and after PCA).

(comparison table)

Looks like it actually helps to achieve a faster training! Not only the training duration, but there is also a little but noticable performance increase.

Well, what could have gone wrong here?

When we apply PCA to our training set, most likely we do not include your label as a consideration. Simply put, if you're using `scikit-learn`, you don't `fit()` the PCA into your label together with the features. Only the features 
will be mapped, let's say, from $$X$$ features to $$Z$$ principal components. The label is ignored by the algorithm. 

As the result, our dimensionality reduction algorithm doesn't know any relationship between our features and our label in the training set. It didn't find these principal components based on your label. They were based on how the projected axes will maximize the variance retained from our original dataset. 

In other words, the principal components (PC) that we got aren't directly associated with the label you would like to predict. Even if we now have way less PC than our initial features, PCA can't be considered as a good strategy to avoid overfitting. If we coincidentally got a prediction improvement after training our supervised model with PCA-reduced dataset, it doesn't mean that our PC are the "most important features" in that particular case. Also, PCA could throw some meaningful information which can supposedly help making prediction.

A different case might happen if you use the model-based feature selection, for instance. Your model would know what is going on between features and label. So it would be more make sense to re-select your features based on their contribution on predicting the label.

Having said that, it doesn't mean that re-train a supervised model with PCA-reduced dataset isn't allowed at all. However, one should alter his/her intention or thinking process of implementing dimensionality reduction. Instead of to obtain "the most important features" for supervised learning, just focus on obtaining fewer features to shorten the training duration or visualization matters, for instances.

Consider something like <a href="https://sebastianraschka.com/Articles/2014_python_lda.html">Linear Discriminant Analysis (LDA)</a>, which unlike PCA, LDA considers the classes when doing the transformation. It computes the axes that will maximize the class separation, so if you visualize your principal components afterwards, for instance, you would get distinguishable data points between classes.

If your main concern is still about overfitting, you can use regularization instead. 

(l2 equation)

As we might know, the regularization term is added to the loss function which is associated with the predicted label.

Oh yes, we only need to apply PCA to our training set, but the same mapping (or parameters) obtained from the training can be re-applied to our validation or test set.

If this short explanation is not digestible enough for you, see how <a href="https://www.coursera.org/learn/machine-learning/lecture/RBqQl/advice-for-applying-pca">Andrew Ng explains</a> the very same issue in his Machine Learning course in Coursera. 