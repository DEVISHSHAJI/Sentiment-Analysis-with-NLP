import nltk
from nltk.corpus import movie_reviews
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.naive_bayes import MultinomialNB
from sklearn.metrics import accuracy_score, classification_report, confusion_matrix

nltk.download('movie_reviews')
documents = [(list(movie_reviews.words(fileid)), category)
             for category in movie_reviews.categories()
             for fileid in movie_reviews.fileids(category)]

import random
random.shuffle(documents)

all_words = nltk.FreqDist(word.lower() for word in movie_reviews.words())
word_features = list(all_words.keys())[:3000]

def document_features(document):
    document_words = set(document)
    features = {f'contains({word})': (word in document_words) for word in word_features}
    return features

featuresets = [(document_features(d), c) for (d, c) in documents]
train_set, test_set = train_test_split(featuresets, test_size=0.25, random_state=42)
classifier = nltk.NaiveBayesClassifier.train(train_set)

y_true = [category for (document, category) in test_set]
y_pred = [classifier.classify(document) for (document, category) in test_set]

print("Accuracy:", accuracy_score(y_true, y_pred))
print("\nClassification Report:\n", classification_report(y_true, y_pred))
print("\nConfusion Matrix:\n", confusion_matrix(y_true, y_pred))

import joblib

# Save the trained classifier to a file
joblib.dump(classifier, 'sentiment_classifier.pkl')
