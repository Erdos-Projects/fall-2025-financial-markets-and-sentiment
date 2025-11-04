
# _Financial Markets and Sentiment_

## **Team:** _Morgan Bryant, Nihan Akis Man, Francis Westhead, Anthony Lamini_

## **Introduction:**

During the year of 2025, the stock market exhibited unusual volatility, seemingly in response to social stimuli. A particular example of interest is Tesla: from January 2025 to May 2025, Tesla and its CEO, Elon Musk, were constant subjects on American social media, and Tesla’s stock was notably volatile during this period. Throughout 2025, various other companies received significant social media attention over different periods, accompanied by unusual stock volatility. The purpose of this project is to investigate whether the amount of social media attention a company receives influences the volatility of its stock. We analyze the stock data of 50 Fortune 500 companies and create various linear and non-linear models using spikes in social media attention as potential predictors.

## **Stakeholders:**
Because any reliable predictor of stock behavior is financially and legally significant, the stakeholders include FCC/SEC regulators, traders and investors, and companies themselves. A positive result would also have implications for social and economic sciences.

## **Dataset:**

### Stock data: 

Our stock data was collected from the historical Yahoo Finance stock return database from 
2023-01-01 to 2025-10-02. To give our models some degree of market context, for each day, we compute a rolling average stock volatility (defined in this project as variance of the returns) over various periods for each stock of study, as well as for the general S&P500 index. To measure daily variance, we use the percent change in stock returns from the previous day, which captures swings in stock returns. 

### Social Media Data:

Our data of interest is the number of social media posts per day referencing a specific company. We define a reference to a company as a mention of: its stock ticker, common company names & misspellings, CEO(s), and highly associated figures (i.e., Bill Gates and Microsoft). 

A significant challenge in this project was collecting this data. Many major platforms prohibit or restrict post collection, making the data impractical or impossible to obtain; the exception was Bluesky. Using Bluesky’s open source API, we collected a count of social media references to each of the 50 companies from 2024-08-01 to 2025-07-31. A significant caveat is that Bluesky is a significantly smaller sample space in comparison to, for example, that of X and Reddit. 

## **Methods:**

[Initial exploratory data analysis (EDA)](initial_data_analysis) revealed no noticeable patterns between stock volatility and social media references. We performed [a second EDA](initial_data_analysis/EDA_part_III.ipynb) where social media data was split into two groups: the group containing dates where a company had a statistically significant “spike” in mentions and the group of dates with no spikes. The two groups had very similar relationships with volatility in every period, again revealing no new relationships. 

With this analysis in mind, we opted to fit various models to attempt to find statistically significant relationships between volatility and spikes in social media mentions. The target of our models is each stock’s volatility. Along with the presence of a spike, we used historical average volatilities and previous day/week volatilities of each stock and the S&P500 index as predictors. We fit our models on individual stocks as well as the full dataset, using temporal train/test splits to account for the time nature of stocks. Models implemented included [multiple linear regression](linear_regression/linear_regression_part_I.ipynb),  [random forests](non_linear_regression/random forest for daily spikes.ipynb), and [GARCH-X](non_linear_regression/garchX_model_final.ipynb). We included [hypothesis testing](linear_regression/linear_regression_part_II.ipynb) and model comparison to gather evidence on the significance of spikes in each model. 

In addition, we implemented a [supervised classification framework](Classification) to assess whether social-media mention data enhances the prediction of large next-day stock movements, defined as days where the absolute next-day return exceeds 1.5× the 20-day rolling volatility. We trained and evaluated Logistic Regression, Random Forest, Extra Trees, and XGBoost models using SMOTE to address class imbalance, 5-fold stratified cross-validation, and an 80/20 time-based data split, with performance measured by F1-score and ROC-AUC metrics.

## **Results:**

In linear and non-linear regression, each model, in general, poorly predicted each stock’s daily volatility. Some linear regression models strongly predicted each stock’s weekly/biweekly volatility with high R^2 scores. However, for both the entire dataset and individual stocks, there was no statistically significant evidence that spikes had an influence in any model. The strongest predictors tended to be S&P500 historical volatilities and previous day volatilities.

In the classification results, the ROC comparison curves show that all models can statistically distinguish between large and normal market moves; however, the underlying predictive power is largely driven by market dynamics, particularly recent returns and volatility. Incorporating social-media mention features did not yield any meaningful performance gains, indicating that sentiment data alone lacks predictive strength, while volatility-based variables remain the primary drivers of short-term movement classification.

We conclude that there is no strong influence of daily stock volatility from the previous day’s social media output, neither in linear regression, non-linear regression, nor classification models. This conclusion is perhaps not surprising, as the stock market is notorious for immediately arbitraging out new predictors, but we find the analysis interesting, as it contradicts our initial perceived relationships. 

## **Future Directions**

- Our social media data comes exclusively from Bluesky. While our data is representative of all Bluesky posts in our time period, it may not capture the entire trend of social media mentions. Additionally, we were unable to scrape more than one year of data. Accessing larger databases and platforms may make this analysis stronger and relationships (or lack thereof) clearer. 

- [Some preliminary EDA](initial_data_analysis/EDA_Part_I.ipynb) revealed potential patterns between CEO tweets/posts and stock volatility. Namely, there appeared to be some relationship between Elon Musk’s daily tweet count and the Tesla stock’s behavior. Musk’s tweet count also positively correlated with the total number of social media mentions. This relationship may have more insights into social media vs stock volatility not captured in the scope of this project. 

- In terms of modelling stock volatility, most of our models were not tuned for financial forecasting. More rigorous and specialized predictors of stock movement may improve modelling capacity. The exception was our GARCH-X model, which is a more robust volatility model, but it poorly modelled stock data with our chosen predictors. 
 
