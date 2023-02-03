# Phase 2 Project - Joel Mott

### Project Overview:

The purpose of this project is to enable a real estate stakeholder to make specific recommendations to families with school-age children who are considering a new home. I use linear regression modeling in order to account for various common factors that such families consider. Beginning with a baseline model as a benchmark, I add and refine multiple predictors until I have a trained model that outperforms said baseline model while satisfying linear regression requirements and issues pertaining to multicollinearity. 

### Business & Data Understanding:

This fictional stakeholder would be a residential real estate company based in and serving King County, Washington, which includes Seattle, many of its neighboring suburbs, and rural areas to the east. This stakeholder wants specific insight on data that will help them make reliable recommendations to both buyers and sellers when it comes to a home's listing price. This stakeholder would almost certainly already be aware of some obvious findings below (how square footage has a strong correlation with listing price), so my scope of work would be to find some sort of insight that brings value to the company that they may not have necessarily considered in-depth yet. In this sense, my recommendations may not necessarily appeal to *every* buyer, such as those who do not have school-age children in mind, but they would still have some relevance to any seller residing in a good school district. 

### Modeling

My approach is an iterative process that begins with a baseline model with just the highest-correlating predictor (square footage) and the target variable (price) without any sort of transformation applied to either. This results in an R-sqaured value of 0.492 and a condition number over 1,600. This shows a strong correlation, but some likely issues with non-normal distribution. 

Next, I add in school district information for each record by cross-referencing the 'zipcode' column in the given dataset with school district information on the King County tax assessor's website and zipdatamaps.com. Some zip codes are zoned for only one school district, while others are zoned for multiple districts. In those cases, rather than sort through every record and pinpoint which school district the house is in, I took the average of the zipcode's district's scores. 

These scores were compiled from Niche.com, a site which assigns a school district a score "grade" of D-/D/D+ through A-/A/A+ (for a total of twelve possible scores) for every school district in the United States. I combined zip codes, district names, and scores into an Excel file, converted it to a CSV file, and then joined it to the preexisting CSV file in order to obtain a final DataFrame with which to work on this project. 

Only at this point was it feasible to perform a train-test-split, so I did so here before doing anything further to build a model. It turns out that all school districts in King County have a score between B- and A+. Out of the entire range of scores, assigning an integer 1-12 for each, this means each district had a score of 7 through 12. As a whole, school districts showed a measureable correlation to price, although certainly not one as strong as square footage. This first attempt at a model only showed an R-squared score of 0.133, so there was definitely a great deal of room for improvement. 

In order to understand just how each school district score correlated with price, I used one-hot encoding to convert them to dummy variables (dropping the first column and using as a reference category). This added school districs with a score of 8 through 12 to the model. This only brought the R-squared metric up 0.152, although the condition number was staying reasonable. In any case, it was time to bring in more predictors. 

The next two were variables dealing with size and layout: square footage and bedrooms. I incorporated these because, after extensive Googling for considerations families have in mind when purchasing a home, these were almost always constant alongside good schools and crime level. I noticed that the square footage column ('sqft_living') wasn't normally distributed, which violates linear regression requirements. Subsequently, I logarithmically transformed it so it would show a normal distribution. 'Bedrooms' was a categorical value that was added alongside the now-transformed 'sqft_living_log' column for the next attempt at a model iteration. This brought the R-squared value up to .482, which was *almost* as high as the baseline score, so further improvement was needed. 

Another idea I had for familial considerations when purchasing a home was whether buying during the summer months had any impact on price since that's when school-age students are out of school for summer break and moving can be less disruptive. I converted this non-numerical variable from the 'date' column using DateTime and isolated just the months. 'Summer_purchase' is a column that showed whether a home purchase took place in the months of June, July, or August. However, this had almost no effect on listing price and the R-squared score. While this didn't improve the model, it was at least somewhat helpful to be able to advise families that they didn't necessarily have to consider raising their bidding price based on a pruchase taking place over the summer. 

Finally, I looked at the distribution of the target variable and found that it was also highly skewed. Subsequently, after applying a logarithmic transformation, the distribution was normal and the final model's R-sqaured score rose to 0.666 with a reasonable condition number easing concerns about multicollinearity or distributions. 

### Regression Results

The results of this final model (shown at the end of the notebook) still have square footage taking the lead on price correlation, but also highlighting a significant impact on home prices in neighborhoods with school districts receiving a Niche.com score at or above an "A-". Of course, there are likely confounding factors at work; the higher-scoring districts are largely in our surrounding Seattle, but balancing school districts with afforadability and square footage are often primary concerns of families as opposed to other location-based considerations. 

Additionally, it was interesting to see how the number of bedrooms had very little impact on listing price and, at that, it was a negative correlation. This may be explained by smaller homes or layouts with fewer rooms still costing quite a lot due ot their location in Seattle. 

### Conclusion

The resuls of this model offer the stakeholder specific recommendations to make to any client selling a home in a high-scoring school district. It also helps inform family buyers of how much to expect to pay for homes in these districts while balancing other common concerns. With this refined model's perspective on their data, the stakeholder would have more specific insight into a large part of the market to give it an advantage over its competitors.
