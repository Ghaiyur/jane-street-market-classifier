# Jane Street Market Classifier

## Introduction 

This dataset contains an anonymized set of features, feature_{0...129}, representing real stock market data. Each row in the dataset represents a trading opportunity, for which you will be predicting an action value: 1 to make the trade and 0 to pass on it. Each trade has an associated weight and resp, which together represents a return on the trade. The date column is an integer which represents the day of the trade, while ts_id represents a time ordering. In addition to anonymized feature values, you are provided with metadata about the features in features.csv.

In the training set, train.csv, you are provided a resp value, as well as several other resp_{1,2,3,4} values that represent returns over different time horizons. These variables are not included in the test set. Trades with weight = 0 were intentionally included in the dataset for completeness, although such trades will not contribute towards the scoring evaluation.

## The curious case of Dropout rate 

The Dropout layer randomly sets input units to 0 with a frequency of rate at each step during training time, which helps prevent overfitting.
Inputs not set to 0 are scaled up by 1/(1 - rate) such that the sum over all inputs is unchanged.
https://keras.io/api/layers/regularization_layers/dropout/

Dropout is a regularization technique. You should use it only to reduce variance (validation performance vs training performance).
It is not intended to reduce the bias, and you should not use it in this way.
Dropout works by probabilistically removing, or “dropping out,” inputs to a layer, which may be input variables in the data sample or activations from a previous layer. With dropout (dropout rate less than some small value), the accuracy will gradually increase and loss will gradually decrease.
When you increase dropout beyond a certain threshold, it results in the model not being able to fit properly.
https://stackoverflow.com/questions/59044351/can-dropout-increases-training-data-performance

Dropout prevents overfitting due to a layer's "over-reliance" on a few of its inputs. Because these inputs aren't always present during training (i.e. they are dropped at random), the layer learns to use all of its inputs, improving generalization.
https://stats.stackexchange.com/questions/374742/does-dropout-regularization-prevent-overfitting-due-to-too-many-iterations

The default interpretation of the dropout hyperparameter is the probability of training a given node in a layer, where 1.0 means no dropout, and 0.0 means no outputs from the layer.
A good value for dropout in a hidden layer is between 0.5 and 0.8. Input layers use a larger dropout rate, such as of 0.8. https://machinelearningmastery.com/dropout-for-regularizing-deep-neural-networks/
