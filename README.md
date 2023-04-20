Note: this repo requires the GloVe pretrained embeddings which can be found at https://nlp.stanford.edu/data/glove.6B.zip

The file was too large to include in the repo and should be downloaded manually.

File Breakdown:


# Title
Detecting Hate Speech in Social Media Applications
  by Haley Matuszak, Byron Phan and Jasmine Chiou

# Abstract 
Hate speech has been a long standing issue, however, it is reaching more people as social media continues to expand. The spread of hate speech online can lead to the promotion of discrimination, harassment and violence against certain groups of people which can create an unsafe and unwelcoming environment for those targeted by it. In this project, we attempted to develop machine learning models that could identify and flag hate speech in social media so that we can create a safer, more inclusive online environment for users.

# Documentation:
## Introduction:
The problem that we are tackling here is detecting hate speech on social media platforms. Hate speech refers to any form of expression, verbal or written, that is intended to “degrade, intimidate, or incite violence or prejudice” against a person or group of people based on their race, ethnicity, religion, gender or sexual orientation. This can include things such as racial slurs and derogatory comments. Hate speech is a problem because it promotes discrimination and exclusion, and it targets groups that are already vulnerable in society; this can lead to feelings of isolation, exclusion, and lowered self-esteem. In extreme cases, hate speech can invite violence and harm which creates a “climate of fear and intimidation”. This can make groups feel that they are in an unsafe environment.

This problem is interesting because social media platforms should be a safe space for people, and the amount of hate speech online can deter people from using the platforms. The social media companies should want to accurately find hate speech and remove those posts as they are losing users from it. It can also help companies to comply with regulations and policies related to online content moderation, and it can help them to maintain a positive brand image and avoid reputational damage from the harmful content on their platform. However, many companies do not want to tackle this issue as they worry they will lose users who feel that their “freedom of speech” is being infringed on, especially when the models falsely accuse them.

There are many approaches to classifying natural language. One is rule-based approaches. Here, we would set a list of rules or keywords that would be indicative of hate speech text. This technique is easy to implement, however, it cannot capture the complexity of hate speech. Hate speech depends a lot on the context in which the words are said because certain phrases of words can also be used in positive contexts. It is also hard to define a set of rules that can cover all of the different possibilities of hate speech, especially as language is always growing and expanding. Another technique that might work here would be a Support Vector Machine. The SVM would use labeled data to find the best boundary or hyperplane that separates the hate speech and non-hate speech data. While they may work on smaller datasets, they have trouble capturing complex relationships which may be present in hate speech. Naive Bayes is a popular technique in NLP which computes the probability that a given text belongs in a particular class based on the frequency of certain words. Again, this algorithm is fast to train and can work with large datasets. However, it assumes that all of the features are independent, which most likely would not be true in hate speech data. Decision trees are another option, splitting the dataset based on the most informative features. They are very easy to interpret, however, we do not believe it could figure out the patterns in this data as it is very complex. We also worried that it would overfit to our dataset and would not generalize well. 
We finally decided that neural networks were the best option for creating our model. Neural networks are very popular in the natural language processing field because they are able to learn complex patterns and relationships in text data. Every model we saw online differs in which layers they put into the model as it is very guess-and-check based. We also decided to create a Naive Bayes model for comparison.
The key components of a neural network are the input layer (where the model receives the input), hidden layers (that perform calculations and pass to the next layer), and the output layer (which produces the final outcome of the model). The neural network also uses an activation function at each hidden layer to allow the neural network to learn the patterns of the data, and weights and biases to shift the activation function and minimize the error between the outputs.  Neural networks also usually have an optimization function that adjusts the weights and biases during training to minimize the loss function, a measure of how well the model is performing. With these components, neural networks are very good at generalizing to unseen data, which we want in the future to be able to expand to all hate speech. They usually also have high accuracy with complex and large datasets and can learn the features without any manual engineering. Some downsides to this technique is that it needs a lot of data, takes a long time to train, can be easily overfit, and is hard to interpret. 
## Setup
Our original dataset was “A Curated Hate Speech Dataset”, published in 2022 `[1]`. The dataset contains English speech sentences taken from social media platforms. It has 451,709 sentences in total, 371,452 of which are non-hate speech and 80,250 are hate speech. The dataset is curated from various sources like Kaggle, GitHub and other websites. The authors noted that the text in each sentence of the final dataset is limited to 180 words. We believed that this dataset was unfairly balanced since there were more non-hate speech instances than hate speech. We decided to randomly sample from all of the non-hate speech sentences to get 79,305 hate speech sentences and 100,000 non-hate speech sentences. We wanted to have slightly more non-hate speech as we believed that the patterns were much more hard to find. Hate speech tends to focus on a few key concepts/groups, and non-hate speech can span from any concept. Therefore, we wanted to give the model a chance at learning the differences. 

