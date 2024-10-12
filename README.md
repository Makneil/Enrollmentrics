# EnrollMEnTRICS
*This is a dashboard developed through Microsoft Power BI to practice my skills in Power BI*

### Introduction
The goal of this project is to developed a dashboard utilizing Microsoft Power BI by conducting descriptive type of data analysis and some exploratory data analysis. The dashboard is mainly focused on visualizing the enrollment data for senior highschool in the Philippines for schoool year 2017-2021 which is publicly available dataset through their website *Department of Education*. The dataset includes summarized number of enrollees categorized by region, sector, level, strand and gender. With these, we can also able to aggregate the enrollment data into different metrics to uncover patterns and correlation between segments.

### Project Highlight
Microsoft Power BI, Power Query, DAX (Data Analysis Expressions), Data Visualizations

---
### Project Overview

##### The Dashboard
<div>
  <img src = "https://github.com/Makneil/Enrollmentrics/blob/main/Dashboard_p1.png", alt = "Dashboard Screenshot 1">
  <br>
  <img src = "https://github.com/Makneil/Enrollmentrics/blob/main/Dashboard_p2.png", alt = "Dashboard Screenshot 1">
</div>
  
##### The Modules
- Top three (3) strand of enrollees
- Top three (3) region of enrollees
- Total enrollees by School Year
- Enrollees distribution by Geographical Island
- Enrollees distribution by Sector
- Enrollees distribution by Region
- Gender distribution by Region
- Gender distribution by Strand
- Gender distribution by Level
- Enrollees Growth Rate

---
### Analysis Key Findings
- The growth in senior high school enrollment in the Philippines during the school years 2017-2018, 2018-2019, and 2019-2020 showed a **positive correlation**. However, the **growth rate declined**, 10.55% (SY 2018-2019), 5.64% (SY 2019-2020), 1.34% (SY 2020-2021) with a difference of -4.91 and -4.3 respectively.
- **Region IV-A Calabarzon** consistently had the highest overall number of enrollees over four consecutive school years, while the **National Capital Region** reported higher enrollment numbers in four specific strands: *ABM, ARTS & DESIGN, SPORTS, and STEM*.
- While females generally have a higher enrollment ratio than males, the strands **MARITIME, SPORTS, STEM, and TVL** show a significantly higher ratio of **male** enrollees compared to females, likely due to the nature of these fields of study.
* Overall, the **TVL** strand consistently registered the highest number of enrollees across all regions over the four years, except in the *BARMM region*, where the GAS strand had higher enrollment. Below are the significant differences across regions and strands:
  1. In the 2017-2018 school year, the **GAS** strand had higher enrollment than TVL strand in **Region II - Cagayan Valley**
  2. The *GAS* strand in the **BARMM - Bangsamoro Autonomous Region in Muslim Mindanao** had higher enrollment than other strands over the four-year period.
  3. In the 2020-2021 school year, the **HUMMS** and **GAS** strands ranked highest in five regions: **HUMMS** was the top choice in *Region II, Region IV-A, Region XII,* and CAR,* while **GAS** led in *Region V*.
- Generally, **Luzon Island** has a higher number of enrollees across various strands over the past four years; however, there are more *MARITIME* enrollees in the *Visayas Island.*
- Although **public sectors** have a greater number of enrollees across regions and school years, the *ABM, MARITIME,* and *STEM* strands attract more enrollees in the **private sector**.
- It was observed that there were **no MARITIME enrollees** in *Region VIII (Eastern Visayas)* and *BARMM (Bangsamoro Autonomous Region in Muslim Mindanao)*. This indicates a lack of participation in maritime programs within these regions.

---
### The development process
<div><br></div>



##### Data Manipulation

Data Source Reference: https://www.deped.gov.ph/alternative-learning-system/resources/facts-and-figures/datasets/

- Transformed and consolidated data into more organized table  using Microsoft Excel by merging Gender and Sectors into one column and added a categorization per row.

From this (Original Data): https://github.com/Makneil/Enrollmentrics/blob/main/Historical-Number-of-Enrollment-in-ALL-SECTOR-Senior-High-School.xlsx

Into this (Working File): https://github.com/Makneil/Enrollmentrics/blob/main/Historical-Number-of-Enrollment-in-ALL-SECTOR-Senior-High-School_Working_File.xlsx

