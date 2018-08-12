# t-SNE *(t-distributed Stochastic Neighbor Embedding)*#


## Amazon Fine Food Reviews Analysis ##

Data Source: https://www.kaggle.com/snap/amazon-fine-food-reviews

The Amazon Fine Food Reviews dataset consists of reviews of fine foods from Amazon.

Number of reviews: 568,454
Number of users: 256,059
Number of products: 74,258
Timespan: Oct 1999 - Oct 2012
Number of Attributes/Columns in data: 10

## Attribute Information: ##

    Id
    ProductId - unique identifier for the product
    UserId - unqiue identifier for the user
    ProfileName
    HelpfulnessNumerator - number of users who found the review helpful
    HelpfulnessDenominator - number of users who indicated whether they found the review helpful or not
    Score - rating between 1 and 5
    Time - timestamp for the review
    Summary - brief summary of the review
    Text - text of the review

## Objective: ##

Given a review, we have to determine whether the review is positive (Rating of 4 or 5) or negative (rating of 1 or 2) based only on the words in the review. For visualization, we deploy t-SNE to plot the data in 2 dimensions to check whether the data is seperable in 2-D space.

This assignment is split in 5 parts for ease of execution.

1. **Pre Processing:** Removal of stop words, punctuation, special characters, HTML tags, stem-ming & lemmatization along with Data Preparation and integrity check is done in this step. The output is written to a file which is read by each of the remaining parts.<br/><br/>
2. **Bag of Words:** The output of 1st step is taken as input. The sparse matrix obtained from BoW text-to-vector method is fed to Truncated SVD for dimensionality reduction. t-SNE is done on TruncatedSVD dimension-reduced data & results are plotted.<br/><br/>
3. **TF-IDF:** The output of 1st step is taken as input. The sparse matrix obtained from TF-IDF text-to-vector method is fed to Truncated SVD and then to t-SNE for plotting.<br/><br/>
4. **Word2Vec:** The output of 1st step is taken as input. Dense matrix is obtained from Word2Vec. Hence, further dimensionality methods are not required. Average-Word2Vec is obtained for each review & Results are plotted after running t-SNE.<br/><br/>
5. **TF-IDF weighted Word2Vec:** The output of 1st step is taken as input. Multiply the W2V value with TF-IDF weight and do the same steps as in Part 4.

## TSNE Plots ##

### Bag of Words ###
![](https://github.com/AdroitAnandAI/t-SNE/blob/master/Images/BoW1.png)
![](https://github.com/AdroitAnandAI/t-SNE/blob/master/Images/BoW2.png)
![](https://github.com/AdroitAnandAI/t-SNE/blob/master/Images/BoW3.png)
![](https://github.com/AdroitAnandAI/t-SNE/blob/master/Images/BoW4.png)

### TF-IDF ###

![](https://github.com/AdroitAnandAI/t-SNE/blob/master/Images/tfidf1.png)
![](https://github.com/AdroitAnandAI/t-SNE/blob/master/Images/tfidf2.png)

### Word2Vec ###

![](https://github.com/AdroitAnandAI/t-SNE/blob/master/Images/w2v1.png)
![](https://github.com/AdroitAnandAI/t-SNE/blob/master/Images/w2v2.png)

### TF-IDF weighted Word2Vec ###

![](https://github.com/AdroitAnandAI/t-SNE/blob/master/Images/tfidf-w2v1.png)
![](https://github.com/AdroitAnandAI/t-SNE/blob/master/Images/tfidf-w2v2.png)

## Main Challenges Encountered: ##

1. Heavy memory usage of t-SNE: In my personal box with 4GB memory, t-SNE was giving frequent memory exceptions, which is obvious. Even after reducing the data points to a subset of < 5K, the program was throwing memory exception. <br/><br/> **Solution Found:** *Ran the code in* **Google Colabs** *Platform. The platform enabled me to use more than 10GB memory and to run the code faster. But there are some hiccups which I faced such as frequent disconnections and kernel dying after 12-13GB memory usage. Also, packages need to be re-installed on every fresh run.*

2. Time Complexity of t-SNE: t-SNE was taking so many hours to execute. But we have to run t-SNE many times for different perplexity values and steps, to check whether any shape or separation is coming out.<br/><br/>**Solution Found:** *Used the* **Multicore-TSNE implementation** *which does parallelization of t-SNE algorithm using multiple cores. The dataset is pretty huge having 3,64,000 data points. We have downsampled the data to 5-25K for faster execution. This significantly reduced execution time.*

3. Interfacing in Google Colabs: The input file in google colabs cannot be loaded from local drives. It is capable to fetch the file from google cloud or gdrive. But google cloud is expensive and in gdrive connection is less stable.<br/><br/>**Solution Found:** *Installed a* **Drive FUSE wrapper & authorised tokens for colab** *to load the google drive as a fuse filesystem. After loading the file system, we can get all the input files from the ’drive/’ directory as if in a local drive. (code attached as ’Google_colab_dump.pdf’)*

# Conclusion: #
1. t-SNE was executed with different perplexity and step values for BoW, TF-IDF, W2V & TF-IDF weighted W2V. The output t-sne plot with different parameters are given in ’tsne_plots.pdf’ file (separately).<br/><br/>
2. Using TF-IDF weighted W2V, we can find some structure of the data, but separation is not there, using any of the above 4 methods.<br/><br/>
3. It is to be noted that if the underlying data in original dimension is overlapped, then tsne plots will also reflect the same in lower dimensions.


