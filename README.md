# Using Machine Learning to Predict U.S. Opioid Long-Term Therapy Patients (Humana Mays Healthcare Competition 2019)

<p align = "center">
<img src="https://user-images.githubusercontent.com/54824400/72117856-0b424680-3304-11ea-8ec4-ecda15d87b61.png"/>
        </p>

## Introduction
During the past few years, there has been a dramatic increase in prescription opioid use disorder (OUD), opioid-related morbidity and mortality. Moreover, according to Centers for Disease Control and Prevention research, one out of four patients who receive long-term opioid therapy in primary care settings struggle with opioid use disorder. A model that is able to extract the knowledge of clinical characteristics associated with the progression of long-term opioid therapy can aid in the identification of disorder at-risk patients to provide the basis for developing targeted clinical interventions. Identifying potential abusers and preventing the outcome is of course far more cost-efficient, both in terms of finance and on the person’s health, than attempting to intervene after abuse has been diagnosed.

This code aims to do just that: to identify members at risk or continued long-term use of opioid therapies to allow for early intervention. To do this, we use provided data containing information of 16k patients (pharmacy claims, surgeries, diagnosis, etc) to create a model to predict if a patient will continue opioid therapy six months after initial prescribing. 

This code was developed as part of our application to the Humana Mays Healthcare competition (2019). For more information you can visit: https://www.humanatamuanalytics.com/