- Import working file into Power BI: Get Data > Excel Workbook > https://github.com/Makneil/Enrollmentrics/blob/main/Historical-Number-of-Enrollment-in-ALL-SECTOR-Senior-High-School_Working_File.xlsx > Load and Transform data
- Tranform tables (Rows and columns) using **Power Query**:
  
  *Rename the tables*
  ```
  = Table.RenameColumns(#"Reordered Columns",{{"Custom", "School year"}})
  = Table.RenameColumns(#"Reordered Columns",{{"Custom", "School year"}})
  = Table.RenameColumns(#"Reordered Columns",{{"Custom", "School year"}})
  = Table.RenameColumns(#"Reordered Columns",{{"Custom", "School year"}})
  ```

  *Append Queries as new to create another table merging the four tables into one*
  ```
  = Table.Combine({#"Total_Enrollees_SY2017-2018", #"Total_Enrollees_SY2018-2019",
  #"Total_Enrollees_SY2019-2020", #"Total_Enrollees_SY2020-2021"})
  ```

  *Create new tables for each categories for Data Modelling purposes, Data visualization sorting and interactive filtering accross visuals*
  
  ```
  ## Region_sort Table
  
  = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("XdHdasJAEAXgV1lyrVBt7c/lJi1FMFFSSEXxYhqXdOlmp+xuBPv0nTFIst4lmY/J4cx+n5Sq0WjFUkzF0mCNXvRfkkkySw6TAbDIoIEzWFGBMepMZB6Ti1E2ODBi1f1dttxHpJpKNnIlU1nu1gWBhxuQEsiXuSzXG0njxXhc0SzVNZoh5WM05wSfygfl6EV7SuvJPMVmHHNAzzeI1RvcrnqJ0m7J7KD9QrANiI2y2vrOAJd3N4bsCnThm5fl2h7BArKKOt7yL1/hBDg6wjwWTD6wrv2PB9eoC+krzmQp3/t2+YEHfbXUdJ5zcRTSQ4sOhewCWmyxu55baMrVeaPbKN7iupnXojtqursDIY+tttpTgUGf1Chsf4wiY1/QEC1VnMGvDjA62YzucfgH", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [Region = _t, Region_sort = _t])
  = Table.TransformColumnTypes(Source,{{"Region", type text}, {"Region_sort", Int64.Type}})

  ## School_year Table
  
  = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("i45WCo5UMDIwNNcFEhZKOkqGSrE6MEELkKAlUNAISdASKGhkABQ0RggaGYAEDYGCJkqxsQA=", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [#"School year" = _t, SY_Sort = _t])
  = Table.TransformColumnTypes(Source,{{"School year", type text}, {"SY_Sort", Int64.Type}})

  ## Strand Table

  = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("JcsxDoAgEATAr5CrKRAVbTESJBE1gDbIK4z/9zi7nd1szqAnDxwaKDzDcvoYUZIUk6lTS7C6Dh1lr4NLzhsseirStWJW/+vYQ3qQA1Ej2P0KIRWbTXR2w2mEUj4=", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [Strand = _t, Strand_sort = _t])
  = Table.TransformColumnTypes(Source,{{"Strand", type text}, {"Strand_sort", Int64.Type}})

  ## Level Table

  = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("i45Wci9KTElVMDRU0lEyVIrVgQsYAQWMlGJjAQ==", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [Level = _t, Level_sort = _t])
  = Table.TransformColumnTypes(Source,{{"Level", type text}, {"Level_sort", Int64.Type}})

  ## Sector Table

  = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("i45WCihNyslMVtJRUorVAfKKMssSS1Jh3OBQ52IfIAbzYwE=", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [Sector = _t, Sector_sort = _t])
  = Table.TransformColumnTypes(Source,{{"Sector", type text}, {"Sector_sort", type text}})
  = Table.RemoveColumns(#"Replaced Value",{"Sector_sort"})
  = Table.AddIndexColumn(#"Removed Columns", "Index", 1, 1, Int64.Type)
  = Table.RenameColumns(#"Added Index",{{"Index", "Sector_sort"}})

  ## Gender Table

  = Table.FromRows(Json.Document(Binary.Decompress(Binary.FromText("i45W8k3MSVWK1YlWckvNBTNjAQ==", BinaryEncoding.Base64), Compression.Deflate)), let _t = ((type nullable text) meta [Serialized.Text = true]) in type table [Gender = _t])
  = Table.TransformColumnTypes(Source,{{"Gender", type text}})
  ```
  *Create New table by merging total enrollees by year for data visualization purposes*
  
  ```
  ## Total_enrollees_by_year
  
  = Table.NestedJoin(#"Total_Enrollees_SY2017-2018", {"Region", "Sector", "Level", "Strand", "Gender"}, #"Total_Enrollees_SY2018-2019", {"Region", "Sector", "Level", "Strand", "Gender"}, "Total_Enrollees_SY2018-2019", JoinKind.LeftOuter)
  = Table.ExpandTableColumn(Source, "Total_Enrollees_SY2018-2019", {"Total Enrollees"}, {"Total_Enrollees_SY2018-2019.Total Enrollees"})
  = Table.RenameColumns(#"Expanded Total_Enrollees_SY2018-2019",{{"Total Enrollees", "Total_enrollees_SY2017-2018"}, {"Total_Enrollees_SY2018-2019.Total Enrollees", "Total_enrollees_SY2018-2019"}})
  = Table.NestedJoin(#"Renamed Columns", {"Region", "Sector", "Level", "Strand", "Gender"}, #"Total_Enrollees_SY2019-2020", {"Region", "Sector", "Level", "Strand", "Gender"}, "Total_Enrollees_SY2019-2020", JoinKind.LeftOuter)
  = Table.ExpandTableColumn(#"Merged Queries", "Total_Enrollees_SY2019-2020", {"Total Enrollees"}, {"Total_Enrollees_SY2019-2020.Total Enrollees"})
  = Table.RenameColumns(#"Expanded Total_Enrollees_SY2019-2020",{{"Total_Enrollees_SY2019-2020.Total Enrollees", "Total_enrollees_SY2019-2020"}})
  = Table.NestedJoin(#"Renamed Columns1", {"Region", "Sector", "Level", "Strand", "Gender"}, #"Total_Enrollees_SY2020-2021", {"Region", "Sector", "Level", "Strand", "Gender"}, "Total_Enrollees_SY2020-2021", JoinKind.LeftOuter)
  = Table.ExpandTableColumn(#"Merged Queries1", "Total_Enrollees_SY2020-2021", {"Total Enrollees"}, {"Total_Enrollees_SY2020-2021.Total Enrollees"})
  = Table.RenameColumns(#"Expanded Total_Enrollees_SY2020-2021",{{"Total_Enrollees_SY2020-2021.Total Enrollees", "Total_enrollees_SY2020-2021"}})
  = Table.RemoveColumns(#"Renamed Columns2",{"School year"})
  ```

  *Create new table for gender distribution by male and female, and merge into one table for data visualization purposes*
  ```
  ## Gender_distribution_male

  = Table.Combine({#"Total_Enrollees_SY2017-2018", #"Total_Enrollees_SY2018-2019", #"Total_Enrollees_SY2019-2020", #"Total_Enrollees_SY2020-2021"})
  = Table.SelectRows(Source, each ([Gender] = "Male"))

  ## Gender_distribution_female

  = Table.Combine({#"Total_Enrollees_SY2017-2018", #"Total_Enrollees_SY2018-2019", #"Total_Enrollees_SY2019-2020", #"Total_Enrollees_SY2020-2021"})
  = Table.SelectRows(Source, each ([Gender] = "Female"))

  ## Gender_distribution

  = Table.NestedJoin(Gender_distribution_male, {"School year", "Region", "Sector", "Level", "Strand"}, Gender_distribution_female, {"School year", "Region", "Sector", "Level", "Strand"}, "Gender_distribution_female", JoinKind.LeftOuter)
  = Table.ExpandTableColumn(Source, "Gender_distribution_female", {"Total Enrollees"}, {"Gender_distribution_female.Total Enrollees"})
  = Table.RenameColumns(#"Expanded Gender_distribution_female",{{"Total Enrollees", "Enrollees_male"}, {"Gender_distribution_female.Total Enrollees", "Enrollees_female"}})
  = Table.RemoveColumns(#"Renamed Columns",{"Gender"})
  ```

  


