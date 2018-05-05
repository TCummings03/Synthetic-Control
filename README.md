# U.S. Gun Laws & Gun Deaths

Hello! That you for viewing my project - "U.S. Gun Laws & Gun Deaths."  The goal of this project was to explore the relationship (if any) between gun laws and gun deaths in the United States.  Furthermore, the project uses the synthetic control model to analyze the effect of Oregon's background check laws that were implemented in 2000.  Unfornately, mass shootings have become more and more common in the United States, and as a result, policy dicussions arise that try to ascertain the best course of action.  Finding solutions to reduce gun violence in the U.S. is important because it has real world implications and lives are on the line.

The data for this project comes from a variety of sources, which will be outlined in a citations section at the end of this project. However, all of the data came from publicly available datasets and the majority of the data came from places like the Center for Disease control, Census Bureau, FBI, and the Bureau of Labor Statistics.

The analysis can be broken up into two sections:

1) Exploratory Data Analysis
2) Contructing the Synthetic Control

Link To Code goes here
Link to datasets goes here


### Section 1: Exploratory Data Analysis

What has been the relationship between gun laws and gun deaths over the past decade and a half? Which states have the highest crude firearm death rate? Which states are the most/least strict when it comes to gun laws? These are the questions this section will attempt to answer.  Here, we will consider the period from 1999-2016 to do our initial analysis.

It is important to note that this data, which comes from the CDC, uses the "Crude Death Rate" as a measure of gun deaths in the United States.  This rate is standardized for each state since it is per 100,000 people. Here is a sample of the dataframe below:

![Initialdf](https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/initialdf_head.png)

By calling the .describe() pandas method on our dataframe, we can learn a few things about the data:

- The average gun death rate in the U.S. from 1999 - 2016 was 11.46, and the median death rate was 11.2
- The min and max cases, 2.2 and 34.0 respectively, are states with either relatively high death rates or relatively low death rates
- The standard deviation was 4.43 over time time period, which means that 95% of the data lies between a gun death rate of 2.6 and 20.29

Which states have the highest and lowest crude death rate?

![CrudeDeathRateBarChart](https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/CrudeDeathsBarChart.png)

The graph above gives us a general outline of where different states (and the District of Columbia) stand on gun death rates. The average gun death rate for all states during this time period is 11.46, as mentioned above. At the higher end of the spectrum, DC's Death rate looks to be over 5x higher than Hawaii. Another interesting aspect to note, the New England states appear to be lower on the chart whereas the Southern States appear to be either in the middle or higher on the chart.  Let's explore the number of gun laws and Gun deaths over this same time period.

![StrictBarChart](https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/StrictBarChart.png)

The plot above seems to have some similarities with the death rate plot presented earlier, where the least strict states appear to be in the south and midwest and the most strict states appear to be in the New England area.

![GunLawsvsGunDeaths](https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/GunLawsvsDeaths.png)

One of the most important thing to note about the plot above is that there are two different y axis. The left axis maps the total number of gun laws in the U.S. while the right axis maps the crude death rate in the U.S. (per 100,000 people).  Also included in the plot is the expiration date for the federal assault weapons ban, which ended in 2004 after 10 years. However, the year after the assault weapons ban, there is a rise in the gun death rate that levels off for the next three years. Although we cannot determine causality just by looking at the plot above, we can say that there have been some of the deadliest mass shootings in U.S. history since the assault weapons ban expired.  Two of these, Virginia Tech and Sandy Hook, are highlighted on the plot above.  What is interesting to notice about these two events is that the subseqeunt years we see a stead increase in the number of gun laws passed with a dramatic increase in gun legislation passed after the shooting at Sandy Hook occured. 

Another interesting note on the relationship between these two figures is that after Sandy hook not only do we see an increase in the number of gun laws passed, but we also see a fairly dramatic increase in the crude death rate in the U.S. as well. Although this project will not touch on those events specifically, they are interesting to note and would benefit from further analysis. 

### Linear Regression (Gun Deaths & Gun policies)

Explain where the gun law data came from. How many laws, the type of categories etc.

The data on gun laws for this project came from a kaggle dataset, which in part, was coded by the Boston University School of Public Health.  The full dataset incldues 133 laws on firearms across 50 states and Washington, DC.  However, this project will focus mainly on the 50 states.  The 133 laws can be broken down into 14 distinct categories:

