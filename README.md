## **IMF Data Analysis and Visualization Project**
![Main Dashboard](https://github.com/user-attachments/assets/afa6bec4-6cdf-4242-b0b1-31511c2836fd)
---

### **Table of Contents**
1. [Objective](#objective)  
2. [Tools and Technologies Used](#tools-and-technologies-used)  
3. [Steps Completed](#steps-completed)  
4. [Example of Preprocessed Data](#example-of-preprocessed-data)  
5. [Challenges and Solutions](#challenges-and-solutions)  
6. [Results and Findings](#results-and-findings)  
7. [Deliverables](#deliverables)  
8. [SQL Scripts](#sql-scripts)  
9. [DAX Formulas](#dax-formulas)  
10. [Resources](#resources)  

- ## Project by Group 5
  
|Contributors | Track |
|-------------|-------|
|[Ifeka Odira Hillary](https://www.linkedin.com/in/odira-ifeka/) | Data Analysis|
|[Deborah Abosede](https://www.linkedin.com/in/deborah-abosede-91b282296/)| Data Analysis|
|[Convenenant Lenu](https://www.linkedin.com/in/convenantlenu/) | Data Analysis|
|[Dr Isaac Israel](https://www.linkedin.com/in/samuel-israel-90893b228/) | Data Analysis/ PR|
|Melvind-P |Backend Developer|
|Kenechukwu | Backend Developer|


## **Objective**
Collaborate with backend developers to extract data from the IMF, preprocess it, analyze historical trends, and create interactive Power BI dashboards to visualize insights.  
### Countries Dashboard
![Countries Dashboard](https://github.com/user-attachments/assets/dc0b3745-ff0d-4548-837b-97c0e2816983)

---

## **Tools and Technologies Used**
- **Power BI**: For interactive dashboard creation and visualization.  
- **PostgreSQL**: For database management and data storage.  
- **SQL**: For querying and transforming the dataset.  
- **Power Query**: For preprocessing and cleaning the data within Power BI.  
- **Draw.io**: For creating the Entity Relationship Diagram (ERD).  
- **ChatGPT**: For guidance and collaboration during the project.  

---

## **Steps Completed**

### **1. Data Extraction and Database Setup**
- Extracted data from the IMF website for **5 European countries**: Germany, Austria, Switzerland, Luxembourg, and Spain.  
- Preprocessed data using Power Query and SQL for consistency and missing value handling.  
- Database schema includes three tables:  
  - `countries`  
  - `annual_data`  
  - `quarterly_data`
   
-  PostGreSQL ERD
 ![ERD DIAGRAM pgerd](https://github.com/user-attachments/assets/9bdd23e8-3fea-4dd2-8b41-93b4879b86b2)

### **2. Database Integration with Power BI**
- Imported data into Power BI directly from the PostgreSQL database.  
- Relationships created:  
  - `annual_data.Country_ID → countries.Country_ID`  
  - `quarterly_data.Country_ID → countries.Country_ID`  

### **3. Dashboards Created**
1. **Main Dashboard**: High-level summary of all data.  
2. **Annual Report Dashboard**: Focused on annual data trends.  
3. **Quarterly Report Dashboard**: Focused on quarterly data trends.  
4. **Country Report Dashboard**: Detailed breakdown by country.  

---

## **Example of Preprocessed Data**
The table below shows an example of long metric names that were renamed for simplicity:

| **Metric**                                              | **Renamed Metric**      |
|---------------------------------------------------------|-------------------------|
| Net International Investment Position                   | Net Investment Position |
| Direct Investment Abroad                                | Direct Inv Abroad       |
| Reserve Assets                                           | Reserve Assets          |
| Portfolio Investment Liabilities                        | Portfolio Inv Liab      |
| General Government Debt Securities                      | Gov Debt Securities     |
| Direct Investment in Reporting Economy                  | Direct Inv Report Econ  |
| Other Investments in Reporting Economy                   | Other Inv Report Econ   |

---

## **Challenges and Solutions**

### **1. Data Size and Complexity**
- **Challenge:** Large datasets and long metric names.  
- **Solution:** Used Power Query for preprocessing and shortened metric names.  

### **2. Import Errors**
- **Challenge:** Issues importing data into PostgreSQL and Power BI.  
- **Solution:** Consolidated and cleaned files before re-importing.  

### **3. Negative Values in Metrics**
- **Challenge:** Negative values in Net International Investment Position.  
- **Solution:** Replaced negative values with `0` in key metrics.  

### **4. Dynamic Country-Level Filtering**
- **Challenge:** Displaying visuals dynamically by country.  
- **Solution:** Created slicers and dynamic text boxes in Power BI.  

---

## **Results and Findings**

### **Findings from SQL Analysis**
- **Top Reserve Assets by Country:** Germany consistently reported the highest reserve assets among the analyzed countries.  
- **Debt Trends:** Austria's debt securities showed a steady increase over the years, indicating a potential reliance on debt financing.  
- **Investment Patterns:** Direct investments in Luxembourg and Switzerland were significantly higher compared to other countries, reflecting their strong positions as financial hubs.  
- **Quarterly Variations:** Quarterly data revealed seasonal patterns, with Q2 and Q3 showing higher investment activities compared to Q1 and Q4.  

---

## **Deliverables**
1. **Database Query Script:** SQL scripts for creating and populating the database.  
2. **Entity Relationship Diagram (ERD):** Visual representation of table relationships.  
3. **Power BI Dashboards:**  
   - Main Dashboard  
   - Annual Report Dashboard  
   - Quarterly Report Dashboard  
   - Country Report Dashboard  
4. **Insights:** Historical trends and patterns.  
5. **Documentation:** Detailed steps, challenges, and solutions.  

---

## **SQL Scripts**

```SQL
-- Create countries table
CREATE TABLE countries (
    Country_ID SERIAL PRIMARY KEY,
    Country VARCHAR(50) NOT NULL
);

-- Create annual_data table
CREATE TABLE annual_data (
    Annual_ID SERIAL PRIMARY KEY,
    Country_ID INT REFERENCES countries(Country_ID),
    Metric VARCHAR(100) NOT NULL,
    Year INT NOT NULL,
    Value DECIMAL NOT NULL
);

-- Create quarterly_data table
CREATE TABLE quarterly_data (
    Quarter_ID SERIAL PRIMARY KEY,
    Country_ID INT REFERENCES countries(Country_ID),
    Metric VARCHAR(100) NOT NULL,
    Quarter VARCHAR(10) NOT NULL,
    Value DECIMAL NOT NULL
);

-- Insert data into countries table
INSERT INTO countries (Country)
VALUES
('Germany'),
('Austria'),
('Switzerland'),
('Luxembourg'),
('Spain');
```

---

## **DAX Formulas**
``` DAX
Main Dashboard
Total Investments = SUM(annual_data[Value])
Average Reserve Assets = AVERAGE(annual_data[Value])


#Annual Return

Yearly Change = 
    CALCULATE(
        SUM(annual_data[Value]),
        PREVIOUSYEAR(annual_data[Year])
    )


#Quarterly Report

Quarterly Change = 
    SUM(quarterly_data[Value]) 
    - CALCULATE(SUM(quarterly_data[Value]), PREVIOUSQUARTER(quarterly_data[Quarter]))


#Country Report

Selected Country Name = SELECTEDVALUE(countries[Country])
Total Country Investment = CALCULATE(SUM(annual_data[Value]), FILTER(countries, countries[Country] = SELECTEDVALUE(countries[Country])))

```
---

## **Resources**
- [InternPulse](https://www.internpulse.com/)
- [IMF Data Repository](https://www.imf.org/en/Data)  
- [Draw.io](https://app.diagrams.net/)  
- [Power BI](https://powerbi.microsoft.com/)  
- [ChatGPT](https://openai.com/)  