## Table of contents
* [Data Overview](#Data-Overview)
* [Flow](#Flow)
* [Technologies](#Technologies)
* [Data Pre-processing](#Data-Pre-processing)
* [Feature Engineering](#Feature-Engineering)
* [Machine Learning](#Machine-Learning)
* [Evaluation](#Evaluate-Prediction-Performance)
* [Notebooks](#Purpose-of-different-notebooks)

## Data Overview
The dataset used in this study was obtained from a national health insurance. This dataset contained medical claims from 16,000 members during a period of time of four years (2015 through 2018). The following definitions were provided:

Opioid Naïve: patient not having an opioid ‘on hand’ in the preceding 90-day period, based on service date and pay day supply count.
Long Term Opioid Therapy (LTOT): continuous use of an opioid medication with 90% of days covered over a 6 month period.
Moreover, the provided data contained 6,086,969 rows (events) x 20 columns (attributes). This means that for every patient, several events took place at the same time. 

A description of the 20 columns (attributes)  in the dataset is provided below:

- ID: person identifier number
- Event description: description of event, which can be any of the following sixteen:

        Inbound Call by Member
        New diagnosis - Top 5 diseases
        Inbound Call by Other
        New diagnosis - Coronary artery disease
        Inbound Call by Provider
        New diagnosis - Diabetes
        Fully Paid Claim
        New diagnosis - Hypertension
        Surgery
        New diagnosis - Chronic Pulmonary Disease
        New Provider
        New diagnosis - Congestive Heart Failure
        RX Claim - Paid
        RX Claim - Rejected
        RX Claim - New drug
        RX Claim - First time mail order
- Attributes: for each type of events, 10 different attributes providing relevant information about the events are given. The only exception is for Rx Claims, where 5 extra attributes are added onto the 10 features, making a total of 15. 

The following image exemplifies the data: 

![Screenshot 2020-01-09 at 16 02 48](https://user-images.githubusercontent.com/55929915/72115058-84d53700-32fa-11ea-95c0-d2c310796649.png)

A literature research was conducted in order to fully explore the underlying risk factors in long-term opioid therapy. We examined our data and aligned the provided information with the results derived from the literature and we determined the possible features that could be extracted: total MME, average daily MME, number of opioid fills, number of benzodiazepine fills and history of alcohol and substance use disorder. It is noteworthy that other features such as “Disability Status” were considered but found to have an insignificant relevance in our data. Afterwards, we decided to test for additional features: number of non-opioid prescriptions, doctors shopping (looking for new providers), mental health disorders and top 5 diagnosis (diabetes, hypertension, CPD, CHF and CAD). Information such as surgery, physician specialty, place of treatment and costs were considered irrelevant for the purpose of the model.

## Flow
1. Data Pre-processing
2. Feature Engineering
3. Machine Learning
4. Evaluate Prediction Performance

## Technologies
This project is created with:
*Python version: 3.7.5
*Pandas version: 0.25.3
*NumPy version: 1.17.4

## Setup
This project is not in PyPI and does not plan to be. Get it by cloning from the GitHub Repo:
```
git clone https://github.com/dannydhkim/Humana-Mays.git
```

## Data Pre-processing
Thoughtful preprocessing of the data enhanced  the ability to interpret the relative importance of risks. It is noted that “as-prescribed” approach was used, which assumes that patients take all prescribed opioids at the prescribed dose and on the schedule recommended by their clinicians. Opioid events could only be applicable to those with both MME attributes and a “Day 0” event. Patients with missing data were screened out of the process. A sample size of n = 12355 was used to extract features to be used and analyzed. Pandas and Numpy were used to transform the data into a new, simpler dataset format which contain a boolean series of LTOT or not and the list of features.

According to the definitions provided, we defined long-term opioid therapy events as having opioid ‘on hand’ for a total of 162 days or above in a 180-day window. Patients who had any LTOT qualifying 180-day window within their longitudinal record after Day 0 were considered to be LTOT. LTOT events were labelled to be used as a validation set for training the machine learning model. 

## Feature Engineering
The following table shows the features extracted and how the extraction was carried out.
![Screenshot 2020-01-09 at 16 32 04](https://user-images.githubusercontent.com/55929915/72116091-f8c50e80-32fd-11ea-9bcb-2c6473f59ced.png)

## Machine Learning
Once patients were labeled as LTOT, we found that the calculated LTOT rate in our database confirmed with the given parameters that were given to us (i.e. greater than 30%) at 36.6%. It is noteworthy that we applied a filter to screen out cancer patients to better represent the general population to arrive at a 33% rate.

A XGBoost framework was used with the following parameters: the data was split into a random 60-40 train and testing set. With 100 rounds of training with a tree model and a binary logistic model to output a probabilistic value.

![Feature Importance](https://user-images.githubusercontent.com/54824400/72118017-96234100-3304-11ea-8fc9-6863e8a0de0e.png)

The figure above depicts the importance of the 14 features assessed in descending order of importance. The six major variables that most highly correlated with being at risk for LTOT: average daily MME, total MME, number of non-opioid prescriptions, number of opioid fills, new provider (doctors shopping) and number of benzodiazepine fills. A higher F-score implies a higher discriminative power, but does not indicate any mutual information (i.e. combination of features may lead to different results).  These results directly correlate with the research carried out by previous studies 

## Evaluate Prediction Performance
To evaluate the performance of the model, we placed our results on a confusion matrix. The sensitivity and specificity in this instance of the application seen in the confusion matrix below is 86.76% and 78.94%, respectively. For our purposes, the model was able to achieve an 82% accuracy and a  based on the training data. Furthermore, we evaluated the performance of the model on a ROC-AUC curve and saw an AUC of 0.83.. 

![Confusion Matrix](https://user-images.githubusercontent.com/54824400/72118085-e7cbcb80-3304-11ea-8e53-8ba0c9d2624f.png)

![AUC](https://user-images.githubusercontent.com/54824400/72118105-f9ad6e80-3304-11ea-95c4-01956a33eda0.png)

## Purpose of different notebooks
Opioid Algorithm: Development of the algorithm to predict long-term opioid dependence on one patient.

Algorithm Implementation: Implementing the class so that the algorithm is scalable for qualitative determination of patient risk based on MME amount for each patient.

Feature Engineering: Compiled list of methods used to extract selected features from the dataset. Methods are made scalable for the dataset to fit any patient.

Data Analysis: We explored some data visualizations but found that it was not very useful for any particular insight.

XGBoost: The machine learning code using XGBoost to implement gradient boost machine on the chosen feature data. The model is evaluated by generating a confusion matrix, accuracy, and AUC curve.

