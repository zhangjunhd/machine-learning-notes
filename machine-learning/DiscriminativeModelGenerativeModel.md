# 什么是判别模型(Discriminative Model)和生成模型(Generative Model)
Let's say you have input data x and you want to classify the data into labels y. A `generative model` learns the joint probability distribution p(x,y) and a `discriminative model` learns the conditional probability distribution p(y|x) - which you should read as 'the probability of y given x'.

Here's a really simple example. Suppose you have the following data in the form (x,y):

```
       (1,0), (1,0), (2,0), (2, 1)

p(x,y) is

             y=0   y=1
            -----------
       x=1 | 1/2   0
       x=2 | 1/4   1/4

p(y|x) is

             y=0   y=1
            -----------
       x=1 | 1     0
       x=2 | 1/2   1/2
```

If you take a few minutes to stare at those two matrices, you will understand the difference between the two probability distributions.

The distribution p(y|x) is the **natural** distribution for classifying a given example x into a class y, which is why algorithms that model this directly are called **discriminative** algorithms. **Generative** algorithms model p(x,y), which can be tranformed into p(y|x) by applying Bayes rule and then used for classification. However, the distribution p(x,y) can also be used for other purposes. For example you could use p(x,y) to generate likely (x,y) pairs.

From the description above you might be thinking that generative models are more generally useful and therefore better, but it's not as simple as that. This [paper][1] is a very popular reference on the subject of discriminative vs. generative classifiers, but it's pretty heavy going. The overall gist is that **discriminative models generally outperform generative models in classification tasks.**

[1]: http://robotics.stanford.edu/~ang/papers/nips01-discriminativegenerative.pdf
