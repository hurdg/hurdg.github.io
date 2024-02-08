---
title: Greenhouse gas emissions in Canada
summary: A descriptive overview of emissions by sector, province/territory, and year.
tags:
  - EDA
date: '2023-09-01T00:00:00Z'

# Optional external URL for project (replaces project detail page).
external_link: ''

image:
  caption: Greenhouse gas emissions by province
  focal_point: Smart

links:
  - icon: twitter
    icon_pack: fab
    name: Follow
    url: https://twitter.com/georgecushen
url_code: ''
url_pdf: ''
url_slides: ''
url_video: ''

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: example
---

# Exploratory Data Analysis of Greenhouse Gas Emissions in Canada

# Introduction

Greenhouse gasses (GHG) in the atmosphere have been suggested to cause environmental threats such as global warming, sea level rise, and increased extreme weather events (Conference Board of Canada, 2013). As of 2020, the GHG emissions of Canada were the 11th highest of any country (ECCC, 2023). Although Canada makes up approximately 0.48% of the global population, its GHG emissions account for 1.5% of total emissions worldwide (United Nations, 2023; Statistics Canada, 2023; ECCC, 2023). As part of the Paris Agreement in 2015, the Government of Canada (GoC) has committed to reduce GHG emissions by 30% in relation to its 2005 levels, by 2030 (ECCC, 2023). More recently, a 2023 GoC initiative has stated a higher target of 40-45% GHG reductions from 2005 levels by 2030 (ECCC, 2023).
As the sources of Canadian GHG emissions are geographically varied and are associated with a range of activities/economic sectors, the path to reducing emissions will involve a study of these sources in the context of GHG reduction opportunities. By comparatively visualizing provincial per-capita GHG emission, historical sector-specific Canadian GHG emissions,  and the relation between Canadian Gross Domestic Product (GDP) and sector-specific GHG emissions, this project aims to contribute to such a study. The goal of this project is to provide a clear and informative overview of GHG emissions in Canada, with a focus on quantifying the relationship between GHG emissions and economic activity.

# Data Set

The datasets in this project will be pulled from publicly accessible government websites. As per the terms and conditions of both the Natural Resources Canada website and the Canada Energy Regulator website; content can be reproduced and used as long as it is for non-commercial purposes, without any additional permission required.
One of the datasets originates from the Canada Energy Regulator website; this showcases a map of Canada with a breakdown of energy emissions usage based on each province. The data will be pulled in a treemap format from the website and stored in a tabular format. Each column will indicate a year from 2010-2020 and each row will indicate an observation based on a single province/territory (C. E. R., 2023). The plan is to create two tabular datasets from this format; one of which will present Natural Gas usage based on each province and the second will present Crude Oil usage based on each province.
The next two datasets will be pulled from the Natural Resources Canada website. Both these datasets will have columns from 2000-2020. The first dataset will present ‘Canada’s GHG Emissions by Sector, End Use and Subsector’. The observations in the first dataset will be split up into the following categories: Residential, Commercial/Institutional, Industrial, Passenger Transportation, Freight Transportation, Off-Road and Agriculture. The values of each observation is listed in the Megatonne unit of measure (Natural Resources Canada, 2023). Both datasets will be presented in a tabular format. The second dataset consists of ‘Commodity Prices and Background Indicators’. As with the data pulled from the Canada Energy Regulator website, this data will also support the main topic in addressing GHG Emissions within Canada. The rows are subdivided into ‘Commodity Prices and Total GDP’ consisting of each sector (Natural Resources Canada, 2023).  
The final dataset encompasses the population of Canada based on each province. The dataset, also presented in tabular format, shows the breakdown of population on a quarterly basis from 2000 through 2020, and each row is separated based on province or territory. This is directly pulled from the Statistics Canada website (Statistics Canada, 2023). One important thing to note is that these population values will be estimates.

# Guiding Questions

Collectively, the guiding questions of this project are designed to depict the spatial-temporal trends of GHG emissions in Canada. The purpose of these depictions are to highlight potential opportunities for reductions in GHG emissions; per-capita representations of spatial GHG emissions are likely to be more relevant than absolute abundances. Absolute levels of GHG emissions may over-emphasize areas that have high GHG emissions due simply to a larger population burden. Similarly, the GHG emissions of a specific economic sector may be best represented relative to the economic contribution of the same sector (measured as a proportion of GDP). In this way, modeling per-capita provincial GHG emissions and sector-specific GHG emissions relative to economic output may reveal areas wherein GHG emissions have been proportionally reduced and, more importantly, areas of potential improvement.

