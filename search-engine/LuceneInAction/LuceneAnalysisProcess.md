<!-- TOC -->

- [Lucene’s analysis process](#lucenes-analysis-process)
    - [USING ANALYZERS](#using-analyzers)
        - [Indexing analysis](#indexing-analysis)
        - [QueryParser analysis](#queryparser-analysis)
        - [Parsing vs. analysis: when an analyzer isn’t appropriate](#parsing-vs-analysis-when-an-analyzer-isnt-appropriate)
    - [WHAT’S INSIDE AN ANALYZER?](#whats-inside-an-analyzer)
        - [What’s in a token?](#whats-in-a-token)
        - [TokenStream uncensored](#tokenstream-uncensored)
        - [Visualizing analyzers](#visualizing-analyzers)
    - [USING THE BUILT-IN ANALYZERS](#using-the-built-in-analyzers)
    - [SOUNDS-LIKE QUERYING](#sounds-like-querying)
    - [SYNONYMS, ALIASES, AND WORDS THAT MEAN THE SAME](#synonyms-aliases-and-words-that-mean-the-same)
        - [Creating SynonymAnalyzer](#creating-synonymanalyzer)
        - [Visualizing token positions](#visualizing-token-positions)
    - [STEMMING ANALYSIS](#stemming-analysis)
        - [StopFilter leaves holes](#stopfilter-leaves-holes)
        - [Combining stemming and stop-word removal](#combining-stemming-and-stop-word-removal)

<!-- /TOC -->

# Lucene’s analysis process
## USING ANALYZERS
```
Analyzing "The quick brown fox jumped over the lazy dog"
  WhitespaceAnalyzer:
    [The] [quick] [brown] [fox] [jumped] [over] [the] [lazy] [dog]

  SimpleAnalyzer:
    [the] [quick] [brown] [fox] [jumped] [over] [the] [lazy] [dog]

  StopAnalyzer:
    [quick] [brown] [fox] [jumped] [over] [lazy] [dog]

  StandardAnalyzer:
    [quick] [brown] [fox] [jumped] [over] [lazy] [dog]

Analyzing "XY&Z Corporation - xyz@example.com"
  WhitespaceAnalyzer:
    [XY&Z] [Corporation] [-] [xyz@example.com]

  SimpleAnalyzer:
    [xy] [z] [corporation] [xyz] [example] [com]

  StopAnalyzer:
    [xy] [z] [corporation] [xyz] [example] [com]

  StandardAnalyzer:
    [xy&z] [corporation] [xyz@example.com]
```

In the meantime, here’s a summary of each of these analyzers:

- `WhitespaceAnalyzer`, as the name implies, splits text into tokens on whitespace characters and makes no other effort to normalize the tokens. It doesn’t lowercase each token.
- `SimpleAnalyzer` first splits tokens at nonletter characters, then lowercases each token. Be careful! This analyzer quietly discards numeric characters but keeps all other characters.
- `StopAnalyzer` is the same as SimpleAnalyzer, except it removes common words. By default, it removes common words specific to the English language (the, a, etc.), though you can pass in your own set.
- `StandardAnalyzer` is Lucene’s most sophisticated core analyzer. It has quite a bit of logic to identify certain kinds of tokens, such as company names, email addresses, and hostnames. It also lowercases each token and removes stop words and punctuation.

### Indexing analysis
Figure 4.1. Analysis process during indexing. Fields 1 and 2 are analyzed, producing a sequence of tokens; Field 3 is unanalyzed, causing its entire value to be indexed as a single token.

![f4.1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/04fig01.jpg)

### QueryParser analysis
```java
QueryParser parser = new QueryParser(Version.LUCENE_30,
                                     "contents", analyzer);
Query query = parser.parse(expression);
```

### Parsing vs. analysis: when an analyzer isn’t appropriate
- Analyzers are used to analyze a specific field at a time and break things into tokens only within that field.
- Analyzers don’t help in field separation because their scope is to deal with a single field at a time. Instead, parsing these documents prior to analysis is required.

## WHAT’S INSIDE AN ANALYZER?
```java
public final class SimpleAnalyzer extends Analyzer {
  @Override
  public TokenStream tokenStream(String fieldName, Reader reader) {
    return new LowerCaseTokenizer(reader);
  }

  @Override
  public TokenStream reusableTokenStream(String fieldName, Reader reader
       throws IOException {
    Tokenizer tokenizer = (Tokenizer) getPreviousTokenStream();
    if (tokenizer == null) {
      tokenizer = new LowerCaseTokenizer(reader);
      setPreviousTokenStream(tokenizer);
    } else
      tokenizer.reset(reader);
    return tokenizer;
  }
}
```

### What’s in a token?
Figure 4.2. A token stream with positional and offset information

![f4.2](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/04fig02.jpg)

### TokenStream uncensored
Figure 4.3. The hierarchy of classes used to produce tokens: TokenStream is the abstract base class; Tokenizer creates tokens from a Reader; and TokenFilter filters any other TokenStream.

![f4.3](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/04fig03.jpg)

Figure 4.4. An analyzer chain starts with a Tokenizer, to produce initial tokens from the characters read from a Reader, then modifies the tokens with any number of chained TokenFilters.

![f4.4](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/04fig04.jpg)

Figure 4.5. TokenFilter and Tokenizer class hierarchy

![f4.5](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/04fig05_alt.jpg)

To illustrate the analyzer chain in code, here’s a simple example analyzer:

```java
public TokenStream tokenStream(String fieldName, Reader reader) {
    return new StopFilter(true,
                          new LowerCaseTokenizer(reader),
                          stopWords);
}
```

### Visualizing analyzers
Listing 4.1. AnalyzerDemo: seeing analysis in action

![l4.1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/120fig01_alt.jpg)

Listing 4.2. AnalyzerUtils: delving into an analyzer

![l4.2](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/121fig01_alt.jpg)

The AnalyzerDemo application lets you specify one or more strings from the command line to be analyzed instead of the embedded example ones:

```
%java lia.analysis.AnalyzerDemo "No Fluff, Just Stuff"

Analyzing "No Fluff, Just Stuff"
  org.apache.lucene.analysis.WhitespaceAnalyzer:
    [No] [Fluff,] [Just] [Stuff]

  org.apache.lucene.analysis.SimpleAnalyzer:
    [no] [fluff] [just] [stuff]

  org.apache.lucene.analysis.StopAnalyzer:
    [fluff] [just] [stuff]

  org.apache.lucene.analysis.standard.StandardAnalyzer:
    [fluff] [just] [stuff]
```

Listing 4.3. Seeing the term, offsets, type, and position increment of each token

![l4.3](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/122fig01_alt.jpg)

We display all token information on the example phrase using SimpleAnalyzer:

```java
public static void main(String[] args) throws IOException {
  AnalyzerUtils.displayTokensWithFullDetails(new SimpleAnalyzer(),
      "The quick brown fox....");
}
```

Here’s the output:

```
1: [the:0->3:word]
2: [quick:4->9:word]
3: [brown:10->15:word]
4: [fox:16->19:word]
```

Analyzing the phrase “I’ll email you at xyz@example.com” with `StandardAnalyzer` produces this interesting output:

```
1: [i'll:0->4:<APOSTROPHE>]
2: [email:5->10:<ALPHANUM>]
3: [you:11->14:<ALPHANUM>]
5: [xyz@example.com:18->33:<EMAIL>]
```

## USING THE BUILT-IN ANALYZERS
Table 4.3. Primary analyzers available in Lucene

| Analyzer | Steps taken |
| --- | --- |
| WhitespaceAnalyzer | Splits tokens at whitespace.|
| SimpleAnalyzer | Divides text at nonletter characters and lowercases.|
| StopAnalyzer | Divides text at nonletter characters, lowercases, and removes stop words.|
| KeywordAnalyzer | Treats entire text as a single token.|
| StandardAnalyzer | Tokenizes based on a sophisticated grammar that recognizes email addresses, acronyms, Chinese-Japanese-Korean characters, alphanumerics, and more. It also lowercases and removes stop words.|

## SOUNDS-LIKE QUERYING
We chose the `Metaphone` algorithm as an example, but other algorithms are available, such as `Soundex`.

Listing 4.4. Searching for words that sound like one another

![l4.4](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/129fig01_alt.jpg)

The trick lies in the MetaphoneReplacementAnalyzer:

```java
public class MetaphoneReplacementAnalyzer extends Analyzer {
  public TokenStream tokenStream(String fieldName, Reader reader) {
    return new MetaphoneReplacementFilter(
                   new LetterTokenizer(reader));
  }
}
```

Listing 4.5. TokenFilter that replaces tokens with their metaphone equivalents

![l4.5](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/130fig01_alt.jpg)

Using our AnalyzerUtils, two phrases that sound similar yet are spelled differently are tokenized and displayed:

```java
public static void main(String[] args) throws IOException {
  MetaphoneReplacementAnalyzer analyzer =
                               new MetaphoneReplacementAnalyzer();
  AnalyzerUtils.displayTokens(analyzer,
                 "The quick brown fox jumped over the lazy dog");

  System.out.println("");
  AnalyzerUtils.displayTokens(analyzer,
                 "Tha quik brown phox jumpd ovvar tha lazi dag");
}
```

We get a sample of the metaphone encoder, shown here:

```
[0] [KK] [BRN] [FKS] [JMPT] [OFR] [0] [LS] [TKS]
[0] [KK] [BRN] [FKS] [JMPT] [OFR] [0] [LS] [TKS]
```

## SYNONYMS, ALIASES, AND WORDS THAT MEAN THE SAME
Listing 4.6. Testing the synonym analyzer

![l4.6](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/131fig01_alt.jpg)

### Creating SynonymAnalyzer
Listing 4.7. SynonymAnalyzer implementation

```java
public class SynonymAnalyzer extends Analyzer {
  private SynonymEngine engine;

  public SynonymAnalyzer(SynonymEngine engine) {
    this.engine = engine;
  }

  public TokenStream tokenStream(String fieldName, Reader reader) {
    TokenStream result = new SynonymFilter(
                          new StopFilter(true,
                            new LowerCaseFilter(
                              new StandardFilter(
                                new StandardTokenizer(
                                 Version.LUCENE_30, reader))),
                            StopAnalyzer.ENGLISH_STOP_WORDS_SET),
                          engine
                         );
    return result;
  }
}
```

Listing 4.8. SynonymFilter: buffering tokens and emitting one at a time

![l4.8](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/ch04ex08-0.jpg)

The design of SynonymAnalyzer allows for pluggable SynonymEngine implementations. SynonymEngine is a one-method interface:

```java
public interface SynonymEngine {
  String[] getSynonyms(String s) throws IOException;
}

public class TestSynonymEngine implements SynonymEngine {
  private static HashMap<String, String[]> map =

    new HashMap<String, String[]>();

  static {
    map.put("quick", new String[] {"fast", "speedy"});
    map.put("jumps", new String[] {"leaps", "hops"});
    map.put("over", new String[] {"above"});
    map.put("lazy", new String[] {"apathetic", "sluggish"});
    map.put("dog", new String[] {"canine", "pooch"});
  }
  public String[] getSynonyms(String s) {
    return map.get(s);
  }
}
```

Listing 4.9. SynonymAnalyzerTest: showing that synonym queries work

![l4.9](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/ch04ex09-0.jpg)
![l4.9-1](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/ch04ex09-1.jpg)

Listing 4.10. Testing SynonymAnalyzer with QueryParser

![l4.10](https://www.safaribooksonline.com/library/view/lucene-in-action/9781933988177/136fig01_alt.jpg)

The test produces the following output:

```
With SynonymAnalyzer, "fox jumps" parses to "fox (jumps hops leaps)"
With StandardAnalyzer, "fox jumps" parses to "fox jumps"
```

### Visualizing token positions
Listing 4.11. Visualizing the position increment of each token

```java
public static void displayTokensWithPositions
  (Analyzer analyzer, String text) throws IOException {

  TokenStream stream = analyzer.tokenStream("contents",
                                            new StringReader(text));
  TermAttribute term = stream.addAttribute(TermAttribute.class);
  PositionIncrementAttribute posIncr =
       stream.addAttribute(PositionIncrementAttribute.class);

  int position = 0;
  while(stream.incrementToken()) {
    int increment = posIncr.getPositionIncrement();
    if (increment > 0) {
      position = position + increment;
      System.out.println();
      System.out.print(position + ": ");
    }

    System.out.print("[" + term.term() + "] ");
  }
  System.out.println();
}
```

We wrote a quick piece of code to see what our SynonymAnalyzer is doing:

```java
public class SynonymAnalyzerViewer {

  public static void main(String[] args) throws IOException {

  SynonymEngine engine = new TestSynonymEngine();

  AnalyzerUtils.displayTokensWithPositions(
    new SynonymAnalyzer(engine),
    "The quick brown fox jumps over the lazy dog");
  }
}
```

And we can now visualize the synonyms placed in the same positions as the original words:

```
2: [quick] [speedy] [fast]
3: [brown]
4: [fox]
5: [jumps] [hops] [leaps]
6: [over] [above]
8: [lazy] [sluggish] [apathetic]
9: [dog] [pooch] [canine]
```

## STEMMING ANALYSIS
### StopFilter leaves holes
This is illustrated from the output of AnalyzerUtils.displayTokensWithPositions:

```
2: [quick]
3: [brown]
4: [fox]
5: [jump]
6: [over]
8: [lazi]
9: [dog]
```

### Combining stemming and stop-word removal
Listing 4.12. PositionalPorterStopAnalyzer: stemming and stop word removal

```java
public class PositionalPorterStopAnalyzer extends Analyzer {
  private Set stopWords;

  public PositionalPorterStopAnalyzer() {
    this(StopAnalyzer.ENGLISH_STOP_WORDS_SET);
  }

  public PositionalPorterStopAnalyzer(Set stopWords) {
    this.stopWords = stopWords;
  }

  public TokenStream tokenStream(String fieldName, Reader reader) {
    StopFilter stopFilter = new StopFilter(true,
                                           new LowerCaseTokenizer(reader),
                                           stopWords);
    stopFilter.setEnablePositionIncrements(true);
    return new PorterStemFilter(stopFilter);
  }
}
```
