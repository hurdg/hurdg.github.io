---
title: Hubble
summary: An example of using the in-built project page.
tags:
  - Deep Learning
date: '2016-04-27T00:00:00Z'

# Optional external URL for project (replaces project detail page).
external_link: ''

image:
  caption: Photo by rawpixel on Unsplash
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

# Analysis 1  
  

## Introduction  
The study of astrophysics seeks to better understand and explain the forces and relationships that govern objects and phenomena across our universe. Through the application of chemistry and physics to stellar observations many innovative insights have been achieved; for example, the temperatures of distant stars have been accurately measured by analyzing the ratios of emitted wavelengths. Two particularly important insights involved the shifts in the emissions spectrums of astronomical objects and the angle between objects and Earth, which have allowed the measurements of the speed and distance of far away objects, respectively. These measurements of speed and distance have led to perhaps the most well-known astrophysical conclusion of all, that the universe is expanding.  

This concept was originally put forward by Edwin Hubble in 1929 (NASA, n.d.). Hubble’s conclusion received much publicity and ran contrary to many commonly held opinions of the time. Despite being commonly cited as one of the most revolutionary discoveries in the history of astrophysics, the underlying evidence is scarcely known. The assertion that the universe is expanding rested on the observations that the further an astronomical object was from earth, the faster it appeared to be moving. Hubble realized that such an observation was best explained by a theory wherein empty space was increasing in size.

![Image1: hubble](/hubble.png)

Despite the wide acceptance of the expanding universe theory, it can be a difficult concept to grasp. In this project, we aim to examine publicly available data pertaining to the speeds and distances of astronomical objects to better understand the conditions and evidence of the expanding universe theory. Our model of the universe's expansion is illustrated below. In this model, the universe is portrayed as a two dimensional grid that expands over a period of time. As the grid doubles in size, the distances between both the red dots and the blue dots correspondingly double as well. Interestingly, the blue dots must have travelled twice as fast (relative to each other) as the red dots, as the distance between the blue dots increased by twice as much as the red dots over the same time period.

![Image2: exp](/Expansion_model.png)

This model of the universe expansion would suggest a linear relationship between the distance an object is from an observer and its speed. As such, we set out to attempt to test the validity of this model by creating a linear function representing this relationship.  

## Description of data source  
  
To create this model, we first obtained data from the Gaia space observatory. The Gaia space observatory was launched by the European Space Agency (ESA) in 2013 (Gaia Collaboration, 2016). The data used in this project is from the third data release, completed in 2023. Initial processing and validation of the data was completed by the Gaia space observatory, the Gaia Data Processing and Analytics Consortium (GPDAC). GPDAC encourages public use of the data, and maintains no reservations or holds. The dataset is licensed under the DbCL v1.0 agreement, which allows users to share, modify, and use the contents as long as they source the dataset (Open Data Commons, n.d.).  

The space observatory is in a fixed orbit near earth, and continuously rotates about an axis, such that, over a period of 6 hours, it will sample across all visible objects perpendicular to its axis (ESA, 2023). The axis of rotation additionally shifts slowly over time to provide full coverage of the satellites surroundings (ESA, 2023). Given the methodical nature of the observatories sampling, the resulting data can not strictly be considered random. However, the observations in the dataset were sampled throughout the possible fields of view, and can be considered representative of the satellites surroundings. Given that the primary purpose of random sampling is to ensure the accurate representation of a population by a sample, we decided to cautiously proceed with tests that require assumptions of randomness. Similarly, the independence of samples are also influenced by the sampling procedure. In this case, it is unlikely that any of the observed astronomical objects are interdependent; even if the samples are observed in the same field of view, they are almost certainly separated by a vast distance that negates gravitational effects or other forces that act over long distances.  
  
## Linear Model  
  
### Data Visualization
  
As the dataset was large ($n$ > 600,000), we first took a random sample of 100,000 observations to increase computational speed. We then created a scatter plot of the speeds of astronomical objects as a function of their distance away from Earth (Figure 2). The plot did vaguely show that farther astronomical objects tended to be moving faster, but a high density of objects that were relatively near Earth and moving at a large range of speeds clouded any clear relationship.  

![Image3: scatter](/plot1.png)

