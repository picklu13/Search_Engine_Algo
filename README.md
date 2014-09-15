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

Implemented the classic PageRank algorithm on the tweet corpus. Rather than scoring each tweet, however, I calculated the PageRank score of users. 

First I built a graph structure based on @mentions. 

The edges are binary and directed. If Bob mentions Alice once, in 10 tweets, or 10 times in one tweet, there is an edge from Bob to Alice, but there is not an edge from Alice to Bob.
The edges are unweighted. If Bob mentions Alice once, in 10 tweets, or 10 times in one tweet, there is only one edge from Bob to Alice.
If a user mentions herself, ignore it.
If a user is never mentioned and does not mention anyone, their pagerank should be zero. Do not include the user in the calculation.

Assumed all nodes start out with equal probability and the probability of the random surfer teleporting is 0.1 . 


----------------------------------
Part 3: Put Together

Finally, developed an integrated tweet ranking system by integrating the cosine similarity (per tweet) and PageRank score (per user).

-----------------------------------

Running the project

Execute Entry.py and supply the file to it as an argument.
./Entry.py mars_tweets_medium.json


This will prompt a menu containing 5 choices :

1. Vector Space Retrieval

2. Page Ranked Retrieval

3. Integrated

4. Exit

5. Tagged Page Rank
 
Project Structure:

● Entry.py:

    ○ The entry point for the system.
    
    ○ Instantiates Other functionalities and performs as the controller of the project
    
● Utilities.py:

  ○ Contains utility functions used throughout the project
  
  ○ Performs functions like
  
    ■ Reading Tweets
    
    ■ Constructing commonly used Data Structures
    
    ■ Counting Tf
    
    ■ Calculating idf
    

● VectorSpaceRetrieval.py

  ○ class VectorSpaceRetrieval:
  
    ■ builds Pre Processed Data by computing weighted tf-idf
    
    ■ constructs a term-document matrix
    
    ■ performs search based on the query in the term document matrix by
        calculating the cosine similarity of each document in the corpus
        

● PageRank.py

  ○ class PageRank:
  
    ■ creates a graph of user as adjacency list as needed by the prtoblem
      statement.
      
    ■ implements the Page Rank Algorithm on the adjacency List so as to rank users
      on the number of incoming links , taking care of the dangling nodes.
      
    ■ returns the result of the page rank to Entry.py
    

● Integrated.py:

  ○ class Integrated:
  
    ■ This creates a combination of Vector Space retrieval System and the Page
    Rank Algorithm.
    
    ■ The aim of this module is to give certain weightage to both the contents of a
    tweet and to the page ranked user.
    
    ■ When this module receives a query from the user, then the Vector Space
    Retrieval System is called to obtain the cosine scores of the documents
    matching the query .
    
    ■ Page Rank of Users calculated in the previous module is also fed as in input to
    this module.
    
    ■ The cosine scores of all the documents are scaled up to 1.0. ie all the cosine
    scores are multiplied by a factor [scalingFactor] where scalingFactor = 1.0 /
    maximum cosine score of any document in the result.
    
    ■ Thus all the docs get a score out of one differential to the maximum score in
    the result Set.
    
    ■ The page rank of all the Users are Also scaled as
    PageRankScaled[i]=1.0/OldPageRank[i] . This is carried out according to the
    Zipf’s Law. In class we have studied that the rank one result carries far more
    importance than the subsequent ranks of any retrieval system. Hence a
    hyperbolic graph will be obtained which will hold all page ranked users in the
    corpus.
    
    ■ The final score of any document is thereby calculated as
    FinalScoreOfDocument(i) = 0.5 * scaledCosineScoreOfDocument(i) +
    PageRankScaled(tweeterOfDocument(i))
    
    ■ I have assigned equal weights to the two retrieval system to get a balance
    between the retrieved results. Further Results have been provided below.

● TaggedPageRank.py 
  
  ○ class TaggedPageRank:
  
  ■ Tries to construct a page rank of users based on the a topic
  
  ■ I have assigned each user with a topic-sensitive label. The topics have been
  constructed by scanning the top 200 most frequent words in the scriptions of
  all the users in the corpus. Then I have grouped the words into 4 main groups
  [social,technology,profession,sports] based on the words appearing in the
  top frequency list. Each of the broad topics or categories have a list of words
  that closely describes the topic or are closely related with the topic. A static
  data structure is maintained holding the topic and the related words as
