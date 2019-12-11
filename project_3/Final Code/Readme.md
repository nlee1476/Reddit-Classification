--------------------------------------------
## Natural Language Processing of Subreddits
--------------------------------------------
Table of Contents 
-Contents of Project File 
1. Research Question 
2. Methodology 
    a. Data Collection 
    b. Data Cleaning and Preprocessing 
    c. Project Issues 
3. EDA 
4. Modeling and Baseline Accuracy 
5. Data Insights 
6. Conclusions and Recommendations 


------------------
Contents of File
------------------
In order the files should be opened accordingly: 
1. Step 1: Data Extraction 
2. Step 2: EDA and Cleaning 
3. Step 3: Modeling and Base Scoring 
4. Step 4: Data Insights 


--------------------
I. Research question
--------------------
Task: Use natural language processing and classification models to predict subreddit posts based on post title and comments. 

My task: To classify between the R/Conservative and R/Liberal subreddits with my metrics of success being accuracy scores. 


--------------------
II. Methodology 
--------------------
### A) Data Collection 
I used the pushshift.io method in order to extract the titles from the conservative and liberal subreddits. The size parameter is 1000 and I did not include any time parameters for this. The extracted data from the pushshift webscrape was the title, subreddit name, author, subreddit it, and the id. 

Once the relevant data was extracted into a list, the data was then converted into a dataframe. 

I got code from Albert who referred me to a blog on how to use praw.Reddit, a webscraping tool used specifically for getting reddit data. This method allowed me to view and extract the top comments from each post. 

The data was then combined into a single dataframe where the strings of comments were appended to the title to create a new combined column. 

The conservative and liberal dataframes were then combined into one complete dataframe and exported into a csv. 


### B) Data Cleaning and Preprocessing 
Data Cleaning: 
#### Step 1: Creating Numerical Categorical Columns 
    - Create a new column where the strings Liberal and Conservative (in reference to the subreddit) were transformed into 0 and 1 respectively. 
    
#### Step 2: Regular Expressions 
 -Created a new column called "tokens" which used the Regular Expression (/w+) to remove punctuation.
 
#### Step 3: Feature Creation 
 - Based on a Flextime lesson, I was inspired to create a couple columns that mapped out the number of words in the string and the average word length within the string. 
 
#### Step 4: Tokenizing and Stemming 
 -The tokens were then stemmed and lemmatized in order to get their base root.
 
#### Step 5: Sentiment Analysis 
 -I conducted a sentiment analysis on the combined title and comments column
 -In order to get a more useful metric I created a variable called 'sent-score' for sentiment score. The way the Sentiment Analysis is conducted, the output returns four values. Neg, Neu, Pos, and compound. When these scores are calculated, the result is either neutral , signified as a 1 in the neutral column and a 0 in the compund column. Depending on whether the score is positive or negative, that number is then added or subracted to 1, giving the compound score. In order to compound more interpretable, the column senti-score was created, where all compound scores greater than 0.2 were given a 1 to denote a positve connotation, and all compound scores less than -0.2 were given a -1 to denote a negative connotation. Neutral scores were given a 0. 
 
#### Step 6: Exploring Word Frequency: 
 - I wanted to see which words appeared most frequently in the dataset. 
 - You will see a cell with a variable entitled more_stop_words. This is because when I ran the frequency distribution, non-essential words appeared and these were the ones I decided to get rid of and add to stop_words. 
 - I then found the frequency distribution for positive/negative and liberal/conservative posts. 
 
 --------------------
III. Modeling and Baseline Accuracy
--------------------
 
 - In my inital run I dcied to use the combined_title&_comment column because for some reason when I used the lems or stems my code would raise a weird error 
 -The target y was the subreddit. 
 
#### 1. TFIDF: 
 - I decided to exclusively use TFIDF for my model due to the nature of my data. The way NLP models work is that they find similar words that pertain to a specific classification, and then predicts classification based on those previous inputs. TFIDF does mostly the same as CountVectorizer, however it penalizes common words and relies more on rare words for its classification. When I ran my inital dataset of 500 of each, the top three coefficients for both conservative and liberals were: "Trump, Biden, and Impeach." In political subreddits, while the views of the authors vary greatly, the syntax of their vocabuklay does not. 
 
#### 2. Models: 
 - I tried 5 models: LogisticRegression, RandomForest, Extratrees, Bagging, SVC, GradientBoostinClassfier, and VotingClassifier 
 - The accuracy scores hovered around 69-70% for most models. 
 - The baseline score is 50% as the data was 
 
### Classification Metrics 
 1. Accuracy: 
     a. svc: .692
     b. Multinomial: .692
     c. Logistic Regression: .682
 2. Precision: .7
 3. Recall : .7 
 4. Sensitivity: .7 
 5. Specificity: .7 

 
 