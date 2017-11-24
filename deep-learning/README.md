# Deep Learning
## Paper
1. [A Learning Algorithm for Boltzmann Machines][0],DAVID H. ACKLEY,GEOFFREY E. HINTON,TERRENCE J. SEJNOWSKI,[COGNITIVE SCIENCE 9, 147-169(1985)][100]
1. [Empirical Evaluation of Gated Recurrent Neural Networks on Sequence Modeling][19],Junyoung Chung, Caglar Gulcehre, KyungHyun Cho, Yoshua Bengio,[11 Dec 2014][119]

## Note
1. [深度学习中的激活函数][2]
1. Neural Network and Deep Learning
    - [cover][3]
    - [Using neural nets to recognize handwritten digits][11]
        - Perceptrons
        - Sigmoid neurons
        - The architecture of neural networks
        - A simple network to classify handwritten digits
        - Learning with gradient descent
        - Implementing our network to classify digits
    - [How the backpropagation algorithm works][12]
        - Warm up: a fast matrix-based approach to computing the output from a neural network
        - The two assumptions we need about the cost function
        - The Hadamard product, s⊙t
        - The four fundamental equations behind backpropagation
        - The backpropagation algorithm
    - [Improving the way neural networks learn][13]
        - The cross-entropy cost function
        - Introducing the cross-entropy cost function
        - Softmax
        - Overfitting and regularization
        - Regularization
        - Why does regularization help reduce overfitting?
        - Other techniques for regularization
    - [A visual proof that neural nets can compute any function][14]
    - [Why are deep neural networks hard to train?][15]
        - The vanishing gradient problem
        - What's causing the vanishing gradient problem? Unstable gradients in deep neural nets
    - [Deep learning][16]
        - Introducing convolutional networks
1. Fundamentals of Deep Learning
    - [cover][1]
    - [Chapter 1. The Neural Network][21]
        - The Neuron
        - Feed-Forward Neural Networks
        - Linear Neurons
        - Sigmoid, Tanh, and ReLU Neurons
    - [Chapter 2. Training Feed-Forward Neural Networks][21]
        - The Fast-Food Problem
        - Gradient Descent
        - The Delta Rule and Learning Rates
        - Gradient Descent with Sigmoidal Neurons
        - The Backpropagation Algorithm
        - Stochastic and Minibatch Gradient Descent
        - Test Sets, Validation Sets, and Overfitting
        - Preventing Overfitting in Deep Neural Networks
    - [Chapter 3. Implementing Neural Networks in TensorFlow][22]
        - Installing TensorFlow
        - Creating and Manipulating TensorFlow Variables
        - TensorFlow Operations
        - Placeholder Tensors
        - Sessions in TensorFlow
        - Navigating Variable Scopes and Sharing Variables
        - Managing Models over the CPU and GPU
        - Specifying the Logistic Regression Model in TensorFlow
        - Logging and Training the Logistic Regression Model
        - Leveraging TensorBoard to Visualize Computation Graphs and Learning
        - Building a Multilayer Model for MNIST in TensorFlow
    - [Chapter 4. Beyond Gradient Descent][23]
        - Local Minima in the Error Surfaces of Deep Networks
        - How Pesky Are Spurious Local Minima in Deep Networks?
        - Flat Regions in the Error Surface
        - When the Gradient Points in the Wrong Direction
        - Momentum-Based Optimization
        - A Brief View of Second-Order Methods
            - Conjugate Gradient Descent
            - Broyden–Fletcher–Goldfarb–Shanno (BFGS)
        - Learning Rate Adaptation
            - AdaGrad—Accumulating Historical Gradients
            - RMSProp—Exponentially Weighted Moving Average of Gradients
            - Adam—Combining Momentum and RMSProp
        - Optimization Algorithms Experiment
    - [Chapter 5. Convolutional Neural Networks][24]
        - Vanilla Deep Neural Networks Don’t Scale
        - Filters and Feature Maps
        - Full Description of the Convolutional Layer
        - Max Pooling
        - Full Architectural Description of Convolution Networks
        - Closing the Loop on MNIST with Convolutional Networks
        - Building a Convolutional Network for CIFAR-10
        - Visualizing Learning in Convolutional Networks
        - Leveraging Convolutional Filters to Replicate Artistic Styles
    - [Chapter 6. Embedding and Representation Learning][25]
        - Learning Lower-Dimensional Representations
        - Principal Component Analysis
        - Motivating the Autoencoder Architecture
        - Implementing an Autoencoder in TensorFlow
        - Denoising to Force Robust Representations
        - Sparsity in Autoencoders
        - When Context Is More Informative than the Input Vector
        - The Word2Vec Framework






[0]: A-Learning-Algorithm-for-Boltzmann-Machines.ipynb
[1]: Fundamentals-of-Deep-Learning/
[2]: activation-function.ipynb
[3]: NeuralNetworkAndDeepLearning/

[11]: NeuralNetworkAndDeepLearning/chap1.ipynb
[12]: NeuralNetworkAndDeepLearning/chap2.ipynb
[13]: NeuralNetworkAndDeepLearning/chap3.ipynb
[14]: NeuralNetworkAndDeepLearning/chap4.md
[15]: NeuralNetworkAndDeepLearning/chap5.ipynb
[16]: NeuralNetworkAndDeepLearning/chap6.ipynb

[21]: Fundamentals-of-Deep-Learning/Fundamentals-of-Deep-Learning-1+2.ipynb
[22]: Fundamentals-of-Deep-Learning/Fundamentals-of-Deep-Learning-3.ipynb
[23]: Fundamentals-of-Deep-Learning/Fundamentals-of-Deep-Learning-4.ipynb
[24]: Fundamentals-of-Deep-Learning/Fundamentals-of-Deep-Learning-5.ipynb
[25]: Fundamentals-of-Deep-Learning/Fundamentals-of-Deep-Learning-6.ipynb



[19]:Empirical-Evaluation-of-Gated-Recurrent-Neural-Networks-on-Sequence-Modeling.ipynb

[100]:http://www.cs.toronto.edu/~fritz/absps/cogscibm.pdf
[119]:https://arxiv.org/abs/1412.3555
