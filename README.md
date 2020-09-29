# Mobile Game Review Classification
Module 5 Final Project by Moses Lin

# Project Goal
The goal of this project is to develop a multi-class classification model that can correctly predict the rating of a review based off of the content of the review. In doing so, developers could possibly use such a model when proactively asking users for feedback in-game in order to gauge how their game is doing without taking a possible hit to their ratings should users leave negative feedback in the app store. After reviews are classified, they may be analyzed more specifically to address problem areas or to focus on desireable features.

# Dataset
Data was gathered from the Google Play store on 9/28/2020 at 12:01PM EST.

Beautifulsoup and Selenium was used to collect a list of App Id URLs for the Top Free and Top Paid games of each of the 17 mobile game genres (Action, Adventure, Arcade, Board, etc). The choice of Top Free and Top Paid games was because of the belief that top games were more likely to have reviews with substantial content in them.

A Google Play Scraper (https://github.com/JoMingyu/google-play-scraper) was then used to obtain up to 250 reviews for each game by looping through the App Id URL list from before. Reviews and characteristics of each game were collected.

Overall, there were 1,041,127 reviews collected from a total of 5122 mobile games.

There is some class imbalance in this dataset with rating 5 and other ratigns, obviously due to the games collected being Top Free and Top Paid games.

![imbalance](/images/classimbalance.png)

To deal with this imbalance, I downsampled to the size of the smallest class, rating 2, resulting in 74308 reviews per class for a total of 371,540 reviews.

# Repository Contents
- Scraping (scraping.ipynb): The process used to obtain the datasets in preparation for cleaning and EDA.
- Exploratory Data Analysis (EDA.ipynb): Analysis of the reviews using NLP methods, and visualization to demonstrate insights.
- Modeling (Modeling.ipynb): Various models were examined and the best model was identified.

# EDA

# Models

# Findings

# Future Steps

