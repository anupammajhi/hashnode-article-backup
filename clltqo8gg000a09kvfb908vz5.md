---
title: "KNN — An Introduction"
datePublished: Sun Jul 28 2019 08:34:16 GMT+0000 (Coordinated Universal Time)
cuid: clltqo8gg000a09kvfb908vz5
slug: knn-an-introduction
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693157876637/76368d96-a557-4e29-8189-3f46a8ace1b1.png
tags: introduction, algorithms, data-science, knn

---

KNN or k-Nearest Neighbors algorithm is quite a classic algorithm to solve classification problems. It is based on a simple assumption that objects in a group (neighbors) or close to the group tend to have similar traits and hence can be classified. You can already make out that this assumption is actually over-simplified. Let’s see how good or bad can this assumption be.

### KNN around us

Though this is over-simplified, you would have noticed around yourself that people with similar interests usually have similar traits. Let’s think of a few examples:

People following similar kinds of news headlines and in a similar age group can roughly be classified into the political parties they will most likely vote for. People with similar calorie intake and a similar amount of physical activity can be classified as healthy or not healthy. Emails with similar choices of words and similar formats can be classified as spam or not spam and so on …

### How does KNN work?

The letter k in a kNN is the number of nearest neighbors that it will take into account to decide to which class an object belongs. So, quite simply, if k=10, we check the 10 closest neighbors of the object we want to classify, and if it has more neighbors of type A and fewer neighbors of other types B or C, we say the object belongs to type/class A.

But how do we measure which is closest? The KNN algorithm compares the position of the data point in question with all the training data points ([Check out this blog if you are not sure what training a model is](https://anupamm.com/blog/model-selection-train-validate-test)). To find the distance we can use any distance metric, even Euclidian distance. Once the model finds out who are its ‘k’ closest neighbors, it just goes ahead with voting among those members to find out where it belongs.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693088564177/548075ac-e1c2-4b60-8cc9-fcab11987b14.png align="center")

Model trying to find where the yellow star belongs based on 5 nearest neighbors

In the above figure, the model is trying to figure out which category the uncategorized data point (yellow star) belongs to (assuming k=5). We can easily see here that out of 5 nearest neighbors, 4 belong to the red squares category. Hence, the yellow star should also be a red square category data point.

### What is the best value of ‘k’?

Firstly, there is no single best universal value of k. Then how do we decide what value of k to use? The answer is it depends. It depends on the data, more precisely the [training and validation](https://anupamm.com/blog/model-selection-train-validate-test) data. While training and validating with different k values we have to make sure we get the least error possible, and then it’s up to us to decide which ‘k’ value to use.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693088565195/732ac8c3-5582-432b-89e6-7da3cae29424.jpeg align="center")

`Example: Validation error for different values of k   image source: AnalyticsVidya`

In the example image above, the plot between different k values vs. validation error shows that we get the lowest error value somewhere around k=10. Hence, in this case, k=10 should be our choice.

### KNN and Curse of Dimensionality

Just like any other data science algorithm, KNN faces the curse of dimensionality. The curse of Dimensionality is basically the difficulty/error that a model faces as we keep increasing the number of features/dimensions. For example, a model using a few features (e.g. age, sex, height) would most likely predict results better than a model using a huge number of such features.

To understand better, let’s imagine we are trying to find out the shoe size of a person based on age and height. Here we just have 2 dimensions to check, i.e. age and height, hence there can only be certain kinds of people with a certain amount of differences/individuality/noise. So we can quite accurately predict what the shoe size could be. But what if the data also has additional dimensions like weight, city, and favorite color? The model can still predict well enough, but there are high chances that the prediction wouldn’t be accurate enough. The reason is that two people may live in different cities but still have similar physical features, but in KNN, those two people will not be very close neighbors. Consider ‘favorite color’ as a feature, that will make it even more difficult.

The solution is to remove unnecessary or irrelevant features/dimensions and keep the number of features to a minimum.

### KNN — Regression

I did mention in the beginning that KNN is a classification algorithm. However, KNN is also used for regression models. For the unaware, a regression model is where we try to find out the actual value instead of a category, e.g. trying to predict height, salary, price, etc. The only difference in KNN for Regression compared to KNN for classification is that, instead of voting among closest neighbors, the model takes an average of the actual value (e.g. salary) of all the k nearest neighbors. There can be different ways to calculate the average.

That brings us to the end of this post on KNN introduction.