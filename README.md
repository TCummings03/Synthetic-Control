# Synthetic Control Model

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
