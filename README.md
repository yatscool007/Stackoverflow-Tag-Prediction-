# Stackoverflow-Tag-Prediction-
<img src = https://github.com/yatscool007/Stackoverflow-Tag-Prediction-/blob/master/Images/sWyuy4Y.jpg>


## The Methodology of approaching the problem
In this case study the probelem that we wanted to solve was how to label the question posed by the user so that the right persson can be able to answer them ,so we posed it as a multilabel classification machine learning problem and the error metric we chose here were 'micro_f1 score','macro_f1 score' and hamming loss where the micro_f1 score was the most important one as the class imbalance can help us a lot in such case where some of the tags or labels are present in most of the questions while some are prsent in very few.

Next important part was how to go about approcahing the propblem,so we decided to featurize the data for text using text featurization techniques like Bag of words and TFIDF.So we went about preprocesing the data by cleaning,removing stopwords and Stemming(snowball stemmer) ,as being a multilabel classification problem which we ought to perform using OneVsRest Classifier,hence we used something like partial coverage where the 5000 top tags i.e tags which appaeared most frequent were able to cover around 99% of the questions .

What we observed was that as most of the text part was in title of the question while the summary part covered code mostly 
so we give 3 times more weightage to the title text and used 0.5M datapoints for it and again featurized the text data using
TFIDF and Bag of words.

The reason we did not use classifier chain here as it works with dense data and the bag of words and tfidf gives sparse data 
matrix and multilearn is not able to convert the sparse data to dense as it gives memory error

Finally we used Logistic Regression and Linear SVM with One Vs Rest Classifier here and tuned the values of alpha in SGDClassifier with log loss and hinge loss to get the best model,on 0.2M datapoints with top 5000 features 

## Results:
<img src = https://github.com/yatscool007/Stackoverflow-Tag-Prediction-/blob/master/Images/Results.PNG>