How have GHG emissions relative to economic output changed over the past two decades in the major sectors of Canada’s economy?

As part of this question, the size of an economic sector will be compared to its GHG emissions. Visualizing this relationship should provide foundational insight into the sources of GHG emissions. Further insights can then be drawn by visualizing the trend in GHG emissions per dollar value of an economic sector. In theory, a decrease in the proportion of GHG emissions per dollar could signify that the sector is becoming more efficient, or ‘green’. In contrast, sectors where GHG emissions are increasing per dollar may warrant further investigation as candidates for GHG emission reduction.
Although the relationship between productivity and sector-specific GHG emissions will be informative, the price of commodities may have an important impact on GHG emissions that is not fully captured by the GHG per dollar ratio. In particular, it is known that higher oil and gas prices enable unconventional oil and gas production methods to become economically viable (Helbling, 2013). Different production methods are likely to have different GHG emission profiles, which may have a material impact on the overall GHG emissions of a sector.

How have the per-capita GHG emissions of Canadian Provinces and Territories changed over the past two decades?

Each region plays a unique role in contributing to Canada’s emission profile, with varying levels of economic activity, energy resources, and climate policies. Understanding how GHG emissions over the past two decades have evolved at the provincial and territorial level provides insights into which regions are making progress in reducing emissions, which ones are maintaining emissions, and which ones require more targeted efforts. Insights can be drawn by visualizing trends in per capita emissions for each province and territory. To provide a comprehensive analysis, the potential impacts of climate policies will be considered in each region by identifying key climate policies and assessing their timelines. Understanding the per capita emissions by region may help us evaluate the equity of emission reductions, in that the burden of reducing emissions is equitably distributed among regions, considering their economic and energy profiles.

# Data Wrangling and Visualization for Guiding Question 1

As our first guiding question looked at the economic impact of GHG emissions, we began by analyzing GHG emissions by sector and the commodity prices which provided GDP values for Canada's largest sectors. The initial dataset consisting of subsectors had duplicate titles for similar GHG end uses (e.g. Space Heating is found in both residential and commercial properties). To account for this, we decided to rename the respective rows with a hyphen to indicate which sector it falls under. We removed the subheaders with totals for each sector to focus our data on the end uses and to create a separate table for column totals (Ghg1 table).
The second dataset for our first guiding question entailed GDP generated by the biggest sectors in Canada on a yearly basis, namely Industrial, Commercial and Agriculture. The dataset also included GDP through electricity generation. For the purpose of our analysis, we focused on the 3 main sectors. Later on, we will be combining these two datasets (GHG Emissions by Sector and GDP by Sector).
The next task was to take our 3 main sectors - Industrial, Commercial, Agriculture and combine the amount of GHG emissions that were emitted by each sector and compare that to the total GDP of that given year. We will need the row of totals from the GDP dataset, as well as the sector totals for each year which we had previously removed from the Ghg dataframe.

Finally, we merged the two dataframes (one consisting of the total GDP generated by sector per year and the second dataset which shows the total GHG emitted by sector per year). Recall, we focused on three main sectors (Industrial, Commercial and Agriculture). Once merged, we proceeded to transpose the data to have each year show by row value as the index and each respective column indicate the sector. The last two columns showcase the total GHG per Year and total GDP per Year.

With both dataframes merged together, this makes it easier to compare and visualize our dataset to address our guiding question.

![Image1: line](/ghg_proj/ghg1.png)

# Data Wrangling and Visualization for Guiding Question 2

To answer our second guiding question, we first aimed to identify the regions making the most significant contributions to total GHG emissions. We approached this by creating line plots to visualize the emission trends from 2000 to 2021. To do this, we defined a list of regions based on the last characters of our Excel files and used a for loop, along with an f string, to read these files. We filtered each file by a specific column and row using the loc function, transposed the data frames, and combined them. We then customized the column names and generated plots using Plotly.

![Image2: line](/ghg_proj/ghg2.png)

For a different perspective on major contributors, we also created a pie chart based on the filtered data for the year 2019.

![Image3: pie](/ghg_proj/ghg3.png)

