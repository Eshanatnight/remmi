# Movie Recomendations

Part of My Final Year CS Engineering Project.

## What is a recommender system?

Recommender systems are among the most popular applications of data science today. They are used to predict the "rating" or "preference" that a user would give to an item. Almost every major tech company has applied them in some form. Amazon uses it to suggest products to customers, YouTube uses it to decide which video to play next on autoplay, and Facebook uses it to recommend pages to like and people to follow.

There are also popular recommender systems for domains like restaurants, movies, and online dating. Recommender systems have also been developed to explore research articles and experts, collaborators, and financial services. YouTube uses the recommendation system at a large scale to suggest you videos based on your history. For example, if you watch a lot of educational videos, it would suggest those types of videos.

---

## Recommender systems can be classified into 3 types

* **Simple recommenders**: offer generalized recommendations to every user, based on movie popularity and/or genre. The basic idea behind this system is that movies that are more popular and critically acclaimed will have a higher probability of being liked by the average audience. An example could be IMDB Top 250.

* **Content-based recommenders**: suggest similar items based on a particular item. This system uses item metadata, such as genre, director, description, actors, etc. for movies, to make these recommendations. The general idea behind these recommender systems is that if a person likes a particular item, he or she will also like an item that is similar to it. And to recommend that, it will make use of the user's past item metadata. A good example could be YouTube, where based on your history, it suggests you new videos that you could potentially watch.

* **Collaborative filtering engines**: these systems are widely used, and they try to predict the rating or preference that a user would give an item-based on past ratings and preferences of other users. Collaborative filters do not require item metadata like its content-based counterparts.

---

## Check Out the Web APP GUI

click on the owl ➡️

<a title="link to deployeyd app" href="https://remmi.herokuapp.com/"><img src="https://i.redd.it/x2g5zvb396h41.jpg" height=100 style="float:right;padding: 0px 250px;"/>
</a></br></br></br>

---

## Dataset Overview

We are using two datasets:

### 1. TMDB 5000 Movie Dataset

This dataset consists of the following files:

* *tmdb_5000_credits.csv*: contains the credits for the movies.

    The first file contains the following features:-

    > **movie_id** - A unique identifier for each movie.
    > **cast** - The name of lead and supporting actors.
    > **crew** - The name of Director, Editor, Composer, Writer etc.

* *tmdb_5000_movies.csv*: contains the metadata for the movies.

    The second file has the following features:-

    > **budget** - The budget in which the movie was made.
    > **genre** - The genre of the movie, Action, Comedy ,Thriller etc.
    > **homepage** - A link to the homepage of the movie.
    > **id** - This is infact the movie_id as in the first dataset.
    > **keywords** - The keywords or tags related to the movie.
    > **original_language** - The language in which the movie was made.
    > **original_title** - The title of the movie before translation or adaptation.
    > **overview** - A brief description of the movie.
    > **popularity** - A numeric quantity specifying the movie popularity.
    > **production_companies** - The production house of the movie.
    > **production_countries** - The country in which it was produced.
    > **release_date** - The date on which it was released.
    > **revenue** - The worldwide revenue generated by the movie.
    > **runtime** - The running time of the movie in minutes.
    > **status** - "Released" or "Rumored".
    > **tagline** - Movie's tagline.
    > **title** - Title of the movie.
    > **vote_average** - average ratings the movie recieved.
    > **vote_count** - the count of votes recieved.

### 2. The Movies Dataset

Which contain the following files:-

> **movies_metadata.csv**: The main Movies Metadata file.
> Contains information on 45,000 movies featured in
> the Full MovieLens dataset. Features include posters,
> backdrops, budget, revenue, release dates,
> languages, production countries and companies.
> **keywords.csv**: Contains the movie plot keywords for our
>MovieLens movies. Available in the form of
>a stringified JSON Object.
> **credits.csv**: Consists of Cast and Crew Information
> for all our movies. Available in the form of
> a stringified JSON Object.
> **links.csv**: The file that contains the TMDB and IMDB IDs
> of all the movies featured in the Full MovieLens dataset.
> **links_small.csv**: Contains the TMDB and IMDB IDs of
> a small subset of 9,000 movies of the Full Dataset.
> **ratings_small.csv**: The subset of 100,000 ratings from 700 users on 9,000 movies.

#### The Full Datasets can be found over

