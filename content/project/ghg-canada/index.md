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

GHG Per capita figure generation.  In a separate section of code, we processed a population CSV file. After removing missing values and resetting the index, we filtered the data to retain only Q1 data for each year. We used regular expressions to extract the four-year digit and create a new column called "Year." Similarly, we did this for the two-character quarter data and placed it in another new column. We then filtered the data to include only rows corresponding to Q1. Following this, we sorted and reordered the columns to achieve a specific structure. From this refined DataFrame, named df_pop, we further filtered the data to obtain the 2019 population statistics. We followed a similar process for GHG data for the year 2019. Finally, we divided the GHG column by the population column and created a bar graph using Matplotlib. This bar plot visualizes the relationship between per capita GHG emissions for each province and territory for the year 2019 to facilitate comparative analysis.

![Image4: byprov](/ghg_proj/ghg4.png)

# Findings and Analysis of Guiding Question 2

The visualizations from both line plots (figure 1b) and pie charts (figure 2b) indicated that Alberta is the largest contributor to GHG emissions, followed by Ontario, Quebec, Saskatchewan, British Columbia, Manitoba, Nova Scotia, New Brunswick, Newfoundland and Labrador, PE Island, Northwest Territories, Nunavut, and Yukon. Alberta alone accounts for approximately 38% or more than a third of all GHG emissions, with Ontario as the next significant contributor, making up 22.6%, which is just under a quarter of the total emissions. However, this method doesn't account for variations in regional population sizes.

When considering per capita GHG emissions, as demonstrated in figure 3b for the year 2019, it becomes clear that the top three per capita emitters, in order, are Alberta, Saskatchewan, and the Northwest Territories. This suggests that individuals in these regions have a relatively high environmental impact in terms of GHG emissions. Despite their smaller populations compared to provinces like Ontario or Quebec, residents in these regions are contributing more emissions per person. This underscores the need for tailored policies and strategies at both the provincial and individual levels to address this impact effectively.

The objective of the comparative visualization in figure 4b was to address the question of how various provinces and territories were managing their GHG emissions over time. The bubble graph proved to be an effective tool for this analysis, allowing us to not only observe trends in GHG emissions but also population growth (on the x-axis) and the size of bubbles representing GHG emissions from the energy sector. Our examination of this visualization revealed a common trend among all provinces: an increase in population between the years 2000, 2010, and 2019. However, there were distinct patterns in GHG emissions management. Notably, Ontario, Nova Scotia, New Brunswick, Prince Edward Island, and the Northwest Territories demonstrated a commendable reduction in emissions over time. A closer look at the data and consultation with relevant literature provided insights into the reasons behind these changes. For instance, Ontario's emissions reduction was primarily attributed to the closure of coal-fired electricity generation plants, as reported by Environment and Climate Change Canada (2023). Quebec also displayed a slight decreasing trend in emissions, which could be linked to reductions in the residential sector, as well as changes in aluminum production and petroleum refining industries. Conversely, British Columbia showed relatively stable emissions. The remaining provinces experienced emissions increases in tandem with population growth. Alberta and Saskatchewan, in particular, saw notable increases due to heightened activities in the oil and gas industries, as noted in the Environment and Climate Change Canada report (2023).

Given the overall goal of reducing GHG emissions, the total amount of emissions particular to a province/territory can be taken to represent the severity of the issue in that region. However, the severity does not necessarily correlate directly with potential for improvement. In many ways, population and GHG emissions are directly linked. Efforts aimed at reducing GHG emissions are likely best focused on the areas with the most potential for improvement, which are not necessarily the areas where the problem is most severe. Figure 5b was created to highlight htis concept. Whereas Ontario was one of the highest GHG emitting provinces on an absolute level, suggesting a sever issue, it's per capita output is relatively modest. On the other hand, Alberta and Saskatchewan were identified amonth the highest total GHG emitters, and remained high at the per capita level. This suggests that the GHG emissions of the regions are severe, but that there may be a relatively high potential for improvement. Given this framing, it is also important to remember the other contribtuting factors associated with GHG emissions. In the same way that population are GHG emissions are somewhat inseperable, so too is economic activity. As stated above, Alberta contributes substantially to the oil and gas industries, which may have a substantial impact on the hypothesized potential for reduction in GHG emissions. Additional studies into the GHG emission profile of Alberta, and also Saskatchewan, will be essential for the further identification of potential areas for GHG emission reductions in Canada.

