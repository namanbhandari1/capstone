# Technical Report: Health Insurance Plan Recommender System

This technical report provides a sequential summary of the technical steps taken to process, explore, define, analyze, and draw conclusions from my data. In it, I outline the core tactics, methodologies, and libraries used in this project.

## Data from CMS via Kaggle

The Centers for Medicare & Medicaid Services (CMS) Center for Consumer Information & Insurance
Oversight (CCIIO) publishes "Exchange Public Use Files", or "PUFs", in order to improve transparency and increase access to data on Qualified Health Plans (QHPs) and Stand-alone Dental Plans (SADPs) offered through the Affordable Care Act's Health Exchanges.

A Kaggle-hosted data exploration prompt, [Health Insurance Marketplace](https://www.kaggle.com/hhs/health-insurance-marketplace), processed the CMS data to facilitate analytics. The processing script is available on [this GitHub repo.](https://github.com/benhamner/health-insurance-marketplace/blob/master/src/process.py)

I was provided with these six CSV files. I've used 3 of them in my analysis (in **bold**):

| PUF CSV                     | Description                                                                                                                             |
|-----------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| **BenefitsCostSharing.csv** | contains plan-level data on essential health benefits and coverage limits. Used to derive benefit names in analysis.                    |
| BusinessRules.csv           | plan-level data on rating business rules, such as allowed relationships (e.g., spouse, dependents) and tobacco use.                     |
| Network.csv                 | identifies provider network URLs.                                                                                                       |
| **PlanAttributes.csv**      |  plan-level data on maximum out of pocket payments, deductibles, HSA eligibility, formulary ID, and other plan attributes.              |
| **Rate.csv**                | plan-level data on individual rates based on an eligible subscriber’s age, tobacco use, and geographic location, and family-tier rates. |
| ServiceArea.csv             | issuer-level data on geographic service areas including state, county, and zip code                                                     |


These PUFs do not include data from the 11 states that have their own State-Based Exchanges, which do not rely on the federal platform for QHP eligibility and enrollment functionality. These states are:

    1. California
    2. Colorado
    3. Connecticut
    4. Idaho
    5. Maryland
    6. Massachusetts
    7. Minnesota
    8. New York
    9. Rhode Island
    10. Vermont
    11. Washington

However, state-level PUFs are available data are available from a separate CMS portal, here: [https://www.cms.gov/CCIIO/Resources/Data-Resources/sbm-puf.html](https://www.cms.gov/CCIIO/Resources/Data-Resources/sbm-puf.html).

## Pre-processing

Two of the CSV files, `Rate.csv` and `BenefitsCostSharing.csv`, were very large in size when loaded into Pandas, at 9.3GB and 6.9GB each! The file sizes would have made it very difficult to perform manipulations or conduct analysis on the data:

    - `Rate.csv`: 12,700,000 rows x 24 cols (9.3GB)
    - `BenefitsCostSharing.csv` - 5,000,000 rows x 32 cols (6.9GB)

### Object vs. category columns

The cause for this was the numerous "object" columns present in my data. As a corollary, to reduce memory usage, most object columns whose unique values comprise _less than 50% of the total values in the column_, can be casted as "category" type columns.

A "category casting" hashes the values in the column, thus compressing the data. In an "object" column each value in the column is stored individually in memory. However, in a "category" column, each unique value is hashed, and instead simply points to the value's _location_ in memory, dramatically reducing memory requiremnets.

So, a column of 5 million "Yeses" and "Nos" (5 million strings) can be hashed into only **two** values in memory, thus significantly compressing the size of that column.

After casting the object columns as categories, I pickled the dataframes to be used in the next notebook.

## Cleaning the benefits and rate data

After pre-processing `benefits` and `rate` and compressing them down to workable sizes, I dropped rows corresponding to historical past years, as we want to analyze the most current year available, 2016. I also some plan variants whose only difference is the amount of subsidy received by the applicant (however, the premium remains the same). In addition, I dropped certain un-needed columns and cleaned the text.

### Crosswalk

The benefits dataframe has, as you might expect, a column called 'BenefitName', which contains each benefit offered in each health insurance plan. Many of the benefits in the column are the same but differ due to misspellings or containing extraneous characters.

I created a crosswalk CSV in which I mapped 'BenefitName' variants to a single name. For example, these three benefits:

    - Cosmetic Orthodontia-Child
    - Cosmetic Orthodontics - Child
    - Cosmetic Orthodontics-Child

Were collapsed into one, renamed benefit:

    - Orthodontia, Cosmetic - Child

### Vectorizing

After mapping plan names with my crosswalk, I one-hot encoded the benefits. To do this, I grouped by plan names and turned the rows benefits associated with each plan into columns for unique plan, filled with 1s and 0s based on coverage. This had the effect of reducing the number of rows, but increasing the number of columns in my data (data was made more rectangular).

<p align="center">
<img src="assets/square2.png" />
</p>

### Identify unique vectors

I merged my rate data and dummied benefits data together on `'PlanId'`. I assigne d