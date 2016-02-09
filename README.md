<H1>Enron Email Corpus - Finding Persons of Interest</H1>
There are not many US citizens who have not heard of Enron; when they do it is in the context of willful corporate greed and fraud. Before its bankruptcy in late 2001 it employed approximately 20,000 employees. (https://en.wikipedia.org/wiki/Enron) This is a project to take the famous Enron Email corpus which is a large dataset of nearly 600,000 emails generated from 158 Enron employees and identify persons of interest (poi). (https://en.wikipedia.org/wiki/Enron_Corpus). A poi is defined by the list of those individuals who were involved in the scandal.

Within the Final_project there is an IPython Notebook titled "Trying Enron with Pandas." It is within this notebook that the work summarized here is found. The poi_id.py file is a requirement for course completion, but not my original or complete workspace. The poi_id.py file will have just the minimum of the requirement to fullfil the course. 

There were three major sources of insight for this project. The first is, of course, the Udacity <i>Introduction to Machine Learning</i> course. The second is <i>Python Machine Learning</i> by Sebastian Raschka. The third is the work of Jason Brownlee at MachineLearningMastery.com

<h3>Exploring and Removing Obvious Problems</h3>
The first step was to remove erroneous, or simply useless rows or columns from the dataset. The following were removed: 
<ol>
<li>The <b>Total</b> row as this is a spreadsheet anomaly. </li>
<li>The <b>THE TRAVEL AGENCY IN THE PARK</b> as this is not a person. </li>
<li>The individuals <b>BHATNAGAR SANJAY</b> and <b>BELFER ROBERT</b>, as their compensation columns did not add up to their <b>total_payments</b>. This could be a typo or data entry error. </li>
<li>Any row that did not have more than two non NaN values (including poi and email_address). Only one dropped. </li>
<li>Column <b>email_address</b> was dropped as it had no value in this process</li>
</ol>

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
