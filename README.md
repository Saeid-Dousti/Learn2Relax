# Learn2Relax
Stress detection on Social Media ([presentation](https://docs.google.com/presentation/d/1iZFROfJrI9I-OIB1vEbSchwLOnL0VAU-T9Yg5wtN2lM/edit#slide=id.p)).

## Motivation
Social media is a major platform where people express their worries and stresses across the world. Learn2Relax was built in order to analyze content and identify stress from reddit data by deploying NLP techniques. Supervised learning with pre-trained word embeddings was deployed on unlabeled data by both discrete and neural models. 

### Installation
- The model is tested on Python 3.7 with dependencies listed in `build/requirements.txt`. To install these Python dependencies, please run 
> `pip install -r requirements.txt` 

Or if you prefer to use conda, 
> `conda install --file requirements.txt`

- Download nltk data packages 
```
python Learn2Relax/configs/config.py
```

### Additional Setup (Optional)
- Install tensorflow for GPU to run BERT model on GPU
```
pip install tensorflow-gpu==1.15
```
- Download and install [Docker application](https://docs.docker.com/get-docker/) to create a containerized application for the inference demo. If you are new to Docker, here’s a [quickstart guide](https://docs.docker.com/get-started/).

## Data

eded data is retrieved from [Dreaddit: A Reddit Dataset for Stress Analysis in Social Media](https://arxiv.org/abs/1911.00133). 

Picture below shows words similarities in the dataset.
<img src="figs/similar_words.png" width="1000">

Top 20 frequent words in stressed posts and non-stressed posts are shown below.

<img src="figs/Top_20_words.png" width="1000">

## Run inference App
To run the streamlit web app in your browser, run following command after all required packages are installed and conda environment is activated.
```
cd Learn2Relax/streamlit
streamlit run app.py
```
If no browser window pops up, paste the Network URL in the browser and you will be able to see the app interface as below. 
<img src="figs/streamlit4.gif" width="500">

To create a containerized application, run
```
docker build --tag learn2relax-streamlit:1.0 .
```

## Analysis
Features for the labeled dataset were generated by five different feature extraction methods:<br> unigram TF-IDF, bigram TF-IDF, Word2Vec with TF-IDF as weights and BERT embeddings

Word2Vec embeddings were also trained with 190k unlabeled Reddit posts. 

After feature extraction, 9 classification models were trained: <br>Logistic regression, Naive Bayes, SVM, AdaBoost, Gradient Boosting, Decision Tree, Random forest, XGBoost and BERT 

### Results
|Featurization Method|Best Model|Accuracy|Precision|Recall|F1-Score|
|:-------------|:----------|:--------|:---------|:------|:--------|
|Unigram TF-IDF|Logistic Regression|84.23%  |82.87%   |90.36%|86.46%  |
|Bigram TF-IDF|SVM       |84.23%  |83.24%   |89.76%|86.38%  |
|Word2Vec + TF-IDF|XGBoost   |85.23%  |82.80%   |92.77%|87.50%  |
|Pretrained Embeddings|XGBoost   |84.56%  |81.58%   |93.37%|87.08%  |
|BERT Embeddings|BERT      |92.74%  |92.90%   |94.58%|93.73%  |

<img src="figs/supervised_results.png" width="650">

||Traditional ML Models        |BERT  |
|:---|:-----------------------------|:------|
|Avg. Training Time|00.449530 sec                |3 min 48.131239 sec|    
 
- Recall is the most important metric here because we want to best prevent misclassification of stress posts as non-stressful which helps us better understand the stressful contents in social media.

- BERT is the most robust model with all four metrics the highest.
- All models are able to provide a confidence score in addition to prediction.
