---
title: "P6"
output: html_document
---

## Summary

This visualization documents the variation in survival rates amongst passengers on the Titanic based on sex and passenger class. It employs a vertical 100% bar, grouped by first sex and then passenger class, to demonstrate the large discrepancy in survival outcomes amongst these various groups.

## Design

I started with the intention of creating a simple visualization that allowed one to very quickly grasp differences in survival rates across sex and passenger class.

Initially, as seen in index.html, I grouped the data first by passenger class (Pclass) and then by sex. This achieved the purpose, but each segment of each bar had to be closely evaluated in order to understand the visualization. Once I grouped by sex first, followed by Pclass, a much clearer trend emerges. The viewer can much more easily identify the pattern of survival rates. The bars are even in order from least likely to survive (3rd class male) to most likely to survive (1st class female).

I chose to do 100% bars instead of normal count bars because, as my dataset, is only a sample of the entire historical event. Moreover, I really wanted to show the trend amongst groups. This would have been more difficult to immediately picture if the bars were of different lengths. While the dataset is not terribly large, I don't think showing the percentages instead of raw counts is misleading or deceitful in this case.

I kept the default colors of red and blue as they are muted and comfortable to look at. Sex is a non-ordinal categorical variable; Pclass is ordinal and categorical. I see no inherent reason or purpose to be served by changing colors. Therefore by keeping the default colors I wanted to avoid any distractions that new colors could introduce.

I tried to keep enough negative space that the visualization would not be too crowded. I wanted to have small spacing within a gender and a larger space between genders.

The default hover tooltip, I find, is very useful as it gives exact percentages and can guide the viewer to be sure what segment of the data he/she is viewing. To me, that's a key advantage of doing this visualization in dimple as opposed to ggplot or matplotlib.

The animation is perhaps somewhat gimmicky. However, I do think it serves to highlight the almost linear progression in declining survival rates across sex and Pclass, and for that reason I think it is worth including.

## Feedback

Initially I saw that 1,0 for survival or not would not be sufficiently explanatory, and so that caused me to make the change in the dataset.

Feedback from friends who saw the first graphic made it clear to me that I had a problem with the grouping of the data. It was taking people longer than necessary to understand what the visualization was trying to convey. After switching grouping though from Pclass, sex to sex, Pclass, this problem went away. Although the visualization shows the same data and the tooltip reveals all of the same percentages for each segment, grouping by sex then pclass aligns the bars in a fashion that turns the graphic from exploratory to explanatory in my view.

## Resources

The dataset comes from Kaggle via Udacity: https://www.kaggle.com/c/titanic

I had explored the data in previous Udacity tasks and used R to update the 1,0 Survival data field to "Yes"/"No", which I think is more easily understood for the viewer.