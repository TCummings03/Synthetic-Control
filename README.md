# U.S. Gun Laws & Gun Deaths

Hello! Thank you for viewing my project - "U.S. Gun Laws & Gun Deaths."  The goal of this project was to explore the relationship (if any) between gun laws and gun deaths in the United States.  In addition, the project uses the synthetic control method to analyze the effect of Oregon's background check laws that were implemented in 2000.  Unfornately, mass shootings have become more and more common in the United States, and as a result, policy dicussions arise that try to ascertain the best course of action.  Finding solutions to reduce gun violence in the U.S. is important because it has real world implications. When it comes to gun policy -- lives are certainly on the line.

The data for this project comes from a variety of sources, which will be outlined in a citations section at the end of this project.  All of the data came from publicly available datasets, and the majority of the data came from places like the Center for Disease control, Census Bureau, FBI, and the Bureau of Labor Statistics.

The analysis can be broken up into two sections:

1) Exploratory Data Analysis
2) Contructing the Synthetic Control

Link To Code goes here
Link to datasets goes here


## Section 1: Exploratory Data Analysis

What has been the relationship between gun laws and gun deaths over the past decade and a half? Which states have the highest crude firearm death rate? Which states are the most/least strict when it comes to gun laws? These are the questions this section will attempt to answer.  Here, we will consider the period from 1999-2016 to do our initial analysis.

It is important to note that this data, which comes from the CDC, uses the "Crude Death Rate" as a measure of gun deaths in the United States.  This rate is standardized for each state since it is per 100,000 people. Here is a sample of the dataframe below:

<img src="https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/initialdf_head.png?raw=true"/>

By calling the .describe() pandas method on our dataframe, we can learn a few things about the data:

- The average gun death rate in the U.S. from 1999 - 2016 was 11.46, and the median death rate was 11.20
- The min and max cases, 2.2 and 34.0 respectively, are states with either relatively high death rates or relatively low death rates
- The standard deviation was 4.43 over time time period, which means that 95% of the data lies between a gun death rate of 2.60 and 20.29

Which states have the highest and lowest crude death rate?

<img src="https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/CrudeDeathsBarChart.png?raw=true"/>

The graph above gives us a general outline of where different states (and the District of Columbia) stand on gun death rates. The average gun death rate for all states during this time period is 11.46, as mentioned above. At the higher end of the spectrum, DC's Death rate looks to be over 5x higher than Hawaii. Another interesting aspect to note, the New England states appear to be lower on the chart whereas the Southern States appear to be either in the middle or higher on the chart.  Let's explore the number of gun laws and Gun deaths over this same time period.

<img src="https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/StrictBarChart.png?raw=true"/>

The plot above seems to have some similarities with the death rate plot presented earlier, where the least strict states appear to be in the south and midwest and the most strict states appear to be in the New England area.

<img src="https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/GunLawsvsDeaths.png?raw=true"/>

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

<img src="https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/Change_Laws_Deaths.png?raw=true"/>

The green area in the graph above is the area in which a state changed it's gun laws (added laws or removed laws) and it lead to a decrease in the death rate the following year.  The red shaded area is where a state changed it's laws and it lead to an increase in the death rate the following year. Believing that more gun laws lead to fewer gun deaths, we would expect to see the majority of points in the green shaded areas and to the right of zero.  The first thing I notice from the graph above is that there doesn't really seem to be a trend with the number of laws passed and the change in death rate. This makes sense -- perhaps it's the quality over quantity idiom at work here.  Just because a state passes a law does not mean it will have an effect on gun laws if the law is not substantial enough to move the needle. Another caveat is that this just looks at the law changed from one year to the next and the resulting effect on the death rate. However, it isn't a longitudial approach, which would have a greater impact on causality.  In fact, although there appear to be outliers around -0.4 in death rate change and -0.3 in death rate change, it will be more interesting to analyze the states that have singluar changes in laws passed to be able to see the effect of individual laws on death rate.

## Section 2: Building the Synthetic Control Model

Figuring out the effect of public policy can be difficult, especially when there are not any feasible ways to run the gold standard randomized, double blind placebo controlled experiment. It is difficult to find an effective control group, and due to lack of alternatives, what is often used as a control to understand the impact of a policy on a given state is a nearby state.  However, this approach can fall apart when there a substantial differences in political and cultural environments for nearby states.  In addition, a qualitative approach can fail to generalize since its conclusions will often be idiosyncratic to the situation at hand.  Although it may seem daunting to figure out a solution to these issues, they present opporunities for innovations in the experimental design of public policy.  One method, which has gained more traction in the public policy arena is the Synthetic Control Method (SCM).  This method created by researchers Abadie, Diamond, andHainmueller (2010).

### What is the Synthetic Control Model?

The synthetic control method tackles the issue of an effective control state by creating a hypothetical "synthetic" counterfactural against which the treatment state is compared.  The control is a weighted average of a group of chosen "donor" states which did not implement the targeted policy during the preintervention and post intervention period.  The two main ingredients that go into constructing the synthetic control are predictor variables, which are variables that help predict the outcome in question, and a set of outcome variables themselves from the pretreatment period.  The result is a "synthetic" state that closely matches the target state during the pretreatment period and acts as a counterfactual for what the target state would have done had it not received the treatment.  The difference between the synthetic control and target state during the post intervention period is the proposed effect of the treatment.  

Policy analysts have the ability to rigorously analyze the synthetic control method.  A simple graphic representation can illustrate how well the synthetic control's path maps on to the target states path during the pretreatment period.  A listing of the donor states and their respective weights allows for greater transparency concerning how the control was contructed. In addition, ADH list a few assumptions that should be satisfied in order to accurately understand the effect of a given policy:

