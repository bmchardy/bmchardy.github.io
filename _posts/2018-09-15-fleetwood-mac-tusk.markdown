---
layout: post
title:  "An Analysis of Fleetwood Mac's Tusk"
date:   2020-09-15 00:00:00 -0400
categories: jekyll update
---
# An Introduction

Tusk is the twelfth studio album by [Fleetwood Mac](https://en.wikipedia.org/wiki/Fleetwood_Mac), a British-American rock group. The 20-track Tusk double album was released on October 12th, 1979 to little fanfare compared to their previous album and 1977 mega-hit, Rumours, which is [ranked by the RIAA](https://www.riaa.com/gold-platinum/?tab_active=top_tallies&ttt=T1A#search_section) as the 10th top-selling album ever. Tusk does not even make the top 100.

Regardless of how it looks in Rumours' shadow, Tusk is a very interesting experimental rock album and is therefore the ideal candidate for a bit of analysis ;)

# Tools

[Genius.com](https://genius.com/) was used to gather song lyrics. The [Julia Language](https://julialang.org/) and [Python](https://www.python.org/) were used to manipulate the data. Excel was used for visualization purposes throughout this project with the exception of the word cloud, which was produced using [Wordart.com](https://wordart.com/)

# Corpus Generation

[Corpora](https://en.wikipedia.org/wiki/Text_corpus) were generated for both the Tusk (1979) album as well as the previous Rumours (1977) album for the sake of comparison. Both Corpora were build from lyrics listed on the website Genius ([here](https://genius.com/albums/Fleetwood-mac/Tusk) for Tusk and [here](https://genius.com/albums/Fleetwood-mac/Rumours) for Rumours). These corpora were compiled to text files aptly named "corpus-tusk.txt" and "corpus-rumours.txt".

The corpora were pre-processed by removal of "!", "?", ".", ",", " ' ", "(", ")". All text was then converted to lowercase.

# Word Frequencies

By counting each term’s frequency over the entire Tusk corpus, we were able to compute word frequencies for the album. A list of 488 unique words was created by Julia from this album corpus. A snapshot of the first 20 terms is available below.

![Term frequency list](fleetwood-mac-tusk/freq_1.png "Term frequency list")

# Term frequency list

A logical thing to now do is question the usefulness of this metric. We have here an ordered list of the most frequent words in the album, however we are not seeing important words. With the exception of ‘love’, we are stuck with words such as ‘and’, ‘the’, ‘that’, etcetera. These are not found to be incredibly important since they are simply conjunctions.
The reason that we see so many words like this in the top 20 is for the simple reason that these words are just common words. You can right now go and look at any text and you will most likely see ‘and’, ‘the’, and ‘that’. This album is no exception.
To summarize: can we say for certain that ‘and’, ‘the’, and ‘that’ are among the most frequently used words across this album? Yes. However, can we say for certain that these words are among the most important words across this album? The answer would be no. There must be a better way of measuring term importance than by simply employing term frequencies.
Term Frequency — Inverse Document Frequencies
Another method we try is one called Term Frequency — Inverse Document Frequencies, or TFIDF for short. TFIDFs promote terms that have a high frequency across the corpus (such as ‘and’, ‘the’, ‘that’, ‘love’), but demote terms that usually have high frequency in text (such as ‘and’, ‘the’, ‘that’).
How does TFIDF work?
Imagine a function, TFIDF, that consumes a local term t’s frequency, tf (as seen in the previous sections) and a measure of how often term t occurs in the ‘real world’, df (called document frequency). Term frequency — inverse document frequency then maps as follows:
Image for post
TFIDF(tf, df) definition
The intuition here is that if the word occurs a lot locally, TFIDF maps to a greater number. If the word occurs a lot in everyday life, we want the TFIDF to map to a smaller number.
Note that in this case, the document frequencies were inverted prior to use, becoming inverse document frequencies (idf). The idfs can then be multiplied by the term frequency to form the equivalent function:
Image for post
An equivalent definition for TFIDF; TFIDF(tf, idf)
The question now becomes how do we build this inverse document frequency idf measure. We know that it needs to accurately represent how each term is used in day-to-day life. The answer involves the use of an external corpus.
Building Custom IDFs
The Open American National Corpus (OANC) was used to build these Inverse Document Frequencies. OANC was chosen because it contains approximately 15 million words (large corpora more accurately represent the "real world") and is freely available! The OANC corpus is hence chosen to represent how words are used in "real life".
For each word in this corpus, we take the number of documents (files) within this corpus, divide the number of occurrences of this term and then take the base-10 logarithm. This gives higher values for words that occur less in this "world" and lower values for the words like ‘the’ that appear more. This process was completed with help from Python. An example of the result is below:
Image for post
IDFs calculated over OANC
Note that ‘the’ has a low idf, implying that it will be of less importance, ‘theatre’ has a higher idf because it appears less in the "world", implying higher importance.
Computing the TFIDF of each word in the Tusk corpus
We now use our idfs calculated from OANC to compute the TFIDF of each term in our Tusk corpus. We simply apply the TFIDF(tf, idf) function defined above to each word’s frequency. Julia is once again used for this task.
The result is a new list of terms from the Tusk corpus, but this time, they are ordered by the TFIDF assigned to each word. Following is a sample of the top 20 words as rated by their TFIDF. Notice that the words with the highest frequencies are no longer necessarily at the top of the list. Also note that those pesky words from before such as ‘and’, ‘the’, and ‘that’ are no longer present at the top. We are left with words that could be seen as more meaningful to the album such as ‘you’, ‘me’, ‘i’, ‘love’, ‘’baby’, ‘over’, etcetera.
Image for post
TFIDFs computed for each word in the Tusk corpus
It is interesting to note that ‘you’ and ‘me’ are by far the highest ranked TFIDFs, with their ranking being 38% and 36% higher than the third TFIDF respectively. This is notable because Tusk deals, in a large way, with relationships.
Next note that ‘love’ and ‘honey’, while only having a term frequency of 39 and 24 respectively, occupy positions 4 and 5 of the TFIDF list. On the term frequency list, these terms occupied positions 12 and 23 respectively. This is a large promotion because of the additional weighting by the TFIDF function.
If the IDF values are to be believed, ‘love’ and ‘honey’ are meaningful terms.
Below is the ordered word frequency list compared to the ordered TFIDF list. Note the change in the type of words listed in the top 20.
Image for post
Top term frequencies vs. Top TFIDFs
The TFIDFs are used as weights to create a word cloud. A larger TFIDF leads to a larger representation on the word cloud since the word has a larger "importance" to the album.
Results
It is an exercise left to the reader to validate the findings of this project thus far; to determine whether the words deemed most "important" are indeed "important". Note that this can become somewhat subjective in nature.
Sources
The Open ANC (OANC) (Nancy Ide, Randi Reppen, Keith Suderman). Primary data (corpus). Department of Computer Science, Vassar College (New York US). Created 2011–05–29. Speech and Language Data Repository (SLDR/ORTOLANG).