We decided that the best models to run for this dataset would be a Naive Bayes, a Convolutional Neural Network, a Recurrent Neural Network, and a CNN-LSTM. We used Google Colaboratory so that we could all work on it together. All of these models have been very successful in natural language processing applications. For Naive Bayes, our parameters were slightly different than the rest of the models. Here, we preprocess the data using the TF-IDF vectorizer from scikit-learn. This converts the text into numerical features that the algorithm can use. We also remove common English words that may not be useful for classification such as “the” and “a”. From this, we create feature matrices which have all of the numerical features of the text. We did not set any custom parameters in the model. The CNN, RNN, and CNN-LSTM had different features from the Naive Bayes. Here we loaded pre-trained word embeddings from glove.6B.100d.txt. Our code reads each line of the file, and stores the words and word vectors in a dictionary. We tokenize our text and count the frequency of each word. Then we convert the text into a sequence of integers where each integer is a word in our overall vocabulary. This is necessary for the neural network to be able to interpret it. We then pad the sequences of integers with zeros so that they all have a length of 100. From this, we are able to create an embedding matrix for the models to take in. Now for the structure of each of the neural networks, we will break it down below:

For the CNN model, we start by creating an embedding layer that maps all of the words in the text to a vector. The weights for this layer are initialized using a pre-trained embedding matrix and do not update the weights during training. We then added a Convolutional layer to the network which applies 32 filters of size 3 to the output of the Embedding layer. We used an activation of ReLU to introduce non-linearity into the model. We then added a MaxPooling layer which reduces the dimensionality of the output, and added a Flatten layer to turn the output into a 1-dimensional array. We add a Dense layer which adds a fully connected layer to the network using ReLU again. We added a Dropout layer, which randomly drops 20% of the neurons from the previous layer to try to avoid overfitting the model. Finally, we added another Dense layer using the sigmoid activation function to map our output to a probability. When we fit the model, we used Early Stopping to avoid overfitting.

For the RNN model, we again started with the Embedding layer, again mapping each word to a dimensional vector. The weights for this layer are initialized using a pre-trained embedding matrix, and we do not update the weights during training. We added a Long Short-Term Memory (LSTM) layer to the network which is known to capture long-term dependencies. We use 64 memory cells here. Finally, we add a Dense layer with the sigmoid activation function to convert the output into a probability between 0 and 1. When we fit the model, we again used Early Stopping to avoid overfitting.

Finally, for our CNN-LSTM, we again start with the Embedding Layer which maps each word in the text to a dimensional vector. The weights for this are initialized using a pre-trained embedding matrix, and they are not updated during training. We started with a Convolutional layer which applies 64 filters of size 3 using the ReLU function. We then added a Pooling layer which reduces the dimensionality of the output. Then, we added a LSTM layer which captures long-teem dependencies both forward and backward using 64 memory cells. We added a Dropout layer that randomly drops 50% of the previous layer during each training iteration to avoid overfitting. We finally added a Dense layer using the sigmoid function to convert the output to a probability between 0 and 1. Again, we used Early Stopping here when fitting the model to prevent overfitting.
## Results
In our experiment, we ran four models that are known to do well on natural language processing problems: Naive Bayes, CNN, RNN, and CNN-LSTM. We have created a table to compare the accuracy, precision, recall, and F1 value for each model seen in Figure 1. However, it is hard to directly compare the models as we do not know if our models are the optimal form and they have different layers. In our project, we each took one of the neural networks and experimented with adding layers and taking away layers, using other neural networks as resources. 

We wanted to equally weight accuracy, precision and recall when evaluating our models because of how our model might be used. Our original idea was to create a model that could detect hate speech in social media posts so that the websites could take the posts down. Therefore, we want to accurately find as much hate speech as possible, but not always flag non-hate speech as it will frustrate our users who are being falsely accused. Therefore, from our results, it seems that all of our models did roughly the same on this dataset. If we had to use a model only from looking at this data, we would probably say that the RNN performed the best as it had the highest accuracy and recall of the four models and was very close in precision scores.