- Dealer regulations
- Buyer regulations
- Prohibitions for high-risk gun possession
- Background checks
- Ammunition regulations
- Possession regulations
- Concealed carry permitting
- Assault weapons and large-capacity magazines
- Child access prevention
- Gun trafficking
- Stand your ground
- Preemption
- Immunity
- Domestic violence

Condensing the 133 laws into these 14 categories will allow us to potentially gain more insight into the types of laws that have impact on the gun death rate.  After condesing the 133 laws, I ran an OLS regression on the data, which yielded the following results:

| RMSE          | Test R^2      | Train R^2  | Avg. 5 fold CV   |
| ------------- |:-------------:| :-----:|:----------------------:|
| 2.849     | 0.536 | 0.553 |0.518|

Largest Positive Coefficients:

|Features|	Estimated Coefficients|
|:-------:|:----------------------:|
|Asltweap_and_largemags|	0.428 |
|Ammunition_reg|	0.386|
|Conceal_carry_perm	|0.258|
|Poss_reg |	0.180 |
|Domestic_violence	|0.075 |
|Dealer_reg |	0.035|

Largest Negative Coefficients: 

|Features|	Estimated Coefficients|
|:-------:|:----------------------:|
|Preemption|	-1.337 |
|Immunity|	-0.722|
|Stand_your_ground	|-0.697|
|Child_access_prev |	-0.664 |
|High_risk_gunpos	|-0.373 |
|Buyer_reg |	-0.191|

From the simple linear regression model above, we can see that the type of law does a fairly good job at predicting gun deaths in a given year/state as a result of what type of gun laws exist.  With a RMSE of 2.85 and a 5-fold cross validation score of .52, we can glean some insight into this relationship by taking a look at the coefficients of each law category. One would expect a negative relationship between gun laws and gun deaths, because the more restrictions on buying guns would, in theory, mean fewer guns because laws would make it more difficult to obtain them.  

The largest coefficients are assualt weapons and large magazine laws and ammunition regulation laws with coefficients of 0.43 and 0.39 respectively.  That means that a one unit increase assualt weapons and large magazine laws and ammunition regulation laws leads to a 0.43 and 0.39 increase in the crude death rate.  On face value, this may seem counterintuitive that more laws would lead to more deaths.  However, we cannot definitevly conclude that more assault weapons bans and magazine restrictions will lead to a higher death rate.  It may be the case that states with a higher mean death rate will also have a higher desire to implement more laws that try to address the death rate.  

Conversely, we see that preemption and immunity have the largest negative coefficients with -1.34 and -0.72 respectively.  If a state has 1 in the preemption category, it means that there are NO laws on the books that prevent local governments from regulating firearms.  This means that local governments have the ability to write their own laws specific to each locality.  This would suggest that localities with more felxibility to implement their own laws are more successful at reducing the death rate.  If a state has 1 in the immunity category, it means that there are NO laws that provide a blanket immunity to gun manufacturers for damages caused by their products. Ostensibly, this means that individuals and local and state governments have the ability to sue gun manufacturers, which may provide them an extra incentive to make sure their products are as safe as possible.  It is interesting to not that the two types of laws with the highest negative coefficients and therefore highest impact on reducing gun deaths are laws that empower local governments to make decisions about their gun laws.  Although we cannot be certain what type of laws cause fewer gun deaths, from the linear regression above, we have gained a little more insight into the types of laws that can be effective.

Let's take a visual approach to seeing the relationship between when a state changes it's laws and the resulting change in death rate:

![Change_Laws_Deaths](https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/Change_Laws_Deaths.png)

The green area in the graph above is the area in which a state changed it's gun laws (added laws or removed laws) and it lead to a decrease in the death rate the following year.  The red shaded area is where a state changed it's laws and it lead to an increase in the death rate the following year. Believing that more gun laws lead to fewer gun deaths, we would expect to see the majority of points in the green shaded areas and to the right of zero.  The first thing I notice from the graph above is that there doesn't really seem to be a trend with the number of laws passed and the change in death rate. This makes sense -- perhaps it's the quality over quantity idiom at work here.  Just because a state passes a law does not mean it will have an effect on gun laws if the law is not substantial enough to move the needle. Another caveat is that this just looks at the law changed from one year to the next and the resulting effect on the death rate. However, it isn't a longitudial approach, which would have a greater impact on causality.  In fact, although there appear to be outliers around -0.4 in death rate change and -0.3 in death rate change, it will be more interesting to analyze the states that have singluar changes in laws passed to be able to see the effect of individual laws on death rate.


You can use the [editor on GitHub](https://github.com/TCummings03/SyntheticControl/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/TCummings03/SyntheticControl/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