# Conclusion

In summary, our analysis of GHG emissions in Canadian provinces and territories reveals a clear hierarchy of contributors, with Alberta leading the way, followed by Ontario, Quebec, Saskatchewan, and other regions. Further resolution to these spatial trends was provided by per capita modelling, which revealed a consistently high GHG output form Alberta, and to a lesser extent, Saskatchewan. These findings underscore the significance of tailored policies, especially when considering per capita emissions, where Alberta and Saskatchewan stand out as high-impact areas. A closer examination of GHG emissions management over time highlights positive trends in several provinces, such as emissions reduction in Ontario, Nova Scotia, New Brunswick, Prince Edward Island, and the Northwest Territories. These reductions are often tied to specific actions, like the closure of coal-fired plants in Ontario and changes in the industrial landscape in Quebec. Conversely, Alberta and Saskatchewan experience emissions increases, largely driven by activities in the oil and gas sectors. This comprehensive analysis calls for region-specific strategies to address GHG emissions effectively. We acquired valuable skills through this process, including proficient data wrangling techniques, the capability to efficiently iterate through multiple datasets simultaneously, and the ability to construct informative dashboards.
As for the economic impact of GHG emissions, there has been a steady increase in GHG emissions for the industrial, freight and passenger transportation sectors, in contrast, there has been a marginal decrease in the residential and commercial sectors over the same period. Our analysis shows that despite the efforts by the government to reduce GHG emissions across Canada, there continues to be an upward trend from the industrial and transportation sectors, this is also evident with the continued rise in GDP for most of the period. 

As we dived deeper into the topic of GHG emissions, we acquired valuable skills, including proficient data wrangling techniques, the capability to efficiently iterate through multiple datasets simultaneously, and the ability to construct informative dashboards.

# Reference List
1. Boyce, S. (2023). Political Governance, Socioeconomics, and Weather Influence Greenhouse Gas Emissions
across Subnational Jurisdictions in Canada (Doctoral thesis; University of Alberta)

2. Natural Resources Canada - Office of Energy Efficiency - Demand Policy and Analysis Division. (2023, February 28). Commodity prices and background indicators. Government of Canada, Natural Resources Canada.
https://oee.nrcan.gc.ca/corporate/statistics/neud/dpa/showTable.cfm?type=HB&sector=aaa&juris=ca&year=2020&rn=5&page=0

3. Natural Resources Canada - Office of Energy Efficiency - Demand Policy and Analysis Division. (2023, March 14). Canada’s GHG emissions by Sector, end use and subsector – including electricity-related emissions. Government of Canada, Natural Resources Canada. https://oee.nrcan.gc.ca/corporate/statistics/neud/dpa/showTable.cfm?type=HB&sector=aaa&juris=ca&rn=3&page=0

4. Slav, I. (2022, August 11). Alberta oil output hits record high. OilPrice.com. https://oilprice.com/Latest-Energy-News/World-News/Alberta-Oil-Output-Hits-Record-High.html

5. Canada, N. R. (2023). Natural Resources Canada. Natural Resources Canada - GHG Emissions by Sector, End Use and Subsector. https://oee.nrcan.gc.ca/corporate/statistics/neud/dpa/showTable.cfm?type=HB&sector=aaa&juris=ca&rn=3&page=0

6. Environment and Climate Change Canada (2023). Canadian Environmental Sustainability Indicators: Global greenhouse gas emissions. Consulted on Sept 23, 2023. www.canada.ca/en/environment-climate-change/services/environmental-indicators/globalgreenhouse-gas-emissions.html.

7. Environment and Climate Change Canada (2023a).Water governance and legislation: provincial and territorial. Retrieved on Oct 10, 2023 from https://www.canada.ca/en/environment-climate-change/services/water-overview/governance-legislation/provincial-territorial.html

