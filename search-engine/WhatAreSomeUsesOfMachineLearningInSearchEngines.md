# What are some uses of machine learning in search engines?
https://medium.com/@nikhilbd/what-are-some-uses-of-machine-learning-in-search-engines-5770f534d46b

Here are some places machine learning is used in search engines:

## Search ranking
Often search engines have multiple phases of ranking that happen in series, such as initial retrieval, primary ranking, contextual ranking, personalized ranking etc. Machine learning is used for ranking at all these phases, often using [Learning to Rank][1] systems.

## Query understanding
Machine Learning is used for “understanding” the search query typed by the user. Some examples of problems that are solved by machine learning:

1. `Query classification`: Search engines run various different classifiers on the search query . E.g. Detecting navigational vs. informational vs. transactional queries. Or news queries vs. local intent queries vs. shopping queries etc.
1. `Spelling suggestion / correction`
1. `Synonyms / Query expansion`: Search engines use synonyms to expand the query keywords and expand the result set.
1. `Intent disambiguation`: E.g. when you search for [eagles], is it Eagles the band or Philadelphia Eagles or the bird (or all 3)?
1. Various other facets that you can interpret a query by

## Url / document understanding
This includes everything that is done to understand a url, i.e. a search result. Url understanding is often done when the search engine crawls/indexes the url. Some examples of this:

1. `Page classification`: Understanding what types of a page it is. E.g. blog vs. news site vs. forum etc.
1. `Spam detection`
1. `Junk / low-quality url detection`
1. `Sentiment analysis`
1. `Entity / relationship detection`: Detecting entities, such as people or places in the page content. Figuring out relationships between entities.
1. Various other facets you can interpret a url by

## Search features
Machine Learning is used for generating search features that are diplayed outside the organic results such as:

1. [Sitelinks][2]
1. Related searches
1. Knowledge graph data (for some search engines) etc.

## Crawling
Crawlers use machine learning to figure out the **optimal rate to crawl a particular url** based on it’s importance, how often it is updated etc.

## User classification
Figuring out what kind of an user you are. This is especially useful for personalized search.


[1]: https://www.quora.com/What-is-the-intuitive-explanation-of-Learning-to-Rank-and-algorithms-like-RankNet-LambdaRank-and-LambdaMART/answer/Nikhil-Dandekar
[2]: https://support.google.com/webmasters/answer/47334?hl=en
