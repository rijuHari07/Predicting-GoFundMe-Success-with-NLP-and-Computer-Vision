# Predicting GoFundMe Success with NLP and Computer Vision

This project helps a nonprofit improve GoFundMe fundraising outcomes by analyzing what is shown in campaign images and what is written in campaign descriptions. I scrape about 1000 campaigns from one category, generate image labels using a computer vision service, build prediction models for high vs low fundraising, run topic modeling to identify common themes, and translate results into actionable recommendations.

## Objectives
- Scrape GoFundMe campaign data for a chosen category
- Extract image URLs, campaign description text, dollars raised, and campaign duration
- Generate image labels from campaign images using Google Vision (or Azure or an LLM)
- Predict high vs low fundraising using logistic regression
- Compare model accuracy across three feature sets
  - Image labels only
  - Description words only
  - Combined image labels plus description words
- Run topic modeling (LDA) on the best performing feature set
- Compare topic weights between highest and lowest fundraising quartiles
- Provide practical fundraising advice based on findings

## Data collected
For each campaign:
- Image URL(s)
- Description text
- Dollars raised
- Campaign duration (days running)

## Method overview
### Web scraping
- Scrape about 1000 campaigns in one category
- Save raw data as a structured dataset (CSV or parquet)

### Image labeling
- Download or stream images using URLs
- Extract image labels using:
  - Google Cloud Vision API, or
  - Azure Computer Vision, or
  - An LLM based vision model via API

### Binary target creation
- Create a column named `binary`
- `binary = 1` if dollars raised is above the median
- `binary = 0` otherwise

### Logistic regression modeling
Train and evaluate three models using logistic regression:
1. Independent variables: image labels (BoW or multi hot)
2. Independent variables: description words (BoW or TF IDF)
3. Independent variables: image labels plus description words (concatenated)

Include campaign duration as a numeric feature, for example:
- Add `duration_days` as a standalone numeric column
- Optionally log transform duration if it is highly skewed

Evaluation:
- Confusion matrix
- Accuracy = 1 - (number of prediction errors / total number of cases)

### Topic modeling
- Run LDA on the best performing feature set from Task D
- Choose an appropriate number of topics (start with 4 to 5, then adjust)
- Assign human readable names to topics based on top words

Quartile comparison:
- Sort by dollars raised (not the binary column)
- Compare highest quartile vs lowest quartile
- Compute average topic weights per quartile
- Present differences in a results table

### Recommendations
- Provide fundraising advice for the nonprofit based on model results and topic differences

