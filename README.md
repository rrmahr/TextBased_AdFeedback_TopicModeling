# Text-Based Ad Feedback TopicModeling
Springboard Data Science Bootcamp Capstone Two Project: Text-Based Ad Feedback Topic Modeling <br>
Developed by: Rebeca Mahr (Spring 2021)
<br>
## Objective:
Create a NLP unsupervised machine learning model to identify 3-6 topics among text-based video ad feedback to inform message comprehension.
<br>
## Background: 
Qualitative feedback in the form of text-based responses from an advertising campaign’s target audience can be invaluable in determining ad message comprehension as well as in making decisions for future rounds of campaign flighting and creative development. However, human analysis of text-based responses requires reading through thousands of responses and manually coding them into themes or topics. Utilizing a machine learning algorithm to identify topics based on textual responses can decrease the amount of manual labor hours spent in analysis and minimize bias due to reviewer subjectivity. This type of algorithm can be utilized by market and advertising researchers, advertising campaign evaluators, and/or marketing analytics teams to supplement digital analytics reporting.
<br><br>
For this project, NLP tools and models were used to analyze the frequency of words and generate topics from text-based ad feedback. This feedback was gathered in surveys distributed among a nicotine-vape-prevention campaign’s target audience. Respondents were shown three video ads and asked what they thought the main message of each video was. These text-based ad feedback responses were analyzed for this project with the goal of identifying three to six key topics that summarize the message comprehension responses.<br>

## Data Overview:
* Dataset 1 – Ad_Feedback_Text (n=1,448): 
	* ID: Unique identifier for respondent
	* Text: Includes text responses
	* Ad: Indicates the specific video advertisement the response was for (encoded)
		* DD: ad message related to vape companies deceiving teens
		* DF: ad message related to vapes making smokers vulnerable to viruses
		* ST: ad message related to exposing the chemicals in vapes
* Dataset 2 – Ad_Feedback_Demos (n=724):
	* ID: Unique identifier for respondent
	* CalculatedAge: Respondent age
	* Race: Respondent self-reported race
	* Gender: Respondent self-reported gender
	* Segment: Audience segment the respondent belongs to (encoded)
	* Region: State where survey was distributed (encoded)
	* Urban_Rural: Indicates whether respondent region is urban or rural based on population density of the city they live in
<br>
<i> Note: Data not publicly available for survey respondent privacy and compliance with IRB protocol</i><br>

[Sample Data File](https://github.com/rrmahr/TextBased_AdFeedback_TopicModeling/blob/main/b_Data_Sample/Sample_Dataset_TextBased_AdFeedback_TopicModeling.xlsx)
<br>

## Data Cleaning:
* Contractions were expanded to ensure words retained their meaning after removing additional characters
* All non-alphanumeric characters were removed (including punctuation)
* All text was standardized in lowercase
* All gibberish / nonsense words were removed
<br>

[Data Cleaning Code](https://github.com/rrmahr/TextBased_AdFeedback_TopicModeling/blob/main/a_Notebooks/a_Data_Wrangling.ipynb)
<br>

## Preprocessing & Feature Engineering:
* Stop words were removed
* Ad feedback text responses were tokenized
* Ad feedback text responses were lemmatized
* A TF-IDF vectorizer object was developed from the processed ad feedback text corpus
* The TF-IDF vectorizer object was split into training and test sets with 30% reserved for testing
<br>

[Preprocessing & Feature Engineering Code](https://github.com/rrmahr/TextBased_AdFeedback_TopicModeling/blob/main/a_Notebooks/c_Preprocessing.ipynb)
<br>

## Exploratory Data Analysis (EDA):
* Survey sample demographic distributions were explored:
	* n: 724
	* ages: 13-19 (17 years on average)
	* gender: 64% female
	* race/ethnicity:
    
race/ethnicity | % of Sample
------------ | -------------
white only | 72%
two +  | 9%
black only | 8.3%
hispanic/latino (any)| 7.2%
other | 3.3%

* A term frequency analysis was conducted to explore the ad feedback text
	* The top 10 words included: 'vape', 'company', 'virus', 'stop','people','bad', 'teen', 'smoke', 'lie','lung'
<br>

[Exploratory Data Analysis (EDA) Code](https://github.com/rrmahr/TextBased_AdFeedback_TopicModeling/blob/main/a_Notebooks/b_EDA.ipynb)
<br>

## Modeling & Hyperparameter Tuning:
* Two Latent Dirichlet Allocation (LDA) topic models were developed
* Two Nonnegative Matrix Factorization (NMF) topic models were developed
* Hyperparameter tuning was conducted using the randomized search approach on the following hyperparameters:
	* n_components: the number of topics
	* max_iter: number of iterations
* Summary of Models:

model | n_components | max_iter
------------ | ------------- | -------------
Initial LDA | 6 | 250
Optimized LDA  | 3 | 450
Initial NMF | 6 | 250
Optimized NMF | 3 | 450

* Selected Model:
	* <strong>Optimized LDA</strong>
		* Final Topics:
			* 0: vape company teen harmful lie target dangerous sick juul harm
			* 1: smoke stop virus vape people damage make lung susceptible vulnerable
			* 2: bad vape immune weaken health know risk chemical quit tell
		* Distribution of Topics by Ad
			* 69% of responses to DD (related to vape companies deceiving teens), had the highest LDA topic probability for topic 0 
			* A total of 75% of responses to DF (related to vapes making smokers vulnerable to viruses) had the highest LDA topic probability for topics 1 (47%) and 2 (28%). 
			* Mapping to ST (related to exposing the chemicals in vapes) was less clear with 51% of responses having the highest LDA topic probability for topic 0 although topic 2 (27%) might be more relevant.
		*  Visualization Results
			* Each of the three topics were well distanced from one another and further apart than the topics in the initial LDA topic model
			* Marginal topic distribution was larger for the three topics than for the topics in the initial LDA topic model
		* Perplexity Score
			* 51% improvement from the initial LDA model (551.04 vs.  1124.55)
<br>

[Modeling & Hyperparameter Tuning Code](https://github.com/rrmahr/TextBased_AdFeedback_TopicModeling/blob/main/a_Notebooks/d_Modeling.ipynb)		

## Applying Model to the Business Problem
Under the assumption that the topics generated by the optimized LDA topic model indeed reflect the topics of the ad feedback text responses, it can be inferred that ad message comprehension was higher for DD and DF while there may have been some confusion regarding the main message of ST.

* Next Steps:
	* Spot check to confirm model results
	* Recommend written ad copy edits for ST prior to next round of campaign flighting
<br>

## Limitations
* Sample selection bias
* Small sample size
* Skewed sample demographic distribution
* Subjectivity in model topic interpretation
* Limited options for NMF topic model evaluation
* Limited computational resources for hyperparameter tuning