8. Government of Canada, C. E. R. (2023). Canada energy regulator / Régie de l’énergie du Canada. CER. https://www.cer-rec.gc.ca/en/data-analysis/energy-markets/provincial-territorial-energy-profiles/provincial-territorial-energy-profiles-explore.html

9. Helbling, T. (2013). On the Rise: High prices and new technology have triggered a sudden surge in oil and gas production in the United States that could shake up global energy markets. Finance & Development, 50(1), ISBN: 9781475542684. International Monetary Fund. DOI: https://doi.org/10.5089/9781475542684.022

10. McKinney,W.(2010). Data Structures for Statistical Computing in Python. Proceedings of the 9th Python In Science Conference 445: 51-56.https://conference.scipy.org/proceedings/scipy2010/pdfs/mckinney.pdf

11. Natural Resources Canada - Office of Energy Efficiency - Demand Policy and Analysis Division. (2023). Commodity prices and background indicators. Natural Resources Canada https://oee.nrcan.gc.ca/corporate/statistics/neud/dpa/showTable.cfm?type=HB&sector=aaa&juris=ca&year=2020&rn=5&page=0

12. Osler. (2023.). Canadian Climate Change Policy Developments. Retrieved from https://www.osler.com/en/resources/regulations/focus/canadian-climate-change-policy-developments

13. Data Network Team. (n.d.). Provinces and territories - Canada, (2023), Opendatasoft. Retrieved October 3,
2023 from https://data.opendatasoft.com/explore/dataset/georef-canada-province%40public/information/
?disjunctive.prov_name_en&location=3,63.54855,-70.22461&basemap=jawg.streets

14. Statistics Canada. Table 17-10-0009-01  Population estimates, quarterly. Consulted on Sept 23, 2023. DOI: https://doi.org/10.25318/1710000901-eng

15. United Nations, Department of Economic and Social Affairs, Population Division (2022). World Population Prospects: The 2022 Revision, custom data acquired via website

16. Plot Types — Matplotlib 3.8.0 Documentation. https://matplotlib.org/stable/plot_types/index.html. Accessed 9 Oct. 2023.

17. Wasom, M. L., (2021). Seaborn: statistical data visualization. Journal of Open Source Software, 6(60), 3021, https://doi.org/10.21105/joss.03021

18. Plotly Technologies Inc (2015). Collaborative data science. https://plot.ly.

19. Wlodkowski, P. Making Interactive Choropleth Maps with Temperature Data, (2021), GitHub repository.
Retrieved October 1, 2023 from https://github.com/pawlodkowski/interactive_climate_map

20. Vu, T. (2022, April 24). Thu-VU92/python-dashboard-panel: Interactive Visualization Dashboard in python with panel. GitHub. https://github.com/thu-vu92/python-dashboard-panel

21. Vu, T. (2022, March 9). How to create a beautiful python visualization dashboard with panel/hvplot. YouTube. https://www.youtube.com/watch?v=uhxiXOTKzfs&ab_channel=ThuVudataanalytics

22. nkmk. (2023, August 8). Pandas: Transpose DataFrame (swap rows and columns). pandas: Transpose DataFrame (swap rows and columns). https://note.nkmk.me/en/python-pandas-t-transpose/

23. Holoviz Contributors. (n.d.-a). Fast List Template. FastListTemplate - Panel v1.2.3. https://panel.holoviz.org/reference/templates/FastListTemplate.html

24. Stratis, K., Santos, A., Weber, B., Hjelle, G., Jablonski, J., Schmitt, J., Finegan, K., & Breuss, M. (2023, February 7). Combining data in pandas with merge(), .join(), and CONCAT(). Real Python. https://realpython.com/pandas-merge-join-and-concat/

25. Kelsey Jordahl, Joris Van den Bossche, Martin Fleischmann, Jacob Wasserman, James McBride, Jeffrey Gerard, … François Leblanc. (2020, July 15). geopandas/geopandas: v0.8.1 (Version v0.8.1). Zenodo. http://doi.org/10.5281/zenodo.3946761

26. Environment and Climate Change Canada (2023). Canadian Environmental Sustainability Indicators: Greenhouse Gas Emissions. Consulted on October 15, 2023. Available at https://www.canada.ca/en/environment-climate-change/services/environmental-indicators/greenhouse-gas-emissions.html







