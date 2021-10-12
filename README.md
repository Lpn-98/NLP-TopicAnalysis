# NLP-TopicAnalysis
This project proposes the Title- Weighed TF-IDF based on VSM and combined it with the LDA model and SVM model, using labelled texts obtained by crawlers on Sina Weibo (which is the biggest social media platform in China that is similar to Twitter), and conducted three experiments: Topic clustering, topic classification, topic analysis.

- [NLP-TopicAnalysis](#nlp-topicanalysis)
  * [Background](#background)
  * [Dataset](#dataset)
    + [Raw data](#raw-data)
    + [Processed data](#processed-data)
  * [Topic clustering - dataset 1](#topic-clustering---dataset-1)
    + [1. Decide topic numbers - based on xLDA](#1-decide-topic-numbers---based-on-xlda)
    + [2. Decide value of parameter gamma](#2-decide-value-of-parameter-gamma)
    + [3.Topic clustering simple analysis - take TW-LDA as example](#3topic-clustering-simple-analysis---take-tw-lda-as-example)
  * [Topic classification - dataset 2](#topic-classification---dataset-2)
    + [SVM modeling](#svm-modeling)
  * [Topic analysis - dataset 1 and 2](#topic-analysis---dataset-1-and-2)
    + [Combine LDA and SVM modeling](#combine-lda-and-svm-modeling)
  * [Conclusion and future work](#conclusion-and-future-work)
    + [Conclusion](#conclusion)
    + [Future work](#future-work)



## Background

This repository includes source code, scripts, and data for my practice project "Topic Analysis of Weibo News Based on Title-Weighted LDA Model". Completed in May 2020. The uploaded code can reproduce the results, and the further standardization of the code is still in progress.

这里存放着一些我为了学习和练习而所做的的项目的相关资料，名字是“基于标题加权的LDA模型的微博新闻主题分析”。完成于2020年5月。目前上传的代码可以复现结果，对于代码的进一步规范化还在进行中。

これは学校の課題のため作ったプロジェクトです。プロジェクトの課題は「タイトル加重LDAモデルに基づくWeiboニューストピック分析」。2020年5月で完成しました。アップロードされたコードは結果を再現でき、コードのさらなる標準化はまだ進行中です。


## Dataset
### Raw data
Two datasets are used and the raw data is crawled from Sina weibo, like this:
![image](https://user-images.githubusercontent.com/58460943/135987422-750adb9a-9563-463a-8ad6-5ca21b5c0c93.png)

**Dataset 1** is all the crawled text content of Sina Weibo offical account "Global Times", which is posted from 2019-01-01 00:00 to 2019-12-31 23:59. This is mainly for topic clustering.

**Dataset 2** is all the crawled text content of Sina Weibo offical account "China Agriculture News", "Sina Education", "Sina Finance", "Xinhua Sports", "Xinhua International" from 2019-12-31 (including this day) forward 1000 texts content, and label each text with the official account type (like label "Agriculture" for ccount "China Agriculture News", label "Education" for account "Sina Education", etc.)

### Processed data
After processing the data, the dataset content like this:

![image](https://user-images.githubusercontent.com/58460943/135989374-655afd1f-cdaa-4a37-b845-32d89251822d.png)


Here is a simple descriptive analysis of dataset 1, for example：

**1. Number of texts posted per month**

![image](https://user-images.githubusercontent.com/58460943/135989238-bd22ddb2-b977-4d16-9f82-e1982ebf7d51.png)

It can be seen that the number of Weibo posts posted every month in 2019 is around 1200-1500, with the least number of posts in February (maybe due to the Spring Festival and corona virus).

**2. Word cloud using frequency(left one) and tf-idf(right one)**

![image](https://user-images.githubusercontent.com/58460943/135989689-554cdffc-cd37-4dc9-b780-2ae04dcf7ee5.png)
![image](https://user-images.githubusercontent.com/58460943/135989717-f2d7f8c3-c0f9-471e-adb0-1bc8d7aff3e3.png)

Both show key keywords in the 2019 news, such as "China", "United States", "Hong Kong" and so on. 

However, in contrast, these two word clouds **reflect the nature and advantages of TF-IDF to correct the weight of high-frequency words**: "China" appears in most news texts, so words only from the frequency of words will occupy a very important position. However, the weight calculated by TF-IDF pays more attention to other words that may be non-high-frequency but important, so the weight of "China" is reduced. At the same time, the weights of some commonly used words such as "have" and "appeared" are also less than the word frequency word cloud. It can be seen intuitively that TF-IDF has better feature words and weights than the word frequency method.


## Topic clustering - dataset 1
### 1. Decide topic numbers - based on xLDA
The basic model is the standard LDA model (xLDA) using word frequency. 

Delete extreme feature words, get feature words, set the number of topics(from 10 to 150), model using xLDA 55 times per situation, calculate the average of cherence score C_v and confusion score in the 5 times after Gibbs sampling converges.

![image](https://user-images.githubusercontent.com/58460943/135995221-56bd84d2-de9d-401b-9ab9-3f3cf69a0e5e.png)

Combining the values, **choose 50 as the best number of topics for following experiments.**

### 2. Decide value of parameter gamma

The advanced model is LDA topic modeling based on TF-IDF, and TW-LDA topic modeling based on title weighted TF-IDF.

Delete extreme feature words, get feature words, set the value of γ (0.0,0.1,0.2,...,1.0), model using xLDA 55 times per situation, calculate the average of cherence score C_v in the 5 times after Gibbs sampling converges.

![image](https://user-images.githubusercontent.com/58460943/135992571-7cbfd57c-f844-4231-9446-8ead7e3ba5ca.png)

Therefre, **choose 0.5 as the value of parameter γ.**

Besides, the C_v of the LDA model based on TF-IDF is greater than the standard LDA model based on word frequency, and regardless of the γ of the TW-LDA model, the C_v of the TW-LDA model is greater than the LDA model based on TF-IDF.

### 3.Topic clustering simple analysis - take TW-LDA as example

**The results are highly interpretable and can be consistent with real life situations or timelines.**

Below shows the top 20 words of top 10 topics:

![image](https://user-images.githubusercontent.com/58460943/135995508-1a795e22-4c1f-4cfd-8e98-68081dd5ba98.png)

Below is the visualization of all the 50 topics:

![image](https://user-images.githubusercontent.com/58460943/135995607-6dfab480-8afe-45e1-a133-483f03010590.png)


Below is the feature words and weights of the same topic in different models:

![image](https://user-images.githubusercontent.com/58460943/135993798-6b439f36-a74b-421d-9182-73c868736dce.png)

Below is the number of posted text per month of "National Day military parade" topic：

![image](https://user-images.githubusercontent.com/58460943/135994324-c9807bc4-cb40-4ee6-b2f3-a39c6e11766b.png)

Below is some examples of texts clustered to "HK Riots" topic：

![image](https://user-images.githubusercontent.com/58460943/135994472-7207b64d-6a0c-44d4-9141-aab34c8daf82.png)

Below is the number of posted text per month of "HK Riots" topic：

![image](https://user-images.githubusercontent.com/58460943/135994584-83b8fd0e-e309-4fa7-88f4-5c647c3504f9.png)


## Topic classification - dataset 2
### SVM modeling

![image](https://user-images.githubusercontent.com/58460943/135998364-87d9e8d5-2e7e-43f2-aa84-ae9b3aa51a4c.png)

γ reaches two local maximums at 0.1 and 0.6, and as γ goes from 0.8 to 1.0, the F1 Score of the model drops rapidly. Considering only the F1 Score as the evaluation standard, **γ is  chosen as 0.6**. In fact, when γ=0.6, the model not only reaches the maximum value of F1 Score, but also its accuracy, precision, and recall.

![image](https://user-images.githubusercontent.com/58460943/135998753-8c3f5ded-ae6b-4cac-829e-c612ce3aa970.png)


**The indexes of SVM model based on word frequency are lower than the SVM model based on TF-IDF, and the performance of the SVM model based on title weighted TF-IDF is higher than that of the SVM model based on word frequency.**

When γ=0.6, the accuracy, precision, recall, and F1 score of the model are the highest among all models. But under different parameters γ, its performance gradually declines. When γ values are 0.9 and 1.0, It may not be as good as the classification effect of the SVM model based on TF-IDF.

The preliminary analysis may be due to the fact that when γ=0.9, the weighted TF-IDF value used by the feature words in a certain title is 10% of its TF-IDF value in all texts plus 90% of its value in all titles. TF-IDF value. When γ=1.0, the weighted TF-IDF value used by the word feature word is 100% of its TF-IDF value in all texts.

In the case of the data set used in this article, for a title feature word that appears in the text, although its weighted TF-IDF value may be more representative of its importance in the title, it may be more important in the text at the same time. There has been too much reduction, which makes the weighted TF-IDF value deviate from the real situation.

**This result also proves the importance of weighting the weights of the title and the text used here, which enables the model to take into account the important role of the title and the feature words in the text.**


## Topic analysis - dataset 1 and 2
### Combine LDA and SVM modeling

Combining the Title-Weighted LDA model in the previous section with the Title-Weighted TF-IDF SVM classification model in this section, we can classify a text according to events and channels. For example, use following text as input:

![image](https://user-images.githubusercontent.com/58460943/136000934-8fe470e5-da29-4787-aa93-94be72200e33.png)
<br>*("\[Hong Kong Government: #IMF Confirm Hong Kong International Financial Center Position#] According to a report on the Hong Kong Special Administrative Region Government News Network on the 30th, the International Monetary Fund (IMF) released an assessment report on the 30th, reaffirming Hong Kong as a global financial center and an intra-regional trade hub. , The status of one of the most open economies in the world. The organization welcomes the Hong Kong Special Administrative Region Government’s recent fiscal stimulus measures to revitalize the economy, and supports the Hong Kong Special Administrative Region Government to adopt a three-pronged approach to control property market risks and increase the affordability of home ownership. : The IMF affirms Hong Kong’s status as an international financial center (with photos).")*

Extract the title and text of this text content, use the established dictionary to calculate the TF-IDF value based on the title weight, and enter it into the title weighted LDA model that has been established in the previous section. The result shows this text has a probability of 0.38997 that may fall in the topic with **"Hong Kong"** in the top20 feature words, and a probability of 0.32516 may fall in the topic with **"Economy"** or **"Financial"** in the top20 feature words, and this has been established. 

Among the 50 topics, the probability that it belongs to other topics is less than 0.18. Then input this text into the SVM model based on the title weighted TF-IDF that has been established, and it can be calculated to have a probability of 0.81514 of belonging to the **"financial"** classification.

## Conclusion and future work
### Conclusion
1. Introduce Title-Weighted TF-IDF model to make full use of the different importance of text in different locations.

3. Apply Title-Weighted TF-IDF model to LDA to generate word-topic and document-topic matrices that are closer to the real situation, improve the quality of topic clustering, and perform visualization to improve interpretability.

5. Apply Title-Weighted TF-IDF model to SVM to improve the classification quality of texts, and combine the LDA model with the SVM model. Through unsupervised and supervised aspects, the Weibo content can be analyzed from two aspect: Potential topics and existing topic labels.

### Future work
1. Further improve the feature word extraction and weighting.

2. Combine external semantic knowledge with other aspect knowledge for experimental analysis.
