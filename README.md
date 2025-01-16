# NSDP_Challenge
Project Title: Advanced Data Analysis and Visualization Challenge

Project Objective: 
To analyze National Summary Data Page (NSDP) datasets to identify trends, and derive actionable insights that enhance transparency, support policy decisions, and improve economic planning.
  To analyze this data, there are two parts,
    1.	Pre-process the data and get it in shape using python
    2.	Build Reports to allow easy analysis of the underlying data

Introduction: 
This project focuses on the analysis of National Summary Data Page (NSDP) dataset, provided by IMF. The NSDP serves as a critical resource for enhancing data transparency and providing standardized economic and financial information.
The analysis aims to uncover trends, and generate actionable insights to support evidence-based policymaking and strategic economic planning. By leveraging advanced data processing tools and methodologies, this project enhances the usability of NSDP datasets for stakeholders, including policymakers, researchers, and financial institutions.

Pre-requisites:
  1. Install Anaconda and use Jupiter notebook to write/execute python code
  2. Install PowerBI Desktop to build/run reports

Files provided:
1. NSDP_Challenge_get_region.ipynb
2. NSDP_preprocess.ipynb
3. NSDP Delay Report.pbix

Execution process:

Python:
1. Launch Jupiter notebook. Open the NSDP_Challenge_get_region.ipynb from your directory.
2. Run the script NSDP_Challenge_get_region.ipynb. This will generate world_bank_country_data_v1.csv in the same directory in your local machine. This CSV file is further used in the 
   NSDP_preprocess.ipynb 
3. Place nsdp_delays_random.xlsx and world_bank_country_data_v1.csv in a folder in your local computer and note down the file path
4. Open the NSDP_preprocess.ipynb file and change the paths to your file paths from the step above
5. Run each cell in squence from the notebook
6. This will generate the required files and analyze the data to answer the questions provided by stakeholders

We are ready with pre-processed data

PowerBI Reporting:
1.	Use the generated file filtered_output_file_v1.csv in PowerBI as the data source
2.	Open the "NSDP Delay Report file.pbix" file provided in PowerBI Desktop
3.	All the filter options are pre-selected. Use the filters to interact with data

PowerBI Analysis:

Filters Description:
Region - Country Filter
This filter allows users to focus on specific regions (e.g., East Asia & Pacific, Europe & Central Asia ) or drill down to individual countries within those regions. For instance, selecting "Japan" from East Asia & Pacific isolates its data in both visuals, enabling precise analysis.

Year Filter
The year filter enables users to analyze trends or summaries for specific time periods (e.g., 2015–2020). Selecting multiple years aggregates data for those periods, while isolating a single year highlights its unique delay trends, helping pinpoint annual variations.

These filters enhance interactivity, making the report customizable for detailed exploration.

**Visual 1: Line Graph - Average Delay by Year and Country**
This graph illustrates the trends in average delays (in days) for various countries over time (2015-2023). Each line represents a country, showing how delays fluctuate yearly. Notable spikes or dips  in delays in specific countries.

This graph interacts with Region - Country Filter and Year Filter to display only those data points that are selected by the user helping the user interact with the data

**Visual 2: Clustered Bar Graph - Average Delay by Region**
The bar graph shows the average delay (in days) across different regions. Each bar represents a region, enabling easy comparison of delay patterns. Regions like "East Asia & Pacific" and "Middle East & North Africa" may have higher averages, highlighting potential inefficiencies or challenges.

This graph interacts with Region - Country Filter and Year Filter to display only those data points that are selected by the user helping the user interact with the data. They can select the Region which will auto selects all the counties in the region and select all years of specific years.

**Visual 3: Bar Graph - Top 5 Countries with Highest Average Delays**
This bar chart highlights the top five countries with the longest average delays (in days). Countries like Japan and Egypt show the highest delays, indicating potential systemic inefficiencies or unique challenges compared to others. 

This visual will not interact with any filters

I have created the below measure to support the visual.
Country Rank = RANKX(ALL(filtered_output_file_v1[Country Name]), [Average Delay], , DESC, Skip)

**Visual 4: Scatter Plot - Outlier Countries (based on z-score)**
The scatter plot uses z-scores to identify outliers among countries in terms of average delays. Outlier points, marked differently, represent countries with delays significantly above or below the norm. This helps in focusing on countries that deviate from typical trends for further analysis.

From the analysis, we found that 
1.	Argentina has a delay of 0 days that might be an outlier
2.	Mexico has a delay of 0 days that might be an outlier

I have implemented the same z-score calculation that I have built in python code. 

In Power BI, I have created the below to implement the visualization.
1.	Calculated columns:
A.	Z_Score = (filtered_output_file_v1[Delay (days)] - AVERAGE(filtered_output_file_v1[Delay (days)])) / STDEV.P(filtered_output_file_v1[Delay (days)])
B.	Is_Outlier = IF(ABS(filtered_output_file_v1[Z_Score]) > 1.8, "Outlier", "Normal")

2.	Measures:
A.	Average Delay = AVERAGE(filtered_output_file_v1[Delay (days)])
B.  Std Dev Delay = STDEV.P(filtered_output_file_v1[Delay (days)])

Assumptions and decisions: 
1. in PowerBI report, an assumption is made that
      Visual 1: Line Graph - Average Delay by Year and Country, and
      Visual 2: Clustered Bar Graph - Average Delay by Region
      Will interact with the filters Region - Country Filter and Year Filter
   
2. For Z-score implementation and indetifying the outliers, the threshold is set to +/- 1.8 which is been adjusted and decided upon after trying the other vales for threshold like 2 and 3
3. The averages are rounded to two decimal places

 For further questions or analysis, please contact priya.kamath.bie@gmail.com








  