For TMDB Dataset
[here](https://www.kaggle.com/datasets/tmdb/tmdb-movie-metadata)

For Movies Dataset
[here](https://grouplens.org/datasets/movielens/latest/).

---

## Simple Recommenders

Simple recommenders are basic systems that recommend the top items based on a certain metric or score. In this section, you will build a simplified clone of IMDB Top 250 Movies using metadata collected from IMDB.

    Note: The IMDB Dataset was removed due to a DMCA violation. We are now using the TMDb dataset.

### **Idea to Solve the Problem**

One of the most basic metrics one can think of is the ranking to decide which top 250 movies are based on their respective ratings.

However, using a rating as a metric has a few caveats:

* It does not take into consideration the popularity of a movie. Therefore, a movie with a rating of 9 from 10 voters will be considered 'better' than a movie with a rating of 8.9 from 10,000 voters.

* This metric will also tend to favor movies with a smaller number of voters with skewed and/or extremely high ratings. As the number of voters increases, the rating of a movie regularizes and approaches towards a value that is reflective of the movie's quality and gives the user a much better idea as to which movie they should choose. While it is difficult to discern the quality of a movie with extremely few voters, you might have to consider external sources to conclude.

Before getting started with this  -

* we need a metric to score or rate movie
* Calculate the score for every movie
* Sort the scores and recommend the best rated movie to the users.

We can use the average ratings of the movie as the score but using this won't be fair enough since a movie with 8.9 average rating and only 3 votes cannot be considered better than the movie with 7.8 as as average rating but 40 votes.
So, I'll be using IMDB's weighted rating (wr) which is given as :-

#### **Weighted Average Rating**

We can use the Weightied Rating (WR) as a metric to rank our movies. The WR is a combination of the following:

![](.\images\weighted_rating.png)

where,

* v is the number of votes for the movie
* m is the minimum votes required to be listed in the chart
* R is the average rating of the movie
* C is the mean vote across the whole report

We will use cutoff m as the 90th percentile.
In other words, for a movie to be featured in the charts, it must have more votes than at least 90% of the movies on the list.

---

## Content Based Filtering

In this recommender system the content of the movie (overview, cast, crew, keyword, tagline etc) is used to find its similarity with other movies. Then the movies that are most likely to be similar are recommended.

![](https://image.ibb.co/f6mDXU/conten.png)

## Plot description based Recommender

We will compute pairwise similarity scores for all movies based on their plot descriptions and recommend movies based on that similarity score. The plot description is given in the **overview** feature of our dataset.


With NLP Natural Language Processing, we have to convert the word vector of each overview
Then compute Term Frequency-Inverse Document Frequency (TF-IDF) vectors for each overview.

Term Frequency is the relative frequency of a word in a document and is given as
    **(term instances/total instances)**.

Inverse Document Frequency is the relative count of documents containing the term and is given as
    **log(number of documents/documents with term)**

The overall importance of each word to the documents in which they appear is equal to **TF * IDF**

This will give us a matrix where each column represents a word in the overview vocabulary (all the words that appear in at least one document) and each row represents a movie, as before.This is done to reduce the importance of words that occur frequently in plot overviews and therefore, their significance in computing the final similarity score.

We will just use scikit-learn module's built-in TfIdfVectorizer class that produces the TF-IDF matrix in a couple of lines.

We see that over 20,000 different words were used to describe the 4800 movies in our dataset.

With this matrix in hand, we can now compute a similarity score. There are several candidates for this; such as the euclidean, the Pearson and the [cosine similarity scores](https://en.wikipedia.org/wiki/Cosine_similarity). There is no right answer to which score is the best. Different scores work well in different scenarios and it is often a good idea to experiment with different metrics.

We will be using the cosine similarity to calculate a numeric quantity that denotes the similarity between two movies. We use the cosine similarity score since it is independent of magnitude and is relatively easy and fast to calculate. Mathematically, it is defined as follows:

Mathematically, it is defined as follows:

![](./images/cosine_similarity.jpg)

### Result

While our system has done a decent job of finding movies with similar plot descriptions, the quality of recommendations is not that great.

*"The Dark Knight Rises"* returns all Batman movies while it is more likely that the people who liked that movie are more inclined to enjoy other Christopher Nolan movies. This is something that cannot be captured by the present system.

---

## **Credits, Genres and Keywords Based Recommender**

It goes without saying that the quality of our recommender would be increased with the usage of better metadata. That is exactly what we are going to do in this section. We are going to build a recommender based on the following metadata: the 3 top actors, the director, related genres and the movie plot keywords.

From the cast, crew and keywords features, we need to extract the three most important actors, the director and the keywords associated with that movie. Right now, our data is present in the form of "stringified" lists , we need to convert it into a safe and usable structure

### Result

We see that our recommender has been successful in capturing more information due to more metadata and has given us (arguably) better recommendations. It is more likely that Marvels or DC comics fans will like the movies of the same production house. Therefore, to our features above we can add *production_company* .
We can also increase the weight of the director , by adding the feature multiple times in the soup.

---

## **Collaborative Filtering**

Our content based engine suffers from some severe limitations. It is only capable of suggesting movies which are close to a certain movie. That is, it is not capable of capturing tastes and providing recommendations across genres.

Also, the engine that we built is not really personal in that it doesn't capture the personal tastes and biases of a user. Anyone querying our engine for recommendations based on a movie will receive the same recommendations for that movie, regardless of who they are.

Therefore, in this section, we will use a technique called Collaborative Filtering to make recommendations to Movie Watchers.
It is basically of two types:-

* **User based filtering**-  These systems recommend products to a user that similar users have liked. For measuring the similarity between two users we can either use pearson correlation or cosine similarity.
This filtering technique can be illustrated with an example. In the following matrixes, each row represents a user, while the columns correspond to different movies except the last one which records the similarity between that user and the target user. Each cell represents the rating that the user gives to that movie. Assume user E is the target.
![](https://cdn-images-1.medium.com/max/1000/1*9NBFo4AUQABKfoUOpE3F8Q.png)

Since user A and F do not share any movie ratings in common with user E, their similarities with user E are not defined in Pearson Correlation. Therefore, we only need to consider user B, C, and D. Based on Pearson Correlation, we can compute the following similarity.
![](https://cdn-images-1.medium.com/max/1000/1*jZIMJzKM1hKTFftHfcSxRw.png)

From the above table we can see that user D is very different from user E as the Pearson Correlation between them is negative. He rated Me Before You higher than his rating average, while user E did the opposite. Now, we can start to fill in the blank for the movies that user E has not rated based on other users.
![](https://cdn-images-1.medium.com/max/1000/1*9TC6BrfxYttJwiATFAIFBg.png)

Although computing user-based CF is very simple, it suffers from several problems. One main issue is that users’ preference can change over time. It indicates that precomputing the matrix based on their neighboring users may lead to bad performance. To tackle this problem, we can apply item-based CF.

* **Item Based Collaborative Filtering** - Instead of measuring the similarity between users, the item-based CF recommends items based on their similarity with the items that the target user rated. Likewise, the similarity can be computed with Pearson Correlation or Cosine Similarity. The major difference is that, with item-based collaborative filtering, we fill in the blank vertically, as oppose to the horizontal manner that user-based CF does. The following table shows how to do so for the movie Me Before You.
![](https://cdn-images-1.medium.com/max/1000/1*LqFnWb-cm92HoMYBL840Ew.png)

It successfully avoids the problem posed by dynamic user preference as item-based CF is more static. However, several problems remain for this method. First, the main issue is **scalability**. The computation grows with both the customer and the product. The worst case complexity is O(mn) with m users and n items. In addition, **sparsity** is another concern. Take a look at the above table again. Although there is only one user that rated both Matrix and Titanic rated, the similarity between them is 1. In extreme cases, we can have millions of users and the similarity between two fairly different movies could be very high simply because they have similar rank for the only user who ranked them both.

### **Single Value Decomposition**

One way to handle the scalability and sparsity issue created by CF is to leverage a **latent factor model** to capture the similarity between users and items. Essentially, we want to turn the recommendation problem into an optimization problem. We can view it as how good we are in predicting the rating for items given a user. One common metric is Root Mean Square Error (RMSE). **The lower the RMSE, the better the performance**.

Now talking about latent factor you might be wondering what is it ?It is a broad idea which describes a property or concept that a user or an item have. For instance, for music, latent factor can refer to the genre that the music belongs to. SVD decreases the dimension of the utility matrix by extracting its latent factors. Essentially, we map each user and each item into a latent space with dimension r. Therefore, it helps us better understand the relationship between users and items as they become directly comparable. The below figure illustrates this idea.

![](https://cdn-images-1.medium.com/max/800/1*GUw90kG2ltTd2k_iv3Vo0Q.png)


### Result

![](./images/loss.png)

---

## Building the Environment

    conda env create -f environment.yml --name mr
