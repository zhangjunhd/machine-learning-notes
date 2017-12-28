<!-- TOC -->

- [Meet Lucene](#meet-lucene)
    - [WHAT IS LUCENE?](#what-is-lucene)
    - [LUCENE AND THE COMPONENTS OF A SEARCH APPLICATION](#lucene-and-the-components-of-a-search-application)
        - [Components for indexing](#components-for-indexing)
        - [Components for searching](#components-for-searching)
        - [The rest of the search application](#the-rest-of-the-search-application)
    - [LUCENE IN ACTION: A SAMPLE APPLICATION](#lucene-in-action-a-sample-application)
        - [Creating an index](#creating-an-index)
        - [Searching an index](#searching-an-index)
    - [UNDERSTANDING THE CORE INDEXING CLASSES](#understanding-the-core-indexing-classes)
        - [IndexWriter](#indexwriter)
        - [Directory](#directory)
        - [Analyzer](#analyzer)
        - [Document](#document)
        - [Field](#field)
    - [UNDERSTANDING THE CORE SEARCHING CLASSES](#understanding-the-core-searching-classes)
        - [IndexSearcher](#indexsearcher)
        - [Term](#term)
        - [Query](#query)
        - [TermQuery](#termquery)
        - [TopDocs](#topdocs)

<!-- /TOC -->

# Meet Lucene
##  WHAT IS LUCENE?
Lucene is a high-performance, scalable `information retrieval` (`IR`) library. `IR` refers to the process of searching for documents, information within documents, or metadata about documents.

## LUCENE AND THE COMPONENTS OF A SEARCH APPLICATION
Figure 1.4. Typical components of search application; the shaded components show which parts Lucene handles.

![1.4](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/01fig03a.jpg)

### Components for indexing
- `Acquire Content`:which involves using a crawler or spider, gathers and scopes the content that needs to be indexed.
- `Build Document`:Once you have the raw content that needs to be indexed, you must translate the content into the `units` (usually called `documents`) used by the search engine. The document typically consists of several separately named fields with values, such as `title`, `body`, `abstract`, `author`, and `url`.
- `Analyze Document`:No search engine indexes text directly: rather, the text must be broken into a series of individual atomic elements called `tokens`.
- `Index Document`:During the indexing step, the document is added to the index.

### Components for searching
- `Search User Interface`:The user interface is what users actually see, in the web browser, desktop application, or mobile device, when they interact with your search application.
- `Build Query`
  - You must then translate the request into the search engine’s `Query` object. We call this the `Build Query` step.
  - Lucene provides a powerful package, called `QueryParser`, to process the user’s text into a query object according to a common search syntax.
- `Search Query`
  - Search Query is the process of consulting the search index and retrieving the documents matching the Query, sorted in the requested sort order.
  - There are three common theoretical models of search:
    - `Pure Boolean model`— Documents either match or don’t match the provided query, and no scoring is done. In this model there are no relevance scores associated with matching documents, and the matching documents are unordered; a query simply identifies a subset of the overall corpus as matching the query.
    - `Vector space model`— Both queries and documents are modeled as vectors in a high dimensional space, where each unique term is a dimension. Relevance, or similarity, between a query and a document is computed by a vector distance measure between these vectors.
    - `Probabilistic model`— In this model, you compute the probability that a document is a good match to a query using a full probabilistic approach.
- `Render Results`:Once you have the raw set of documents that match the query, sorted in the right order, you then render them to the user in an intuitive, consumable manner.

### The rest of the search application
- `Administration Interface`:tune the size of the RAM buffer, how many segments to merge at once, how often to commit changes, or when to optimize and purge deletes from the index.
- `Analytics Interface`:Lucene-specific metrics that could feed the analytics interface include:
  - How often which kinds of queries (single term, phrase, Boolean queries, etc.) are run
  - Queries that hit low relevance
  - Queries where the user didn’t click on any results (if your application tracks click-throughs)
  - How often users are sorting by specified fields instead of relevance
  - The breakdown of Lucene’s search time
- `Scaling`:There are two dimensions to scaling: **net amount of content**, and **net query throughput**.

## LUCENE IN ACTION: A SAMPLE APPLICATION
### Creating an index
Listing 1.1 shows the Indexer command-line program, originally written for Erik’s introductory Lucene article on java.net. It takes two arguments:

- A path to a directory where we store the Lucene index
- A path to a directory that contains the files we want to index

Listing 1.1. Indexer, which indexes .txt files

![l1.1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/ch01ex01-0.jpg)

Go ahead and type ant Indexer, and you should see output like this:

```
% ant Indexer

Index *.txt files in a directory into a Lucene index.
Use the Searcher target to search this index.

Indexer is covered in the "Meet Lucene" chapter.

Press return to continue...

Directory for new Lucene index: [indexes/MeetLucene]

Directory with .txt files to index: [src/lia/meetlucene/data]

Overwrite indexes/MeetLucene? (y, n) y
Running lia.meetlucene.Indexer...
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/apache1.0.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/apache1.1.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/apache2.0.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/cpl1.0.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/epl1.0.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/freebsd.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/gpl1.0.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/gpl2.0.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/gpl3.0.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/lgpl2.1.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/lgpl3.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/lpgl2.0.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/mit.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/mozilla1.1.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/
 mozilla_eula_firefox3.txt
Indexing /Users/mike/lia2e/src/lia/meetlucene/data/
 mozilla_eula_thunderbird2.txt
Indexing 16 files took 757 milliseconds

BUILD SUCCESSFUL
```

### Searching an index
The Searcher program, originally written for Erik’s introductory Lucene article on java.net, complements Indexer and provides command-line searching capability. Listing 1.2 shows Searcher in its entirety. It takes two command-line arguments:

- The path to the index created with Indexer
- A query to use to search the index

Listing 1.2. Searcher, which searches a Lucene index

![l1.2](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/ch01ex02-0.jpg)

Let’s run Searcher and find documents in our index using the query ‘patent’

```
% ant Searcher

Search an index built using Indexer.
Searcher is described in the "Meet Lucene" chapter.

Press return to continue...

Directory of existing Lucene index built by
 Indexer:  [indexes/MeetLucene]

Query:  [patent]

Running lia.meetlucene.Searcher...
Found 8 document(s) (in 11 milliseconds) that
 matched query 'patent':
/Users/mike/lia2e/src/lia/meetlucene/data/cpl1.0.txt
/Users/mike/lia2e/src/lia/meetlucene/data/mozilla1.1.txt
/Users/mike/lia2e/src/lia/meetlucene/data/epl1.0.txt
/Users/mike/lia2e/src/lia/meetlucene/data/gpl3.0.txt
/Users/mike/lia2e/src/lia/meetlucene/data/apache2.0.txt
/Users/mike/lia2e/src/lia/meetlucene/data/lpgl2.0.txt
/Users/mike/lia2e/src/lia/meetlucene/data/gpl2.0.txt
/Users/mike/lia2e/src/lia/meetlucene/data/lgpl2.1.txt

BUILD SUCCESSFUL
Total time: 4 seconds
```

## UNDERSTANDING THE CORE INDEXING CLASSES
As you saw in our Indexer class, you need the following classes to perform the simplest indexing procedure:

- IndexWriter
- Directory
- Analyzer
- Document
- Field

Figure 1.5. Classes used when indexing documents with Lucene

![1.5](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/01fig05.jpg)

### IndexWriter
`IndexWriter` is the central component of the indexing process. This class creates a new index or opens an existing one, and adds, removes, or updates documents in the index. Think of IndexWriter as an object that gives you write access to the index but doesn’t let you read or search it. IndexWriter needs somewhere to store its index, and that’s what Directory is for.

### Directory
The `Directory` class represents the location of a Lucene index. It’s an abstract class that allows its subclasses to store the index as they see fit. In our Indexer example, we used FSDirectory.open to get a suitable concrete FSDirectory implementation that stores real files in a directory on the file system, and passed that in turn to IndexWriter’s constructor.

### Analyzer
Before text is indexed, it’s passed through an `analyzer`. The analyzer, specified in the IndexWriter constructor, is in charge of extracting those tokens out of text that should be indexed and eliminating the rest. Analyzer is an abstract class, but Lucene comes with several implementations of it.

- Some of them deal with skipping `stop words` (frequently used words that don’t help distinguish one document from the other, such as a, an, the, in, and on);
- some deal with conversion of tokens to lowercase letters, so that searches aren’t case sensitive;
- and so on.

### Document
The `Document` class represents a collection of fields. Think of it as a virtual document—a chunk of data, such as a web page, an email message, or a text file—that you want to make retrievable at a later time.

### Field
Each document in an index contains one or more named `fields`, embodied in a class called Field. Each field has a name and corresponding value, and a bunch of options, described in section 2.4, that control precisely how Lucene will index the field’s value.

## UNDERSTANDING THE CORE SEARCHING CLASSES
The basic search interface that Lucene provides is as straightforward as the one for indexing. Only a few classes are needed to perform the basic search operation:

- IndexSearcher
- Term
- Query
- TermQuery
- TopDocs

### IndexSearcher
`IndexSearcher` is to searching what IndexWriter is to indexing: the central link to the index that exposes several search methods. You can think of IndexSearcher as a class that opens an index in a read-only mode. It requires a Directory instance, holding the previously created index, and then offers a number of search methods, some of which are implemented in its abstract parent class Searcher; the simplest takes a Query object and an int topN count as parameters and returns a TopDocs object. A typical use of this method looks like this:

```java
Directory dir = FSDirectory.open(new File("/tmp/index"));
IndexSearcher searcher = new IndexSearcher(dir);
Query q = new TermQuery(new Term("contents", "lucene"));
TopDocs hits = searcher.search(q, 10);
searcher.close();
```

### Term
A `Term` is the basic unit for searching. Similar to the Field object, it consists of a pair of string elements: the name of the field and the word (text value) of that field.

During searching, you may construct Term objects and use them together with TermQuery:

```java
Query q = new TermQuery(new Term("contents", "lucene"));
TopDocs hits = searcher.search(q, 10);
```

### Query
Lucene comes with a number of concrete `Query` subclasses. So far in this chapter we’ve mentioned only the most basic Lucene Query: TermQuery. Other Query types are BooleanQuery, PhraseQuery, PrefixQuery, PhrasePrefixQuery, TermRangeQuery, NumericRangeQuery, FilteredQuery, and SpanQuery.

### TermQuery
`TermQuery` is the most basic type of query supported by Lucene, and it’s one of the primitive query types. It’s used for matching documents that contain fields with specific values, as you’ve seen in the last few paragraphs.

### TopDocs
The `TopDocs` class is a simple container of pointers to the top N ranked search results—documents that match a given query. For each of the top N results, TopDocs records the int docID (which you can use to retrieve the document) as well as the float score.