However, another good measure of a model is how they generalize to other datasets. Our original training dataset consisted of social media posts. So, we wanted to see how they would do on a wider range of content. We found two databases: the Davidson Hate Speech `[2]` Dataset and the Hate Speech Dataset `[3]`. The Davidson Hate Speech Dataset consists of 1,430 instances of hate speech and 1,430 instances of non-hate speech. This dataset mainly deals with hate speech that is racist, sexist, homophobic, and offensive. The Hate Speech Dataset consists of 1,196 instances of both hate speech and non-hate speech. The data set was taken from a white supremist forum. We created a table to see the results of the Davidson Dataset on our previously trained models and a table to see the results of the Hate Speech Dataset seen in Figure 2 and 3 respectively. (Jasmine add into this Paragraph!!!) 

In the case of both datasets, we can see that all four of our models generalized very well. They perform about the same on the original dataset. We originally expected that the CNN and RNN would overfit and learn specific features that were in social media posts, so they would not generalize as well. We also wondered if the type of text would have an impact. Usually social media posts are short and informal, while our testing datasets were a little longer and more sentence-based. We also predicted that Naive Bayes and CNN-LSTM would be most resiliant and be able to generalize to new datasets because of how they can learn general rules. In the Davidson Dataset, we can see that RNN and Naive Bayes did slightly better, but not significantly. In the Hate Speech Dataset (HSD), we can see that RNN performed the best overall. It is possible that with more hyperparameter tuning that these standings may change. If we consider both our results in the training session and in the testing session, it seems that either Naive Bayes or an RNN are the best choice for our model. Possibly, with more hypoterparameter tuning, the RNN might outperform the Naive Bayes, but the Naive Bayes is very simple to implement and does almost as well.

## Discussion
Of the models tested, the recurrent neural network exhibited the best performance by a narrow margin with an accuracy of 0.829 and a F1 score of 0.808. The RNN also generalized very well to different datasets, and so we determined that it was probably the best model for creating a hate speech detection system. Please refer to the metrics in the appendix below. To put these metrics into perspective, we compared these results to those reported in a review article of hate speech detection methods by Rottger et al `[4]`. The review tests several models—including BERT fined-tuned models and Google Jigsaw Perspective—on various specific hate-speech detection tasks. Google Jigsaw was found to be superior overall with an accuracy of 0.766. Our models generally outperform these models because they were tested on a single task: generally categorizing speech as hate or non-hate. If our models were rigorously tested on more specific tasks like those mentioned in the review, e.g. specifically detecting threatening speech, they might display poorer metrics. 
A more comparable task to our generalized hate-speech detection would be general spam detection. We can use a review of spam detection methods by Crawford et al. `[5]` as a more analogous comparison. In their survey, they found that an accuracy of up to 0.996 was achieved using a naive Bayes model trained on movie reviews. All of our models lag behind this performance. The difference in featurization techniques between the reviewed models and our naive Bayes model is a likely cause for the performance discrepancy. Because we used TF-IDF, a sparse vectorization method, we can expect worse performance because sparse vectorization requires more data to effectively train. Our RNN model performs slightly better because they utilize dense vectorization, which requires less data. The benefit from dense vectorization offset the training complexity introduced by the RNN architecture.
We chose to use pretrained GloVe vectors as our word embeddings because of the nature of our dataset. Because of the large and sparse vocabulary as well as the presence of non-English tokens due to spelling and grammatical errors, word embedding techniques like Word2Vec and FastText had a hard time learning the embeddings for the dataset. These observations are included in `embeddings.py`, where a TSNE plot demonstrates the existence of nonstandard words and the weak ability for the word embeddings to capture meaning. Observe that for a common word like "dog," the nearest neighbors have little to nothing in common with respect to meaning. Therefore, using pretrained GloVe vectors trained on a reliable English dataset like Wikipedia was the best option. In the future, we would like to find a cleaner dataset and then use a context based embedding to try and capture the context of the hate speech examples. 
We decided that it might give us more insight to look at which sentences each model was failing on. We printed out the first five incorrect predictions with the actual label and predicted label. You can see them in the main jupyter notebook. We were then able to make general observations about each model.
Our Naive Bayes model predictably mislabeled non-hate sentences with words powerfully associated with hate speech, most often profanity, even if no hate was intended. Some false negative cases were also observed when hateful speech did not contain strong profanity and simply expressed animosity towards a particular group.
Our CNN exhibited similar behavior. A false positive was registered when the sentence contained non-hate insults. Another pattern we observed in the data was the poor labeling in the dataset. Sentences like 'bolest reba recite' were labeled as hate. In our first incorrect example for the CNN, the sentence 'i really hate being a b****' demonstrates the model's inability to deal with long-term negation dependencies. This explains how the LSTM generally performs better as it is able to take into account these long term dependencies. 
RNNs are very good at long-term dependencies, so we noticed that some of the examples that CNN misclassified were caught by the RNN. However, it seems that they also got some of the same sentences incorrect. We noticed again that many false positives were caused by profanity in sentences that otherwise had no hateful aspects. We would expect that the RNN would be better at considering context, but long sentences such as the sentence 'view on wikipedia...' still gave the RNN trouble. 
Because RNNs struggle with vanishing gradients, we would expect our combined CNN-LSTM model to perform better with long sentences such as these. As expected, many of the incorrect prediction examples for the CNN-LSTM were simply poorly labeled data. The single false negative example shown in the notebook is also poorly labeled.

