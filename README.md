# Identifying originating subreddit of a comment from a selection of two similar subreddits

### Problem Statement

The problem can be broken down as follows:
1. we have a comment;
2. we have selection of two closely relates subreddits;
2. we need to identify which of the two subreddits the comment belongs to.

The goal of the project is to build a model to automate this classification process.

### Data

API from Pushshift was used to collect approximate 15000 comments from each of the following subreddits:  
**1. /r/buildapc:** “is a community-driven subreddit dedicated to custom PC assembly. Anyone is welcome to seek the input of our helpful community as they piece together their desktop.” [14505 comments, full-text] [[1]](https://www.reddit.com/r/buildapc/)   
**2. /r/buildapcforme:** “dedicated to helping those looking to assemble their own PC without having to spend weeks researching and trying to find the right parts. From basic budget PCs to HTPCs to high end gaming rigs and workstations, if you come, we will build it.” [14751 comments, full-text] [[2]](https://www.reddit.com/r/buildapcforme/)

### Executive Summary

Suppose we are given a comment. We are also given two subreddits that are closely related. This project looks into the following questions:  
1. can we identify which of the two subreddits the comment comes from based just on the content of the comment? 
2. And provided we can identify the source, how to build a model to automate the process?

TF-IDF is short for term frequency–inverse document frequency. It is a numerical statistic that reflects how important a word is to a document in a collection of text. TF-IDF values of words in a comment can be considered as signal characteristic of the textual content. Analysis showed that there are approximately 130 words in each subreddit that have high TF-IDF as well as different distribution. Some of them may be the same and some maybe different for the two subreddits. Also since there are two subreddits, I chose 285 as number of features to account for all possible scenarios, including a margin for errors. Two classification algorithms I looked into are multinomial naïve bayes and ensemble trees with gradient boosting. Both did well. Their performances are shown in the tables below.

**Table 1: Multinomial Naive Bayes - Classification Scores**

|               | precision | recall | f1-score | support |
|---------------|-----------|--------|----------|---------|
| buildapc      | 0.97      | 0.94   | 0.95     | 3624    |
| buildapcforme | 0.94      | 0.97   | 0.95     | 3690    |  



**Table 2: Ensemble Trees with Gradient Boosting - Classification Scores**

|               | precision | recall | f1-score | support |
|---------------|-----------|--------|---------:|---------|
| buildapc      | 0.98      | 0.94   | 0.96     | 3626    |
| buildapcforme | 0.94      | 0.98   | 0.96     | 3688    |

My work shows that it is possible to predict the originating subreddit of a comment given enough data. A closer look at the subreddits make the biggest difference between the subreddits apparent. /r/buildapcforme is more structured and there are specifications on how to how to ask a question. Off-topic discussions are highly discouraged. Answers are also more concise and specific to the question. This difference in approach is the most likely reason behind the linguistic differences. The most impactful words in /r/buildapcforme are more specific in nature and provide evidence to this end.

**Table 3: Most Impactful Words by Subreddit**

| **/r/buildapc** | **/r/buildapcforme** |
|:---------------:|:--------------------:|
|        pc       |          gt          |
|   pcpartpicker  |         need         |
|      build      |       specific       |
|       just      |         tower        |
|       cpu       |         parts        |
|       case      |        budget        |
|       new       |       included       |
|      gaming     |          pc          |
|       know      |        monitor       |

### Presentation Slides

Presentation slides can be found at this [link](https://docs.google.com/presentation/d/1VWvXB5wIVROsgMrCPC8wrP6UWXK9AevRttuMvmZ4bzM/edit?usp=sharing).

### Summary

1. In this project I studied full text of close to 15 thousand comments from two subreddits: /r/buildapc and /r/buildapcforme. It's evident that given enough data, it is possible to predict the originating subreddit of a comment.
2. Each subreddit has approximately 130 words that are distinguishing and have high signal. From each subreddit, I refer to 10 such words with the highest TF-IDF as fingerprint words. This is because their presence identifies their originating subreddit with very high probability.
3. Individual comments can be transformed to a sequence of TF-IDF values. The idea underlying this work is that the distribution of signals in a comment can be used to identify source subreddit. 
4. Multinomial Naïve Bayes model as well as and ensemble tree model with Gradient Boosting both did well. 
5. While outside the scope of this report, other variations of ensemble tree model did well as well. 

### References

1. https://www.reddit.com/r/buildapc/
2. https://www.reddit.com/r/buildapcforme/