##### Data Modelling

*Created relationship and ensure the cadinality were correct accross tables, specially for those newly created tables for categorization **School_year**, **Region_sort**, **Sector**, **Gender**, **Level**, **Strand** one-to-many to the main table to have interactive filtering accross visuals.*
<img src = "https://github.com/Makneil/Enrollmentrics/blob/main/Data%20Model.png" alt="Data model">


##### DAX (Data Analysis Expressions) 

*Created measures for the growth rate accross school year*

```
## SY2017_2018_Total

SY2017_2018_Total = CALCULATE(SUM('Total_enrollees_SY2017-2021'[Total Enrollees]),
'Total_enrollees_SY2017-2021'[School year] = "SY 2017-2018")

## SY2018_2019_GR

SY2018_2019_GR = DIVIDE([SY2018_2019_Total] - [SY2017_2018_Total], [SY2017_2018_Total],0)

## SY2018_2019_Total

SY2018_2019_Total = CALCULATE(SUM('Total_enrollees_SY2017-2021'[Total Enrollees]),
'Total_enrollees_SY2017-2021'[School year] = "SY 2018-2019")

## SY2019_2020_GR

SY2019_2020_GR = DIVIDE([SY2019_2020_Total] - [SY2018_2019_Total], [SY2018_2019_Total],0)

## SY2019_2020_Total

SY2019_2020_Total = CALCULATE(SUM('Total_enrollees_SY2017-2021'[Total Enrollees]),
'Total_enrollees_SY2017-2021'[School year] = "SY 2019-2020")

## SY2020_2021_GR

SY2020_2021_GR = DIVIDE([SY2020_2021_Total] - [SY2019_2020_Total], [SY2019_2020_Total],0)

## SY2020_2021_Total

SY2020_2021_Total = CALCULATE(SUM('Total_enrollees_SY2017-2021'[Total Enrollees]),
'Total_enrollees_SY2017-2021'[School year] = "SY 2020-2021")
```