## Conclusion
In our final project, we wanted to create a machine learning model that could accurately detect hate speech in social media posts. Our hope is that platforms could use these models to find hate speech and take it down to create a more safe environment for their users. We created four models: Naive Bayes, CNN, RNN, and CNN-LSTM. Our best results when we tested on our original dataset were with the RNN model, however, the RNN was not able to generalize to our other two datasets. Our more resilient models were Naive Bayes and our CNN-LSTM. If we want a quick and simple model that performs the best, Naive Bayes seems like the best choice for hate speech detection. Our model was nowhere nearly as accurate as other hate speech detection methods, however, maybe with more training data and time to experiment, we could make ours a little more competitive.
## References
`[1] D. Mody, “A curated hate speech dataset,” Mendeley Data, 03-Oct-2022. [Online]. Available: https://data.mendeley.com/datasets/9sxpkmm8xn/1. [Accessed: 12-Apr-2023].`

`[2] T-Davidson, “T-Davidson/hate-speech-and-offensive-language: Repository for the paper ‘Automated hate speech detection and the problem of offensive language’, ICWSM 2017,” GitHub. [Online]. Available: https://github.com/t-davidson/hate-speech-and-offensive-language. [Accessed: 12-Apr-2023].`

`[3] Vicomtech, “Vicomtech/Hate-speech-dataset: Hate speech dataset from Stormfront Forum manually labelled at sentence level.,” GitHub. [Online]. Available: https://github.com/Vicomtech/hate-speech-dataset. [Accessed: 12-Apr-2023]. `

`[4] P. Röttger, B. Vidgen, D. Nguyen, Z. Waseem, H. Margetts, and J. Pierrehumbert, “HateCheck: Functional Tests for Hate Speech Detection Models,” Proceedings of the 59th Annual Meeting of the Association for Computational Linguistics and the 11th International Joint Conference on Natural Language Processing (Volume 1: Long Papers). Association for Computational Linguistics, 2021. doi: 10.18653/v1/2021.acl-long.4.`

`[5] Crawford, M., Khoshgoftaar, T.M., Prusa, J.D. et al. Survey of review spam detection using machine learning techniques. Journal of Big Data 2, 23 (2015). https://doi.org/10.1186/s40537-015-0029-9`

# Appendix A

Figure 1: The results from the four models of our initial dataset

|  | Accuracy | Precision | Recall | F1|
|---|---|---|---|---|
|Naive Bayes|0.800|0.771|0.780|0.776
|CNN|0.805|0.795|0.754|0.774
RNN|0.829|0.806|0.809|0.808
CNN-LSTM|0.825|0.791|0.820|0.806

Figure 2: The results from using the Davidson Hate Speech dataset with the previously trained model 

|  | Accuracy | Precision | Recall | F1|
|---|---|---|---|---|
Naive Bayes|0.801|0.753|0.897|0.819
CNN|0.760|0.757|0.766|0.761
RNN|0.792|0.781|0.811|0.796
CNN-LSTM|0.790|0.770|0.827|0.797

Figure 3:: The results from using the Hate Speech dataset with the previously trained model 

|  | Accuracy | Precision | Recall | F1|
|---|---|---|---|---|
Naive Bayes|0.763|0.752|0.783|0.767
CNN|0.757|0.785|0.707|0.744
RNN|0.793|0.805|0.773|0.788
CNN-LSTM|0.787|0.791|0.780|0.786
