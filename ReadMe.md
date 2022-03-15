# Web APIs & NLP
## <u>Problem Statement</u>
The marketing department for a liberal arts college needs to increase student enrollment for their Psychology and Anthropology departments. These study disciplines share many similarities, therefore they need distinctive keywords to differentiate their marketing campaigns to increase enrollment.  

As a data scientist for the college, I need to find the most predictive words/phrases that help classify psychology and anthropology using subreddit posts as a resource.

## <u>Data Dictionary</u>
<a href="https://github.com/pushshift/api">Pushshift API</a><br>

| Parameter |                      Description                     |       Default       |                      Accepted Values                      |   |
|:---------:|:----------------------------------------------------:|:-------------------:|:---------------------------------------------------------:|---|
| q         | Search term.                                         | N/A                 | String / Quoted String for phrases                        |   |
| ids       | Get specific comments via their ids                  | N/A                 | Comma-delimited base36 ids                                |   |
| size      | Number of results to return                          | 25                  | Integer <= 500                                            |   |
| fields    | One return specific fields (comma delimited)         | All Fields Returned | string or comma-delimited string                          |   |
| sort      | Sort results in a specific order                     | "desc"              | "asc", "desc"                                             |   |
| sort_type | Sort by a specific attribute                         | "created_utc"       | "score", "num_comments", "created_utc"                    |   |
| aggs      | Return aggregation summary                           | N/A                 | ["author", "link_id", "created_utc", "subreddit"]         |   |
| author    | Restrict to a specific author                        | N/A                 | String                                                    |   |
| subreddit | Restrict to a specific subreddit                     | N/A                 | String                                                    |   |
| after     | Return results after this date                       | N/A                 | Epoch value or Integer + "s,m,h,d" (i.e. 30d for 30 days) |   |
| before    | Return results before this date                      | N/A                 | Epoch value or Integer + "s,m,h,d" (i.e. 30d for 30 days) |   |
| frequency | Used with the aggs parameter when set to created_utc | N/A                 | "second", "minute", "hour", "day"                         |   |
| metadata  | display metadata about the query                     | false               | "true", "false"                                           |   |

## <u>EDA - Exploratory Data Analysis</u>
Data was collected from the two subreddit posts of <a href='https://www.reddit.com/r/AskAnthropology/'>r/AskAnthropology</a> and <a href ='https://www.reddit.com/r/psychology/'>r/psychology</a>. Initially 68_000 comments were pulled from r/AskAnthropology and 50_000 comments were pulled from r/psychology.  After (1)dropping the Unnamed columns and merging the two, the total number of rows dropped down to 50_000 total for a more managable preprocessing and modeling. (2) A function was built to CountVectorize the columns and counts the words to get a list of the top 25. (3) The data was then split into X and y to start the train_test_split, X represented the comments while y represented the two subreddit posts.

## <u>Model and Data Evaluation</u>
The Models used in this evaluation included: Logistic Regression, Multinomial Naive Bayes, and Random Forest Classifier. The baseline score was 50% because the data was split evenly at 25_000 per subreddit.  The best result was Logistic Regression with a training score of <b>92.7%</b> and a testing score of <b>86.9%</b>. The other model results are below:

| Model                 | Preprocessor      | Max Iternations | Train Score | Test Score |
|-----------------------|-------------------|-----------------|-------------|------------|
| Logistic RegressionCV | CountVectorizer   | 500             | 92.62%      | 86.46%     |
| Logistic Regression   | TfidfVectorizer() | default         | 92.72%      | 86.99%     |
| MultinomialNB         | CountVectorizer   | default         | 87.11%      | 84.96%     |
| MultinomialNB         | TfidfVectorizer() | default         | 89.80%      | 86.11%     |
| Random Forest         | RegexpTokenizer   | default         | rf - 80.7%  | Et - 82.5% |


## <u>Conclusion/Recommendations</u>
Based on the findings from the data, the reddit postings show a close similarity with psychology and anthropology.  The differences in keywords for psychology included: study, message,feel, psychology, research
Anthropology keywords include: different, human,culture,lot(?),say, modern. It is recommended to target ads for psychology using emotion words similar to ‘feel’, such as 'mood', 'sense' and 'impression'.  Secondly, the marketing department should use keywords that appeal to an audience looking to do more rigorous research and study. For Anthropology, the marketing team should highlight ‘different’ cultures to garner interest. Secondly, they should use language that shows the progression of humans from ancient periods in history to more modern periods.


