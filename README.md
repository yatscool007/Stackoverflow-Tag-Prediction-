# Stackoverflow-Tag-Prediction-
<img src = https://github.com/yatscool007/Stackoverflow-Tag-Prediction-/blob/master/Images/sWyuy4Y.jpg>


<h1>1. Business Problem </h1>

<h2> 1.1 Description </h2>
<p style='font-size:18px'><b> Description </b></p>
<p>
Stack Overflow is the largest, most trusted online community for developers to learn, share their programming knowledge, and build their careers.<br />
<br />
Stack Overflow is something which every programmer use one way or another. Each month, over 50 million developers come to Stack Overflow to learn, share their knowledge, and build their careers. It features questions and answers on a wide range of topics in computer programming. The website serves as a platform for users to ask and answer questions, and, through membership and active participation, to vote questions and answers up or down and edit questions and answers in a fashion similar to a wiki or Digg. As of April 2014 Stack Overflow has over 4,000,000 registered users, and it exceeded 10,000,000 questions in late August 2015. Based on the type of tags assigned to questions, the top eight most discussed topics on the site are: Java, JavaScript, C#, PHP, Android, jQuery, Python and HTML.<br />
<br />
</p>

<p style='font-size:18px'><b> Problem Statemtent </b></p>
Suggest the tags based on the content that was there in the question posted on Stackoverflow.
<p style='font-size:18px'><b> Source:  </b> https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/</p>

<h2> 1.2 Source / useful links </h2>
Data Source : https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/data <br>
Youtube : https://youtu.be/nNDqbUhtIRg <br>

<h2> 1.3 Real World / Business Objectives and Constraints </h2>

1. Predict as many tags as possible with high precision and recall.

2. Incorrect tags could impact customer experience on StackOverflow.

3. No strict latency constraints.

<h1>2. Machine Learning problem </h1>

<h2> 2.1 Data </h2>

<h3> 2.1.1 Data Overview </h3>

Refer: https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/data
<br>
All of the data is in 2 files: Train and Test.<br />
<pre>
<b>Train.csv</b> contains 4 columns: Id,Title,Body,Tags.<br />
<b>Test.csv</b> contains the same columns but without the Tags, which you are to predict.<br />
<b>Size of Train.csv</b> - 6.75GB<br />
<b>Size of Test.csv</b> - 2GB<br />
<b>Number of rows in Train.csv</b> = 6034195<br />
</pre>
The questions are randomized and contains a mix of verbose text sites as well as sites related to math and programming. The number of questions from each site may vary, and no filtering has been performed on the questions (such as closed questions).<br />
<br />
__Data Field Explaination__

Dataset contains 6,034,195 rows. The columns in the table are:<br />
<pre>
<b>Id</b> - Unique identifier for each question<br />
<b>Title</b> - The question's title<br />
<b>Body</b> - The body of the question<br />
<b>Tags</b> - The tags associated with the question in a space-seperated format (all lowercase, should not contain tabs '\t' or ampersands '&')<br />
</pre>

<br />

<h2>2.2 Mapping the real-world problem to a Machine Learning Problem </h2>
<h3> 2.2.1 Type of Machine Learning Problem </h3>

<p> It is a multi-label classification problem  <br>
<b>Multi-label Classification</b>: Multilabel classification assigns to each sample a set of target labels. This can be thought as predicting properties of a data-point that are not mutually exclusive, such as topics that are relevant for a document. A question on Stackoverflow might be about any of C, Pointers, FileIO and/or memory-management at the same time or none of these. <br>
__Credit__: http://scikit-learn.org/stable/modules/multiclass.html
</p>

<h3>2.2.2 Performance metric </h3>

<b>Micro-Averaged F1-Score (Mean F Score) </b>: 
The F1 score can be interpreted as a weighted average of the precision and recall, where an F1 score reaches its best value at 1 and worst score at 0. The relative contribution of precision and recall to the F1 score are equal. The formula for the F1 score is:

<i>F1 = 2 * (precision * recall) / (precision + recall)</i><br>

In the multi-class and multi-label case, this is the weighted average of the F1 score of each class. <br>

<b>'Micro f1 score': </b><br>
Calculate metrics globally by counting the total true positives, false negatives and false positives. This is a better metric when we have class imbalance.
<br>

<b>'Macro f1 score': </b><br>
Calculate metrics for each label, and find their unweighted mean. This does not take label imbalance into account.
<br>

https://www.kaggle.com/wiki/MeanFScore <br>
http://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html <br>
<br>
<b> Hamming loss </b>: The Hamming loss is the fraction of labels that are incorrectly predicted. <br>
https://www.kaggle.com/wiki/HammingLoss <br>

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
