# Some hightlights of my suggested approach

### Feature extraction
The current set of features (with 15 features) is rather small. I suggest that we use more features. Of course increasing the number of features while having a small training set (just about 50 data points) will increase the likelihood of overfitting. When this happens we can apply regularization methods like L1, L2 or dropout to solve the overfitting problem.

* The first set of features we can try is from `dc.feat.RDKitDescriptors` which generates 200 features. They describe chemical and physical properties of a molecule. I beleive that the 15 features you generated from **mayachemtools** is just a subset of the one generated from `dc.feat.RDKitDescriptors`. We can use this set of feature with both shallow and deep learning.

* The next step to try is to use molecular fingerprints from `DeepChem`. I think the two most commonly used approaches are "Extended Connectivity Fingerprint" (or ECFP) and `GaphConv`. `GaphConv` is state of the art (https://arxiv.org/abs/1509.09292). However it is an embedding approach and we have to use it as the top layer in a neural network model. If we want to use `GaphConv` fingerprint for shallow models such as Random Forest or XGBoost, we have to train a unsuppervised embedding model first.


### Models
* First we should try *Linear Regression* and *Logistic Regression* as baseline models for regression and classification problems. Then we can use more complex model such as *Random Forest*, *XGBoost* and deeplearning models. From my experience, `XGBoost` works extremly well for classification problems, so it is worth trying it.


### Training and model selection
* Since our training set is rather small, we should not split it into three sets (train, validation and test). Instead we should split into two: train and test. Since we don't have a validation set, we can use cross-validation to optimize the hyperparameters and select the best overall model using the test set. After having the best model, we retrain using all the data before doing prediction on the ZINC dataset.

* To optimize the hyperparameter, we should use the library `hyperopt` which is the most popular Bayesian Optimization library for machine learning.


**However, I think these suggested approaches may only work if we have more training data. The Current dataset with 52 data points is too small.**
