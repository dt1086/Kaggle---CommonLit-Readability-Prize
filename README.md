# Kaggle---CommonLit-Readability-Prize
**Author**: David Tian

## Overview
The goal of this competition was to rate the complexity of literary passages for grades 3–12. Because the target variable is continuous, as the result of a Bradley-Terry analysis (a statistics idea related to paired comparisons), this was a regression problem, as opposed to a clasification problem. For a simple and intuitive explanation on the Bradley-Terry analysis, please click [here](https://jamesmccaffrey.wordpress.com/2020/01/31/an-intuitive-explanation-of-the-bradley-terry-model/).

## Data Overview
While there were concerns over the data provided, with one participant going as far to call the data "... completely worthless”, calling out the fact that a pairwise comparison is a poor choice for the data due to subjective differences and inconsistency (due to humans rating the passages), I ignored any data concerns for the sake of my learning. However, I want to note that there is a valid concern for the compeition host to consider: whichever model scored the best may have major biases/flaws due to the data it was trained on, resulting in unintended consequences.

The data provided consisted of a training set (2,834 rows) and a validation set (7 rows). 

## Data Exploration
Below is a distribution of the `target` variable, where the lower the target variable, the more difficult the passage was (more suitable for 12th graders).

![image](https://miro.medium.com/max/352/1*k9YKLcoMBX7EcI9Cte6XFQ.png)

I also created frequency distributions along with the average word length, split by passage complexities. I found that as the passages became less difficult (had a higher target score), that the average word length decreased. This made sense to me intuitively, since I believed that, in general, longer words were harder to read. This [paper](https://journals.sagepub.com/doi/pdf/10.1080/10862967609547176#:~:text=These%20studies%20verify%20that%20rate,1974c%3B%20Coleman%2C%201971) confirmed my belief:
<figure>
    <blockquote>
        <p>… as [reading] prose becomes less difficult the average word length becomes smaller, i.e., there is a high correlation between average word length and prose difficulty.</p>
    </blockquote>
</figure>

![image](https://miro.medium.com/max/700/1*9_hEjPSq3omeGALcfHMvbw.png)
![image](https://miro.medium.com/max/700/1*jnbEMneL6DBV2EdwjGLgBQ.png)
![image](https://miro.medium.com/max/700/1*NFEoJjMiKaKgPsdyhikLHA.png)
![image](https://miro.medium.com/max/700/1*phqW4D4jHRBCbBzXf1XC4Q.png)
![image](https://miro.medium.com/max/700/1*wJcO9lVFNnJqbMpZpfvxIw.png)


I created different columns where I performed various text-cleaning methods (tokenizing, removing punctuation/numbers, lemmatizing, stemming). Here is a summary of the columns I would go on to model and what they represent:
* `excerpt` - passage text
* `excerpt_cleaned` - `excerpt` with stop words, punctuation, white spaces and numbers removed
* `excerpt_pos_lemmatized` - `excerpt_cleaned` lemmatized based on part-of-speech tagging
* `excerpt_porter_stemmed ` - `excerpt_cleaned` tokenized and stemmed based on porter stemmer
* `excerpt_snowball_stemmed ` - `excerpt_cleaned` tokenized and stemmed based on snowball stemmer
* `excerpt_lancaster_stemmed ` - `excerpt_cleaned` tokenized and stemmed based on lancaster stemmer

## Modeling Process

### Narrowing Down Independent Variable
The first question I wanted to answer in my modeling process was to narrow down which field would optimize my model results. A linear model using stochastic gradient descent (SGD) was explored to evaluate accuracy scores between the different text fields. Across both CountVectorizer and TFIDFVectorizer, I saw that the `excerpt` field had the best results.

### Narrowing Down Machine Learning Algorithms
I explored a number of machine learning algorithms (LinearSVR, SGD, Decision Tree, Linear Regression, KNN, Random Forest, Passive Aggressive Regressor, XGBoost and ADABoost), along with a couple of vectorizers (CountVectorizer and TFIDF), and found that the Linear Support Vector Regressor using a TFIDF vectorizer performed the best, with an RMSE of **.835** on my testing set.

However, my best performing model involved using a Hugging Face transformer. I chose to experiment with the xlnet-base-cased model, and after determining the optimal learning rate, I was able to achieve an RMSE of **.599** on my testing set.

## Contact Information
For any additional questions, please contact me at **dt1086@stern.nyu.edu**

## Repository Structure
```
├── README.md                                         <- The top-level README for reviewers of this project
├── Hugging_Face_Exploration.ipynb                    <- Learning Rate Exploration in Google Colab
├── Jupyter Notebook - FINAL.ipynb                    <- Exploratory Data Analysis and Modeling in Jupyter Notebook 
└──  Data                                             <- Sourced externally
```