Despite the concerns brought about by the scatter plot, we further tested the conditions required of a linear relationship. To test the condition of normality, we calculated the standardized residuals of the speeds and created a normal probability plot. Additionally, we assessed the homoscedasticity of the linear model by plotting the standardized residuals relative to the observed distance values.  

![Image4: sidebyside](/plot2.png)

The standardized residuals roughly followed a normal distribution, but somewhat deviated from the theoretical values, particularly near both extremes. From this alone, the appropriateness of a linear model was questionable. Furthermore, the standardized residuals of the speeds of astronomical objects were not evenly distributed relative to the distance away from Earth. This signifies that the variance in speeds is not constant; consequently, the use of a linear model to represent the relationship between speed and distance is definitively not appropriate.  

## Two Sample Model  
  
Without the option of a linear model, we opted to shift our study question and test the relationship in other ways. By reclassifying the distances of astronomical objects as categorical variables, a comparison of the mean speeds of each distance category would be possible.  
  
### Categorizing the Distance Variable  
  
Ideally, the categorization of the distance variable would be delineated based on a meaningful attribute. Unfortunately, we failed to obtain an astrophisically significant cutoff value. As such, we prioritized ensuring an equal distance range within the created categories. We therefore defined a cutoff value as half of the distance to the farthest observed astronomical object, which was equivalent to 17,833 parsecs. Astronomical objects between the cutoff value and the maximum observed distance were categorized as “Far”, and objects less than, or equal to, the cutoff value were classified as "Near".  samples. The distributions of astronomical objects, and their speeds, relative to the cutoff value are shown in the below figure.

![Image6: test](/plot3.png)

As can be seen in the scatterplot, the astronomical objects appear to be unevenly distributed relative to the cutoff value. In fact, the *Near* group contains 594865 astronomical objects, while the *Far* group is only composed of 2693. One possible explanation is that there are simply less stars farther away. However, we suspect that it is more likely an observation bias; as light diffuses as it radiates outward, we suspect that the brightness of a star is inversely related to its visibility from far distances. To account for this possibility, we specified our populations in question to only consist of *observable* astronomical objects.

**Population 1 (*Near*)**: Observable astronomical objects within 17109.07 pa of Earth  
**Population 2 (*Far*)** Observable astronomical objects between 17109.07 and 34218.14 of Earth  
  
Additionally, the drastic inequality in sample size did cause concern regarding its impact on the two-sample test. However, we determined that the principle effect of unequal sample sizes would be greater variance in the group with the smaller sample size. In our case, this was the far group. Greater variance in the far group is likely to manifest as an increased likelihood of a type II error. This principle is visualized in the below figure. The increased variance is represented by a wider and shorter distribution in the lower instance of the "B" sample. This change creates more opportunities for a conclusion of no significant difference. when in fact the population means are different, as represented by the area in red.

![Image7: typeII](/TypeII.png)

We further calculated the variances of each sample, and found that they were relatively similar.
  
$\bar{\sigma}_{Near} =$ `r round(sd(data$speed[data$distance_class=="Near"]),3)`  
$\bar{\sigma}_{Far} =$ `r round(sd(data$speed[data$distance_class=="Far"]),3)`  
  
## Data Visualization  

With that in mind, we further visualized the data with a pair of boxplots. In the boxplots, the distributions suggested a notable difference in speeds between the *Near* and *Far* astronomical objects, with the *Far* group appearing to have a mean speed near 1.5 km/s compared to 0.5 km/s for the *Near* group. Surprisingly, despite its lower standard deviation (above), the near group appeared to have a larger interquartile range.

![Image7: boxplot](/plot4.png)

Following the visualizations, we continued with the two-sample test. Given the different sample variances and unequal sample size, the most appropriate test would be the Welch $T$-Test, which accounts for unequal variances. Additionally, the large sizes of our samples allow the application of the Central Limit Theorem, and further enable the use of the Welch $T$-Test. We defined the null hypothesis as the mean speeds of *Near* astronomical objects being greater or equal to *Far* objects, and the alternate hypothesis as the mean speed of *Far* astronomical objects being greater than the mean speed of those that are *Near*. 
  
$\bar{\mu}_{Near} = 0.437_{\frac{km}{s}}$  
$\bar{\mu}_{Far} = 1.419_{\frac{km}{s}}$  