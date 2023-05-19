# DSC80-Project3-Report
by Xiaojie Chen(A17015417), Chenri Luo(A16636808)
## Introduction



## Cleaning and EDA
### Data Cleaning

#### 1. We **left merged** the recipes and interactions datasets together and then **filled all ratings of 0 with** `np.nan`. 

- This is because NaN values are typically excluded from statistical calculations such as mean, median, and standard deviation. By converting 0 ratings to NaN, we can ensure that these calculations accurately represent the ratings provided by users, without distorting the statistical measures due to explicit low ratings.

#### 2. We **dropped the duplicate id column**, which is `recipe_id`. 

- As after merging two data frames, which are ratings and interactions, both dataframes have the recipe id columns, which are unneccessary to have two columns. So we delete one to save some time when doing the analysis. 

#### 3. We found the **averate rating per recipe** as a Series and then added this Series to the merged dataframe. 

- As for the same recipe, it may have several rows (i.e receive more than one ratings and reviews), so by getting a average rating of each recipe, we can get a single representative rating that indicates overall sentiment or perceived quality of each recipe. 

#### 4. We cleaned the `nutrition` column, from values that are list-look-like strings to actual lists, and then we created a separate column for every unique value in the `nutrition` lists, so now each value in list of the `nutrition` column has its own column titled `calories`, `total fat`, `sugar`, `sodium`, `protein`, `saturated fat`, and `carbohydrates`.  

- By giving a separate column for each type of nutrient, it makes our analysis process faster and easier for inidivual analysis of each nutrient and also filtering like filter and sort recipes based on specific nutritional criteria. 

#### 5. Using the same procedure, we also clean the `tags`, `steps`,  and `ingredients` columns into actual lists, which are all originally list-look-like strings. 

#### 6. We dropped the `review` column. 

- It doesn't have much usage. 

#### 7. We removed duplicate rows of unique recipe and only saved one row for each recipe. 

- As we have gained the data of average ratings for each recipe, and we can perform analysis on the average ratings for each recipe instead of the several ratings coming from severl users. 

#### 8. We created a filtered_df that only contains receipes with 720 or less cooking minutes 

- The original merged DataFrame contains many receipes that have unreasonable cooking minutes, 1051200 minutes for instance, and most of the recipes fall in the range from 0 - 720 minutes. And we consider 720 mintues(12 hours) as a resonable cooking time. Therefore, we only want to focus on recipes with 720 or less cooking minutes. 

- `head` of our cleaned DataFrame
                                                                                                                                             
  
### Univariate Analysis 

#### 1. The distribution of average rating of recipes

- In order to display the distribution of average rating, we create a histogram of column 'average rating' from filtered_df, which number of bins = 10. From the histogram, we can see that about 64% of average rating is in the range of 4.75 - 5. The number of the rest of the average ratings(rating from 1 - 4.75) add up to only 36% of the total. 

#### 2. The distribution of cooking minutes of recipes

- In order to display the distribution of cooking minutes, we create a histogram of column 'minutes' from filtered_df, which number of bins = 240. From this hisogram, we can see the distribution is right skewed. Half of the recipes cooking minutes are in the range from 20 - 60 minutes, among them, about 9 percent of cooking mintues are in 30-34 range. 


### Bivariate Analysis

We want to use bivariate analysis to determine if there is a statistical link between the other variables and the variables that we are interested in, if so, how strong and in which direction that link is.

#### 1. We create a a scatter plot of cooking minuetes vs food calories

- When analyzing this dataframe, we believe calories may be potentially related to average rating and cooking minutes. So we first created a scatter plot of cooking minuetes vs food calories. However, when we observe the data of calories, we find that there are also a lot of food calories that we consider to be outliers. So we filtered the data down to foods with less than 3,000 calories. From this scatter plot, we can see a slightly positive correlation, which is calculated to be about 0.11. 

#### 2. We create a scatter plot of average rating vs food calories

