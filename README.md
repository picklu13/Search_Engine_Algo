Search_Engine_Algo
==================

Part 1: Vector Space Retrieval

From the dataset http://infolab.tamu.edu/static/670/mars_tweets_medium.json
Built a vector space retrieval system only based on tweets' texts.
Tokenized each tweet's text using whitespaces and punctuations as delimiters and did NOT remove stop words.


Used tf-idf to build the vector space model. Each tweet text was considered as a document, and stored inverse document frequency for each term. Both term frequency and inverse document frequency is calculated to be log base 2 scaled.

Return results ordered by the cosine similarity between the tf-idf of the tweet text and the query.

Tweets with higher similarity to the query should be first.
Did not calculate cosine similarity on tweets that do not contain any of the terms in the query. 
If a query returns more than 50 results, only return the first 50.
If any of the tokens in the query do not occur in the tweet corpus, then the idf will be undefined. Your program should returns no results.

----------------------------------
Part 2: PageRank

We're going to implement the classic PageRank algorithm on our tweet corpus. Rather than scoring each tweet, however, you will find the PageRank score of users. First you should build a graph structure based on @mentions. You should verify with your twitter graph to make sure the following statements are true about your implementation:

The edges are binary and directed. If Bob mentions Alice once, in 10 tweets, or 10 times in one tweet, there is an edge from Bob to Alice, but there is not an edge from Alice to Bob.
The edges are unweighted. If Bob mentions Alice once, in 10 tweets, or 10 times in one tweet, there is only one edge from Bob to Alice.
If a user mentions herself, ignore it.
If a user is never mentioned and does not mention anyone, their pagerank should be zero. Do not include the user in the calculation.
Assume all nodes start out with equal probability and the probability of the random surfer teleporting is 0.1 . You need to decide when to stop running the PageRank calculation. As same as Part 1, your program returns the top 50 results (which are users, not tweets!) with highest PageRank scores.

You may need to install some python libraries: numpy and (optionally) scipy. NumPy is a tool for doing numerical computations in Python. It primarily deals with math operations on large arrays and matrices. NumPy is written in C and can do number crunching much faster than a Python program. There is a NumPy Tuturial. Scipy is built on top of NumPy and has support for sparse matrices.

Hints:

Correctly parsing @mentions in a tweet is error-prone. Use the entities field.
Try numpy.matrix.
PageRank creates a O(n^2) matrix in memory where almost all of the values are the same. With some clever thinking you can use a sparse matrix instead of a matrix to make the program faster.

----------------------------------
Part 3: Put Together

Finally, you should develop an integrated tweet ranking system by integrating the cosine similarity (per tweet) and PageRank score (per user). We want this part to be open-ended. There are no restrictions on what scheme you use to integrate them. Clearly describe your method and the reason you pick it in the readme file. You should provide a discussion of your three ranking systems (including some sample queries and sample results). What do you observe? Why do you prefer one to the other? Compare them and summarize into readme file.