- None of the donor pool candidates can have a similar policy change
- The policy in the treated state cannot affect the outcome in the donor region states
- The variables used to form the weights in the synthetic control must have similar values in donor and treated regions
- Those variables and the outcome variables must have an approximate linear relationship

Steps in the Synthetic Control Method:

1. Identify predictors of the outcome variable
2. Identify possible donor states to synthesize the control state.
3. Choose a method for selecting predictor weights.
4. Assess the pretreatment period goodness of fit of the synthetic control state (generated
using the Synth package).
5. Analyze results


After looking at various states and their respective gun laws, I settled on analyzing the effect of Oregon's background check laws on its crude death rate. When choosing which states/laws I wanted to focus on, I mainly employed two criteria: which laws were well known in the public dicourse today, and which states did not pass any laws for a significant length of time pretreatment and post treatment, so the effect of the treatment could be adequately analyzed.  Oregon, which passed a series of background check laws(6) in 2000, along with two dealer regulation laws, was the perfect candidate based on these criteria. Background checks also have vast support among manyAmericans, so a deeper look into their effectiveness at lowering the crude death rate could provide some valuable insight. Alas, my outcome variable is the crude death rate, my treatment is the background check laws, and my pretreatment and post-treatment periods are 1991-2000 and 2001-2014 respectively.  

1. Predictor Variables - predictor variables should affect outcomes before and after treatment.

- hsdiploma - percentage of people with a high school diploma
- povrate	- the poverty rate 
- violcrimrate - violent crime rate
- conservative - percentage of people who identify as "conservative"
- pct_white - percentage of population that is white
- unemp_rate - the unemployment rate
- gunownership_proxy - a gun ownership proxy (firearm suicides/suicides)

2. Donor States - donor states must not have passed similar laws

In order to satisfy this assumption, the below 7 states had to be excluded from the donor pool. This left 42 potential states that could be used to create the synthetic control.

- Maryland
- Pennsylvania
- Tennessee
- Massachusetts
- North Carolina
- Illinois
- Connecticut

3. Huber Regression

Unlike ridge regression, Huber provides a linear loss to samples that are classified as outliers. However, "the loss function is not heavily influenced by the outliers while not completely ignoring their effect." (sklearn - http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.HuberRegressor.html). After including all of our predictor variables and including outcome lags (1992, 1994, 1996, 1998, 1999, 2000), the HuberRegressor returns a RMSE of 0.171, and a 2 - fold cross validation score of 0.756.  This low RMSE suggests that the model fits the data well -- let us continue this analysis by looking at a plot of the synthetic control. 

4. Assess pre treatment fit - the synthetic control should fit the real Oregon closely during the pretreatment period so it can be used as a control during the post treatment period.  We can assess the pretreatment period by both looking at the results of the weights and visually asses by plotting the synthetic control and real Oregon during the pretreatment period:


|index|Oregon|	Synth_Preds|
|:---:|:----:|:-----------:|
|1991|	12.500|	14.177|
|1992	|14.000|	14.012|
|1993	|12.800|	15.018|
|1994	|14.300|	14.610|
|1995	|13.600|	13.925|
|1996	|13.100|	13.150|
|1997	|13.000|	13.051|
|1998	|13.200|	13.099|
|1999	|11.500|	11.374|
|2000	|11.000|	10.945|
|hsdiploma|	82.160|	87.999|
|povrate|	12.160|	12.089|
|violcrimrate|	461.550|	461.582|
|conservative|	29.377|	34.359|
|pct_white|	0.877|	0.920|
|unemp_rate|	5.533|	5.532|
|gunownership_proxy|	0.615|0.674|


<img src="https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/RealOvsSynthO.png?raw=true"/>

The synthetic Oregon appears to match fairly closely with the real Oregon.  It is important to note that Synthetic Oregon's predictions are less accurate in the earlier years in the decade but improve towards the end of the decade.  This is important because it suggests that the synthetic control will be an effective control against the counterfactual Oregon that did not pass the background check laws.  As far as the predictor variables, we do see some variation amongst individual predictors, but overall we see that the synthetic control method has mapped on to real Oregon closely.  Furthermore, a look at the graph shows this dovetailing towards the latter years of the 1990s decade, and a convergence to the real Oregon.

5. Analyze results:

<img src="https://github.com/TCummings03/SyntheticControl/blob/master/Synthetic_Control_Files/RealOvsSynthFULL.png?raw=true"/>

After looking at the plot above of the Synthetic Oregon vs. Real Oregon, we notice that the two begin to diverge after the laws was passed in 2000.  However, we are more concered with the difference between the two during the period after the treatment occurs.  Furthermore, we want to know if the difference lasted or if it was temporary.  As we can see from the plot above, the difference between the synthetic control model and real Oregon is not zero for the period of time after the treatment and up until 2009. In 2010, they are nearly the same followed by a period of divergence and then convergence again in 2014.  The average difference between the synthetic control and real Oregon over the post treatment period was 0.66, which means that on average, the crude death rate in Oregon deacreased by 0.66 from 2000-2014.  Although the synthetic control model does provide an elegant way of creating a counterfactual against which to compare a treated dependent variable, it is important to note that we cannot definitely say the background check laws were causal.  However, we can say that we are closer to causality as a result of using the synthetic control model to predict what Oregon would have done if it had not implemented background check laws.

Further considerations:

- 

SOURCES:

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
