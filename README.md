# A Machine Learning case-study to suggest tags based on the content that was there in the question posted on StackOverflow

<h2> Business Objective: </h2>
<p> Given a question{title, description}, predict tags.
 <br><b>Note: </b> There is no strict latency constraint but we have to make sure that we predict as many tags as possible with high precision and recall since incorrect tags could impact customer experience
  
<h2> Data Collection: </h2>
<p>
We get our data from :  https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/data 
</p>

<h2> Mapping to ML problem: Data</h2>
<p>The questions are randomized and contains a mix of verbose text sites as well as sites related to math and programming. The number of questions from each site may vary, and no filtering has been performed on the questions (such as closed questions).<br>
Dataset contains 6,034,195(~6M) rows. The columns in the table are:<br>
<ol>Id - Unique identifier for each question</ol>
<ol>Title - The question's title</ol>
<ol>Body - The body of the question</ol>
<ol>Tags - The tags associated with the question in a space-seperated format</ol>
</p>

<h2> Mapping to ML problem: Performance Metrices </h2>
<p>The problem here is a multi-label classification problem. It can be thought of as predicting properties of a data-point that are not mutually exclusive, such as topics that are relevant for a document. Taking into consideration the problem type and the business constraints, we boil down to the following performance metrices:
<ol><b>Micro averaged F1-score/ Mean F-score:</b> It can be seen as the weighted average of F1 score of each class https://www.kaggle.com/wiki/MeanFScore </ol>
<ol><b>Hamming loss:</b> The Hamming loss is the fraction of labels that are incorrectly predicted. https://www.kaggle.com/wiki/HammingLoss </ol>
</p>

<h2> EDA: Data Preprocessing and Analysis of Tags</h2>
<p> First, we remove duplicates from our dataset(we are left with roughly ~4M data points). In our tag-analysis(~42k tags), we found that a few tags occur a very large no of times but most of them occur sparsely. We also found that most of the questions have 2 or 3 tags. We also observed that majority of the most frequent tags are programming languages(C#, Java, pHp, JavaScript ...) and Android, iOS, Linux, Windows are among the top most frequent Operating Systems <br>
  Now we perform the following preprocessing steps:<br>
  <ol>Sample 0.5M data points(keeping the computational constraint in mind)</ol>
  <ol>Separate code from body and remove special characters from question title and description</ol>
  <ol>Give more weightage to title : Add title three times to the question</ol>
  <ol>Remove stop words (except 'C'), HTML tags</ol>
  <ol>Convert all the characters into small letters and use SnowballStemmer to stem the words</ol>
</p>

<h2> Data Preparation: </h2>
<p> To solve the multi-label classification problem, we adapt the ML algorithm to directly perform multi-label classification rather than transforming the problem into different subsets of problems. We also sample the number of tags instead of considering all of them(due to limitation of computing power).<br>
 We split our data into train and test subsets using a 80-20 split.<br>
 We use tf-idf vectorizer to vectorize our questions(title+body). We also try out CountVectorizer but tf-idf vectorized features give better results
</p>

<h2> Modeling: </h2>
<p> To solve our multi-label classification problem, we use Logistic Regression with OneVsRest Classifier. <br>
 Since our i/p data is high dimensional and we know that typically linear models work very well with high-dim data and moreover LR is computationally very cheap, thus we choose LR as our final model. We tried with Lin SVM too but it gave almost similar results<br>
 <b>Note</b>: We use hyperparameter tuning to find the best alpha for Logistic Regression
</p>

<h2> Conclusion: </h2>
<p> Our best model was able to achieve a Mean F-score of > 0.5
</p>
