<!-- TOC -->

- [Adding search to your application](#adding-search-to-your-application)
    - [IMPLEMENTING A SIMPLE SEARCH FEATURE](#implementing-a-simple-search-feature)
        - [Searching for a specific term](#searching-for-a-specific-term)
        - [Parsing a user-entered query expression: QueryParser](#parsing-a-user-entered-query-expression-queryparser)
    - [USING INDEXSEARCHER](#using-indexsearcher)
        - [Creating an IndexSearcher](#creating-an-indexsearcher)
        - [Performing searches](#performing-searches)
        - [Working with TopDocs](#working-with-topdocs)
        - [Paging through results](#paging-through-results)
        - [Near-real-time search](#near-real-time-search)
    - [UNDERSTANDING LUCENE SCORING](#understanding-lucene-scoring)
        - [How Lucene scores](#how-lucene-scores)
        - [Using explain() to understand hit scoring](#using-explain-to-understand-hit-scoring)
    - [LUCENE’S DIVERSE QUERIES](#lucenes-diverse-queries)
        - [Searching by term: TermQuery](#searching-by-term-termquery)
        - [Searching within a term range: TermRangeQuery](#searching-within-a-term-range-termrangequery)
        - [Searching within a numeric range: NumericRangeQuery](#searching-within-a-numeric-range-numericrangequery)
        - [Searching on a string: PrefixQuery](#searching-on-a-string-prefixquery)
        - [Combining queries: BooleanQuery](#combining-queries-booleanquery)
        - [Searching by phrase: PhraseQuery](#searching-by-phrase-phrasequery)
        - [Searching by wildcard: WildcardQuery](#searching-by-wildcard-wildcardquery)
        - [Searching for similar terms: FuzzyQuery](#searching-for-similar-terms-fuzzyquery)
        - [Matching all documents: MatchAllDocsQuery](#matching-all-documents-matchalldocsquery)
    - [PARSING QUERY EXPRESSIONS: QUERYPARSER](#parsing-query-expressions-queryparser)
        - [Query.toString](#querytostring)
        - [TermQuery](#termquery)
        - [Term range searches](#term-range-searches)
        - [Numeric and date range searches](#numeric-and-date-range-searches)
        - [Prefix and wildcard queries](#prefix-and-wildcard-queries)
        - [Boolean operators](#boolean-operators)
        - [Phrase queries](#phrase-queries)
        - [Fuzzy queries](#fuzzy-queries)
        - [MatchAllDocsQuery](#matchalldocsquery)
        - [Grouping](#grouping)
        - [Setting the boost for a subquery](#setting-the-boost-for-a-subquery)

<!-- /TOC -->

# Adding search to your application
Table 3.1. Lucene’s primary searching API

| Class | Purpose |
| --- | --- |
| IndexSearcher | Gateway to searching an index. All searches come through an IndexSearcher instance using any of the several overloaded search methods.|
| Query (and subclasses) | Concrete subclasses encapsulate logic for a particular query type. Instances of Query are passed to an IndexSearcher’s search method.|
| QueryParser | Processes a human-entered (and readable) expression into a concrete Query object.|
| TopDocs | Holds the top scoring documents, returned by IndexSearcher.search.|
| ScoreDoc | Provides access to each search result in TopDocs.|

## IMPLEMENTING A SIMPLE SEARCH FEATURE
### Searching for a specific term
Listing 3.1. Simple searching with TermQuery

![l3.1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/077fig01_alt.jpg)

### Parsing a user-entered query expression: QueryParser
Figure 3.1. QueryParser translates a textual expression from the end user into an arbitrarily complex query for searching.

![f3.1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/03fig01.jpg)

Listing 3.2. QueryParser, which makes it trivial to translate search text into a Query

![l3.2](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/078fig01_alt.jpg)

Table 3.2. Expression examples that QueryParser handles

| Query expression | Matches documents that... |
| --- | --- |
| java | Contain the term java in the default field|
| java junit | Contain the term java or junit, or both, in the default field|
| java OR junit | - |
| +java +junit | Contain both java and junit in the default field |
| java AND junit | - |
| title:ant | Contain the term ant in the title field|
| title:extreme–subject:sports | Contain extreme in the title field and don’t have sports in the subject field|
| title:extreme AND NOT subject:sports	| - |
| (agile OR extreme) AND methodology | Contain methodology and must also contain agile and/or extreme, all in the default field|
| title:"junit in action" | Contain the exact phrase “junit in action” in the title field|
| title:"junit action"~5 | Contain the terms junit and action within five positions of one another, in the title field |
| java*	| Contain terms that begin with java, like javaspaces, javaserver, java.net, and the exact tem java itself.|
| java~ | Contain terms that are close to the word java, such as lava|
| lastmodified: [1/1/09 TO 12/31/09] | Have lastmodified field values between the dates January 1, 2009 and December 31, 2009|

## USING INDEXSEARCHER
### Creating an IndexSearcher
Figure 3.2. The relationship between the common classes used for searching

![f2.3](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/03fig02.jpg)

### Performing searches
Table 3.3. Primary IndexSearcher search methods

| IndexSearcher.search method signature | When to use |
| --- | --- |
| TopDocs search(Query query, int n) | Straightforward searches. The int n parameter specifies how many top-scoring documents to return.|
| TopDocs search(Query query, Filter filter, int n) | Searches constrained to a subset of available documents, based on filter criteria.|
| TopFieldDocs search(Query query, Filter filter, int n, Sort sort) | Searches constrained to a subset of available documents based on filter criteria, and sorted by a custom Sort object|
| void search(Query query, Collector results) | Used when you have custom logic to implement for each document visited, or you’d like to collect a different subset of documents than the top N by the sort criteria.|
| void search(Query query, Filter filter, Collector results) | Same as previous, except documents are only accepted if they pass the filter criteria.|

### Working with TopDocs
Table 3.4. TopDocs methods for efficiently accessing search results03_Ch03.fm

| TopDocs method or attribute | Return value |
| --- | --- |
| totalHits | Number of documents that matched the search|
| scoreDocs | Array of ScoreDoc instances that contains the results|
| getMaxScore() | Returns best score of all matches, if scoring was done while searching (when sorting by field, you separately control whether scores are computed)|

### Paging through results
You can choose from a couple of implementation approaches:

- Gather multiple pages’ worth of results on the initial search and keep the resulting ScoreDocs and IndexSearcher instances available while the user is navigating the search results.
- Requery each time the user navigates to a new page.

### Near-real-time search
Listing 3.3. Near-real-time search

![l3.3](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/ch03ex03-0.jpg)
![l3.3-1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/ch03ex03-1.jpg)

## UNDERSTANDING LUCENE SCORING
### How Lucene scores
Figure 3.3. Lucene uses this formula to determine a document score based on a query.

![f3.3](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/03fig03_alt.jpg)

Table 3.5. Factors in the scoring formula

| Factor |  Description |
| --- | --- |
| tf(t in d) | Term frequency factor for the term (t) in the document (d)—how many times the term t occurs in the document.|
| idf(t) | Inverse document frequency of the term: a measure of how “unique” the term is. Very common terms have a low idf; very rare terms have a high idf.
| boost(t.field in d) | Field and document boost, as set during indexing (see section 2.5). You may use this to statically boost certain fields and certain documents over others.|
| lengthNorm(t.field in d) | Normalization value of a field, given the number of terms within the field. This value is computed during indexing and stored in the index norms. Shorter fields (fewer tokens) get a bigger boost from this factor.|
| coord(q, d) | Coordination factor, based on the number of query terms the document contains. The coordination factor gives an AND-like boost to documents that contain more of the search terms than other documents.|
| queryNorm(q) | Normalization value for a query, given the sum of the squared weights of each of the query terms.|

### Using explain() to understand hit scoring
Listing 3.4. The explain() method

![l3.4](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/088fig01_alt.jpg)

![l3.4-1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/089fig01.jpg)

## LUCENE’S DIVERSE QUERIES
### Searching by term: TermQuery
```java
public void testKeyword() throws Exception {
  Directory dir = TestUtil.getBookIndexDirectory();
  IndexSearcher searcher = new IndexSearcher(dir);

  Term t = new Term("isbn", "9781935182023");
  Query query = new TermQuery(t);
  TopDocs docs = searcher.search(query, 10);
  assertEquals("JUnit in Action, Second Edition",
               1, docs.totalHits);

  searcher.close();
  dir.close();
}
```

### Searching within a term range: TermRangeQuery
```java
public void testTermRangeQuery() throws Exception {
  Directory dir = TestUtil.getBookIndexDirectory();
  IndexSearcher searcher = new IndexSearcher(dir);
  TermRangeQuery query = new TermRangeQuery("title2", "d", "j",
                                            true, true);
  TopDocs matches = searcher.search(query, 100);
  assertEquals(3, matches.totalHits);
  searcher.close();
  dir.close();
}
```

### Searching within a numeric range: NumericRangeQuery
```java
public void testInclusive() throws Exception {
  Directory dir = TestUtil.getBookIndexDirectory();
  IndexSearcher searcher = new IndexSearcher(dir);
  // pub date of TTC was September 2006
  NumericRangeQuery query = NumericRangeQuery.newIntRange("pubmonth",
                                                          200605,
                                                          200609,
                                                          true,
                                                          true);

  TopDocs matches = searcher.search(query, 10);
  assertEquals(1, matches.totalHits);
  searcher.close();
  dir.close();
}

public void testExclusive() throws Exception {
  Directory dir = TestUtil.getBookIndexDirectory();
  IndexSearcher searcher = new IndexSearcher(dir);

  // pub date of TTC was September 2006
  NumericRangeQuery query = NumericRangeQuery.newIntRange("pubmonth",
                                                          200605,
                                                          200609,
                                                          false,
                                                          false);
  TopDocs matches = searcher.search(query, 10);
  assertEquals(0, matches.totalHits);
  searcher.close();
  dir.close();
}
```

### Searching on a string: PrefixQuery
Listing 3.5. PrefixQuery

![l3.5](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/093fig01_alt.jpg)

### Combining queries: BooleanQuery
Listing 3.6. Using BooleanQuery to combine required subqueries

![l3.6](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/094fig01_alt.jpg)

```java
public static boolean hitsIncludeTitle(IndexSearcher searcher,
                                       TopDocs hits, String title)
  throws IOException {
  for (ScoreDoc match : hits.scoreDocs) {
    Document doc = searcher.doc(match.doc);
    if (title.equals(doc.get("title"))) {
      return true;
    }
  }
  System.out.println("title '" + title + "' not found");
  return false;
}
```

Listing 3.7. Using BooleanQuery to combine optional subqueries.

![l3.7](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/095fig01_alt.jpg)

### Searching by phrase: PhraseQuery
Listing 3.8. PhraseQuery

![l3.8](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/096fig01_alt.jpg)

### Searching by wildcard: WildcardQuery
Listing 3.9. WildcardQuery

![l3.9](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/099fig01_alt.jpg)

### Searching for similar terms: FuzzyQuery
```java
public void testFuzzy() throws Exception {
  indexSingleFieldDocs(new Field[] { new Field("contents",
                                               "fuzzy",
                                               Field.Store.YES,
                                               Field.Index.ANALYZED),
                                     new Field("contents",
                                               "wuzzy",
                                               Field.Store.YES,
                                               Field.Index.ANALYZED)
                                   });

  IndexSearcher searcher = new IndexSearcher(directory);
  Query query = new FuzzyQuery(new Term("contents", "wuzza"));
  TopDocs matches = searcher.search(query, 10);
  assertEquals("both close enough", 2, matches.totalHits);

  assertTrue("wuzzy closer than fuzzy",
             matches.scoreDocs[0].score != matches.scoreDocs[1].score);

  Document doc = searcher.doc(matches.scoreDocs[0].doc);
  assertEquals("wuzza bear", "wuzzy", doc.get("contents"));
  searcher.close();
}
```

### Matching all documents: MatchAllDocsQuery
```java
Query query = new MatchAllDocsQuery(field);
```

## PARSING QUERY EXPRESSIONS: QUERYPARSER
### Query.toString
```java
public void testToString() throws Exception {
  BooleanQuery query = new BooleanQuery();
  query.add(new FuzzyQuery(new Term("field", "kountry")),
            BooleanClause.Occur.MUST);
  query.add(new TermQuery(new Term("title", "western")),
            BooleanClause.Occur.SHOULD);
  assertEquals("both kinds", "+kountry~0.5 title:western",
               query.toString("field"));
}
```

### TermQuery
```java
public void testTermQuery() throws Exception {
  QueryParser parser = new QueryParser(Version.LUCENE_30,
                                       "subject", analyzer);
  Query query = parser.parse("computers");
  System.out.println("term: " + query);
}
```

produces this output:

```
term: subject:computers
```

### Term range searches
Listing 3.10. Creating a TermRangeQuery using QueryParser

![l310](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/103fig01_alt.jpg)

### Numeric and date range searches
QueryParser does include certain built-in logic for parsing dates when they appear as part of a range query, but the logic doesn’t work when you’ve indexed your dates using NumericField.

### Prefix and wildcard queries
```java
public void testLowercasing() throws Exception {
  Query q = new QueryParser(Version.LUCENE_30,
          "field", analyzer).parse("PrefixQuery*");
  assertEquals("lowercased",
      "prefixquery*", q.toString("field"));

  QueryParser qp = new QueryParser(Version.LUCENE_30,
                                   "field", analyzer);
  qp.setLowercaseExpandedTerms(false);
  q = qp.parse("PrefixQuery*");
  assertEquals("not lowercased",
      "PrefixQuery*", q.toString("field"));
}
```

### Boolean operators
```java
QueryParser parser = new QueryParser(Version.LUCENE_30,
                                     "contents", analyzer);
parser.setDefaultOperator(QueryParser.AND_OPERATOR);
```

Table 3.6. Boolean query operator shortcuts

| Verbose syntax | Shortcut syntax |
| --- | --- |
| a AND b	| +a +b |
| a OR b | a b |
| a AND NOT b | +a –b |

### Phrase queries
```java
public void testPhraseQuery() throws Exception {
  Query q = new QueryParser(Version.LUCENE_30,
                            "field",
                            new StandardAnalyzer(
                              Version.LUCENE_30))
                .parse("\"This is Some Phrase*\"");

  assertEquals("analyzed",
      "\"? ? some phrase\"", q.toString("field"));

  q = new QueryParser(Version.LUCENE_30,
                      "field", analyzer)
                .parse("\"term\"");
  assertTrue("reduced to TermQuery", q instanceof TermQuery);
}

public void testSlop() throws Exception {
  Query q = new QueryParser(Version.LUCENE_30,
       "field", analyzer)
          .parse("\"exact phrase\"");
  assertEquals("zero slop",
      "\"exact phrase\"", q.toString("field"));

  QueryParser qp = new QueryParser(Version.LUCENE_30,
                                   "field", analyzer);
  qp.setPhraseSlop(5);
  q = qp.parse("\"sloppy phrase\"");
  assertEquals("sloppy, implicitly",
      "\"sloppy phrase\"~5", q.toString("field"));
}
```

### Fuzzy queries
```java
public void testFuzzyQuery() throws Exception {
  QueryParser parser = new QueryParser(Version.LUCENE_30,
                                       "subject", analyzer);
  Query query = parser.parse("kountry~");
  System.out.println("fuzzy: " + query);

  query = parser.parse("kountry~0.7");
  System.out.println("fuzzy 2: " + query);
}
```

This produces the following output:

```
fuzzy: subject:kountry~0.5
fuzzy 2: subject:kountry~0.7
```

### MatchAllDocsQuery
QueryParser produces the MatchAllDocsQuery when you enter `*:*`.

### Grouping
```java
public void testGrouping() throws Exception {
  Query query = new QueryParser(
      Version.LUCENE_30,
      "subject",
      analyzer).parse("(agile OR extreme) AND methodology");
  TopDocs matches = searcher.search(query, 10);

  assertTrue(TestUtil.hitsIncludeTitle(searcher, matches,
                                       "Extreme Programming Explained"));
  assertTrue(TestUtil.hitsIncludeTitle(searcher,
                                       matches,
                                       "The Pragmatic Programmer"));
}
```

Figure 3.7. A Query can have an arbitrary nested structure, easily expressed with QueryParser’s grouping. This query is achieved by parsing the expression (+"brown fox" +quick) "red dog".

![f3.7](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/03fig07_alt.jpg)

### Setting the boost for a subquery
A caret (^) followed by a floating-point number sets the boost factor for the preceding query. For example, the query expression junit^2.0 testing sets the junit TermQuery to a boost of 2.0 and leaves the testing TermQuery at the default boost of 1.0. 
