# NaturalLanguageProcessing
Course work completed for NLP course, Fall 2018.


Homework 2: 
Part-of-speech tagging; given a training file with 50,000 words, implement the viterbi algorithm for pos tagging. 

My Implementation:
To start off, I opened the training file from the Berkeley Restaurant corpus and took counts of all of the word-tag pairs to get the tag most frequently assigned to that word using a default dictionary/counter duo with the entire training set (in all honesty, after completing the assignment, this was probably useless since I will have to do it again with the beginning and end sentence markers but this was a part of my baseline). I chose not to split the training set to maximize the systems training. I then created a new text file with the training data, setting a designated beginning of sentence line ('0','<s<','<s<') and end of sentence line replacing the periods (index, '</s<','</s<'). I could’ve made this as lists but creating a new file was easier. I used this to created separate word, tag and index lists and remade a word-tag count using the default dictionary a second time with the beginning and end of sentence markers.


I made a count of the number of times a single word showed up in the wordlist and set a minimum value of 3 for unknown words. I chose 3 randomly but chose to stick with that throughout the rest of the testing after comparing the number of words that would be added if I changed this value to 4 or 5. The words that appeared 4 times had more meaning than what was captured at 3 (i.e. “thanks”, “end”, “sort”, “sorry”, “first”). Even though they only appear 4 times in the corpus, I still want them there if needed for future test sets. I created my emission probabilities by making an if statement separating the low frequency word case and the normal case. Since my matrix for the emission is all possible words against all possible tags, I used pandas to fill in my matrix values with “0” for the tags that are not associated with my word.


I created a bigram tag model by getting a bigram list, bigram counts and unigram counts to be used in the transition and added a 0.01 smoothing. I know other people in the class used higher values, but I didn’t want to increase this value as I was already getting probabilities above 1 in my transition.


Finally, I created my Viterbi matrix and ran through the sentences I created from the testlines. I made a list within a list of the entire sentences (testlines) from the test set posted on Moodle. This part of my code gave me the issue of not catching the last item in the list, but I moved the line that appended all the sentences within the if statement so the iteration would capture the last line. In my final output, I replaced all of the beginning and ending markers to the original training set format. I believe my system can work on any unseen data as I tried to retain as much training on my system as possible.


Homework 3:
Given a compilation of positive and negative hotel reviews from the training set, train the system to recognize positive and negative reviews from the test set. 

My Implementation:
For this homework, I chose the basic Naïve Bayes approach for sentiment analysis. I will explain two different approaches I used for the assignment and which had a better accuracy rate:


I created my own test set key by reading through the test set and marking the answers and ‘POS’ or ‘NEG’ based on the review. I did this so I would not have to split the limited training data available. I wanted to use as much as I could to train and test the test set for accuracy.


For my first approach, I created a library of both the positive and negative reviews, as well as a list of all words in each class. I imported log from math to support my log prior and log likelihood calculations from the naïve bayes algorithm. I also imported counter from collections to get word counts in each class. From the positive and negative review words, I removed any non-alphanumeric characters from the words from the list and made all words lowercase. The same treatment I applied to the training data was applied to the testing data (removing alphanumeric characters and lower-case the words). I also made a list of all the reviews and words in both the positive and negative training sets. In BOTH approaches I then created the log prior probabilities for the negative and positive classes separately and the log likelihood of the words based on word count (counter from collections). I then implemented the naïve bayes algorithm and output the results into a text file that I tested against my test set key using the evaluation script from the second homework. With this first approach my accuracy hovered around 76%. I assumed there might’ve been an issue with my counts but after checking through them multiple times I decided to try another approach.


For my second approach, I decided to leave the non-alphanumeric characters as they are but to remove any common words between the two classes, as well as all stop words that could be classifying the review incorrectly. From nltk, I imported the stop word corpus and turned it into a set. I then created positive and negative libraries, similar to the first approach, but this time straight from the training set without altering them in any way. I made counts of all the words in each class and together. I made a list of all common words that occur in both the positive and negative training sets and removed these words, as well as any stop words, from each class word count. Because I didn’t lowercase any of the words or take away punctuation in the training set, some stop words still managed to slip into the word count. However, after implementing the algorithm, my accuracy jumped up to 86%. I noticed that 6 of the 7 mistakes on the output of my system were positive reviews being labeled negative. After changing the approach several times and trying to find the best system, this is the one that gives me the best accuracy of 86%.


Homework 4:
Named Entity Recognizer, given passages from a biology textbook, build a system that will tag and recognize genes (BIO tagging system) in the test file. 

My Implementation:
For the final assignment I took my original Viterbi code from the second assignment and used it as the base for the NER approach. Originally, I had end of sentence markers with each period in the sentences but removed this since there were many more periods throughout the sentences. I left smoothing on the transition but also add-1 smoothing on the emission tables and logs on both probabilities.


For unknown words, I subcategorized the words based on their shape, as well as common prefixes and suffixes for genes. In addition to this, I made categories for numbers only, title- case words, long and short words and a standard UNK for anything that didn’t get captured in the above categories. After doing an 80/20 split on my training data, the dev set gave me an F-1 score of 45.62%.


I also attempted to increase accuracy (when I was still just around F1=30%) by stemming the training and test set words. This had a max F-1 score of 42.5% and I abandoned this method when I found that lowercase and uppercase letters had a great deal to do with the shape feature implementation.


For unseen test data, I believe the UNK categories will work well as I used common shape features, as well as prefixes and suffixes, that occurred more than once and were only categorized as B or I in the training set.
