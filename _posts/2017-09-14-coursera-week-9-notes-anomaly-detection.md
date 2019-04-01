---
layout: post
title:  "Week 9 notes: Anomaly Detection"
date: 2017-09-14 22:14:00 +0700
tags:
  - notes
  - machine learning
no-post-nav: 0
comments: 1
---

##### **Introduction**
No notes.

##### **Density Estimation**

###### **Problem Motivation**

Build model p(x) from data.

If p(x<sub>test</sub>) < ɛ, it's an anomaly.

###### **Gaussian Distribution**

**Gaussian (Normal) Distribution**

Say $$ x \in \mathbb{R}$$, if x is a distributed Gaussian with mean μ, standard deviation σ, and variance σ<sup>2</sup>; then the probability density function (hàm mật độ xác suất) is:

$$ p(x ; \mu , \sigma ^{2}) = \frac{1}{\sqrt{2\pi\sigma^{2}}}e^{-\frac{(x-\mu)^{2}}{2\sigma^{2}}}$$

<hr>
<center><img src="https://i.imgur.com/9c3sNhT.png"/></center>
<center>Figure 1. The red curve is the standard normal distribution</center>
<center><i><a href="https://en.wikipedia.org/wiki/Normal_distribution">Source: Wikipedia - Normal distribution</a></i></center>
<hr>

**Parameter estimation**

Given the dataset x<sup>(1)</sup>, ..,x<sup>(m)</sup>; where x<sup>(i)</sup> is a distributed Gaussian. The task is to find μ and σ<sup>2</sup>:

$$ \mu = \frac{1}{m}\sum_{i=1}^{m}x^{(i)}$$

$$ \sigma^{2} =\frac{1}{m}\sum_{i=1}^{m}(x^{(i)}-\mu)^{2}$$

In machine learning people tend to lean 1/m formula but in practice whether it is 1/m or 1/(m-1) it makes essentially no difference assuming m is reasonably large.

###### **Algorithm**

**Density estimation**

Traning set: x<sup>(1)</sup>,...,x<sup>(m)</sup>.

Each sample is $$ x \in \mathbb{R}$$

$$ p(x) = \prod_{j=1}^{n} p(x_{j}; \mu_{j}, \sigma_{j}^{2})$$

$$ \mu_{j} = \frac{1}{m}\sum_{i=1}^{m}x_{j}^{(i)}$$

$$ \sigma_{j}^{2} = \frac{1}{m}\sum_{i=1}^{m}(x_{j}^{(i)} - \mu_{j})^{2}$$

**Anomaly detection algorithm**

1. Compute μ<sub>1</sub>,..,μ<sub>n</sub>; σ<sub>1</sub>,..,σ<sub>n</sub>.
2. Given new example x, compute p(x). Anomaly if p(x) < ɛ.

##### **Building an Anomaly Detection System**

###### **Developing and Evaluating an Anomaly Detection System**

**The important of real-number evaluation**

When developing a learning algorithm (choosing features, etc.), making decisions is much easier if we have a way of evaluating our learning algorithm (a number to compare for instance).

Assume we have some labeled data, of anomalous and non-anomalous examples. (y = 0 if normal, y = 1 if anomalous).

Training set x<sup>(i)</sup> (unlabeled).

Cross validation set (x<sub>cv</sub><sup>(i)</sup>, y<sub>cv</sub><sup>(i)</sup>).

Test set (x<sub>test</sub><sup>(i)</sup>, y<sub>test</sub><sup>(i)</sup>).

**Aircraft engines motivating example**

1000 good engines (normal).

20 flawed engines (anomalous).

