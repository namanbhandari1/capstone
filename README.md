# Capstone Project
## Recommender System for Health Insurance Plans
_By Naman Bhandari_


### Background

Purchasing a health insurance plan can be a complicated thing to navigate. Many people skip purchasing health insurance due to a number of factors, often cited as a combination of insurance being:
- too expensive
- too limited in what's covered
- too complex

The ACA attempted to fix this problem by requiring consumers of healthcare to purchase insurance, or pay a fine. The ACA was successful in reducing the cost of healthcare for low-income consumers, and addressed some of the complexities of purchasing insurance by making insurance available through federal and state exchanges, but did not do enough to help consumers understand what they're getting in their insurance plan. It still remains opaque and somewhat complex.

### The Problem

When consumers of health insurance feel overwhelmed by choices and information, they can experience information overload and avoid making a choice as a result. Worse yet, without appropriate support through the process of choosing an insurance plan, many people could wind up with a plan that does not meet their needs and become saddled with bills as a result.  

When consumers choose an insurance plan, they need to be able to weigh trade-offs between cost and coverage for insurance features. Picking a plan that provides the coverage needed at an affordable priceis a difficult task. It is especially difficult when you don’t have a great understanding of the language that insurers use to describe plans.  

### An Approach

The Centers for Medicaid and Medicare Services (CMS) provide data on health and dental plans offered to individuals and small businesses through the US Health Insurance Marketplace. This data contains all the various health plans offered on the Health Insurance Exchanges in the 50 states, including the  cost for each plan and the benefits covered in those plans.

Using this data, we can build a system in which consumers can select the features they want in a health insurance plan and be presented a list of plans with their requested features and the price of the plan. We can evaluate the accuracy of our model by evaluating the mathematical distance (cosine similarity) between the features selected and the plans returned.

Using a recommendation engine such as this, consumers can select the benefits they want (examples: weight loss programs, chiropractic care, imaging, etc.) and be shown plans relevant to their health and be able to make better decision when purchasing insurance. A nimble, easy to navigate plan customization may also result in more uninsured people to purchase health insurance.

### Data

I will be using data available in this Kaggle competition (https://www.kaggle.com/hhs/health-insurance-marketplace/home) to perform analysis and build a recommender system.

The data I will be using is split up into various CSV files totaling 3.4GB in disk size. When loaded, the CSV files are larger, but can be shrunk by casting object type columns as category type columns. In addition, the data is also presented in SQL database format, which I will consider using. The raw data has close to 800 different benefits, some of which will be combined under single categories based on domain knowledge.

---

## Milestone 3

### EDA

- Started with 3 different CSV files as part of analysis, retrieved from Kaggle:
    - Rate.csv (12,694,445 rows, 24 columns, 9.3GB loaded)
    - BenefitsCostSharing.csv (5,048,408, 32 columns, 6.9GB loaded)
    - PlanAttributes.csv (77353 rows, 176 columns, \__GB loaded)
    
- Shrank all CSVs by:
    - Casting object columns as _category_ columns
    - Filtered data by 2016 year (as only most recent year is relevant to recommendation analysis)

- Engineered dataframes by:
    - Merging some features together to account for mis-spellings and features that are "similar enough" to each other
    - Creating dummy columns for all the unique features in `Benefits` so users can select by those features
    - Added some columns from `PlanAttributes` to `Benefits` so users can select by those columns too 
    
- EDA:
    - # of plans offered in 39 states
    - Avg. plan premium in 39 states
    - Distribution of monthly premiums for: Individuals and Tobacco Users
    
### Data Dictionary

My data consists of 215 unique plan "types" over an assortment of 207 unique features. The below data dictionary grouped into 3-4 "types" of features:

Rate: (users can filter by the ones in **bold**)
- 'BusinessYear',
- **'StateCode',**
- 'RateEffectiveDate',
- 'RateExpirationDate',
- 'PlanId',
- 'RatingAreaId',
- **'Tobacco',**
- **'Age',**
- **'IndividualRate',**
- 'IndividualTobaccoRate',
- **'Couple',**
- **'PrimarySubscriberAndOneDependent',**
- **'PrimarySubscriberAndTwoDependents',**
- **'PrimarySubscriberAndThreeOrMoreDependents',**
- **'CoupleAndOneDependent',**
- **'CoupleAndTwoDependents',**
- **'CoupleAndThreeOrMoreDependents',**

Benefits
- 215 "unique features"

Attributes (users can filter by these)
- 'IsNoticeRequiredForPregnancy',
- 'IsReferralRequiredForSpecialist',
- 'ChildOnlyOffering',
- 'WellnessProgramOffered',
- 'DiseaseManagementProgramsOffered',
- 'OutOfCountryCoverage',
- 'NationalNetwork'

### Questions

1. Do you have data fully in hand and if not, what blockers are you facing?  
Ans: I have all my data. My data was obtained via Kaggle.

2. Have you done a full EDA on all of your data?  
Ans: Not all of it, but in process. I do have my data set up and ready to model.

3. Have you begun the modeling process? How accurate are your predictions so far?  
Ans: I am doing a recommender system. My system is finding health insurance plans with around 90%+ accuracy, so it's performing well.

4. What blockers are you facing, including processing power, data acquisition, modeling difficulties, data cleaning, etc.? How can we help you overcome those challenges?  
Ans: I would like to figure out the most efficient way to generate a new vector based on user input. Currently, I'm creating a new dataframe populated with 0s and then mapping a 1 for each feature selected by a user. That becomes my vector against which I calculate cosine similarity.

5. Have you changed topics since your lightning talk? Since you submitted your Problem Statement and EDA?  Ans: I have not changed my topics since my lightning talk or since I submitted by Problem Statement.

6. If so, do you have the necessary data in hand (and the requisite EDA completed) to continue moving forward?  
Ans: I do have the necessary data in hand and have performed the required EDA to continue moving forward.

7. What is your timeline for the next week and a half? What do you have to get done versus what would you like to get done?  
Ans: Over the next week and a half, I will set up my data structures in a way that users can efficiently select features via prompt and using auto-complete, and be displayed a set of health insurance plans that they can get more information about. Also, I would like to show users whether or not the features they selected ARE ACTUALLY included in the plans shown to them.

8. What topics do you want to discuss during your 1:1?  
Ans: I want to discuss the best way to set up a scheme that asks for user input via an auto-complete form.