## Evaluating Text with Natural Language Processing

_By: Jordan Gates_

### This project is intended to be a tool for people who manage multiple communities with similar topics on Reddit. <br> 
### Problem Statement:

As a manager of multiple similar subreddits, how can you identify users who would be interested in one of the other subreddits that you manage? The ability to identify posts that would be better suited for another subreddit that you manage would help grow your communities and help users find more relevant info.

### Using this natural language processing tool

In order to run this program correctly, first open the notebook titled "01_subreddit_scraper.ipynb" and run all cells. When it has finished running, you should have a csv file titled "reddit_posts.csv" that contains 1000 posts from the "MTB" and "gravelcycling" subreddits. Next, open the notebook titled "02_nlp_evaluation.ipynb", and run all cells. The commented code sections provide a bit more detailed insight into what is really going on under the hood, and how everything is supposed to work.

For this project we will be using posts from the ‘MTB’ and ‘gravelcycling’ subreddits. These subreddits are intended for mountain biking and gravel cycling enthusiasts. Because these categories are very similar and have overlapping communities, it should be a good example of how we can leverage NLP to identify the differences between the two. After scraping the data from each subreddit there were 1000 posts from each subreddit, giving us a baseline accuracy of 50% (Eg. If we predict that every post is from the ‘gravelcycling’ subreddit, we would be right 50% of the time.) In order to outperform this baseline I started by using the TF-IDF Vectorizer to format the text from each post in a way that is meaningful to the **Logistic Regression** model that I started with. The result was an accuracy score of 89% on testing data - a noticeable improvement over the baseline accuracy of 50%.

In order to further improve the model, I experimented with adding additional 'stopwords' - words that are common in both subreddits but do not provide the model with any meaningful signal. By ignoring these words, the model is able to see the differences between the subreddits more clearly. By using these additional stopwords, the first model’s accuracy improved from 89% to 90.4%. These stopwords were selected by creating lists of words that occurred the most frequently overall as well as the words that occurred the most in one subreddit vs the other, and picking out the words that clearly have no relevance to their respective subreddit.

There were 4 different models tested in the "nlp_evaluation" notebook: Logistic Regression with TF-IDF, Logistic Regression with CVEC, Random Forest with TF-IDF, and Random Forest with CVEC. With an accuracy score of 90.4%, the Logistic Regression with TF-IDF model had the highest accuracy, but it is interesting to see in the confusion matrix visualizations how the different models tended to predict one subreddit more frequently than the other.

#### How is this program useful?

The incorrect predictions are the posts that you would want to look at to see if the post was really about a topic that would be better suited for a different subreddit that you manage. For example, several of these posts on the mtb subreddit are likely to be about gravel bikes - this would be a good opportunity to recommend to that used that they check out your gravelcycling forum. Instead of having to read through 1k posts to see which ones are better suited for a different subreddit, you can just look at the much shorter list of incorrect predictions to determine what the actual topic of the post was, and recommend that the user check out your more relevant subreddit.

#### Potential Improvements

Moving forward, some potential improvements to this program could be:
* Using not only the post content, but the title text as well
* Determining which users have already posted on both subreddits in order to exclude them prior to evaluation
* Implementing Optical Character Recognition to analyze any photos that have text in them
* Creating a csv file that contains the incorrectly predicted posts, making it easy to look through them and decide which ones would be better suited for another subreddit. 

#### Data Reference

|  *subreddit*| *id:* | *title* | *selftext* | *bin_subreddit* |
| :----------- | :----------- | :----------- | :----------- | :----------- |
| Subreddit title | Used ID | Post Title | Text contained in post | Binarized version of the 'subreddit' column. 0 for 'gravelcycling' and 1 for 'MTB' |