We split the data into:
* Training set: 6000 (60%) good engines (y = 0, but we assume they're unlabeled).

* CV: 2000 (20%) good engines (y = 0), 10 anomalous (y = 1).

* Test: 2000 (20%) good engines (y = 0), 10 anomalous (y = 1).

We work on the training set, compute μ<sub>1</sub>,..,μ<sub>n</sub>; σ<sub>1</sub>,..,σ<sub>n</sub> and the model p(x).

**Algorithm evaluation**

Fit model p(x) on training set (the vast majority of them are normal).

On a cross validation/test example x, predict:
* y = 1 if p(x) < ε (anomaly).
* y = 0 if p(x) > ε (normal).

and see how often it gets the label right.

Classification accuracy is not a good way to measure the algorithm's performance, because of skewed classes (so an algorithm that always predicts y = 0 will have high accuracy). Instead, use these possible evaluation metrics:
* True positive, false positive, false negative, true negative.
* Precision/Recall.
* F<sub>1</sub>-score.

Can also use cross validation set to choose parameter ε: try many different values of ε and pick the one that minimizes (say) F<sub>1</sub>-score, or otherwise does well on your cv set.

###### **Anomaly Detection vs. Supervised Learning**

Problem: If we have the label data (we know some of them are anomalies, some are not anomalies), why don't we just use supervised learning?

**Describe when to use anomaly detection versus supervised learning**

Anomaly detection:
* Very small number of positive examples (y = 1), (0-20 is common), large number of negatives (y = 0) examples.
* Many different "types" of anomalies. Hard for any algorithm to learn from positive examples what the anomalies look like; future anomalies may look nothing like any of the anomalous examples we've seen so far.

Supervised learning:
* Large number of positive and negative examples.
* Enough positive examples for algorithm to get a sense of what positive examples are like, future positive examples likely to be similar to ones in training set.

###### **Choosing what features to use**

**Non-gaussian features**

If we plot the Gaussian distribution p(x<sub>i</sub>; μ<sub>i</sub>, σ<sub>i</sub><sup>2</sup>) of the feature x<sub>i</sub> and the graph is not a bell shaped curve, we can play with different transformations of the data in order to make it more Gaussian.

_The algorithm will usually work okay even if we don't, but if we use these transformations to make the data more gaussian, it might work a bit better._

<hr>
<center><img src="https://i.imgur.com/J2xD2ud.png"/></center>
<center>Figure 2. Gaussian and Non-gaussian feature</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

For example, in the second feature, if we take the `log(x)` transformations, we can make it become Gaussian.

**Error analysis for anomaly detection**

Want p(x) large for normal examples x.

Want p(x) small for anomalous examples x.

What if: p(x) is comparable (say, both large) for normal and anomalous examples.

We need to look at that example and figure out what went wrong, and see if we can come up with a new feature x<sub>2</sub> that helps to distinguish between this bad example, compared to the rest of normal examples.

**Monitoring computers in a data center**

Choose feature that might take on unusually large or small values in the event of an anomaly.
* x<sub>1</sub> = memory use of computer.
* x<sub>2</sub> = number of disk accesses/sec.
* x<sub>3</sub> = CPU load.
* x<sub>4</sub> = network traffic.

The new features may be the combination of available features:
* x<sub>5</sub> = x<sub>3</sub> / x<sub>4</sub>.
* x<sub>6</sub> = x<sub>3</sub><sup>2</sup> / x<sub>4</sub>.

##### **Predicting Movie Ratings**

###### **Problem Formulation**

**Example: Predicting movie ratings**

User rates movies using zero to five starts.

<hr>
<center><img src="https://i.imgur.com/nM1jQwa.png"/></center>
<center>Figure 3. User rates movies using zero to five starts</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

Some notations:
* n<sub>u</sub>: no. users
* n<sub>m</sub>: no. movies
* r(i, j) = 1 if user j has rated movie i.
* y<sup>(i, j)</sup> = rating given by user j to move i (defined only if r(i, j) = 1).

Problem: predict the values of the question marks (to recommend it to the user).

###### **Content Based Recommendations**

**Content-based recommender systems**

Let's say each movies have two features:
* x<sub>1</sub> = degree of romantic.
* x<sub>2</sub> = degree of action.

<hr>
<center><img src="https://i.imgur.com/1XzmeKY.png"/></center>
<center>Figure 4. Two new features of 5 movies</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

Now each movie can be represented by a feature vector, for example the first movie "Love at last": x<sup>(1)</sup>=[1 0.9 0]<sup>T</sup> (including bias).

For each user j, learn a parameter θ<sup>(j)</sup>. Predict user j as rating movie i with (θ<sup>(j)</sup>)<sup>T</sup>x<sup>(i)</sup> stars.

We denote m<sup>(j)</sup> = no. of movies rated by user j.

To learn θ<sup>(j)</sup>, this is actually a linear regression problem:

$$ \min_{\theta^{(j)}}\frac{1}{2m^{(j)}}\sum_{i:r(i,j) = 1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2}$$

If want, we can also add the regularization term (ignore bias term of θ course); and we can also eliminate the constant m<sup>(j)</sup> because it doesn't affect the optimization objective.

$$ \min_{\theta^{(j)}}\frac{1}{2}\sum_{i:r(i,j) = 1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2} + \frac{\lambda}{2}\sum_{k=1}^{n}(\theta_{k}^{(j)})^{2}$$

To learn θ<sup>(1)</sup>,..,θ<sup>(n<sub>u</sub>)</sup>:

$$ \min_{\theta^{(1)},..,\theta^{(n_{u})}}\frac{1}{2}\sum_{j=1}^{n_{u}}\sum_{i:r(i,j) = 1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2} + \frac{\lambda}{2}\sum_{j=1}^{n_{u}}\sum_{k=1}^{n}(\theta_{k}^{(j)})^{2} ~~~~~ (1)$$

And the gradient descent update:

<hr>
<center><img src="https://i.imgur.com/PZI4Aw1.png"/></center>
<center>Figure 5. Gradient Descent update</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

It maybe very difficult to get the features for the movies/items. In the next lesson, we will talk about an approach that does not assume that we have these features.

##### **Collaborative Filtering**

###### **Collaborative Filtering**

Let consider the problem when we don't know the values of the movies' features. (e.g. x<sub>1</sub> and x<sub>2</sub> are unknown).

Further more, now the users will tell us how much they like romantic and action movies. For example θ<sup>(Alice)</sup> = θ<sup>(1)</sup> = [0 5 0]<sup>T</sup>, it means that Alice likes romantics movies (θ<sub>1</sub><sup>(1)</sup> = 5) and doesn't really like action movies (θ<sub>2</sub><sup>(1)</sup> = 0).

From these figures, it becomes possible to try to infer what are the values of x<sub>1</sub> and x<sub>2</sub> for each movie.

For example; the movie Love at last is liked by Alice, Bob and hated by Carol, Dave. Since Alice, Bob like romantic movies; Carol, Dave like action movies; we can reasonably conclude that this is a romantic movie, and x<sup>(1)</sup> might be [1 1.0 0.0]<sup>T</sup>.

Mathematically, what we're really asking is what feature vector should x<sup>(1)</sup> be so that:
* (θ<sup>(1)</sup>)<sup>T</sup>x<sup>(1)</sup> ≈ 5.
* (θ<sup>(2)</sup>)<sup>T</sup>x<sup>(1)</sup> ≈ 5.
* (θ<sup>(3)</sup>)<sup>T</sup>x<sup>(1)</sup> ≈ 0.
* (θ<sup>(4)</sup>)<sup>T</sup>x<sup>(1)</sup> ≈ 0.

**Optimization algorithm**

Given θ<sup>(1)</sup>,..,θ<sup>(n<sub>u</sub>)</sup>, to learn x<sup>(i)</sup>:

$$ \min_{x^{(i)}}\frac{1}{2}\sum_{j:r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{k=1}^{n}(x_{k}^{(i)})^{2}$$

Given θ<sup>(1)</sup>,..,θ<sup>(n<sub>u</sub>)</sup>, to learn x<sup>(1)</sup>,..,x<sup>(n<sub>m</sub>)</sup>:

$$ \min_{x^{(1)},..,x^{(n_{m})}}\frac{1}{2}\sum_{i=1}^{n_{m}}\sum_{j:r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{i=1}^{n_{m}}\sum_{k=1}^{n}(x_{k}^{(i)})^{2} ~~~~~ (2)$$

**Summary**

Given x<sup>(1)</sup>,..,x<sup>(n<sub>m</sub>)</sup> (and movie ratings), can estimate θ<sup>(1)</sup>,..,θ<sup>(n<sub>u</sub>)</sup>.

Given θ<sup>(1)</sup>,..,θ<sup>(n<sub>u</sub>)</sup>, can estimate x<sup>(1)</sup>,..,x<sup>(n<sub>m</sub>)</sup>.

_Chicken and egg problem_

Initially, we can random some value for θ, then use it to learn x, then use x to learn better θ, then use θ to learn better x, and so on... This actually works and if you do this, this will actually cause your algorithm to converge to a reasonable set of features for your movies and a reasonable set of parameter for your different users.

In the next lesson we will improve this algorithm and make it quite a bit more computationally efficient.

###### **Collaborative Filtering Algorithm**

There's a more efficient algorithm that doesn't need to go back and forth, but can solve for θ and x simultaneously.

Put (1) and (2) together:

$$ J(x^{(1)},..,x^{(n_{m})},\theta^{(1)},..,\theta^{(n_{u})})=\frac{1}{2}\sum_{(i,j):r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})^{2}+\frac{\lambda}{2}\sum_{i=1}^{n_{m}}\sum_{k=1}^{n}(x_{k}^{(i)})^{2}+\frac{\lambda}{2}\sum_{j=1}^{n_{u}}\sum_{k=1}^{n}(\theta_{k}^{(j)})^{2}$$

$$ \min_{x^{(1)},..,x^{(n_{m})},\theta^{(1)},..,\theta^{(n_{u})}}J(x^{(1)},..,x^{(n_{m})},\theta^{(1)},..,\theta^{(n_{u})})$$

If we hold the x constant, and minimize J with the respect to θ, it becomes (1) problem. If we do the opposite, it becomes (2) problem.

We can get away with the convention x<sub>0</sub> = 1, thus no need for θ<sub>0</sub>. x and θ are now both n dimension.

**Collaborative filtering algorithm**
1. Initialize θ<sup>(1)</sup>,..,θ<sup>(n<sub>u</sub>)</sup>, x<sup>(1)</sup>,..,x<sup>(n<sub>m</sub>)</sup> to small random values.

2. Minimize J(θ<sup>(1)</sup>,..,θ<sup>(n<sub>u</sub>)</sup>, x<sup>(1)</sup>,..,x<sup>(n<sub>m</sub>)</sup>) using gradient descent (or an advanced optimization algorithm). E.g. for every j = 1,..,n<sub>u</sub>, i = 1,..,n</sub>m</sub>:

$$ x_{k}^{(i)}=x_{k}^{(i)}-\alpha(\sum_{j:r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})\theta_{k}^{(j)}+\lambda x_{k}^{(i)})$$

$$ \theta_{k}^{(j)}=\theta_{k}^{(j)}-\alpha(\sum_{i:r(i,j)=1}((\theta^{(j)})^{T}x^{(i)}-y^{(i,j)})x_{k}^{(i)}+\lambda \theta_{k}^{(j)})$$

_In this formula we're no longer have the convention x<sub>0</sub> = 1._

3. For a user with parameter θ and a movie with (learned) features x, predict a star rating of θ<sup>T</sup>x.

##### **Low rank matrix factorization**

###### **Vectorization: low rank matrix factorization**

Talk about the vectorization implementation of collaborative filtering algorithm and other things you can do with this algorithm.

Take all the data (move and user) into a matrix:

$$ Y = \left [ \begin{matrix}
 5&5&0&0\\
 5&?&?&0 \\
 ?&4&0&? \\
 0&0&5&4 \\
 0&0&5&0
\end{matrix} \right ]$$

Predicted ratings matrix:

<hr>
<center><img src="https://i.imgur.com/VJVORKz.png"/></center>
<center>Figure 6. Predicted ratings matrix, (i,j) = predictive rating of user j on movie i.</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

This predicted ratings matrix can be written as XΘ<sup>T</sup>, where:

<hr>
<center><img src="https://i.imgur.com/Ee3u8d4.png"/></center>
<center>Figure 7. X and Θ matrix.</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

This algorithm called Low Rank Matrix Factorization, this term comes from the property that the matrix XΘ<sup>T</sup> has a "low rank" property in linear algebra.

**Finding related movies**

For each product i, we learn a feature vector x<sup>(i)</sup>. Although neither we know what exactly these features are, nor they're easy to visualize; the algorithm will usually learn very meaningful features for capturing whatever are the most important properties of a movies that causes you to like or dislike it.

Problem: How to find movies j related to movie i?

If the distance between movie i and j is small (e.g. $$ \left \| x^{(i)}  - x^{(j)} \right \|$$ small), then they might be similar. (You can use KNN to find K most similar movies to movie i).

###### **Implementation Detail: Mean Normalization**

Let's consider a user who hasn't rated any movies.

<hr>
<center><img src="https://i.imgur.com/qiit13G.png"/></center>
<center>Figure 8. Eve hasn't rated any movies.</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

According to the optimization formula, the only factor that affects to θ<sup>5</sup> is regularization term. By minimizing this term, θ<sup>5</sup> will end up equals 0.

So when we go to predict how user 5 would rate any movies, the result is 0. Then we can't recommend this user any movie to watch.

The idea of mean normalization will help us fix this problem:

<hr>
<center><img src="https://i.imgur.com/C7HVfvm.png"/></center>
<center>Figure 9. Build Y matrix, compute the mean of each row, then subtract from each row the average rating for that movie. (normalizing each movie to have an average rating of zero).</center>
<center><i><a href="https://www.coursera.org/learn/machine-learning">Source: Coursera Machine Learning course</a></i></center>
<hr>

Then we learn θ and x from the new Y matrix.

For user j, on movie i predict: θ<sup>(j)</sup>x<sup>(i)</sup> + μ<sub>i</sub>.

For user 5, on movie i predict: θ<sup>(5)</sup>x<sup>(i)</sup> + μ<sub>i</sub> = μ<sub>i</sub>.

This is reasonable because if a person hasn't rated/watched any movie, we just predict for each of the movies the average rating that those movies got.

If we have some movie with no ratings, we can solve it similarly (subtract from the column instead of from row).
