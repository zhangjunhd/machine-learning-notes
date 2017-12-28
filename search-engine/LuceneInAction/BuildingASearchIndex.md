<!-- TOC -->

- [Building a search index](#building-a-search-index)
    - [HOW LUCENE MODELS CONTENT](#how-lucene-models-content)
        - [Documents and fields](#documents-and-fields)
        - [Flexible schema](#flexible-schema)
        - [Denormalization](#denormalization)
    - [UNDERSTANDING THE INDEXING PROCESS](#understanding-the-indexing-process)
    - [BASIC INDEX OPERATIONS](#basic-index-operations)
        - [Adding documents to an index](#adding-documents-to-an-index)
        - [Deleting documents from an index](#deleting-documents-from-an-index)
        - [Updating documents in the index](#updating-documents-in-the-index)
    - [FIELD OPTIONS](#field-options)
        - [Field options for indexing](#field-options-for-indexing)
        - [Field options for storing fields](#field-options-for-storing-fields)
        - [Field options for term vectors](#field-options-for-term-vectors)
        - [Reader, TokenStream, and byte[] field values](#reader-tokenstream-and-byte-field-values)
        - [Field option combinations](#field-option-combinations)
        - [Field options for sorting](#field-options-for-sorting)
        - [Multivalued fields](#multivalued-fields)
    - [BOOSTING DOCUMENTS AND FIELDS](#boosting-documents-and-fields)
        - [Boosting documents](#boosting-documents)
        - [Boosting fields](#boosting-fields)
        - [Norms](#norms)
    - [INDEXING NUMBERS, DATES, AND TIMES](#indexing-numbers-dates-and-times)
        - [Indexing numbers](#indexing-numbers)
        - [Indexing dates and times](#indexing-dates-and-times)
    - [FIELD TRUNCATION](#field-truncation)
    - [NEAR-REAL-TIME SEARCH](#near-real-time-search)
    - [OPTIMIZING AN INDEX](#optimizing-an-index)
    - [OTHER DIRECTORY IMPLEMENTATIONS](#other-directory-implementations)
    - [CONCURRENCY, THREAD SAFETY, AND LOCKING ISSUES](#concurrency-thread-safety-and-locking-issues)
        - [Thread and multi-JVM safety](#thread-and-multi-jvm-safety)
        - [Accessing an index over a remote file system](#accessing-an-index-over-a-remote-file-system)
        - [Index locking](#index-locking)

<!-- /TOC -->

# Building a search index
## HOW LUCENE MODELS CONTENT
### Documents and fields
At a high level, there are three things Lucene can do with each field:

- The value may be indexed (or not).
- If it’s indexed, the field may also optionally store term vectors, which are collectively a miniature inverted index for that one field, allowing you to retrieve all of its tokens.
- Separately, the field’s value may be stored, meaning a verbatim copy of the unanalyzed value is written away in the index so that it can later be retrieved. This is useful for fields you’d like to present unchanged to the user, such as the document’s title or abstract.

### Flexible schema
Unlike a database, Lucene has no notion of a fixed global schema.

The second major difference between Lucene and databases is that Lucene requires you to flatten, or denormalize, your content when you index it.

### Denormalization
Lucene documents are flat. Such recursion and joins must be denormalized when creating your documents.

## UNDERSTANDING THE INDEXING PROCESS
Figure 2.1. Indexing with Lucene breaks down into three main operations: extracting text from source documents, analyzing it, and saving it to the index.

![2.1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/02fig01.jpg)

- Extracting text and creating the document
- Analysis
- Adding to the index

Figure 2.2. Segmented structure of a Lucene inverted index

![2.2](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/02fig02.jpg)

## BASIC INDEX OPERATIONS
### Adding documents to an index
There are two methods for adding documents:

- addDocument(Document)—Adds the document using the default analyzer, which you specified when creating the IndexWriter, for tokenization.
- addDocument(Document, Analyzer)—Adds the document using the provided analyzer for tokenization. But be careful! In order for searches to work correctly, you need the analyzer used at search time to “match” the tokens produced by the analyzers at indexing time.

Listing 2.1. Adding documents to an index

![l2.1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/ch02ex01-0.jpg)
![l2.1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/ch02ex01-1.jpg)

In the getWriter method, we create the IndexWriter with three arguments:

- `Directory`, where the index is stored.
- The analyzer to use when indexing tokenized fields.
- MaxFieldLength.UNLIMITED, a required argument that tells IndexWriter to index all tokens in the document.

```java
public static int hitCount(IndexSearcher searcher, Query query)
 throws IOException {
  return searcher.search(query, 1).totalHits;
}
```

This method runs the search and returns the total number of hits that matched.

### Deleting documents from an index
IndexWriter provides various methods to remove documents from an index:

- deleteDocuments(Term) deletes all documents containing the provided term.
- deleteDocuments(Term[]) deletes all documents containing any of the terms in the provided array.
- deleteDocuments(Query) deletes all documents matching the provided query.
- deleteDocuments(Query[]) deletes all documents matching any of the queries in the provided array.
- deleteAll() deletes all documents in the index. This is exactly the same as closing the writer and opening a new writer with create=true, without having to close your writer.

Listing 2.2. Deleting documents from an index

![l2.2](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/041fig01_alt.jpg)

- maxDoc() returns the total number of deleted or undeleted documents in the index,
- numDocs() returns only the number of undeleted documents.

### Updating documents in the index
IndexWriter provides two convenience methods to replace a document in the index:

- updateDocument(Term, Document) first deletes all documents containing the provided term and then adds the new document using the writer’s default analyzer.
- updateDocument(Term, Document, Analyzer) does the same but uses the provided analyzer instead of the writer’s default analyzer.

Listing 2.3. Updating indexed Documents

![l2.3](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/043fig01_alt.jpg)

## FIELD OPTIONS
### Field options for indexing
The options for indexing (Field.Index.*) control how the text in the field will be made searchable via the inverted index. Here are the choices:

- `Index.ANALYZED`—Use the analyzer to break the field’s value into a stream of separate tokens and make each token searchable. This option is useful for normal text fields (body, title, abstract, etc.).
- `Index.NOT_ANALYZED`—Do index the field, but don’t analyze the String value. Instead, treat the Field’s entire value as a single token and make that token searchable. This option is useful for fields that you’d like to search on but that shouldn’t be broken up, such as URLs, file system paths, dates, personal names, Social Security numbers, and telephone numbers. This option is especially useful for enabling “exact match” searching.
- `Index.ANALYZED_NO_NORMS`—A variant of `Index.ANALYZED` that doesn’t store norms information in the index. Norms record index-time boost information in the index but can be memory consuming when you’re searching.
- `Index.NOT_ANALYZED_NO_NORMS`—Just like Index.NOT_ANALYZED, but also doesn’t store norms. This option is frequently used to save index space and memory usage during searching, because single-token fields don’t need the norms information unless they’re boosted.
- `Index.NO`—Don’t make this field’s value available for searching.

### Field options for storing fields
The options for stored fields (Field.Store.*) determine whether the field’s exact value should be stored away so that you can later retrieve it during searching:

- Store.YES —Stores the value. When the value is stored, the original String in its entirety is recorded in the index and may be retrieved by an IndexReader. This option is useful for fields that you’d like to use when displaying the search results (such as a URL, title, or database primary key). Try not to store very large fields, if index size is a concern, as stored fields consume space in the index.
- Store.NO —Doesn’t store the value. This option is often used along with Index.ANALYZED to index a large text field that doesn’t need to be retrieved in its original form, such as bodies of web pages, or any other type of text document.

### Field options for term vectors
Term vectors are a mix between an indexed field and a stored field. They’re similar to a stored field because you can quickly retrieve all term vector fields for a given document: term vectors are keyed first by document ID. But then, they’re keyed secondarily by term, meaning they store a miniature inverted index for that one document. Unlike a stored field, where the original String content is stored verbatim, term vectors store the actual separate terms that were produced by the analyzer, allowing you to retrieve all terms for each field, and the frequency of their occurrence within the document, sorted in lexicographic order.

You can choose separately whether these details are also stored in your term vectors by passing these constants as the fourth argument to the Field constructor:

- TermVector.YES—Records the unique terms that occurred, and their counts, in each document, but doesn’t store any positions or offsets information
- TermVector.WITH_POSITIONS—Records the unique terms and their counts, and also the positions of each occurrence of every term, but no offsets
- TermVector.WITH_OFFSETS—Records the unique terms and their counts, with the offsets (start and end character position) of each occurrence of every term, but no positions
- TermVector.WITH_POSITIONS_OFFSETS—Stores unique terms and their counts, along with positions and offsets
- TermVector.NO—Doesn’t store any term vector information

### Reader, TokenStream, and byte[] field values
There are a few other constructors for the Field object that allow you to use values other than String:

- Field(String name, Reader value, TermVector termVector) uses a Reader instead of a String to represent the value. In this case, the value can’t be stored (the option is hardwired to Store.NO) and is always analyzed and indexed (Index.ANALYZED). This can be useful when holding the full String in memory might be too costly or inconvenient—for example, for very large values.
- Field(String name, Reader value), like the previous value, uses a Reader instead of a String to represent the value but defaults termVector to TermVector.NO.
- Field(String name, TokenStream tokenStream, TermVector termVector) allows you to preanalyze the field value into a TokenStream. Likewise, such fields aren’t stored and are always analyzed and indexed.
- Field(String name, TokenStream tokenStream), like the previous value, allows you to preanalyze the field value into a TokenStream but defaults termVector to TermVector.NO.
- Field(String name, byte[] value, Store store) is used to store a binary field. Such fields are never indexed (Index.NO) and have no term vectors (TermVector.NO). The store argument must be Store.YES.
- Field(String name, byte[] value, int offset, int length, Store store), like the previous value, indexes a binary field but allows you to reference a sub-slice of the bytes starting at offset and running for length bytes.

### Field option combinations
Table 2.1. A summary of various field characteristics, showing you how fields are created, along with common usage examples

| Index | Store | TermVector | Example usage |
| --- | --- | --- | --- |
| NOT_ANALYZED_NO_NORMS	| YES	| NO | Identifiers (filenames, primary keys), telephone and Social Security numbers, URLs, personal names, dates, and textual fields for sorting|
| ANALYZED | YES | WITH_POSITIONS_OFFSETS | Document title, document abstract |
| ANALYZED | NO | WITH_POSITIONS_OFFSETS | Document body |
| NO | YES | NO | Document type, database primary key (if not used for searching) |
| NOT_ANALYZED | NO | NO | Hidden keywords |

### Field options for sorting
- If the field is numeric, use `NumericField`
- If the field is textual, such as the sender’s name in an email message, you must add it as a `Field` that’s indexed but not analyzed using `Field.Index.NOT_ANALYZED`
- If you aren’t doing any boosting for the field, you should index it without norms, to save disk space and memory, using `Field.Index.NOT_ANALYZED_NO_NORMS`:

```java
new Field("author", "Arthur C. Clark", Field.Store.YES,
          Field.Index.NOT_ANALYZED_NO_NORMS);
```

`Fields` used for sorting must be indexed and must contain one token per document. Typically this means using `Field.Index.NOT_ANALYZED` or `Field.Index.NOT_ANALYZED_NO_NORMS` (if you’re not boosting documents or fields), but if your analyzer will always produce only one token, such as `KeywordAnalyzer`, `Field.Index.ANALYZED` or `Field.Index.ANALYZED_NO_NORMS` will work as well.

### Multivalued fields
This is perfectly acceptable and encouraged, as it’s a natural way to represent a field that legitimately has multiple values.

## BOOSTING DOCUMENTS AND FIELDS
### Boosting documents
By changing a document’s boost factor, you can instruct Lucene to consider it more or less important with respect to other documents in the index when computing relevance.

Listing 2.4. Selectively boosting documents and fields

![l2.4](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/049fig01_alt.jpg)

### Boosting fields
Just as you can boost documents, you can also boost individual fields.

To achieve this behavior, we use the setBoost(float) method of the Field class:

```java
Field subjectField = new Field("subject", subject,
                               Field.Store.YES,
                               Field.Index.ANALYZED);
subjectField.setBoost(1.2F);
```

### Norms
During searching, norms for any field being searched are loaded into memory, decoded back into a floating-point number, and used when computing the relevance score.

## INDEXING NUMBERS, DATES, AND TIMES
### Indexing numbers
```java
doc.add(new NumericField("price").setDoubleValue(19.99));
```

### Indexing dates and times
```java
doc.add(new NumericField("timestamp").setLongValue(new Date().getTime()));

doc.add(new NumericField("day").setIntValue((int) (new Date().getTime()/24/3600)));

Calendar cal = Calendar.getInstance();
cal.setTime(date);
doc.add(new NumericField("dayOfMonth").setIntValue(cal.get(Calendar.DAY_OF_MONTH)));
```

## FIELD TRUNCATION
`IndexWriter` allows you to truncate per-Field indexing so that only the first N terms are indexed for an analyzed field. When you instantiate IndexWriter, you must pass in a `MaxFieldLength` instance expressing this limit. MaxFieldLength provides two convenient default instances: `MaxFieldLength.UNLIMITED`, which means no truncation will take place, and `MaxFieldLength.LIMITED`, which means fields are truncated at 10,000 terms. You can also instantiate MaxFieldLength with your own limit.

## NEAR-REAL-TIME SEARCH
```java
IndexReader getReader()
```

This method immediately flushes any buffered added or deleted documents, and then creates a new read-only IndexReader that includes those documents.

## OPTIMIZING AN INDEX
Optimizing only improves searching speed, not indexing speed.

IndexWriter exposes four methods to optimize:

- optimize() reduces the index to a single segment, not returning until the operation is finished.
- optimize(int maxNumSegments), also known as partial optimize, reduces the index to at most maxNumSegments segments. Because the final merge down to one segment is the most costly, optimizing to, say, five segments should be quite a bit faster than optimizing down to one segment, allowing you to trade less optimization time for slower search speed.
- optimize(boolean doWait) is just like optimize, except if doWait is false then the call returns immediately while the necessary merges take place in the background. Note that doWait=false only works for a merge scheduler that runs merges in background threads, such as the default ConcurrentMergeScheduler.
- optimize(int maxNumSegments, boolean doWait) is a partial optimize that runs in the background if doWait is false.

## OTHER DIRECTORY IMPLEMENTATIONS
Table 2.2. Lucene’s several core Directory implementations

| Directory | Description |
| --- | --- |
| SimpleFSDirectory	| A simplistic Directory that stores files in the file system, using java.io.* APIs. It doesn’t scale well with many threads.|
| NIOFSDirectory | A Directory that stores files in the file system, using java.nio.* APIs. This does scale well with threads on all platforms except Microsoft Windows, due to a longstanding issue with Sun’s Java Runtime Environment (JRE).|
| MMapDirectory | A Directory that uses memory-mapped I/O to access files. This is a good choice on 64-bit JREs, or on 32-bit JREs where the size of the index is relatively small.|
| RAMDirectory | A Directory that stores all files in RAM.|
| FileSwitchDirectory	| A Directory that takes two directories in, and switches between these directories based on file extension.|

## CONCURRENCY, THREAD SAFETY, AND LOCKING ISSUES
### Thread and multi-JVM safety
Figure 2.3. A single IndexWriter can be shared by multiple threads.

![2.3](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/02fig03.jpg)

### Accessing an index over a remote file system
Table 2.3. Issues related to accessing a Lucene index across remote file systems

| Remote file system | Notes |
| --- | --- |
| Samba/CIFS 1.0 | The standard remote file system for Windows computers. Sharing a Lucene index works fine.|
| Samba/CIFS 2.0 | The new version of Samba/CIFS that’s the default for Windows Server 2007 and Windows Vista. Lucene has trouble due to incoherent client-side caches.|
| Networked File System (NFS) | The standard remote file systems for most Unix OSs. Lucene has trouble due to both incoherent client-side caches as well as how NFS handles deletion of files that are held open by another computer.|
| Apple File Protocol (AFP) | Apple’s standard remote file system protocol. Lucene has trouble due to incoherent client-side caches.|

### Index locking
Table 2.4. Locking implementations provided by Lucene

| Locking class name | Description |
| -- | --- |
| NativeFSLockFactory |	This is the default locking for FSDirectory, using java.nio native OS locking, which will never leave leftover lock files when the JVM exits. But this locking implementation may not work correctly over certain shared file systems, notably NFS.|
| SimpleFSLockFactory	| Uses Java’s File.createNewFile API, which may be more portable across different file systems than NativeFSLockFactory. Be aware that if the JVM crashes or IndexWriter isn’t closed before the JVM exits, this may leave a leftover write.lock file, which you must manually remove.|
| SingleInstanceLockFactory	| Creates a lock entirely in memory. This is the default locking implementation for RAMDirectory. Use this when you know all IndexWriters will be instantiated in a single JVM.|
| NoLockFactory	| Disables locking entirely. Be careful! Only use this when you are absolutely certain that Lucene’s normal locking safeguard isn’t necessary—for example, when using a private RAMDirectory with a single IndexWriter instance.|

Listing 2.5. Using file-based locks to enforce a single writer at a time

![2.5](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/063fig01_alt.jpg)
