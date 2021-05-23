# DSI16-Capstone

This project is the final part of the General Assembly, Data Science Immersive course that I completed in May 2021. The aim of this project is to demonstrate the skills and techniques needed to start a new career in Data Science.

<img src="https://user-images.githubusercontent.com/20221706/119270877-d253fa80-bbf6-11eb-9d97-859dac723c9a.png" width="200">

I decided that I wanted to focus my project around several different Natural Language Processing (NLP) techniques and to explore the potential value of social media posts from Reddit.com.
Using a subset of UK regional subreddits I was able to collect 500,000 user posted comments from the year 2020.
From this base data I was able to perform some key feature engineering and use NLP API’s (Google Cloud, IBM Watson) to enrich the dataset.
I ran into some difficulties when dealing with such a large number of data points across a diverse number of topics.

### Data collection

After deciding on Reddit as my focus I researched the Reddit API tools and found that even though they are very comprehensive they were not exactly precise enough when dealing with historical data.
The two objects I was pulling from Reddit was Submissions ('the original image / link / text that a user posts to the subreddit) and Comments (the user replies that branch out from each Submission)
Each Submission and each of the child Comments have a unique ID.

![pasted image 0 (1)](https://user-images.githubusercontent.com/20221706/119270912-fd3e4e80-bbf6-11eb-82c6-4ffec85fcf33.png)

Using the Reddit API and its PRAW (Python Reddit API Wrapper) library I was able to pull a submission and its child comments with a single call. However I found the native Reddit API was focused around managing Reddit as a live environment and was lacking any tools that allow you to target historical dates older than a few weeks.

I found an internet archiving project called pushshift.io that was storing Reddit posts on a third party site, using their API tools I was able to query the unique submission ID on a subreddit between two dates.

With this information I was able to write a script with two loops, the first loop gets all  the submissions in our target Subreddit between a start date and end date from pushshift.io . Once I have my list of id's I can then loop through them and pull the submission data plus all child comments via the PRAW library and output the data to a local CSV file.

![wordcloud](https://user-images.githubusercontent.com/20221706/119270964-3971af00-bbf7-11eb-9942-6b8aff5c4120.png)


### Data wrangling

The most difficult part of this project ways of mapping the raw data and finding if any value that could be predicted.

I wanted to start my focus on NLP and developed a test that would compare different processes and see if any had a more predictive quality.
In Reddit every comment has a scores depending on how many users up vote it. However when I look at all my comments I scrapped, it was not evenly distributed.

![Score_hist](https://user-images.githubusercontent.com/20221706/119270941-1d6e0d80-bbf7-11eb-9e1d-0a667ffefe9a.png)

The vast majority of comments only had a score around 0 or 1 and a very small percentage had over 100. I knew that the chances of finding a strong correlation was low but I proceeded to take a subset of my data and collected 24,000 pairs of Parent and Child comments. 

With this subset I used four different NLP sentiment score techniques to evaluate each of the text strings. Word Sentiment analysis is a process of assigning a score to select words in a string depending on predetermined criteria, each of the different scoring techniques I am using would have been trained by different teams on different data, I am curious to see if any of the techniques has any kind of correlation with my data. 

1. Prebuilt Word Sentiment library. Basic library of words with objectivity and positivity/negativity score
2. VADER Sentiment Analysis. VADER (Valence Aware Dictionary and sEntiment Reasoner) is a lexicon and rule-based sentiment analysis tool that is specifically attuned to sentiments expressed in social media, and works well on texts from other domains.
3. Google Cloud Natural Language AI, Sentiment analysis, Understand the overall opinion, feeling, or attitude sentiment expressed in a block of text.
4. IBM Watson Natural Language Understanding API, Analyses the general sentiment of your content or the sentiment toward specific target phrases. 

As suspected there was no strong correlation between the sentiment scores and Reddit scores for any of the different NLP techniques.

![NLP Heatmap](https://user-images.githubusercontent.com/20221706/119270986-57d7aa80-bbf7-11eb-8091-ac4d5576d8ff.png)

When we ran some predictive models we did not get any score that would be benefit in a production setting.
Some of the models we used include LinearRegression, DecisionTreeRegressor, RandomForestRegressor and MLPRegressor (neural network). However we never got above a train/test score above 0.3/0.2 which is not actonable.

![coef](https://user-images.githubusercontent.com/20221706/119271005-6c1ba780-bbf7-11eb-8f20-9fad008d4d88.png)

### Network Analysis

Moving away from NLP we also wanted to map the relationship between users on the Subreddits we scrapped. I wanted to map out the structure representing the connections between the users and the direction of who is responding to whom. I used the NetworkX python library to build up a graph representing the users relationships and plot it to a set of visual images.

![network_01](https://user-images.githubusercontent.com/20221706/119271013-7c338700-bbf7-11eb-98c7-1cc1c8638a1d.png)
![network_02](https://user-images.githubusercontent.com/20221706/119271016-805fa480-bbf7-11eb-86f3-e4d968b0999f.png)

### Conclusion

NLP techniques can be a very useful tools for adding more features to your data set however to get the most from it you need a clear definition of your goals and a high domain knowledge of your sources. The NLP techniques I learned can be a good starting point for analysing a collection of text data but there needs to be some structure already set in the data collection or stringent rules during the data wrangling stages to get the most out of it.

