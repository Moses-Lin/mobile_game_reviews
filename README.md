# Mobile Game Review Classification
Module 5 Final Project by Moses Lin

# Project Goal
The goal of this project is to develop a multi-class classification model that can correctly predict the rating of a review based off of the content of the review. In doing so, developers could possibly use such a model when proactively asking users for feedback in-game in order to gauge how their game is doing without taking a possible hit to their ratings, should users leave negative feedback in the app store. After reviews are classified, they may be analyzed more specifically to address problem areas or to focus on development or further development of desireable features.

# Dataset
Data was gathered from the Google Play store on 9/28/2020 at 12:01PM EST.

Beautifulsoup and Selenium was used to collect a list of App Id URLs for the Top Free and Top Paid games of each of the 17 mobile game genres (Action, Adventure, Arcade, Board, etc). The choice of Top Free and Top Paid games was because of the belief that top games were more likely to have reviews with substantial content in them.

A Google Play Scraper (https://github.com/JoMingyu/google-play-scraper) was then used to obtain up to 250 reviews for each game by looping through the App Id URL list from before. Reviews and characteristics of each game were collected.

Overall, there were 1,041,127 reviews collected from a total of 5122 mobile games.
- Note: Scraping gives different # of games and reviews each time, but will always be around ~1,000,000 reviews and ~5100 games.

There is some class imbalance in this dataset with rating 5 and other ratings, obviously due to the games collected being Top Free and Top Paid games.

![imbalance](/images/classimbalance.png)

To deal with this imbalance, I downsampled to the size of the smallest class, rating 2, resulting in 74308 reviews per class for a total of 371,540 reviews.
This downsampling also helps with running models later on that would normally take too long.

# Repository Contents
- Scraping (scraping.ipynb): The process used to obtain the datasets in preparation for cleaning and EDA.
- Exploratory Data Analysis (EDA.ipynb): Analysis of the reviews using NLP methods, and visualization to demonstrate insights.
- Modeling (Modeling.ipynb): Various models were examined and the best model was identified.

# EDA
Looking at the top 20 words across all mobile games did not provide much insight, as they're all undoubtedly linked to games and having fun. For single words, there is not much of a clear cut difference between rating 1 and rating 2. One thing of note though was the strong prevalence of the term ad, time, and money.

### Most common Words

![rating1](/images/top20mostcommonwordsrating1.png)

Rating 3 and 4 seems to simply be a mix of rating 1 & 2, and rating 5. Rating 5 offers only positive words, but nothing particularly insightful as to why a mobile game is doing well. Perhaps one insight is the word graphics.

![rating5](/images/top20mostcommonwordsrating5.png)

### Most common N-grams

In order to gain further information as to the context of some of these words, I also took a look at bigrams and trigrams. It is clear that the largest indicator for low ratings of 1 to 3 are concerned with the quantity of ads, and the time wasted watching said ads. Even as the rating goes up, it is very apparent the most common bigrams and trigrams are associated with ads.

![trigrams1](/images/top20mostcommontrigramsrating1.png)

There is one insight to be gained from reviews in ratings 3 - 4, and that is the prevalence of "please fix problem" or something similar. Perhaps this is an indication for the ratings in which developers should look for issues in, as these tend to lean more towards constructive criticism with the mix of positive and negative phrases than simply angry reviews that are common in 1 star ratings.

![trigrams3](/images/top20mostcommontrigramsrating3.png)

Rating 5 does not provide any real insights from bigrams/trigrams as they are simply a collection of positive phrases. There is also the issue of certain applications asking its users for 5 star ratings with the incentive of giving rewards. As a result, this category may not provide the most insightful feedback for developers. This is also a consideration I have to make when modeling, as there may be many reviews that are entirely nonsensical due to this phenomenon.

![trigrams5](/images/top20mostcommontrigramsrating5.png)

# Modeling
After dealing with class imbalance with downsampling, a baseline model was created using a dummy classifier. The accuracy and F1 score of the model was both 20%, which is expected given the now even distribution of 5 classes. The goal is to create a model that can provide significantly better results than random guessing.

### Performance Metric
Accuracy was selected as the main metric as True Positive and True Negatives are most important in determining a valid representation of a mobile game's performance. In addition, the dataset is balanced after downsampling, so accuracy will be favored in this scenario.
F1 score was also considered as the cost of a False Negative and False Positive is the same: The developer gets an incorrect rating that is not reflective of their mobile game's performance.

### Models
Three different models were fit to the dataset after TF-IDF Vectorization: Naive Bayes, Decision Tree, and Random Forest.
- Naive Bayes (Accuracy: 45.0% | F1 Score: 44.8%)
- Decision Tree (Accuracy: 35.4% | F1 Score: 35.1%)     
- Random Forest (Accuracy: 44.5% | F1 Score: 43.0%)

Note: Support Vector Machine was considered but crashes the kernel due to the dataset still being too large despite downsampling.

# Findings

### About the Model
The best model was Naive Bayes with an Accuracy of 45% and F1 score of 44.8%. This is better than the baseline model, but is still relatively inaccurate in determining reviews. The loss in accuracy is most likely due to downsampling, as before downsampling was applied, the baseline model provided an accuracy and F1 score of 33%, while Naive Bayes had an Accuracy of 62% and F1 Score of 51%. There is much room for improvement.

### About EDA
It is clear that by looking at common words and n-grams for reviews for all games in general, it is rather difficult to gain any real insights on specific problem areas (except for the sheer magnitude of ads) or favorable features. By aggregating all the reviews for every type of game, all the specific problems that may plague certain games (such as terrible lootbox rates notorious in Asian mobile games) are blanketed by very general reviews. 

### Limitations:
- I am running these models on a MacBook Air with 1.1 Ghz Dual-Core Intel Core i3. Perhaps a stronger computer could be used or a cloud service like Amazon Web Services could be used to run models that I normally cannot.
- There is a lot of specific mobile game jargon that may not be picked up or simply removed during the pre-processing step. 
- Rating is not necessarily a good indicator of app performance. As previously mentioned, many games bait users into giving good reviews by offering in-game rewards for doing so. In addition, games that are well received by an audience may not have a sustainable model and can "die" much earlier than expected. Revenue would be a much better indicator, but such information is not publicly available most of the time.

# Future Steps
- Work with a specific group of mobile games that fill a niche. As previously stated, it will be easier to find problem areas specific to these games, and insights will less likely be blanketed by common low-understanding reviews.

- Determine if there is a significant difference between review aspects in different App Stores (Some mobile games do not run very well on certain platforms and may encounter bugs more frequently than other platforms. For example some games are not properly ported onto certain platforms, or certain games have separate servers for each platform and thus have varying quality of service depending on the population of players on said platform.)

- Work with mobile games in a different language/region. (Different cultures view mobile gaming differently, as spending on lootboxes or "gachas" in mobile games is more accepted and maybe even expected in Asia)

