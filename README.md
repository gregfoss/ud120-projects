<H1>Enron Email Corpus - Finding Persons of Interest</H1>
There are not many US citizens who have not heard of Enron; when they do it is in the context of willful corporate greed and fraud. Before its bankruptcy in late 2001 it employed approximately 20,000 employees. (https://en.wikipedia.org/wiki/Enron) This is a project to take the famous Enron Email corpus which is a large dataset of nearly 600,000 emails generated from 158 Enron employees and identify persons of interest (poi). (https://en.wikipedia.org/wiki/Enron_Corpus). A poi is defined by the list of those individuals who were involved in the scandal.

Within the Final_project there is an IPython Notebook titled <i>Trying Enron with Pandas</i>. It is within this notebook that the work summarized here is found. The poi_id.py file is a requirement for course completion, but not my original or complete workspace. The poi_id.py file will have just the minimum of the requirement to fullfil the course and only the single algorithm that I found most effective. 

There were three major sources of insight for this project. The first is, of course, the Udacity <i>Introduction to Machine Learning</i> course. The second is <i>Python Machine Learning</i> by Sebastian Raschka. The third is the work of Jason Brownlee at MachineLearningMastery.com

<h3>Exploring and Removing Obvious Problems</h3>
There were 146 features and 18 pois in this dataset. 
Outliers were clearly identified in the salary and bonus features however, these outliers are valuable and not removed. One outstanding outlier in this area was removed...that was 'TOTAL' as this was clearly a spreadsheet anomaly.  
The first step was to remove erroneous, or simply useless rows or columns from the dataset. The following were removed: 
<ol>
<li>The <b>TOTAL</b> row as this is a spreadsheet anomaly. </li>
<li>The <b>THE TRAVEL AGENCY IN THE PARK</b> as this is not a person. </li>
<li>The individuals <b>BHATNAGAR SANJAY</b> and <b>BELFER ROBERT</b>, as their compensation columns did not add up to their <b>total_payments</b>. This could be a typo or data entry error, but I did not trust that the data for these two was accurate.</li>
<li>Any row that did not have more than two non NaN values (including poi and email_address). Only one dropped. </li>
<li>Column <b>email_address</b> was dropped as it had no value in this process</li>
</ol>
The following is a table of counts of non NaN values. I decided to remove columns that did not have many values. The striked rows were removed. 
<table style="width:25%">
  <tr>
    <td>poi</td><td>141</td>
  </tr>
  <tr bgcolor="#FF0000">
    <td><strike>director_fees</strike></td><td>14</td>
  </tr>
  <tr>
    <td>deferred_income</td><td>48</td>
  </tr>
  <tr>
    <td>long_term_incentive</td><td>65</td>
  </tr>
  <tr>
    <td>percent_to_poi</td><td>85</td>
  </tr>
  <tr>
    <td>percent_from_poi</td><td>85</td>
  </tr>
  <tr>
    <td>salary</td><td>94</td>
  </tr>
  <tr bgcolor="#FF0000">
    <td><strike>deferral_payments</strike></td><td>37</td>
  </tr>
  <tr>
    <td>total_payments</td><td>121</td>
  </tr>
  <tr>
    <td>exercised_stock_options</td><td>99</td>
  </tr>
  <tr>
    <td>bonus</td><td>81</td>
  </tr>
  <tr>
    <td>restricted_stock</td><td>108</td>
  </tr>
  <tr bgcolor="#FF0000">
    <td><strike>restricted_stock_deferred</strike></td><td>15</td>
  </tr>
  <tr>
    <td>total_stock_value</td><td>124</td>
  </tr>
  <tr>
    <td>expenses</td><td>94</td>
  </tr>
  <tr bgcolor="#FF0000">
    <td><strike>load_advances</strike></td><td>3</td>
  </tr>
  <tr>
    <td>other</td><td>90</td>
  </tr>
</table>

<h3>Feature Addition</h3>
The features relating to email need to be modified as their natural number doesn't mean anything. Some people just get or send more email. However, email percentages to and from pois should be a valuable feature. I decided not to include the shared_poi percentage as pois are likely to blanket email departments and also, a poi would not wisely engage in illicit behavior while emailing multiple people at once.
The following features were added: 
<ol>
<li><b>percent_to_poi</b>: A percentage of emails a person sent to a known person of interest. </li>
<li><b>percent_from_poi</b>: A percentage of emails a person received from a known person of interest. </li>
</ol> 
Subsequently, the features that related to these percentages were removed. 
<ol>
<li><b>from_this_person_to_poi</b></li>
<li><b>from_poi_to_this_person</b></li>
<li><b>shared_receipt_with_poi</b></li>
<li><b>to_messages</b></li>
<li><b>from_messages</b></li>
</ol>

<h3>Feature Selection</h3>
The next step is to find those features that have the most importance. To do this I employed SelectKBest and the feature.importance_ values from a Random Forest. I set the number of K values in SelectKBest equal to 'all' (k='all') so I could see the weighting of all of them. The results are in the tables below: 

<table style="width:25%">
  <tr><td><b><i>Features</b></i></td><td><b><i>SelectKBest</b></i></td><td><b><i>Random Forest</b></i></td></tr>
  <tr><td>deferred_income</td><td>11.1845801</td><td>0.064717</td></tr>
  <tr><td>long_term_incentive</td><td>9.6222121</td><td>0.058101</td></tr>
  <tr><td>percent_to_poi</td><td>15.9781241</td><td>0.124385</td></tr>
  <tr><td>percent_from_poi</td><td>2.9639901</td><td>0.061409</td></tr>
  <tr><td>salary</td><td>17.7178741</td><td>0.065869</td></tr>
  <tr><td>total_payments</td><td>9.0792751</td><td>0.069305</td></tr>
  <tr><td>exercised_stock_options</td><td>24.4310681</td><td>0.109634</td></tr>
  <tr><td>bonus</td><td>20.2571851</td><td>0.105969</td></tr>
  <tr><td>restricted_stock</td><td>8.8641111</td><td>0.064484</td></tr>
  <tr><td>total_stock_value</td><td>23.6122891</td><td>0.092194</td></tr>
  <tr><td>expenses</td><td>5.8153281</td><td>0.090443</td></tr>
  <tr><td>other</td><td>4.0852121</td><td>0.093490</td></tr>
</table>

I chose the top five values from both SelectKBest and the Random Forest feature.importances_. Because they were not exactly the same I ended up with a total of eight features in my selection. These were: <b>deferred_income, percent_to_poi, salary,	exercised_stock_options,	bonus,	total_stock_value, expenses, other.</b> I then removed the fields that were not in the top five of either the SelectKBest or the Random Forest feature.importances_.  

<h3>Cross-Validation</h3>
Cross-validation is the process of randomly splitting the data into training and testing sets. It is important not to just split the data down the middle as often data might be sorted in a particular way...leading to a training set that is nothing like the testing set. A popular technique when training and testing the data is not to split the data into two components, but into three. These would be a training set, a validation set and a testing set. With just a training and testing set, over the course of multiple tests during the model selection process the test set, in so many words, becomes the training set and leads to overfitting. However, in this case, we have a very very small set of data and splitting the data into three parts is just not feasible. 
I used sklearn's train_test_split and StratifiedKFold to split my data into training and test splits. The train_test_split ratio I used was 70/30. I found the StratifiedKFold worked more reliably and I used a simple 10 folds. In the case of a fold of 10, the training set is divided into 10 pieces. During the 10 iterations, 9 of the folds are used for training and 1 is used for testing. There is a special case of k-fold that is referred to as the <i>leave-one-out</i> method. It is recommended for very small datasets and is when the number of folds equals the number of training samples so that only one training sample is used for testing during each iteration. I did not employ this technique with much success. Perhaps when they refer to a <i>very small</i> set of data, it is even smaller than what we have. 

<h3>Normalization and Standardization</h3>
I used sklearn's MinMaxScaler and StandardScaler to respectively normalize and standardize the data. Normalizing the data would take the values and range them from 0 to 1. Standardizing the data will return a standard deviation of the values from a mean of zero. I created and saved three sets to be used easily later. X_train, X_train_norm and X_train_std. 

I applied Principal Component Analysis when it seemed to work with an algorithm. It didn't often work. I concluded that there is not much dimensionality in this data and this must be the reason PCA was not having much, and often a negative effect. 

<h3>Various SkLearn Algorithms</h3>
Following are the results of various tried Machine Learning Algorithms. Again, details on how I implemented them are in the <i>Trying Enron with Pandas.ipynb</i> Note that the values below are from sklearn.metrics classification_report and do not have the same results as I received from the tester.py provided by Udacity. 
<table style="width:50%">
  <tr><td><b><i>Algorithm</b></i></td><td><b><i>Precision</b></i></td><td><b><i>Recall</b></i></td><td><b><i>F1-score</b></i></td>
      <td><b><i>Support</b></i></td><td><b><i>Cross-Val Accuracy</b></i></td></tr>
  <tr><td>K-Nearest (true)</td><td>0.97</td><td>0.72</td><td>0.83</td><td>40</td><td>0.891 +/- 0.077</td></tr>
  <tr><td>K-Nearest (false)</td><td>0.15</td><td>0.67</td><td>0.25</td><td>3</td><td>0.891 +/- 0.077</td></tr>
  <tr><td>K-Nearest (piped true)</td><td>0.95</td><td>0.93</td><td>0.94</td><td>40</td><td>0.809 +/- 0.111</td></tr>
  <tr><td>K-Nearest (piped false)</td><td>0.25</td><td>0.33</td><td>0.29</td><td>3</td><td>0.809 +/- 0.111</td></tr>
  <tr><td>Support Vector Machine (true)</td><td>1</td><td>.03</td><td>0.05</td><td>40</td><td>0.850 +/- 0.039</td></tr>
  <tr><td>Support Vector Machine (false)</td><td>.07</td><td>1</td><td>0.13</td><td>3</td><td>0.850 +/- 0.039</td></tr>
  <tr><td>Decision Tree (true)</td><td>0.95</td><td>0.95</td><td>0.95</td><td>40</td><td>0.792 +/- 0.102</td></tr>
  <tr><td>Decision Tree (false)</td><td>0.33</td><td>0.33</td><td>0.33</td><td>3</td><td>0.792 +/- 0.102</td></tr>
  <tr><td>Logistic Regression (true)</td><td>93</td><td>1</td><td>96</td><td>40</td><td>0.855 +/- 0.085</td></tr>
  <tr><td>Logistic Regression (false)</td><td>0.00</td><td>0.00</td><td>0.00</td><td>3</td><td>0.855 +/- 0.085</td></tr>
  <tr><td>Adaptive Boosting (true)</td><td>0.97</td><td>0.82</td><td>0.89</td><td>40</td><td>0.843 +/- 0.127</td></tr>
  <tr><td>Adaptive Boosting (false)</td><td>0.22</td><td>0.67</td><td>0.33</td><td>3</td><td>0.843 +/- 0.127</td></tr>
</table>

<h3>Algorithm Parameter Tuning</h3>
Parameter tuning is fundamental in ensuring that the algorithm is not over or underfit. I played around with the parameters on all the aforementioned algorithms. I ended up using GridSearchCV to optimize the parameters of all these. I did not use GridSearchCV in a pipeline as I was not getting that to work properly. I noted this as something on which I need to work. Again, more detail on my thoughts during this process are in <i>Trying Enron with Pandas.ipynb</i>. 

Here is where confession comes in. I ended up picking KNeighbors as the classifier that performed the best. It was also probably the easiest to implement. But I didn't want to pick KNeighbors, it was not as fancy as Adaboost and I thought surely I could tune Adaboost to beat the tar out of KNeighbors. KNeighbors had simple options, Adaboost was more difficult conceptually. For instance, to tune Adaboost I used a range of n_estimators from 1 to 200 for a real and discrete classifier of Adaboost and ran then through GridSearchCV. I then waited about 20 minutes and it came back with a result that said my optimal parameters for my dataset were: <br>
"Best parameters of Adaboost SAMME.R: {'n_estimators': 19}"<br>
"Best scrore of Adaboost SAMME.R: 0.908163265306"<br>
I tried tuning Adaboost in every way I could imagine and every way I could find in research, but that is the best I could get out of it. Unfortunately, when evaluated for precision and recall...it did poorly on precision. Meanwhile, the simple and scrappy little KNeighbors classifier that I had long ago dismissed as a simpleton, had an accuracy of 91 and a precision double that of Adaboost. 
For that matter, Naive-Bayes untuned performed better than some of the algorithms I spent days fiddling with. <br>
<h4>Conclusion to Algorithm Selection</h4>
Algorithm selection requires an understanding of the principles behind the algorithm. However, Algorithm selection appears part art and part science. I'm sure seasoned data-scientists have a touch of intuition when selecting an algorithm. 

<h3>Evaluation</h3> 
Sklearn numerous evaluation techniques in the <i>metrics</i> package. I used accuracy, classification_report and confusion_matrix. The accuracy is simply the percentage of correct hits. Precision is simply the ratio of true positive results to all positive results. <b><i>P = TP/(TP+FP)</i></b>. Recall is similarly the ratio of true positive results to all possible positive results. <b><i>R = TP/(TP+FN)</i></b>. These are all returned by the classification_report. The confusion matrix returns just the values:  <i>TruePositives, TrueNegatives, FalsePositives, FalseNegatives </i> in an array. 

<h3>Final Thoughts</h3>
This process has been very rewarding and challenging. Probably my largest challenge was to work through the actual Python as I am a beginner. The data was difficult and educating to work with. It is a small dataset, after my data cleaning it had only 141 rows of data and only 18 persons of interest. Through this long process I learned how to use GridSearchCV to find optimal settings for the algorithms, how to validate the data using various tools (not just accuracy), and how to chain processes together via Pipeline. 
<h4>Recommendations</h4>
If I would recommend any improvement, it would be to allow people to use other means of programming these algorithms such as the IPython Notebook or even R. As this is not a lesson really in programming, rather in Machine Learning, the code should be secondary. The starter code provided actually caused me more misunderstandings and problems than just working in pandas. My project really got rolling and I gained much more understanding when I disguarded the starter code and just started from nothing. At this point in my project...I still haven't 'retrofitted' my IPython notebook to the layout Udacity requires. 

<h2>Big Confession</h2>
I have been done with this assignment since Christmas break. I misread the rubric and though precision had to be a .93. The clock was ticking and I didn't know what to do. I tried even Ant Colony Optimization thinking that might do it...it didn't get .93 precision. I read and did all the examples in <i>Python Machine Learning</i> by Sebastian Raschka. When I still couldn't get the elusive .93 precision I bought <i>IPython Interactive Computing and Visualization Cookbook</i> and <i>Learning IPython for Interactive Computing and Data Visualization</i> both by Cyrille Rossant and worked through all the examples. I was in a panic and set a drop-dead date for myself of 01/13/2016 at noon. I thought, "well, this is probably the end of my Udacity study. Machine Learning is pivotal and if I can't get the required results after all this work...well then I'm just not going to make it." While writing up this summary I was again perusing the rubric to make sure I addressed all the concepts in there...not 10 minutes ago...I realized...it's not .93...it is .3!
I'll be submitting tomorrow. 

