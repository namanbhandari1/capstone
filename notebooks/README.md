# Notebooks Directory

This directory contains the notebooks which comprise the project.

## Directory Structure

```
.
├── 01-Preprocessing.ipynb
├── 02-Clean_Benefits.ipynb
├── 03-Clean_Rate_and_Plan_then_Merge.ipynb
├── 04-EDA.ipynb
├── 05-Recommender.ipynb
└── README.md
```

## Outline

* *[01-Preprocessing](./01-Preprocessing.ipynb)*

> Notebook in which pre-processing of our two main CSVs is performed by compressing the data.

* *[02-Clean_Benefits](./02-Clean_Benefits.ipynb)*

> Filter data by year of interest (2016) and drop un-needed columns.

* *[03-Vectorize_Plans](./03-Vectorize_Plans.ipynb)*

> One-hot encode benefits associated with each plan, and create vectors of unique benefits which can be analyzed. Add plan data to a Postgres SQL server with foreign key on an AWS EC2 instance.

* *[04-EDA](./04-EDA.ipynb)*

> Exploratory data analysis of our data, including analysis of benefits versus premiums and overall distribution of premiums.

* *[05-Recommender](./05-Recommender.ipynb)*

> A vector is created based on the benefits selected by the user and is compared against a matrix of unique benefit vectors. By performing a calculation called `cosine similarity`, which calculates the mathematical distance between two vectors, a list of matching plan types are shown to the user. The user can also view additional details about the plans presented to make the best selection.
