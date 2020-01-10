# Using Machine Learning to Predict U.S. Opioid Long-Term Therapy Patients (Humana Mays Healthcare Competition 2019)
During the past few years, there has been a dramatic increase in prescription opioid use disorder (OUD), opioid-related morbidity and mortality. Moreover, according to Centers for Disease Control and Prevention research, one out of four patients who receive long-term opioid therapy in primary care settings struggle with opioid use disorder. A model that is able to extract the knowledge of clinical characteristics associated with the progression of long-term opioid therapy can aid in the identification of disorder at-risk patients to provide the basis for developing targeted clinical interventions. Identifying potential abusers and preventing the outcome is of course far more cost-efficient, both in terms of finance and on the person’s health, than attempting to intervene after abuse has been diagnosed.

This code aims to do just that: to identify memeber at risk or conitnued long-term use of opioid therapies to allow for early intervention. To do this, we use provided data containing information of 14k patients (pharmacy claims, surgeries, diagnosis, etc) to create a model to predict if a patient will continue opioid therapy six months after initital prescribing. 

This code was developed as part of our application to the Humana Mays Healthcare competition (2019). For more information you can visit: https://www.humanatamuanalytics.com/

## Data Overview
The dataset used in this study was obtained from a national health insurance. This dataset contained medical claims from 14,000 members during a period of time of four years (2015 through 2018). The following definitions were provided:

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
- Attributes: for each type of events, 10 different attributes providing relevant information about the events are given. The only exception is for Rx Claims, where 5 extra attributes are added onto the 10 features, making a total of 15. The exhaustive list of attributes can be found in section 4 (Table for Attributed).

![alt text](http://url/to/img.png)
