# Response: Aditi Mungale

For this challenge, I decided to use Python as well as Microsoft Azure’s Text Analytics API.

## My approach

I approached this problem by first brainstorming about how I determine the tone of a piece of text. Typically, I'll look at the adjectives/verbs in a sentence and determine their strength based on what my mind categorizes as strongly worded (Ex. "Hate" has a more intense connotation than "dislike"). My ability to determine the connotation of various words comes from education and personal experiences. Additionally, the length of a sentence, the position of words, and the use of figurative language also affects the attitude of the text. 

To translate this process over to code, I recognized that I would most likely need an existing resource that had already scored the strength of connotation of various key words/phrases("the" or "for" aren't significant, but "love" or "like" are because they contribute to the sentiment of a passage). By assigning scores to each word, it would be easier to determine an overall sentiment score. For instance, "Hate" would be assigned a score of "-1" whereas "Dislike" would be assigned a score of "-.5"(a score of -1 would be the most negative while a score of 1 would be the most positive).

After some research, I decided to use the NLTK library because it already has a built-in sentiment analyzer(VADER) which fulfills all of my criteria(pre-sorted, scored). Since VADER is usually used for shorter pieces of text, I determined that I would need to seperate the text so that each sentence would be an item in an array. I would then need to make a loop where I would apply VADER’s analysis method to each item(sentence) in that array. This ended up working in my favor because I also got a sentiment score for each sentence. 

To ascertain the sentiment score of the entire text, the program would keep a running total of the sum of the scores of each sentence and the total number of sentences. When the program finishes, it should return the average rating of the text (final sum of the compound scores/number of sentences analyzed). When I was actually coding, I decided to add a total for all of the neg, neu, and pos scores too because I was curious to see how they would compare to the averaged compound score.

After running the input.txt with the python program, I wanted to see how it would compare to Microsoft Azure’s Text Analytics API's Sentiment Analysis feature. I divided the text into smaller String passages so I could properly use their API. Unfortunately, their tool doesn’t provide a compound score, unlike VADER, so I worked around this by averaging the total positive, negative, and neutral scores--like I did with VADER when calculating its overall score components. I've also included the results from this method below as well as my thoughts on the different scores.





## My estimation of the sentiment score:

After a first glance over the text, I estimated that the sentiment score would be close to 0 or be characterized as slightly“positive”. Lines 1-23 had more negative tone words whereas lines 25-53 had a more positive tone. Because the second half of the text is slightly larger, when combining the two sections together, there should be a “leftover” tone that is slightly positive. I would guess that the score is probably between .1 to .2.





## Code(how I used vader)
```
This is what I put in the command line to set up VADER: !pip install vaderSentiment

from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
with open("c:\\aditi\\input.txt") as text_file:
    contents = text_file.read()
	
replaced_string = contents.replace('"', '')
splited_string = replaced_string.split(".")

sid_obj = SentimentIntensityAnalyzer()

total_neg =0
total_compound=0
total_sentences =0
total_positive =0
total_neutral = 0

for item in splited_string:
	sentiment_dict = sid_obj.polarity_scores(item)
	total_compound = total_compound + sentiment_dict['compound']
	total_neg = total_neg + sentiment_dict['neg']
	total_positive = total_positive + sentiment_dict[ 'pos']
	total_neutral = total_neutral + sentiment_dict['neu']
	total_sentences = total_sentences + 1
	
print("total_compound ", total_compound)
print("total_neg ", total_neg/ total_sentences)
print("total_positive ", total_positive/ total_sentences)
print("total_neutral ", total_neutral/ total_sentences)
print("total_sentences ", total_sentences)	
print("Final Sentiment Score ", total_compound / total_sentences)
```


## Overall sentiment score:

*** **I found that the overall sentiment score was .13445.** *** after running input.txt through the VADER program. 
I also calculated the overall positive, negative, and neutral scores(total score/total sentences):

negative = 0.11369230769230769<br/>
positive=  0.13288461538461538<br/>
neutral = 0.7149999999999999


However, after running the same text through Azure’s text analytics service and calculating the overall (total score/total sentences), they scored the text as: 

negative=  0.4519230769230769<br/>
positive=  0.23884615384615387<br/>
neutral=  0.27076923076923076



## What it means:

According to most websites, a compound score between -.05 and .05 can be considered neutral. Because the score produced by the VADER program is definitely larger than this range, the text is categorized as “positive”. 

As shown previously, there was a difference between the score from the VADER program and Azure’s text analytics. I believe this variation can be explained by the way the two services are scored. With VADER, words are scored on a scale of -4 to 4 based on five set heuristics. Afterwards, these scores are normalised so that a score between -1 to 1 is returned. However with Azure’s service, they directly score keywords on a scale of -1 to 1, while using machine learning to sort the words. Essentially, the two services use two different models to produce their sentiment score.

For instance, when I ran the same sentence “"I had the best day of my life." through both programs, Azure’s service scored it as 1.00 whereas VADER scored it as .6369.



	

## Sources/Links

NLTK/Vader understanding:

https://towardsdatascience.com/design-your-own-sentiment-score-e524308cf787 <br/>
https://realpython.com/python-nltk-sentiment-analysis/ <br/>
https://towardsdatascience.com/sentimental-analysis-using-vader-a3415fef7664 <br/>
https://medium.com/@piocalderon/vader-sentiment-analysis-explained-f1c4f9101cd9 


Looked at code structure to see how they used VADER in a loop:<br/>
https://blog.quantinsti.com/vader-sentiment/ 

Most of code body came from here, such as how to set up VADER, and how they accessed individual parts of the VADER output(like how to isolate ‘compound’ variable):<br/>
https://www.geeksforgeeks.org/python-sentiment-analysis-using-vader/ 

How to read file in Python:<br/>
https://www.codegrepper.com/code-examples/python/how+to+read+files+in+python+with 

Python string replace method(to remove quotations):<br/>
https://www.w3schools.com/python/ref_string_replace.asp 
https://www.delftstack.com/howto/python/python-remove-quotes-from-string/ 

Python split method(split into sentences):<br/>
https://www.w3schools.com/python/ref_string_split.asp 

Loops in Python:<br/>
https://www.w3schools.com/python/python_for_loops.asp 

Azure sentiment analysis<br/>
https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-sentiment-analysis?tabs=version-3-1 <br/>
https://docs.microsoft.com/en-us/azure/cognitive-services/text-analytics/quickstarts/client-libraries-rest-api?tabs=version-3-1&pivots=programming-language-python 









___
## [](https://github.com/ACM-Research/Coding-Challenge-F21#no-collaboration-policy)No Collaboration Policy

**You may not collaborate with anyone on this challenge.**  You  _are_  allowed to use Internet documentation. If you  _do_  use existing code (either from Github, Stack Overflow, or other sources),  **please cite your sources in the README**.

## [](https://github.com/ACM-Research/Coding-Challenge-F21#submission-procedure)Submission Procedure

Please follow the below instructions on how to submit your answers.

1.  Create a  **public**  fork of this repo and name it  `ACM-Research-Coding-Challenge-F21`. To fork this repo, click the button on the top right and click the "Fork" button.

2.  Clone the fork of the repo to your computer using  `git clone [the URL of your clone]`. You may need to install Git for this (Google it).

3.  Complete the Challenge based on the instructions below.

4.  Submit your solution by filling out this [form](https://acmutd.typeform.com/to/zF1IcBGR).

## Assessment Criteria 

Submissions will be evaluated holistically and based on a combination of effort, validity of approach, analysis, adherence to the prompt, use of outside resources (encouraged), promptness of your submission, and other factors. Your approach and explanation (detailed below) is the most weighted criteria, and partial solutions are accepted. 

## [](https://github.com/ACM-Research/Coding-Challenge-S21#question-one)Question One

[Sentiment analysis](https://en.wikipedia.org/wiki/Sentiment_analysis) is a natural language processing technique that computes a sentiment score for a body of text. This sentiment score can quantify how positive, negative, or neutral the text is. The following dataset in  `input.txt`  contains a relatively large body of text.

**Determine an overall sentiment score of the text in this file, explain what this score means, and contrast this score with what you expected.**  If your solution also provides different metrics about the text (magnitude, individual sentence score, etc.), feel free to add it to your explanation.   

**You may use any programming language you feel most comfortable. We recommend Python because it is the easiest to implement. You're allowed to use any library/API you want to implement this**, just document which ones you used in this README file. Try to complete this as soon as possible as submissions are evaluated on a rolling basis.

Regardless if you can or cannot answer the question, provide a short explanation of how you got your solution or how you think it can be solved in your README.md file. However, we highly recommend giving the challenge a try, you just might learn something new!




