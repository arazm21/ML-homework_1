# ML-homework_1

the competition 

    house-prices-advanced-regression-techniques

is a simple kaggle competition as an intro to machine learning, where we have to predict the house of a property based on about 80 features.it has only 1400 rows, which gives us the chance to test different methods more effectively and quickly.

firstly, my goal is to get rid of NaN values. for categorical data, i decided to replace NaN value with UNKNOWN. the reason is the following - 
many features contain a lot of information in NaN values, for example, if the pool quality is NaN, that often means that the house does not have a pool which is, of course related to the cost.
for numerical features i decided to simply use 0, as i gave slightly better results in my tests compared to an average.

second, my goal is to remove object types and turn them into number either using label (WOE) or one-hot encoding.

if we take a look at the following example:

    PoolQC: Pool quality
		
       Ex	Excellent
       Gd	Good
       TA	Average/Typical
       Fa	Fair
       NA	No Pool

I will assign 0 to the lowest possible feature (NA in this case) and 4 to the highest (Ex in this case), because there is a clear hierarchy of 'goodness' which can easily be turned into numbers. I will do the same for all similar features.
U use the method called kfold mean encoding. it works like WOE, except it can be used in regression tasks. unlike many other ways to fill categorical data, this method reduces bleeding significantly by dividing the dataset into many parts and looking at other chunks to make decisions on how to replace the data. for cases where there are fewer than 3 unique values, i simply use OHE.

for scaler, i chose a normal scaler, as the minmax scaler gave me far worse performance. i think the reason is that outliers have too big of an effect and the target does not really increase linearly, therefore minmax gives very wrong results.
i wanted to use IQR filtering in my attempts, however in the end chose not to, because i believe that 1000 rows of data is not enought to trully determine what is an outlier and what is simply a different datapoint, and i thought that might have given me wrong estimation. 

next up, we have a correlation based feature selection. i wanted my model to have the ability to learn from many different sources and i use correlation filtering to get rid of similar features, because using many of them will not give me new information. between the two, i choose to retain the one most closely related to the target.

and at last, i use RFE to get my feature number down. for linear models, i found that having fewer features was better.

i decided to use a random forrest model, as it is better than linear regression and i thought that boosting models whould be "cheating". with linear regression, i had 0.191 RMSLE, with random forrest - 0.163. 

in the end, i got a result of Score: 0.16373

for hyperparameter tuning, i used gridsearch, simply because the dataset was small enought for me to be able to use it. i tested several models and in the end decided to use random forest, as i already explained. logistic regression was complete garbage, and while linear regression did decently for most of the guesses, it was really off when it came to numbers that where towards the edge and that caused big spikes in the loss function. random forrest is what i chose, althought XGBoost or somethingsimilar would, of course, be far better for this task. given that my validation and test scores where almost identical, i believe it is fair to say that my model was not over or underfitting.