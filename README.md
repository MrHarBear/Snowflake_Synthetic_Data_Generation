# Snowflake Synthetic Data Generation
In the world of ML and AI, data is king. But real-world data can be a real pain to deal with. Using real data is basically a legal minefield and comes with so much scrutiny. And rightfully so, we want to make sure our individual data and privacy are protected. However, the cost and time involved in collecting, cleaning, and storing data can be astronomical and often completely inhibits a project from getting started.

That's where synthetic data comes in – it's like the stunt-double of the data world, the heroes behind the scenes that actually make the actions happen!

### What We'll Do in this lab:

Synthetic data means different things to different audiences, and we find this illustration helpful to clarify the differences. 
![Synthetic Data Spectrum](https://github.com/user-attachments/assets/fa33d863-71b4-4d00-b48a-7bf654c9eaf8)

- **Left**: we have synthetic data that has the same structure as an original dataset with valid types, but doesn’t necessarily mimic the statistical patterns of real data. This kind of synthetic data is a good fit for using an LLM like Llama to help generate.

- **Right**: we have data that is not only structurally similar and valid, but also mimics the univariate distributions and correlations between columns. This is where the Snowflake capability that we’ve built shines


Snowflake now offers a built-in stored procedure called `GENERATE_SYNTHETIC_DATA` (in PuPr as of 2025-02-05) that can generate synthetic data, while preserving the statistical distribution and structural pattern of the original data. In this example, we utilize the new procedure to generate and pad out data for an insurance fraud use case. We then fit this new data through our XGBoost model and see if the performance improves. Note that becuase we haven't done anything to specifically augment the data, we wouldn't expect the model performance to improve significantly, if at all. 

Our goal here is to demonstrate how easy it is for one to synethically generate data on Snowflake, whether it's the LLM route or the stored-procedure route, and how we can make sensitive production data available in lower environments in a privacy-enhancing manner. This use case has applications across a variety of industries including healthcare, technology and financial services.


This is a simple workflow highlighting the capabilities we have in Snowflake when it comes to sythentically generating new datasets. The architecture below illustrates the basics steps of how we will execute the synthetic data generation process in Snowflake. 
![Synthetic Data ML Model Workflow](https://github.com/user-attachments/assets/12c327ab-1892-4b2f-b6e5-efea5f0e579b)


## Requirements
- We are using the `ACCOUNTADMIN` role in this case. If you are using a different role, ensure the appropriate privileges are granted or modify the script to fit your role.
> [!IMPORTANT]
> If you are using a trial account, the `SETUP.sql` part will give you some errors. Specifically, you will not be able to create `NETWORK RULE` and `INTEGRATION` since this feature is not enabled for trial accounts. So please ignore those. Similarly, instead of running the first notebook (i.e. part 0), you will need to manually upload the `csv` files into Snowflake and in the `DATA` schema.

## Setup
1. Run the `Build25London - Synthetic_Data_Generation.sql` script in Snowflake, this will create all the necessary objects
- **Database**: `BUILD_LONDON_25`
- **Schema**: `NOTEBOOK` and `Data`
- **Warehouse**: `Demo_BUILD_WH`
- **Network Rule**: `GITHUB_NETWORK_RULE`
- **External Access Integration**: `GITHUB_EXTERNAL_ACCESS_INTEGRATION`
2. Upload the two Notebooks into the `BUILD_LONDON_25.NOTEBOOK` schema. Use the `Demo_BUILD_WH` Warehouse.
<img width="579" alt="Notebook setting" src="https://github.com/user-attachments/assets/ee83c200-76ca-436a-8fb7-4e27d1f5e2b0" />

3. Run through the notebooks and enjoy! If you are using a trial account, you will need to manually upload the `csv` files into Snowflake and in the `DATA` schema. The name of the data table must match the name of the csv files exactly. e.g. `claim_data.csv` should be `SWT2024_DEMO_AUTO_INSURANCE.DATA.CLAIM_DATA` table in Snowflake.

## Disclaimer:

