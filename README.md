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