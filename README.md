# IMDB-Big-Data-Project

Objective
This project's goal is to work with big data in an AWS environment and further our Machine Learning and Predictive Modeling toolkit, against a large scale dataset.

Collaborators
Alex Qaddourah, Marissa Fink, Vishal Narayanan, Abbie Amiotte, Neha Thakar

Dataset
my_output_csv contains the entire dataset filtered for those with over 250 votes and no blank rows.

Data Structure
titleId: (string) - a tconst, an alphanumeric unique identifier of the title.
ordering: (integer) – a number to uniquely identify rows for a given titleId.
title: (string) – the localized title.
region: (string) - the region for this version of the title.
language: (string) - the language of the title.
types: (array) - Enumerated set of attributes for this alternative title. One or more of the following: "alternative","dvd","festival", "tv", "video", "working", "original", "imdbDisplay". Newvalues may be added in the future without warning.
attributes: (array) - Additional terms to describe this alternative title, not enumerated.
isOriginalTitle: (boolean) – 0: not original title; 1: original title.
tconst: (string) - alphanumeric unique identifier of the title.
titleType: (string) – the type/format of the title (e.g. movie, short, tvseries, tvepisode, video, etc).
primaryTitle: (string) – the more popular title / the title used by the filmmakers on promotional materials at the point of release.
originalTitle: (string) - original title, in the original language.
isAdult: (boolean) - 0: non-adult title; 1: adult title.
startYear: (YYYY) – represents the release year of a title. In the case of TV Series, it is the series start year.
endYear: (YYYY) – TV Series end year. for all other title types.
runtimeMinutes: primary runtime of the title, in minutes.)
genres: (string array) – includes up to three genres associated with the title.
averageRating: weighted average of all the individual user ratings.
numVotes: number of votes the title has received.
Analysis
Initial data cleaning was followed by extracting Genre statistics because every movie fell into one or more than one Genre. After extracting that, it was imperative to find a relation between the average rating and number of votes.

1

Machine Learning to predict successful movies for reccomendation
In order to predict a successful movie, we can implement machine learning algorithms that classify a movie as successful or not. After loading in the data, we can create a new binary (1/0) column that identifies a movie as a success based on the average rating being greater than 6.5. The predictor variables are genre, Title Type, Movie Run Time, Year, Adult film (Yes/no), and number of votes. These variables are transformed into a vector of features, and then fed into a model that predicts the outcome variable. The models provide us with probability scores for each observation, giving insight into the features that play an important part in determining a movie's success.

The models that are included in our analysis is Logistic Regression & Random Forest, as they are classifers that can yield better predictive power. By classifying a movie's success, we can use the metrics as a potential evaulator for the number & type of films that we would like to reccommend for a given user.

The classifier algorithms can be found here: https://github.com/MSBX5420/team-mount-massive/blob/master/Predicting%20a%20movie's%20success%20-%20Logistic%20%26%20Random%20Forest%20Classifier%20(Full%20Dataset).ipynb

Design Document
https://www.lucidchart.com/publicSegments/view/26803019-e87d-47f8-a2c7-ca28b518e84c

Recommendation System
The simple recommendation system is based off a weighted rating formula from IMDB, (v/(v+m) * R) + (m/(m+v) * C), where v = number of votes for the movie, m = minimum number of votes to be counted, R = average rating for the movie, and C = average rating over all movies. The purpose of this formula is to reduce potential biases based on number of votes. For example, a movie may have a very high average rating, but with only a few votes, it's difficult to differentiate how successful that movie is versus a potentially lower scoring film with ten times as many votes. After calculating the weighted average for each movie, the recommendation system will produce a the top 5 highest scoring films based on the weighted rating and three inputs from the user, genre of choice, type (i.e. movie, TV show, videogame, etc.), and age.

The original recommendation system was first built exclusively in Python using a for loop and a UDF. https://github.com/MSBX5420/team-mount-massive/blob/master/Python%20Recommendation%20System.ipynb For the weighted rating function, the m value was calculated as movies in the 10th percentile. While this system was effective and retrieved a Top 5 list without a user needing to alter the code (a user just has to call the recommend function with three inputs), the for loop is slow.

The second recommendation system was built in PySpark using a UDF and spark.sql to view the Top 5 recommendations. https://github.com/MSBX5420/team-mount-massive/blob/master/PySpark_Recommendation_System%20(3).ipynb In the weighed rating function, m was given an arbitrary 100 value (which effectively included every movie since the data had been cleaned to remove movies with less than 250 votes). This system requires more data input from the user in the form of a SQL query, but still produces the desired results.

Both systems could be easily built out further to include other requirements from a user, or combined with a UI for simpler use.

Deployment
For future use, we would want to serialize our models and use a Flask API to feed the model into. The API lets us generate a fully functional web app that the user can input Genre, Age of the Film, and any other predictors used in the model. The app will generate 5 of the top recommended movies for the user and display them! Furthermore, we could use Flask and also use Heroku to get a permanent URL for the API.

Final Conclusions
To conclude, our team used a variation of machine learning and descriptive statistics to gather several key ideas. First, our highest predictor was the number of votes given to a movie. This makes sense, the most popular movies of our time are ones that have been seen many times, thus given the highest number of votes. Second, our most popular genre was Drama films. For a more improved model we would like to look at if adding actors/actresses or directors as predictors would add depth to our model. This aspect of our model would help to make our model more robust. We can recommend movies simply based on an actor or actress, instead of just the genre or other surface level factors.