- We also wanted to observe whether calories were related to average rating, so we created a scatter plot of average rating vs food calories and used different colors for each point to distinguish minutes. But this graph doesn't show any inclination. So there's no connection between the two variables. 

#### 3. We create side-by-side boxplots for minutes of recipes with average rating under 4.6 vs above 4.6

- By analyzing this data frame, we know that the mean average rating is about 4.6. Therefore, we divided the data frame into two parts, one is the average rating of less than 4.6(low average rating), the other is greater than 4.6(high average rating). We want to know if there is a difference between cooking minutes with a high average rating and cooking minutes with a low average rating. As can be seen from the figure, there is a slight difference between the two groups. The low average rating of cooking minutes median was 40, while the other group was 35. And their Q3 is 65 and 60, respectively.



### Interesting Aggregates


## Assessment of Missingness
### NMAR Analysis

The column `description` is NMAR, because users might have chosen not to provide a description for certain recipes or left it blank intentionally. In order to further investigate potentially make it Missing at Random (MAR), additional data we would like to obtain can be user demographics, such as age, gender, or location, so we can further investigate whether the missingness of description depend on these factors or not. For example, if older users tend to not writing description when rating a recipe, it will change the missingness mechansim of `description` to MAR. 

### Missingness Dependency

#### 1. depends on

The missingness of column `average rating` **does** depend on the column `n_steps`. 

##### Null hypothesis: 
the distribution of `n_steps` is the same when column `average rating` is missing and when column `average rating` is not missing. 

##### Alternative hypothesis: 
the distribution of `n_steps` is not the same when column `average rating` is missing and when column `average rating` is not missing.
 
##### Distribution of column `n_steps` when column `average rating` is missing and not missing

<iframe src="assets/steps_missingness_1st.html" width=800 height=600 frameBorder=0></iframe>

##### test statistic

We choose k-s as our test statistic 

##### Result interpretation: 

After doing 500 times of shuffling of the n_steps column and calculates the k-s test statistic, we get a p-value of 0.0, indicating that we should reject the null hypothesis under any siginificance value. We can draw the conclusion that  column `average rating` is MAR dependent on `n_steps`. 

<iframe src="assets/steps_missingness_2nd.html" width=800 height=600 frameBorder=0></iframe>

#### 2. does not depend on

The missingness of column `average rating` **does not** depend on the column `sodium`. 

##### Null hypothesis: 
the distribution of `sodium` is the same when column `average rating` is missing and when column `average rating` is not missing.

##### Alternative hypothesis: 
the distribution of `sodium` is not the same when column `average rating` is missing and when column `average rating` is not missing.

##### Distribution of column `sodium` when column `average rating` is missing and not missing

<iframe src="assets/sodium_missingness_1st.html" width=800 height=600 frameBorder=0></iframe>

##### Result interpretation: 

After doing 1000 times of shuffling of the sodium column and calculates the k-s test statistic, we get a p-value of 0.128, indicating that we fail to reject the null hypothesis under siginificance value of 0.05. We can draw the conclusion that the missingness of column `average rating` does not depend on column `sodium`.

<iframe src="assets/sodium_missingness_2nd.html" width=800 height=600 frameBorder=0></iframe>


## Hypothesis Testing

#### 1.clarification
We categorize the recipes into two different groups, using a threshold that is the mean average-rating of all recipes, which is around 4.6 
high average-rating: recipes with average-rating that is higher than 4.6
low average-rating: recipes with average-rating that is lower than 4.6

#### 2.null hypothesis
In the population, cooking time (minutes) of recipes with a high average-rating and recipes with a low average-rating have the same distribution, and the observed differences in our samples are due to random chance.


#### 3.alternative hypothesis
In the population, recipes with a high average-rating have shorter cooking time than recipes with low average-rating. The observed difference in our samples cannot be explained by random chance alone.

#### 4.test statistic
mean minutes of high average-rating recipes - mean minutes of low average-rating recipes

#### 5.significance level
0.05

#### 6.p-value
0.0
 
#### 7.justification

<iframe src="assets/permutation.html" width=800 height=600 frameBorder=0></iframe>

















